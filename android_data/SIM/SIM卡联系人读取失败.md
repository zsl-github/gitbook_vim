---
title: SIM卡联系人读取失败
date: 2017-03-20 10:09:15
tags: SIM
categories:
    - bug
    - 
password: 
---

# 问题背景

今天遇到一个问题，就是手机在`fota`前后，`SIM`卡联系人不显示

# 具体分析原因

```
AP侧：
11-13 15:04:17.733  2688  3039 D RILJ    : [4068]> RIL_REQUEST_GET_ADN_RECORD [SUB0]
11-13 15:04:17.739  2688  2728 D RILJ    : [4068]< RIL_REQUEST_GET_ADN_RECORD {0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0} [SUB0]
11-13 15:04:17.740  2688  2688 D SimPhoneBookAdnRecordCache: Querying ADN record done
11-13 15:04:17.740  2688  2688 D SimPhoneBookAdnRecordCache: Max ADN count is: 0, Valid ADN count is: 0, Email count is: 0, Valid Email count is: 0, Add number count is: 0, Valid Add number count is: 0, Max number length is: 0, Max addnumber length is: 0, Max name length is: 0, Max mEmailLen length is: 0, Max secondName count is: 0, Valid secondName count is: 0, Max secondName length is: 0, Is hidden entry support : 0
11-13 15:04:17.740  2688  2688 D SimPhoneBookAdnRecordCache: Loading all ADN records done
11-13 15:04:17.741  2688  2743 D IccPhoneBookIM: [IccPbInterfaceManager] Load ADN records done
11-13 15:04:17.741  2688  3039 D IccProvider: [IccProvider] adnRecords.size=0
11-13 15:04:17.741  2688  3039 D IccProvider: [IccProvider] query end ADN_SUB, subId = 1
11-13 15:04:17.743  3738  5022 D SimCardContactsManager: (1) 1: ALREADY FETCH SIM CARD CONTACTS
11-13 15:04:17.743  3738  5022 D SimCardContactsManager: (1) 1: CONTACTS count : 0
11-13 15:04:17.745  3738  5022 I ReadIccProviderService: (1) 1: simCardContacts count = 0

modem 侧
[0063/0000] MSG 07:04:17.586 Android QCRIL/Low [         qcril_pbm.c   1186] RIL[0][main] qcril_qmi_pbm_get_adn_record_hndlr: function entry
[0063/0001] MSG 07:04:17.586 Android QCRIL/Medium [         qcril_pbm.c   1229] RIL[0][main] qcril_qmi_pbm_get_adn_record_hndlr: For all active session, the max records is 0, the valid records is 0
[0063/0001] MSG 07:04:17.586 Android QCRIL/Medium [         qcril_pbm.c   1231] RIL[0][main] qcril_qmi_pbm_get_adn_record_hndlr: For all active session, the max email is 0, the max ad num is 0
[0063/0001] MSG 07:04:17.586 Android QCRIL/Medium [         qcril_pbm.c   1234] RIL[0][main] qcril_qmi_pbm_get_adn_record_hndlr: max_num_len: 0, max_ad_num_len: 0, max_name_len: 0, max_email_len: 0
[0063/0001] MSG 07:04:17.586 Android QCRIL/Medium [         qcril_pbm.c   1236] RIL[0][main] qcril_qmi_pbm_get_adn_record_hndlr: max_sencond_name: 0, used_sencond_name: 0, max_sencond_name_len: 0
[0063/0001] MSG 07:04:17.586 Android QCRIL/Medium [         qcril_pbm.c   1237] RIL[0][main] qcril_qmi_pbm_get_adn_record_hndlr: is_hidden_entry_support: (null)

```

# 总结

分析`SIM`卡联系人加载过程，主要可以从`AP`跟`MODEM`来查看

## AP侧

直接搜索关键字`RIL_REQUEST_GET_ADN_RECORD`，可以看到具体的查询结果
```
11-13 15:04:17.739  2688  2728 D RILJ    : [4068]< RIL_REQUEST_GET_ADN_RECORD {0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0} [SUB0]
```

## MODEM侧

### QCAT

筛选`BUG`类型`0x1FEB`,具体截图如下

![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/sim_contact_qcat.png)

### QXDM

直接在`QXDM`中打开`LOG`，然后`全选`->`右键`->`Export All Text To File`，就可以保存为`txt`文件

接着在保存的文件中搜索`get_adn_record`，即可看到关键信息

```
[0063/0001] MSG 07:04:17.586 Android QCRIL/Medium [         qcril_pbm.c   1229] RIL[0][main] qcril_qmi_pbm_get_adn_record_hndlr: For all active session, the max records is 0, the valid records is 0
```


