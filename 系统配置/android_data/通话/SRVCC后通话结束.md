---
title: ARVCC后通话结束
date: 2017-11-23 17:27:39
tags: 通话
categories:
    - 挂断
    - 
password: 
---

这个问题分析如下
1.弹出4G网络无法连接 是因为在接通后发生了SRVCC，切换到了GSM网络，所以弹出4G网无法连接的提示，这个是正常的
2.稍后弹出来的那个提示语Error occurred on audio device，是开启了自动录音功能，由于开启录音的时候电话已经在挂断了,通话服务就已经结束了，会去关掉对应的audio mic和receiver设备，弹出提示是正常行为！
3.至于电话自动挂断的原因，通过LOG来看，在通话接通后，发生了SRVCC，切换到GSM网络，之后网络侧发生了Channel Release，导致通话挂断


```
//About diag_log_20171121_1549491511250589427 qxdm log
2017 Nov 21  07:49:51.645  [84]  0x1830  IMS VoLTE Session Setup//建立volte通话
Version = 1
Dialled String Length = 50
Dialled String = sip:[2409:8809:86D1:3DE7:1749:DB0B:362A:D781]:8901
Direction = MT
Call Id Length = 50
Call Id = asbc9k1rkns80nxtyk9oy8rltyenkysqy0yk@10.132.170.46
Type = 0
Originating Uri Length = 61
Originating Uri = tel:667113;noa=subscriber;srvattri=national;phone-context=+86
Terminating Uri Length = 58
Terminating Uri = tel:664213;phone-context=ims.mnc000.mcc460.3gppnetwork.org
Result = OK
Call Setup Delay = 0

2017 Nov 21  07:49:52.011  [EF]  0x1831  IMS VoLTE Session End//volte会话结束
Version = 1
Dialled String Length = 50
Dialled String = sip:[2409:8809:86D1:3DE7:1749:DB0B:362A:D781]:8901
Direction = MT
Call Id Length = 50
Call Id = asbc9k1rkns80nxtyk9oy8rltyenkysqy0yk@10.132.170.46
Type = 0
Originating Uri Length = 61
Originating Uri = tel:667113;noa=subscriber;srvattri=national;phone-context=+86
Terminating Uri Length = 58
Terminating Uri = tel:664213;phone-context=ims.mnc000.mcc460.3gppnetwork.org
End Cause = SRVCC //原因是发生了SRVCC
Call Setup Delay = 0

2017 Nov 21  07:49:52.050  [AF]  0x713A  UMTS UE OTA  --  CONNECT
Message Direction = From UE
chan_type = 0 (0x0)
prot_disc_check = 3 (0x3)
trans_id_or_skip_ind = 8 (0x8)
prot_disc = 3 (0x3) (GSM_CALL_CONTROL) //切换到GSM网络
msg_type = 7 (0x7)
prot
  call_ctrl_prot
    CONNECT
      facility_incl = 0 (0x0)
      connected_sub_incl = 0 (0x0)
      user_user_incl = 0 (0x0)
      ss_ver_incl = 0 (0x0)
      stream_id_incl = 0 (0x0)

2017 Nov 21  07:49:52.189  [49]  0x5B2F  GSM DSDS RR Signaling Message  --  Channel Release//通话结束，因为网络侧发生了channel release
Subscription ID = 1
	Channel Type = DCCH (0)
	Direction = Downlink
	Message Type = Channel Release (13)
	Message Length in bytes = 3
	L3 Message in Hex:
	  06 0D 00
Decoded Message:
chan_type = 0 (0x0)
prot_disc_check = 6 (0x6)
trans_id_or_skip_ind = 0 (0x0)
prot_disc = 6 (0x6) (GSM_RR_MANAGEMENT)
msg_type = 13 (0xd)
prot
  rr_man_prot
    CHANNEL_RELEASE
      rr_cause
        rr_cause_val = 0 (0x0)
      ba_range_incl = 0 (0x0)
      group_chan_desc_incl = 0 (0x0)
      group_ciph_key_num_incl = 0 (0x0)
      gprs_res_incl = 0 (0x0)
      ba_list_pref_incl = 0 (0x0)
      utran_freq_list_incl = 0 (0x0)
      cell_select_ind_incl = 0 (0x0)
      cell_chan_description_incl = 0 (0x0)
      enh_dtm_cs_release_incl = 0 (0x0)
      vgcs_ciphering_params_incl = 0 (0x0)
      group_chan_desc2_incl = 0 (0x0)
      talker_identity_incl = 0 (0x0)
      talker_priority_status_incl = 0 (0x0)
      vgcs_amr_config_incl = 0 (0x0)
      individual_priority_incl = 0 (0x0)

```


