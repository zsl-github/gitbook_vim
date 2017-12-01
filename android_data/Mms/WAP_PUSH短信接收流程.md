---
title: wap push 短信接收流程
date: 2017-04-14 13:28:55
tags: android
categories:
    - mms
    - 总结
password: 
---

# 问题描述

今天上午分析一个问题，是手机无法接收`wap push`类型的消息。不过，从底层来看，是已经接收到了信息的，后来跟高通确认，原来是高通上层本身是不处理这类消息，导致上层不会显示此类信息

# LOG分析

```
通过modem log跟framework来看，都是已经成功接收到wappush短信的。
modem log：
[3008/0002/0003/0004] MSG 11:08:57.790 NAS MN/High [ mn_sms_msg_util.c 1053] DS: SUB 1 =MNCNM= MN sending ACK over CS

[0063/0002] MSG 11:08:58.626 Android QCRIL/High [ qcril.c 4187] RIL[0][event] qcril_send_unsol_response_epilog: UI <--- RIL_UNSOL_RESPONSE_NEW_SMS (1003) --- RIL [RID 0, Len 274, (null)]
[0063/0002] MSG 11:08:58.659 Android QCRIL/High [ qcril.c 5682] RIL[0][main] onRequest: UI --- RIL_REQUEST_SMS_ACKNOWLEDGE (37) ---> RIL [RID 0, token id 995, data len 8] //可以看到接收到了新的信息

framework侧关键LOG
10-30 19:08:58.746 2120 2288 D GsmInboundSmsHandler: dispatchWapPdu() returned -1//接收到wappush信息
10-30 19:08:58.747 2120 2288 D GsmInboundSmsHandler: WaitingState.processMessage:4
10-30 19:08:58.777 2120 2288 D GsmInboundSmsHandler: successful broadcast, deleting from raw table.
10-30 19:08:58.797 2120 2288 D GsmInboundSmsHandler: Deleted 1 rows from raw table.
10-30 19:08:58.797 2120 2288 D GsmInboundSmsHandler: ordered broadcast completed in: 138 ms//成功发出广播通知上层
10-30 19:08:58.797 2120 2288 D GsmInboundSmsHandler: WaitingState.processMessage:3
10-30 19:08:58.797 2120 2288 D GsmInboundSmsHandler: leaving Delivering state
10-30 19:08:58.797 2120 2288 D GsmInboundSmsHandler: entering Delivering state
10-30 19:08:58.797 2120 2288 D GsmInboundSmsHandler: DeliveringState.processMessage:4
10-30 19:08:58.797 2120 2288 D GsmInboundSmsHandler: leaving Delivering state
10-30 19:08:58.797 2120 2288 D GsmInboundSmsHandler: entering Idle state
10-30 19:08:58.797 2120 2288 D GsmInboundSmsHandler: IdleState.processMessage:5
10-30 19:08:58.797 2120 2288 D GsmInboundSmsHandler: Idle state processing message type 5
10-30 19:08:58.799 2120 2288 D GsmInboundSmsHandler: mWakeLock released wakelockcount = 0


```


