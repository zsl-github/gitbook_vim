---
title: SIM卡属性查询问题
date: 2017-11-27 17:15:06
tags: SIM
categories:
    - 属性
    - 
password: 
---

最近新项目上有一个需求，就是需要通过第三方应用能查询到SIM卡的属性，包括SIM卡容量、保存的联系人数量、邮箱等。这个需求在实现过程中，遇到了一个奇怪的问题，后面会进行描述，现在先看下此需求在android N上面的实现

# android N

## 提供查询接口

在类
`frameworks/opt/telephony/src/java/com/android/internal/telephony/IccProvider.java`

```java
@Override
public Cursor query(Uri url, String[] projection, String selection,
        String[] selectionArgs, String sort) {
    if (DBG) log("query"+" ,url = "+url.toString());

    switch (URL_MATCHER.match(url)) {
        ....
        case PROP:
            return loadSimProperty(SubscriptionManager.getDefaultSubscriptionId());
        case PROP_SUB:
            return loadSimProperty(getRequestSubId(url));
            
        default:
            throw new IllegalArgumentException("Unknown URL " + url);
    }
}

```
其中`PROP`是`URL_MATCHER.addURI("icc", "prop",PROP)`

## 查询方法loadSimProperty

此方法仍然是在类
`frameworks/opt/telephony/src/java/com/android/internal/telephony/IccProvider.java`
中实现的

```java
    private Cursor loadSimProperty(int subId){
    	int slotId = SubscriptionManager.getSlotId(subId);
		int state = mTelephonyManager.getSimState(slotId);
		Rlog.i(TAG, "loadSimProperty: subId = " + subId + ",slotId = " + slotId+ ", state = " + state);
    	final MatrixCursor cursor = new MatrixCursor(SIM_PROPERTY_COLUMN_NAMES, 1);
    	Object[] simprop = new Object[11];
		if (state == 1) {
			simprop[0] = "SIM";
			simprop[1] = 0;
			simprop[2] = 0;
			simprop[3] = 0;
			simprop[4] = 0;
			simprop[5] = 0;
			simprop[6] = 0;
			simprop[7] = 0;
			simprop[8] = 0;
			simprop[9] = 0;
			simprop[10] = false;
		} else {
			mPhone = PhoneFactory.getPhone(slotId);
			AppType appType = mPhone.getCurrentUiccAppType();
			String simType = transforAppTypeToSimType(appType);
			int capacity[] = getAdnRecordsCapacity(subId);
			simprop[0] = simType;
			simprop[1] = capacity[0];
			simprop[2] = capacity[2];
			simprop[3] = capacity[4];
			simprop[4] = capacity[10];
			simprop[5] = capacity[8];
			simprop[6] = capacity[6];
			simprop[7] = capacity[9];
			simprop[8] = capacity[7];
			simprop[9] = capacity[12];
			simprop[10] = true/* capacity[13]!=0 ? true:false */;
		}
		Rlog.i(TAG, "loadSimProperty: simType = " + simprop[0]
				+ ", max_count_ADN = " + simprop[1]
				+ ", max_count_EMAIL = " + simprop[2]
				+ ", max_count_ANR = "+ simprop[3]
				+ ", max_secondName_count = " + simprop[4]
				+ ", max_name_len  = " + simprop[5]
				+ ", max_numer_len  = "+ simprop[6]
				+ ", max_email_len = " + simprop[7]
				+ ", max_addNumber_len = " + simprop[8]
				+ ", max_secondName_len = " + simprop[9]
				+ ", EditMode = "+ simprop[10]);
        cursor.addRow(simprop);
    	return cursor;
    }

```
## 关键方法getAdnRecordsCapacity

这个方法还是在类
`frameworks/opt/telephony/src/java/com/android/internal/telephony/IccProvider.java`
中实现的
```java
    private int[] getAdnRecordsCapacity (int subId){
		try {
			IIccPhoneBook iccIpb = IIccPhoneBook.Stub.asInterface(ServiceManager.getService("simphonebook"));
			if (iccIpb != null) {
				iccIpb.getAdnRecordsInEfForSubscriber(subId, IccConstants.EF_ADN);
				return iccIpb.getAdnRecordsCapacityForSubscriber(subId);
			} else {
				if (DBG)log("IIccPhoneBook is Null");
			}
		} catch (RemoteException ex) {
			// ignore it
		} catch (SecurityException ex) {
			if (DBG)log(ex.toString());
		}
		if (DBG) log("getAdnRecordsCapacity  result is null ,subId = "+subId);
		return null;
    }
```

## 方法getAdnRecordsCapacityForSubscriber

`iccIpb`对应的类是

`frameworks/opt/telephony/src/java/com/android/internal/telephony/UiccPhoneBookController.java`
具体的方法实现是

```java
    @Override
    public int[] getAdnRecordsCapacityForSubscriber(int subId)
           throws android.os.RemoteException {
        IccPhoneBookInterfaceManager iccPbkIntMgr =
                             getIccPhoneBookInterfaceManager(subId);
        if (iccPbkIntMgr != null) {
            return iccPbkIntMgr.getAdnRecordsCapacity();
        } else {
            Rlog.e(TAG,"getAdnRecordsCapacity iccPbkIntMgr is" +
                      " null for Subscription:"+subId);
            return null;
        }
    }
```

## 方法getAdnRecordsCapacity

这个才是最主要的方法，这个方法是在类`frameworks/opt/telephony/src/java/com/android/internal/telephony/IccPhoneBookInterfaceManager.java`中实现的

```java
    public int[] getAdnRecordsCapacity() {
        int capacity[] = new int[14];

        if (isSimPhoneBookEnabled()) {
            if (mSimPbAdnCache != null) {
                capacity[0] = mSimPbAdnCache.getAdnCount();
                capacity[1] = mSimPbAdnCache.getUsedAdnCount();
                capacity[2] = mSimPbAdnCache.getEmailCount();
                capacity[3] = mSimPbAdnCache.getUsedEmailCount();
                capacity[4] = mSimPbAdnCache.getAnrCount();
                capacity[5] = mSimPbAdnCache.getUsedAnrCount();
                
                capacity[6] = mSimPbAdnCache.getMaxNumLength();
                capacity[7] = mSimPbAdnCache.getMaxAddNumLength();
                capacity[8] = mSimPbAdnCache.getMaxNameLength();
                capacity[9] = mSimPbAdnCache.getMaxEmailLength();
                capacity[10] = mSimPbAdnCache.getSecondNameCount();
                capacity[11] = mSimPbAdnCache.getUsedSecondNameCount();
                capacity[12] = mSimPbAdnCache.getMaxSecondNameLength();
                capacity[13] = mSimPbAdnCache.getEditMode();
                
            } else {
                loge("mAdnCache is NULL when getAdnRecordsCapacity.");
            }
        }
        if (DBG) logd("getAdnRecordsCapacity: max adn=" + capacity[0]
            + ", used adn=" + capacity[1] + ", max email=" + capacity[2]
            + ", used email=" + capacity[3] + ", max anr=" + capacity[4]
            + ", used anr=" + capacity[5]);

        return capacity;
    }
```

## 重点看mSimPbAdnCache 是如何获取SIM卡信息的

我们在`getAdnRecordsCapacity`方法中，重点分析的是`iccIpb.getAdnRecordsCapacityForSubscriber(subId)`
其实它还有另外一个重要的方法，是在它之前执行的
```java
iccIpb.getAdnRecordsInEfForSubscriber(subId, IccConstants.EF_ADN);
return iccIpb.getAdnRecordsCapacityForSubscriber(subId);
```

我们可以看下`getAdnRecordsInEfForSubscriber`方法，它也是在`frameworks/opt/telephony/src/java/com/android/internal/telephony/UiccPhoneBookController.java`
中实现的，具体实现如下

```java
    @Override
    public List<AdnRecord> getAdnRecordsInEfForSubscriber(int subId, int efid)
           throws android.os.RemoteException {
        IccPhoneBookInterfaceManager iccPbkIntMgr =
                             getIccPhoneBookInterfaceManager(subId);
        if (iccPbkIntMgr != null) {
            return iccPbkIntMgr.getAdnRecordsInEf(efid);
        } else {
            Rlog.e(TAG,"getAdnRecordsInEf iccPbkIntMgr is" +
                      "null for Subscription:"+subId);
            return null;
        }
    }
```
而`getAdnRecordsInEf`也是在`frameworks/opt/telephony/src/java/com/android/internal/telephony/IccPhoneBookInterfaceManager.java`中实现的

```java
    public List<AdnRecord> getAdnRecordsInEf(int efid) {

        if (mPhone.getContext().checkCallingOrSelfPermission(
                android.Manifest.permission.READ_CONTACTS)
                != PackageManager.PERMISSION_GRANTED) {
            throw new SecurityException(
                    "Requires android.permission.READ_CONTACTS permission");
        }

        efid = updateEfForIccType(efid);
        if (DBG) logd("getAdnRecordsInEF: efid=0x" + Integer.toHexString(efid).toUpperCase());

        synchronized(mLock) {
            checkThread();
            AtomicBoolean status = new AtomicBoolean(false);
            Message response = mBaseHandler.obtainMessage(EVENT_LOAD_DONE, status);

            if (isSimPhoneBookEnabled() &&
                (efid == IccConstants.EF_PBR || efid == IccConstants.EF_ADN)) {
                if (mSimPbAdnCache != null) {
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

它这时候又走进了`frameworks/opt/telephony/src/java/com/android/internal/telephony/uicc/SimPhoneBookAdnRecordCache.java`中(通过mAdnCache.requestLoadAllAdnLike)方法

看下具体实现

```java
    public void
    requestLoadAllAdnLike(Message response) {
        if (mAdnLoadingWaiters != null) {
            mAdnLoadingWaiters.add(response);
        }

        synchronized (mLock) {
            if (!mSimPbRecords.isEmpty()) {//保证联系人只加载一次
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

看下查询方法`queryAdnRecord`

```java
    public void queryAdnRecord () {
        mRecCount = 0;
        mAdnCount = 0;
        mValidAdnCount = 0;
        mEmailCount = 0;
        mAddNumCount = 0;
        
        
        log("start to queryAdnRecord");

        mCi.getAdnRecord(obtainMessage(EVENT_QUERY_ADN_RECORD_DONE));
        mCi.registerForAdnRecordsInfo(this, EVENT_LOAD_ADN_RECORD_DONE, null);

        try {
            mLock.wait();
        } catch (InterruptedException e) {
            Rlog.e(LOG_TAG, "Interrupted Exception in queryAdnRecord");
        }

        mCi.unregisterForAdnRecordsInfo(this);
    }

```

可以看到，就是在这个时候，加载了`mSimPbAdnCache`的相关属性值

# android O

其实上面的流程，`android N`跟`android O`前面都是一样的，就是在`return iccPbkIntMgr.getAdnRecordsCapacity();`
的方法中，出现了差异。因为我们跟踪的话，会发现在类`frameworks/opt/telephony/src/java/com/android/internal/telephony/IccPhoneBookInterfaceManager.java`

具体方法是空的

```java
    public int[] getAdnRecordsCapacity() {
        if (DBG) logd("getAdnRecordsCapacity" );
        int capacity[] = new int[10];

        return capacity;
    }
```
这个困扰了我很久，一直不知道这里为什么是一个空方法。而且，在`framework/opt/telephony`下没有发现`SimPhoneBookAdnRecordCache.java`这个类。后来偶然发现，在仓
`vendor/qcom/proprietary/telephony-fwk/opt/telephony/src/java/com/qualcomm/qti/internal/telephony/`下发现了`QtiSimPhoneBookAdnRecordCache.java`，后来一直不知道二者是如何
联系起来的。直到抓取LOG后发现，在流程中，最后竟然走到了`QtiSimPhoneBookAdnRecordCache.java`下，

```
01-10 00:34:22.759 D/IccProvider( 3560): [IccProvider] getRequestSubId url: content://icc/adn/subId/2147483643
01-10 00:34:22.759 D/IccProvider( 3560): [IccProvider] loadFromEf: efType=0x6F3A, subscription=2147483643
01-10 00:34:22.759 D/QtiSubscriptionController( 3560): getPhoneId, received dummy subId 2147483643
01-10 00:34:22.760 D/IccPhoneBookIM( 3560): [IccPbInterfaceManager] getAdnRecordsInEF: efid=0x6F3A
01-10 00:34:22.760 D/QtiSimPhoneBookAdnRecordCache( 3560): start to queryAdnRecord
01-10 00:34:22.762 I/QtiRilInterface( 3560): getAdnRecord()
01-10 00:34:22.763 D/QtiRilInterface( 3560): AdnOemHookCallback: onOemHookResponse = [B@27f1478
01-10 00:34:22.763 I/QtiRilInterface( 3560): numInts: 10
```
所以猜测，应该是在调用`getAdnRecordsCapacity`的方法时，走到了高通目录下。而且，在高通下，还有一个跟`IccPhoneBookInterfaceManager.java`相似的类
`QtiIccPhoneBookInterfaceManager.java`。这个类中的方法我们也看下

```
    public int[] getAdnRecordsCapacity() {
        int capacity[] = new int[10];

        if (isSimPhoneBookEnabled()) {
            if (mSimPbAdnCache != null) {
                capacity[0] = mSimPbAdnCache.getAdnCount();
                capacity[1] = mSimPbAdnCache.getUsedAdnCount();
                capacity[2] = mSimPbAdnCache.getEmailCount();
                capacity[3] = mSimPbAdnCache.getUsedEmailCount();
                capacity[4] = mSimPbAdnCache.getAnrCount();
                capacity[5] = mSimPbAdnCache.getUsedAnrCount();
                
                capacity[6] = mSimPbAdnCache.getMaxNumLength();
                capacity[7] = mSimPbAdnCache.getMaxAddNumLength();
                capacity[8] = mSimPbAdnCache.getMaxNameLength();
                capacity[9] = mSimPbAdnCache.getMaxEmailLength();
                capacity[10] = mSimPbAdnCache.getSecondNameCount();
                capacity[11] = mSimPbAdnCache.getUsedSecondNameCount();
                capacity[12] = mSimPbAdnCache.getMaxSecondNameLength();
                capacity[13] = mSimPbAdnCache.getEditMode();
                
            } else {
                loge("mAdnCache is NULL when getAdnRecordsCapacity.");
            }
        }
        if (DBG) logd("getAdnRecordsCapacity: max adn=" + capacity[0]
            + ", used adn=" + capacity[1] + ", max email=" + capacity[2]
            + ", used email=" + capacity[3] + ", max anr=" + capacity[4]
            + ", used anr=" + capacity[5] + ", max number length =" + capacity[6]
            + ", max add number length =" + capacity[7]  + ", max name length =" + capacity[8]
            + ", max email length =" + capacity[9]  + ", max second name =" + capacity[10]
            + ", used sencond name =" + capacity[11]  + ", max second name length  =" + capacity[12]
            + ", edit mode =" + capacity[13]);

        return capacity;
    }
```
可以看到，它的实现跟`android N`相似。如果我们继续跟流程的话，可以发现，在进入`IccPhoneBookInterfaceManager.java`类后，就转到`QtiIccPhoneBookInterfaceManager.java`中了

我们可以看下具体的流程

在类`frameworks/opt/telephony/src/java/com/android/internal/telephony/UiccPhoneBookController.java`中的

```java
    @Override
    public List<AdnRecord> getAdnRecordsInEfForSubscriber(int subId, int efid)
           throws android.os.RemoteException {
        IccPhoneBookInterfaceManager iccPbkIntMgr =
                             getIccPhoneBookInterfaceManager(subId);
        if (iccPbkIntMgr != null) {
            return iccPbkIntMgr.getAdnRecordsInEf(efid);
        } else {
            Rlog.e(TAG,"getAdnRecordsInEf iccPbkIntMgr is" +
                      "null for Subscription:"+subId);
            return null;
        }
    }
```
方法，就在`getAdnRecordsInEf`方法可以转到`qcon`仓下了

在类`vendor/qcom/proprietary/telephony-fwk/opt/telephony/src/java/com/qualcomm/qti/internal/telephony/QtiIccPhoneBookInterfaceManager.java`中

```java
    @Override
    public List<AdnRecord> getAdnRecordsInEf(int efid) {

        if (mPhone.getContext().checkCallingOrSelfPermission(
                android.Manifest.permission.READ_CONTACTS)
                != PackageManager.PERMISSION_GRANTED) {
            throw new SecurityException(
                    "Requires android.permission.READ_CONTACTS permission");
        }

        efid = updateEfForIccType(efid);
        if (DBG) logd("getAdnRecordsInEF: efid=0x" + Integer.toHexString(efid).toUpperCase());

        synchronized(mLock) {
            checkThread();
            AtomicBoolean status = new AtomicBoolean(false);
            Message response = mBaseHandler.obtainMessage(EVENT_LOAD_DONE, status);

            if (isSimPhoneBookEnabled() &&
                (efid == IccConstants.EF_PBR || efid == IccConstants.EF_ADN)) {
                if (mSimPbAdnCache != null) {
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

继续跟踪`mSimPbAdnCache.requestLoadAllAdnLike`方法

此方法是在类`vendor/qcom/proprietary/telephony-fwk/opt/telephony/src/java/com/qualcomm/qti/internal/telephony/uicccontact/QtiSimPhoneBookAdnRecordCache.java`中实现的

```java
    public void
    requestLoadAllAdnLike(Message response) {
        if (mAdnLoadingWaiters != null) {
            mAdnLoadingWaiters.add(response);
        }

        synchronized (mLock) {
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

继续跟踪`queryAdnRecord();`方法

```java
    public void queryAdnRecord () {
        mRecCount = 0;
        mAdnCount = 0;
        mValidAdnCount = 0;
        mEmailCount = 0;
        mAddNumCount = 0;
        
        log("start to queryAdnRecord");

        mQtiRilInterface.getAdnRecord(
                obtainMessage(EVENT_QUERY_ADN_RECORD_DONE),
                mPhoneId);
        mQtiRilInterface.registerForAdnRecordsInfo(this, EVENT_LOAD_ADN_RECORD_DONE, null);

        try {
            mLock.wait();
        } catch (InterruptedException e) {
            Rlog.e(LOG_TAG, "Interrupted Exception in queryAdnRecord");
        }

        mQtiRilInterface.unregisterForAdnRecordsInfo(this);
    }
```
这个方法的查询结果会给到`EVENT_QUERY_ADN_RECORD_DONE`事件，我们稍后看查询后的结果
现在到了查询`ADN`的方法了

`getAdnRecord`具体实现

在类`vendor/qcom/proprietary/telephony-fwk/opt/telephony/src/java/com/qualcomm/qti/internal/telephony/QtiRilInterface.javajava`
中具体实现

```java
    public void getAdnRecord(Message callbackMsg, int phoneId) {
        byte[] requestData = new byte[INT_SIZE];
        ByteBuffer reqBuffer = mQcRilHook.createBufferWithNativeByteOrder(requestData);
        final int rspLength = 10;//这个是返回的数据长度

        reqBuffer.putInt(phoneId);
        AdnOemHookCallback oemHookCb = new AdnOemHookCallback(callbackMsg, rspLength);

        mQcRilHook.sendQcRilHookMsgAsync(
                QcRilHook.QCRIL_EVT_HOOK_GET_ADN_RECORD,
                requestData,
                oemHookCb,
                phoneId);

        logi("getAdnRecord()");
    }
```
继续跟踪`mQcRilHook.sendQcRilHookMsgAsync`方法

在类`vendor/qcom/proprietary/qcrilOemHook/src/com/qualcomm/qcrilhook/QcRilHook.java`中

```java
    public void sendQcRilHookMsgAsync(int requestId, byte[] payload, OemHookCallback oemHookCb,
            int phoneId) {
        validateInternalState();
        int payloadLength = 0;
        if (payload != null) {
            payloadLength = payload.length;
        }

        byte[] request = new byte[mHeaderSize + payloadLength];
        ByteBuffer reqBuffer = createBufferWithNativeByteOrder(request);

        addQcRilHookHeader(reqBuffer, requestId, payloadLength);
        if (payload != null) {
            reqBuffer.put(payload);
        }
        sendRilOemHookMsgAsync(requestId, request, oemHookCb, phoneId);
    }
```

继续往下看`sendRilOemHookMsgAsync`

```java
    private void sendRilOemHookMsgAsync(int requestId, byte[] request, IOemHookCallback oemHookCb,
            int phoneId) throws NullPointerException {
        logv("sendRilOemHookMsgAsync: Outgoing Data is "
                + IccUtils.bytesToHexString(request));

        try {
            mService.sendOemRilRequestRawAsync(request, oemHookCb, phoneId);
        } catch (RemoteException e) {
            Log.e(LOG_TAG, "sendOemRilRequestRawAsync RequestID = " + requestId
                    + " exception, unable to send RIL request from this application", e);
        } catch (NullPointerException e) {
            Log.e(LOG_TAG, "NullPointerException caught at sendOemRilRequestRawAsync." +
                    "RequestID = " + requestId + ". Throw to the caller");
            throw e;
        }
    }
```
方法`mService.sendOemRilRequestRawAsync(request, oemHookCb, phoneId);`在类

`vendor/qcom/proprietary/qcrilOemHook/src/com/qualcomm/qcrilmsgtunnel/QcrilMsgTunnelIfaceManager.java`中进行

```java
    public void sendOemRilRequestRawAsync(byte[] request, IOemHookCallback oemHookCb, int sub) {
        try {
            sendRequestAsync(CMD_INVOKE_OEM_RIL_REQUEST_ASYNC, request, oemHookCb, sub);
        } catch (RuntimeException e) {
            Log.w(TAG, "sendOemRilRequestRawAsync: Runtime Exception", e);
        }
    }
```

然后再看下`sendRequestAsync`方法

```java
    private void sendRequestAsync(int command, Object arg1, Object arg2, int sub) {
        MessageRequestAsync request = new MessageRequestAsync(arg1, arg2, sub);
        Message msg = mMessageHandler.obtainMessage(command, request);
        msg.sendToTarget();
    }
```
继续看这个`Message`执行的地方`CMD_INVOKE_OEM_RIL_REQUEST_ASYNC,`消息

```java
    private Handler mMessageHandler = new Handler() {

        @Override
        public void handleMessage(Message msg) {
            MessageRequest request;
            MessageRequestAsync requestAsync;
            Message onCompleted;
            AsyncResult ar;

            if (DBG) Log.d(TAG, "handleMessage what = " + msg.what);

            switch (msg.what) {

                case CMD_INVOKE_OEM_RIL_REQUEST_ASYNC:
                    requestAsync = (MessageRequestAsync) msg.obj;
                    onCompleted = obtainMessage(
                            EVENT_INVOKE_OEM_RIL_REQUEST_ASYNC_DONE, requestAsync);
                    mQcrilOemhookMsgTunnel[requestAsync.sub].invokeOemRilRequestRaw(
                            (byte[]) requestAsync.arg1, onCompleted);
                    break;
```
继续看`mQcrilOemhookMsgTunnel[requestAsync.sub].invokeOemRilRequestRaw((byte[]) requestAsync.arg1, onCompleted);`

在类`vendor/qcom/proprietary/qcrilOemHook/src/com/qualcomm/qcrilmsgtunnel/QcrilOemhookMsgTunnel.java`中

```java
public void invokeOemRilRequestRaw(byte[] data, Message response) {
    acquireWakeLock();
    QcRilRequest rr
    = QcRilRequest.obtain(RIL_REQUEST_OEM_HOOK_RAW, response);

    logi("invokeOemRilRequestRaw: serial=" + rr.mSerial + " length=" + data.length);
    logd(rr.mSerial + " > " + requestToString(rr.mRequest) + "[" +
            bytesToHexString(data, data.length) + "]");

    if (mIQtiOemHook == null) {
        loge("oemHookService is not running.");
        sendResponse(rr, RADIO_NOT_AVAILABLE, null);
    } else {
        queueRequest(rr);//增加到查询列表，不影响具体逻辑
        try {
            logd("Calling HIDL oemHookRawRequest");
            mIQtiOemHook.oemHookRawRequest(rr.mSerial, primitiveArrayToArrayList(data));//接着跟踪
        } catch (RemoteException ex) {
            loge("oemHookRawRequest remote error serial=" + rr.mSerial + " " + ex);
            QcRilRequest req = findAndRemoveRequestFromList(rr.mSerial);
            sendResponse(req, GENERIC_FAILURE, null);
        }
    }
}
```
看此方法的具体实现`mIQtiOemHook.oemHookRawRequest(rr.mSerial, primitiveArrayToArrayList(data))`
代码跟到此处已经不知道如何继续跟踪下去了，后续再慢慢了解。这里先记录下修改方法，主要是在

`vendor/qcom/proprietary/qcril/qcril_qmi/qcril_pbm.c`类中进行SIM卡状态操作的



查询的结果是在`EVENT_QUERY_ADN_RECORD_DONE`消息中接收的，我们看下返回结果的处理

首先在类`vendor/qcom/proprietary/telephony-fwk/opt/telephony/src/java/com/qualcomm/qti/internal/telephony/QtiRilInterface.java`中回调方法

```java
    protected class AdnOemHookCallback extends OemHookCallback {
        Message mAppMessage;
        int mRspLength;

        public AdnOemHookCallback(Message msg, int length) {
            super(msg);
            mAppMessage = msg;
            mRspLength = length;
        }

        @Override
        public void onOemHookResponse(byte[] response, int phoneId) throws RemoteException {
            if (response != null) {
                logd("AdnOemHookCallback: onOemHookResponse = " + response.toString());

                int[] values = parseInts(response, mRspLength);//返回的数组长度
                AsyncResult.forMessage(mAppMessage, values, null);
            } else {
                AsyncResult.forMessage(mAppMessage, null,
                        new Exception("QCRIL_EVT_HOOK_GET_ADN_RECORD failed"));
            }
            mAppMessage.sendToTarget();
        }

        @Override
        public void onOemHookException(int phoneId) throws RemoteException {
            logd("AdnOemHookCallback: onOemHookException");

            AsyncResult.forMessage(mAppMessage, null,
                    new Exception("com.android.internal.telephony.CommandException: MODEM_ERR"));
            mAppMessage.sendToTarget();
        }
    }

```
然后到类
`vendor/qcom/proprietary/telephony-fwk/opt/telephony/src/java/com/qualcomm/qti/internal/telephony/uicccontact/QtiSimPhoneBookAdnRecordCache.java`的消息中进一步处理


```java
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
                mMaxNameLen = ((int[]) (ar.result))[6];
```
至此，流程已经全部走完

