---
title: 联系人之主动查询SIM卡联系人流程
date: 2017-04-13 19:43:12
tags: android
categories:
- 联系人
- 流程
password: 
---

最近开始着手联系人模块，需要对联系人的各个模块进行梳理，先从开机查询`SIM`卡联系人流程开始吧。


# 主动获取联系人

在类
`frameworks/opt/telephony/src/java/com/android/internal/telephony/IccProvider.java`
中提供了一个接口，这个接口可以供其他应用主动去访问获取`SIM`卡中的联系人
**query()**方法进行`SIM`卡的查询。可以在`LOG`看到此方法打印的LOG
```
10-30 19:30:51.394 D/IccProvider( 2004): [IccProvider] getRequestSubId url: content://icc/prop/subId/6

```

代码中的具体实现如下

```java
@Override
public Cursor query(Uri url, String[] projection, String selection,
        String[] selectionArgs, String sort) {
    if (DBG) log("query"+" ,url = "+url.toString());

    switch (URL_MATCHER.match(url)) {
        case ADN://默认SIM卡联系人
            return loadFromEf(IccConstants.EF_ADN,
                    SubscriptionManager.getDefaultSubscriptionId());

        case ADN_SUB://查询不同卡联系人
            return loadFromEf(IccConstants.EF_ADN, getRequestSubId(url));

        case FDN://查询默认FDN号码
            return loadFromEf(IccConstants.EF_FDN,
                    SubscriptionManager.getDefaultSubscriptionId());

        case FDN_SUB://查询不同卡的FDN号码
            return loadFromEf(IccConstants.EF_FDN, getRequestSubId(url));

        case SDN:
            return loadFromEf(IccConstants.EF_SDN,
                    SubscriptionManager.getDefaultSubscriptionId());

        case SDN_SUB:
            return loadFromEf(IccConstants.EF_SDN, getRequestSubId(url));

        case ADN_ALL:查询所有联系人
            return loadAllSimContacts(IccConstants.EF_ADN);

        default:
            throw new IllegalArgumentException("Unknown URL " + url);
    }
}

```

我们现在只看查询联系人的流程(ADN)，继续跟踪**loadFromEf()**方法
`LOG`中的打印信息如下

```
10-30 19:30:51.496 D/IccProvider( 2004): [IccProvider] query start ADN_SUB, subId = 6
```
其中`efType`是`SIM`卡中的地址空间，里面会存放对应的联系人字段，具体字段定义是在类
`frameworks/opt/telephony/src/java/com/android/internal/telephony/uicc/IccConstants.java`
中有定义

```java
public interface IccConstants {
    // GSM SIM file ids from TS 51.011
    static final int EF_ADN = 0x6F3A;
    static final int EF_FDN = 0x6F3B;
    static final int EF_GID1 = 0x6F3E;
    static final int EF_GID2 = 0x6F3F;
    static final int EF_SDN = 0x6F49;
    static final int EF_EXT1 = 0x6F4A;
    static final int EF_EXT2 = 0x6F4B;
    static final int EF_EXT3 = 0x6F4C;
    static final int EF_EXT5 = 0x6F4E;
    static final int EF_EXT6 = 0x6fc8;   // Ext record for EF[MBDN]
    static final int EF_MWIS = 0x6FCA;
    static final int EF_MBDN = 0x6fc7;
    static final int EF_PNN = 0x6fc5;
    static final int EF_OPL = 0x6fc6;
    static final int EF_SPN = 0x6F46;
    static final int EF_SMS = 0x6F3C;
    static final int EF_ICCID = 0x2fe2;
    ....
}
```
方法的具体实现如下

```java
private MatrixCursor loadFromEf(int efType, int subId) {
    log("query start " + intToString(efType) + ", subId = " + subId);
    if (DBG) log("loadFromEf: efType=0x" +
            Integer.toHexString(efType).toUpperCase() + ", subscription=" + subId);

    List<AdnRecord> adnRecords = null;
    try {
        //实际实现framework/opt/telephony/src/java/com/android/internal/telephony/UiccPhoneBookController.java
        IIccPhoneBook iccIpb = IIccPhoneBook.Stub.asInterface(
                ServiceManager.getService("simphonebook"));
        if (iccIpb != null) {
            //此类AdnRecord是联系人详细信息类
            adnRecords = iccIpb.getAdnRecordsInEfForSubscriber(subId, efType);
        }
    } catch (RemoteException ex) {
        // ignore it
    } catch (SecurityException ex) {
        if (DBG) log(ex.toString());
    }

    if (adnRecords != null) {
        // Load the results
        final int N = adnRecords.size();
        // final MatrixCursor cursor = new MatrixCursor(ADDRESS_BOOK_COLUMN_NAMES, N);
        log("adnRecords.size=" + N);
        for (int i = 0; i < N ; i++) {
            //把联系人导入到数据库
            loadRecord(adnRecords.get(i), cursor, i);
        }
        log("query end " + intToString(efType)+ ", subId = "+subId);
        return cursor;
    } else {
        // No results to load
        Rlog.w(TAG, "Cannot load ADN records");
        return new MatrixCursor(COLUMN_NAMES_FOR_ASUS);
    }
}
```
主要查看方法`getAdnRecordsInEfForSubscriber()`，这个方法是在类
`frameworks/opt/telephony/src/java/com/android/internal/telephony/UiccPhoneBookController.java`
中实现的，具体实现如下

```java
@Override
public List<AdnRecord> getAdnRecordsInEfForSubscriber(int subId, int efid)
       throws android.os.RemoteException {
    IccPhoneBookInterfaceManager iccPbkIntMgr =
                         getIccPhoneBookInterfaceManager(subId);
    if (iccPbkIntMgr != null) {
        //主要查看此方法
        return iccPbkIntMgr.getAdnRecordsInEf(efid);
    } else {
        Rlog.e(TAG,"getAdnRecordsInEf iccPbkIntMgr is" +
                  "null for Subscription:"+subId);
        return null;
    }
}
```

具体跟踪`getAdnRecordsInEf()`方法，这个方法是在类
`frameworks/opt/telephony/src/java/com/android/internal/telephony/IccPhoneBookInterfaceManager.java`
中实现的，具体如下

```java
public List<AdnRecord> getAdnRecordsInEf(int efid) {

    //检查权限
    if (mPhone.getContext().checkCallingOrSelfPermission(
            android.Manifest.permission.READ_CONTACTS)
            != PackageManager.PERMISSION_GRANTED) {
        throw new SecurityException(
                "Requires android.permission.READ_CONTACTS permission");
    }

    //判断是GSM SIM还是USIM SIM
    efid = updateEfForIccType(efid);
    if (DBG) logd("getAdnRecordsInEF: efid=0x" + Integer.toHexString(efid).toUpperCase());

    synchronized(mLock) {
        checkThread();
        AtomicBoolean status = new AtomicBoolean(false);
        Message response = mBaseHandler.obtainMessage(EVENT_LOAD_DONE, status);

        if (isSimPhoneBookEnabled() &&
            (efid == IccConstants.EF_PBR || efid == IccConstants.EF_ADN)) {
            if (mSimPbAdnCache != null) {
                //从SIM卡中获取联系人
                mSimPbAdnCache.requestLoadAllAdnLike(response);
                waitForResult(status);
            } else {
                loge("Failure while trying to load from SIM due to uninit  sim pb adncache");
            }
        } else {
            if (mAdnCache != null) {
                mAdnCache.requestLoadAllAdnLike(
                        efid, mAdnCache.extensionEfForEf(efid), response);
                waitForResult(status);
            } else {
                loge("Failure while trying to load from SIM due to uninitialised adncache");
            }
        }
    }
    return mRecords;
}
```
`LOG`打印信息

```
10-30 19:30:52.339 D/IccPhoneBookIM( 2004): [IccPbInterfaceManager] getAdnRecordsInEF: efid=0x4F30
```
查看联系人获取方法`requestLoadAllAdnLike()`,具体实现是在类
`frameworks/opt/telephony/src/java/com/android/internal/telephony/uicc/SimPhoneBookAdnRecordCache.java`
中实现的，具体实现如下

```java
public void
requestLoadAllAdnLike(Message response) {
    if (mAdnLoadingWaiters != null) {
        mAdnLoadingWaiters.add(response);
    }

    synchronized (mLock) {
        //判断联系人是否已经从SIM卡中读取，防止多次加载
        if (!mSimPbRecords.isEmpty()) {
            log("ADN cache has already filled in");
            if (mRefreshAdnCache) {
                mRefreshAdnCache = false;
                refreshAdnCache();
            } else {
                notifyAndClearWaiters();
            }

            return;
        }

        queryAdnRecord();
    }
}
```
我们是看的开机加载流程，所以LOG打印信息如下

`LOG`打印信息

```
10-30 19:30:51.404 D/SimPhoneBookAdnRecordCache( 2004): start to queryAdnRecord

```
说明方法走的是`queryAdnRecord`，此方法具体实现如下

```java
public void queryAdnRecord () {
    mRecCount = 0;
    mAdnCount = 0;
    mValidAdnCount = 0;
    mEmailCount = 0;
    mAddNumCount = 0;
    
    
    log("start to queryAdnRecord");

    //已经开始走到ril层，查询SIM卡中联系人，查询结果在信息EVENT_QUERY_ADN_RECORD_DONE中接收，这个查询的结果不是联系人的详细信息，而是SIM卡中联系人最大数量、当前数量、可用数量等
    mCi.getAdnRecord(obtainMessage(EVENT_QUERY_ADN_RECORD_DONE));
    //此方法是监听从modem层接收主动上报信息的，应该是发起联系人查询后主动上报的，这个获取的是详细联系人信息，也就是SIM卡中每一个联系人的信息
    mCi.registerForAdnRecordsInfo(this, EVENT_LOAD_ADN_RECORD_DONE, null);

    try {
        //等待查询结束
        mLock.wait();
    } catch (InterruptedException e) {
        Rlog.e(LOG_TAG, "Interrupted Exception in queryAdnRecord");
    }

    //取消监听
    mCi.unregisterForAdnRecordsInfo(this);
}
```
这个方法已经开始走到`ril`层了，继续
`frameworks/opt/telephony/src/java/com/android/internal/telephony/RIL.java`
中的`getAdnRecord`方法

```java
@Override
public void
getAdnRecord(Message result) {
    RILRequest rr = RILRequest.obtain(RIL_REQUEST_GET_ADN_RECORD, result);

    if (RILJ_LOGD) riljLog(rr.serialString() + "> " + requestToString(rr.mRequest));

    send(rr);
}
```
`EVENT_QUERY_ADN_RECORD_DONE`查询事件反馈的`LOG`中的信息如下

```
10-30 19:30:51.404 D/RILJ    ( 2004): [3877]> RIL_REQUEST_GET_ADN_RECORD [SUB0]
10-30 19:30:51.414 D/RILJ    ( 2004): [3877]< RIL_REQUEST_GET_ADN_RECORD {500, 57, 200, 1, 500, 1, 42, 15, 0, 0, 0, 14, 40, 0} [SUB0]//返回的查询结果

```
返回的结果会在类
`frameworks/opt/telephony/src/java/com/android/internal/telephony/uicc/SimPhoneBookAdnRecordCache.java`
中接收到消息进行处理，稍后会介绍

`EVENT_QUERY_ADN_RECORD_DONE`的具体反馈结果如下

```java
log("Max ADN count is: " + mAdnCount
    + ", Valid ADN count is: " + mValidAdnCount
    + ", Email count is: " + mEmailCount
    + ", Valid Email count is: " + mValidEmailCount
    + ", Add number count is: " + mAddNumCount
    + ", Valid Add number count is: " + mValidAddNumCount
    
    + ", Max number length is: " + mNumLen
    + ", Max addnumber length is: " + mAddNumLen
    + ", Max name length is: " + mNameLen
    + ", Max mEmailLen length is: " + mEmailLen
    + ", Max secondName count is: " + mSecondNameCount
    + ", Valid secondName count is: " + mValidSecondNameCount
    + ", Max secondName length is: " + mSecondNameLen
    + ", Is hidden entry support : " + mEditMode
 );
05-10 01:51:46.383 D/SimPhoneBookAdnRecordCache( 2108): Max ADN count is: 500, Valid ADN count is: 1, Email count is: 100, Valid Email count is: 0, Add number count is: 500, Valid Add number count is: 0, Max number length is: 42, Max addnumber length is: 15, Max name length is: 14, Max mEmailLen length is: 42, Max secondName count is: 0, Valid secondName count is: 0, Max secondName length is: 0, Is hidden entry support : 0

//实际联系人存储了一个   name:test1 number:11111 email:null

05-10 01:56:27.877 D/SimPhoneBookAdnRecordCache( 2108): Max ADN count is: 500, Valid ADN count is: 2, Email count is: 100, Valid Email count is: 0, Add number count is: 500, Valid Add number count is: 0, Max number length is: 42, Max addnumber length is: 15, Max name length is: 14, Max mEmailLen length is: 42, Max secondName count is: 0, Valid secondName count is: 0, Max secondName length is: 0, Is hidden entry support : 0
//实际联系人存储了两个   name:test1 number:11111 email:null name:test2 number:22222 email:null


05-10 02:06:30.131 D/SimPhoneBookAdnRecordCache( 2108): Max ADN count is: 500, Valid ADN count is: 3, Email count is: 100, Valid Email count is: 1, Add number count is: 500, Valid Add number count is: 0, Max number length is: 42, Max addnumber length is: 15, Max name length is: 14, Max mEmailLen length is: 42, Max secondName count is: 0, Valid secondName count is: 0, Max secondName length is: 0, Is hidden entry support : 0

//实际联系人存储了三个   name:test1 number:11111 email:null name:test2 number:22222 email:null name:test3 number:33333 email:33333@qq.com

05-10 02:53:07.503 D/SimPhoneBookAdnRecordCache( 2108): Max ADN count is: 500, Valid ADN count is: 3, Email count is: 100, Valid Email count is: 1, Add number count is: 500, Valid Add number count is: 1, Max number length is: 42, Max addnumber length is: 15, Max name length is: 14, Max mEmailLen length is: 42, Max secondName count is: 0, Valid secondName count is: 0, Max secondName length is: 0, Is hidden entry support : 0

//实际联系人存储了三个   name:test1 number:11111 number2:111112 email:null name:test2 number:22222 email:null name:test3 number:33333 email:33333@qq.com

05-10 03:27:48.752 D/SimPhoneBookAdnRecordCache( 2108): Max ADN count is: 500, Valid ADN count is: 3, Email count is: 100, Valid Email count is: 1, Add number count is: 500, Valid Add number count is: 2, Max number length is: 42, Max addnumber length is: 15, Max name length is: 14, Max mEmailLen length is: 42, Max secondName count is: 0, Valid secondName count is: 0, Max secondName length is: 0, Is hidden entry support : 0

//实际联系人存储了三个   name:test1 number:11111 number2:111112 email:null name:test2 number:22222 number2:222222 email:null name:test3 number:33333 email:33333@qq.com

05-10 03:34:48.723 D/SimPhoneBookAdnRecordCache( 2108): Max ADN count is: 500, Valid ADN count is: 4, Email count is: 100, Valid Email count is: 2, Add number count is: 500, Valid Add number count is: 2, Max number length is: 42, Max addnumber length is: 15, Max name length is: 14, Max mEmailLen length is: 42, Max secondName count is: 0, Valid secondName count is: 0, Max secondName length is: 0, Is hidden entry support : 0

//实际联系人存储了四个  name:test1 number:66666 email:666666@qq.com name:test1 number:11111 number2:111112 email:null name:test2 number:22222 number2:222222 email:null name:test3 number:33333 email:33333@qq.com

总结:Max ADN--最大可存储联系人 500  Valid ADN count--当前SIM卡存储的联系人数量 Email count--最大可存储邮箱数 Valid Email count--当前存储的邮箱数Max number length--最大可存储号码长度 超过会提示存储失败 Max addnumber length--怀疑是一个联系人可以添加的最大号码个数(待验证)
Add number count--可添加第二号码的最大个数
Valid Add number count-第二号码个数,一个联系人有多个号码,其余号码就是这个数值
Max name length--名称最大长度(似乎联系人模块默认限制可输入长度是14)
Max mEmailLen length--邮箱最大长度,超过会存储失败

//联系人号码长度超过会报此类错误,需要查看modem文档确认
05-10 02:15:41.439 D/RilRequest( 2108): [5221]< RIL_REQUEST_UPDATE_ADN_RECORD error: com.android.internal.telephony.CommandException: SYSTEM_ERR ret={4}

//联系人中邮箱的长度超过最大值,保存失败
05-10 02:44:38.376 D/RilRequest( 2108): [5302]< RIL_REQUEST_UPDATE_ADN_RECORD error: com.android.internal.telephony.CommandException: MODEM_ERR ret={4}

```


对应的`LOG`如下

```
10-30 19:30:51.435 D/SimPhoneBookAdnRecordCache( 2004): Max ADN count is: 500, Valid ADN count is: 57, Email count is: 200, Valid Email count is: 1, Add number count is: 500, Valid Add number count is: 1, Max number length is: 42, Max addnumber length is: 15, Max name length is: 14, Max mEmailLen length is: 40, Max secondName count is: 0, Valid secondName count is: 0, Max secondName length is: 0, Is hidden entry support : 0

```

我们继续看`EVENT_LOAD_ADN_RECORD_DONE`监听事件

```java
class RILReceiver implements Runnable {
    byte[] buffer;

    RILReceiver() {
        buffer = new byte[RIL_MAX_COMMAND_BYTES];
    }

    @Override
    public void
    run() {
        ....
        //处理底层上报的消息
        processResponse(p);
    }
private void
processResponse (Parcel p) {
    int type;

    type = p.readInt();

    if (type == RESPONSE_UNSOLICITED || type == RESPONSE_UNSOLICITED_ACK_EXP) {
        //主动上报
        processUnsolicited (p, type);
    } else if (type == RESPONSE_SOLICITED || type == RESPONSE_SOLICITED_ACK_EXP) {
        RILRequest rr = processSolicited (p, type);
        if (rr != null) {
            if (type == RESPONSE_SOLICITED) {
                decrementWakeLock(rr);
            }
            rr.release();
            return;
        }
    } else if (type == RESPONSE_SOLICITED_ACK) {
        int serial;
        serial = p.readInt();

        RILRequest rr;
        synchronized (mRequestList) {
            rr = mRequestList.get(serial);
        }
        if (rr == null) {
            Rlog.w(RILJ_LOG_TAG, "Unexpected solicited ack response! sn: " + serial);
        } else {
            decrementWakeLock(rr);
            if (RILJ_LOGD) {
                riljLog(rr.serialString() + " Ack < " + requestToString(rr.mRequest));
            }
        }
    }
}
private void
processUnsolicited (Parcel p, int type) {
    int response;
    Object ret;

    response = p.readInt();

    // Follow new symantics of sending an Ack starting from RIL version 13
    if (getRilVersion() >= 13 && type == RESPONSE_UNSOLICITED_ACK_EXP) {
        Message msg;
        RILRequest rr = RILRequest.obtain(RIL_RESPONSE_ACKNOWLEDGEMENT, null);
        msg = mSender.obtainMessage(EVENT_SEND_ACK, rr);
        acquireWakeLock(rr, FOR_ACK_WAKELOCK);
        msg.sendToTarget();
        if (RILJ_LOGD) {
            riljLog("Unsol response received for " + responseToString(response) +
                    " Sending ack to ril.cpp");
        }
    }

    try {switch(response) {
          //个人猜测是查询完后会上报此命令
          case RIL_UNSOL_RESPONSE_ADN_RECORDS:
                if (RILJ_LOGD) unsljLog(response);

                if (mAdnRecordsInfoRegistrants != null) {
                    mAdnRecordsInfoRegistrants.notifyRegistrants(
                                            new AsyncResult(null, ret, null));
                }
                break;
    }
}

```
打印的`LOG`如下

```
10-30 19:30:51.435 D/SimPhoneBookAdnRecordCache( 2004): Loading ADN record done
```
处理此消息跟`EVENT_QUERY_ADN_RECORD_DONE`是同一个`handle`中

```java
@Override
public void
handleMessage(Message msg) {
    AsyncResult ar = (AsyncResult)msg.obj;
    int efid;

    switch(msg.what) {
        case EVENT_INIT_ADN_DONE:
            ar = (AsyncResult)msg.obj;
            if (ar.exception == null) {
                invalidateAdnCache();

                Intent intent = new Intent(ACTION_ADN_INIT_DONE);
                //intent.addFlags(Intent.FLAG_RECEIVER_REPLACE_PENDING);//delete by liuming for only one SIM can load
                SubscriptionManager.putPhoneIdAndSubIdExtra(intent, mPhoneId);
                log("broadcast intent ACTION_ADN_INIT_DONE for mPhoneId=" + mPhoneId+ ", isAdnInitDone = "+isAdnInitDone);
                mContext.sendStickyBroadcastAsUser(intent, UserHandle.ALL);
            } else {
                log("Init ADN done Exception: " + ar.exception);
            }

            break;

        case EVENT_QUERY_ADN_RECORD_DONE:
            log("Querying ADN record done");
            if (ar.exception != null) {
                synchronized (mLock) {
                    mLock.notify();
                }

                for (Message response : mAdnLoadingWaiters) {
                    sendErrorResponse(response, "Query adn record failed" + ar.exception);
                }
                mAdnLoadingWaiters.clear();
                break;
            }
            
            mAdnCount = ((int[]) (ar.result))[0];
            mValidAdnCount = ((int[]) (ar.result))[1];
            mEmailCount = ((int[]) (ar.result))[2];
            mValidEmailCount = ((int[]) (ar.result))[3];
            mAddNumCount = ((int[]) (ar.result))[4];
            mValidAddNumCount = ((int[]) (ar.result))[5];
            
            
            log("Max ADN count is: " + mAdnCount
                + ", Valid ADN count is: " + mValidAdnCount
                + ", Email count is: " + mEmailCount
                + ", Valid Email count is: " + mValidEmailCount
                + ", Add number count is: " + mAddNumCount
                + ", Valid Add number count is: " + mValidAddNumCount
                
                + ", Max number length is: " + mNumLen
                + ", Max addnumber length is: " + mAddNumLen
                + ", Max name length is: " + mNameLen
                + ", Max mEmailLen length is: " + mEmailLen
                + ", Max secondName count is: " + mSecondNameCount
                + ", Valid secondName count is: " + mValidSecondNameCount
                + ", Max secondName length is: " + mSecondNameLen
                + ", Is hidden entry support : " + mEditMode
             );

            if(mValidAdnCount == 0 || mRecCount == mValidAdnCount) {
                sendMessage(obtainMessage(EVENT_LOAD_ALL_ADN_LIKE_DONE));
            }
            break;

        //获取联系人的详细信息
        case EVENT_LOAD_ADN_RECORD_DONE:
            log("Loading ADN record done");
            if (ar.exception != null) {
                break;
            }
            //获得查询到的联系人
            SimPhoneBookAdnRecord[] AdnRecordsGroup = (SimPhoneBookAdnRecord[])(ar.result);
            for (int i = 0 ; i < AdnRecordsGroup.length ; i++) {
                if (AdnRecordsGroup[i] != null) {
                    mSimPbRecords.add(new AdnRecord(0,
                                            AdnRecordsGroup[i].getRecordIndex(),
                                            AdnRecordsGroup[i].getAlphaTag(),
                                            AdnRecordsGroup[i].getNumber(),
                                            AdnRecordsGroup[i].getEmails(),
                                            AdnRecordsGroup[i].getAdNumbers()));
                    mRecCount ++;
                }
            }

            if(mRecCount == mValidAdnCount) {
                sendMessage(obtainMessage(EVENT_LOAD_ALL_ADN_LIKE_DONE));
            }
            break;
        //获取SIM卡联系人之后，执行此方法，此方法是用来给上层发送消息的
        case EVENT_LOAD_ALL_ADN_LIKE_DONE:
            log("Loading all ADN records done");
            synchronized (mLock) {
                mLock.notify();
            }

            notifyAndClearWaiters();
            break;

        case EVENT_UPDATE_ADN_RECORD_DONE:
            log("Update ADN record done");
            Exception e = null;

            if (ar.exception == null) {
                int index = msg.arg1;
                AdnRecord adn = (AdnRecord) (ar.userObj);
                int recordIndex = ((int[]) (ar.result))[0];

                if(index == 0) {
                    //add contact
                    log("Record number for added ADN is " + recordIndex);
                    mNewRecordNumber = recordIndex;
                    adn.setRecordNumber(recordIndex);
                    mSimPbRecords.add(adn);
                    mValidAdnCount ++;
                } else if (adn.isEmpty()){
                    //delete contact
                    int adnRecordIndex = mSimPbRecords.get(index - 1).getRecordNumber();
                    log("Record number for deleted ADN is " + adnRecordIndex);
                    if(recordIndex == adnRecordIndex) {
                        mSimPbRecords.remove(index - 1);
                        mValidAdnCount --;
                    } else {
                        e = new RuntimeException(
                            "The index for deleted ADN record did not match");
                    }
                } else {
                    //Change contact
                    int adnRecordIndex = mSimPbRecords.get(index - 1).getRecordNumber();
                    log("Record number for changed ADN is " + adnRecordIndex);
                    if(recordIndex == adnRecordIndex) {
                        adn.setRecordNumber(recordIndex);
                        mSimPbRecords.set(index - 1, adn);
                    } else {
                        e = new RuntimeException(
                            "The index for changed ADN record did not match");
                    }
                }
            } else {
               e = new RuntimeException("Update adn record failed",
                            ar.exception);
            }

            if (mAdnUpdatingWaiter != null) {
                AsyncResult.forMessage(mAdnUpdatingWaiter, null, e);
                mAdnUpdatingWaiter.sendToTarget();
                mAdnUpdatingWaiter = null;
            }
            break;

        case EVENT_SIM_REFRESH:
            ar = (AsyncResult)msg.obj;
            log("SIM REFRESH occurred");
            if (ar.exception == null) {
                IccRefreshResponse refreshRsp = (IccRefreshResponse)ar.result;
                if (refreshRsp == null) {
                    if (DBG) log("IccRefreshResponse received is null");
                    break;
                }

                if(refreshRsp.refreshResult == IccRefreshResponse.REFRESH_RESULT_FILE_UPDATE ||
                    refreshRsp.refreshResult == IccRefreshResponse.REFRESH_RESULT_INIT) {
                      invalidateAdnCache();
                }
            } else {
                log("SIM refresh Exception: " + ar.exception);
            }
            break;
    }

}
```

`EVENT_QUERY_ADN_RECORD_DONE`查询的是`SIM`卡总体信息，比如`SIM容量` `已经保存的联系人数量等`
EVENT_LOAD_ADN_RECORD_DONE`的查询数据是所有详细联系人信息，保存在`mSimPbRecords`中.上面两条消息处理结束以后，如果发现`SIM`卡中的联系人已经全部获取，就会执行`EVENT_LOAD_ALL_ADN_LIKE_DONE`
这个方法主要是用来返回查询结果给上层的。还记得前面提到的`IccPhoneBookInterfaceManager.java`中的`getAdnRecordsInEf()`方法，这个方法中有这样一段代码

```java
AtomicBoolean status = new AtomicBoolean(false);
Message response = mBaseHandler.obtainMessage(EVENT_LOAD_DONE, status);
```
这个消息最终是传到`mAdnLoadingWaiters`中保存起来的。处理`EVENT_LOAD_ADN_RECORD_DONE`的时候，会执行`notifyAndClearWaiters`方法，这个方法中会把查询的结果给到
`IccPhoneBookInterfaceManager`类进行处理

```
private void
notifyAndClearWaiters() {
    if (mAdnLoadingWaiters == null) {
        return;
    }

    for (int i = 0, s = mAdnLoadingWaiters.size() ; i < s ; i++) {
        Message response = mAdnLoadingWaiters.get(i);

        if ((response != null)) {
            AsyncResult.forMessage(response).result = mSimPbRecords;
            response.sendToTarget();
        }
    }

    mAdnLoadingWaiters.clear();
}
```
`IccPhoneBookInterfaceManager`类接收消息后，处理逻辑如下

```java
protected class IccPbHandler extends Handler {
    public IccPbHandler(Looper looper) {
        super(looper);
    }

    @Override
    public void handleMessage(Message msg) {
        AsyncResult ar;

        switch (msg.what) {
            case EVENT_GET_SIZE_DONE:
                ar = (AsyncResult) msg.obj;
                synchronized (mLock) {
                    if (ar.exception == null) {
                        mRecordSize = (int[])ar.result;
                        // recordSize[0]  is the record length
                        // recordSize[1]  is the total length of the EF file
                        // recordSize[2]  is the number of records in the EF file
                        logd("GET_RECORD_SIZE Size " + mRecordSize[0] +
                                " total " + mRecordSize[1] +
                                " #record " + mRecordSize[2]);
                    }
                    notifyPending(ar);
                }
                break;
            case EVENT_UPDATE_DONE:
                ar = (AsyncResult) msg.obj;
                if(ar.exception != null) {
                    if(DBG) logd("exception of EVENT_UPDATE_DONE is" + ar.exception );
                }
                synchronized (mLock) {
                    mSuccess = (ar.exception == null);
                    notifyPending(ar);
                }
                break;
            case EVENT_LOAD_DONE:
                ar = (AsyncResult)msg.obj;
                synchronized (mLock) {
                    if (ar.exception == null) {
                        if(DBG) logd("Load ADN records done");
                        mRecords = (List<AdnRecord>) ar.result;
                    } else {
                        if(DBG) logd("Cannot load ADN records");
                        mRecords = null;
                    }
                    notifyPending(ar);
                }
                break;
        }
    }
```
这样，就从`SIM`卡中成功获取了联系人，保存到了`mRecords`中。但是，接下来系统又是如何保存到数据库的呢？我们再继续看下
在上面`loadFromEf`的方法中，我们还没有走完。这个方法接下来部分如下

```java
if (adnRecords != null) {
    // Load the results
    final int N = adnRecords.size();
    // final MatrixCursor cursor = new MatrixCursor(ADDRESS_BOOK_COLUMN_NAMES, N);
    log("adnRecords.size=" + N);
    for (int i = 0; i < N ; i++) {
        //此方法是把联系人保存到cursor中
        loadRecord(adnRecords.get(i), cursor, i);
    }
    log("query end " + intToString(efType)+ ", subId = "+subId);
    return cursor;
} else {
    // No results to load
    Rlog.w(TAG, "Cannot load ADN records");
    return new MatrixCursor(COLUMN_NAMES_FOR_ASUS);
}
```

`loadRecord`的具体实现如下

```java
private void loadRecord(AdnRecord record, MatrixCursor cursor, int id) {
    if (!record.isEmpty()) {
        String alphaTag = record.getAlphaTag();
        String number = record.getNumber();
        
        String secondName = null;
        String[] anrs = record.getAdditionalNumbers();
        int recordNumber = record.getRecordNumber();

        contact[0] = alphaTag;
        contact[1] = number;

        String[] emails = record.getEmails();
        if (emails != null) {
            StringBuilder emailString = new StringBuilder();
            for (String email: emails) {
                log("Adding email:" + Rlog.pii(TAG, email));
                emailString.append(email);
                //emailString.append(",");
            }
            contact[2] = emailString.toString();
        }
        contact[3] = id;
        contact[4] = null;
        if (anrs != null) {
            StringBuilder anrString = new StringBuilder();
            for (String anr : anrs) {
                if (DBG) log("Adding anr:" + anr);
                anrString.append(anr);
                //anrString.append(":");
            }
            contact[5] = anrString.toString();
        }
        contact[6] = recordNumber;
        if (DBG) log("loadRecord: " + alphaTag + ", " + number + ", "+ recordNumber + ", " + contact[2] + ", "+ contact[5]);
        cursor.addRow(contact);
    }
}
```
上述方法在`LOG`中打印如下

```
10-30 19:30:51.499 D/IccProvider( 2004): [IccProvider] loadRecord: 罗帅昆山, 15037106639, 7, null, 15073106639//名称 号码 
```

# 联系人数据库路径


```java
static {
    URL_MATCHER.addURI("icc", "adn", ADN); //sIM卡
    URL_MATCHER.addURI("icc", "adn/subId/#", ADN_SUB);
    URL_MATCHER.addURI("icc", "fdn", FDN);
    URL_MATCHER.addURI("icc", "fdn/subId/#", FDN_SUB);
    URL_MATCHER.addURI("icc", "sdn", SDN);
    URL_MATCHER.addURI("icc", "sdn/subId/#", SDN_SUB);

}

```

