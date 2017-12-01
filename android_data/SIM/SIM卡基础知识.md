---
title: SIM卡基础知识
date: 2017-04-18 15:24:12
tags: android
categories:
    - SIM
    - 基础知识
password: 
---

`SIM`卡全称是`Subscriber Identification Module`,也就是`用户身份识别卡`，它里面含有芯片,上面存储了数字移动电话客户的信息，加密的密钥以及用户的电话簿等内容，可供GSM网络客户身份进行鉴别，并对客户通话时的语音信息进行加密

# SIM卡分类

我们平时听过的卡 `SIM`, `USIM`, `UIM`等统称为`UICC — Universal Integrated Circuit Card`，`UICC`上承载不同的应用，就形成不同功能的`SIM`卡

```
UICC——Universal Integrated Circuit Card。在UICC卡上可以包括多种逻辑应用(对应终端向前兼容)：
用户标识模块（SIM，Subscriber Identity Module）
通用用户标识模块（USIM，Universal Subscriber Identity Module）
IP多媒体业务标识模块（ISIM，IP Multimedia Service Identity Module）
可移动用户识别模块(RUIM，Removable User Identity Module)
```

# SIM卡功能

SIM卡的主要功能 
1. 存储用户相关数据。SIM卡存储的数据可分为四类：第一类是固定存放的数据。这类数据在ME（Mobile Equipment）被出售之前由SIM卡中心写入，包括国际移动用户识别号（IMSI）、鉴权密钥（KI）等；第二类是暂时存放的有关网络的数据。如位置区域识别码（LAI）、移动用户暂时识别码（TMSI）等；第三类是业务代码，如个人识别码（PIN）、解锁码（PUK）等；第四类是电话号码簿，是手机用户存储的电话号码。 
2. 用户PIN的操作和管理。SIM卡本身是通过PIN码来保护的，PIN是一个四位到八位的个人密码，只有当用户输入正确的PIN码时，SIM卡才能被启用，移动终端才能对SIM卡进行存取，也只有PIN认证通过后，用户才能上网通话。 
3. 用户身份鉴权。确认用户身份是否合法，鉴权过程是在是在网络和SIM卡之间进行的，而鉴权时间一般是在移动终端登记入网和呼叫。 
4. SIM卡中的保密算法及密钥。SIM卡中最敏感的数据是保密算法。PIN码可由用户在手机上自己设定，PUK码由运营者持有。 

# SIM卡内部结构

`SIM`卡内部有5个模块，并且每个模块都对应一个功能

```
微处理器CPU（8位）
程序存储器ROM
工作存储器RAM
数据存储器EEPROM
串行通信单元
```
这5个模块被胶封在SIM卡铜制接口后与普通IC卡封装方式相同。这5个模块必须集成在一块集成电路中，否则其安全性会受到威胁，因为芯片间的连线可能成为非法存取和盗用SIM卡的重要线索SIM卡在与手机连接时，最少需要5个连接线

```
电源（Vcc）
时钟（CLK）
数据I/O口（Data）
复位（RST）
接地端（GND）
```

# SIM卡文件系统介绍

`android`中，对`SIM`的操作，主要就是对`SIM`卡存储信息的操作，而存储信息都是在`SIM`卡文件中的，我们看下`SIM`卡的文件系统

在一张`UICC`卡上，会挂载不同的应用，应用的组成其实就相当于一个`单片机+文件系统`，下面主要看SIM卡的文件系统构成，如下图（网上找的图）：

![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/sim_ef_1.png)

参考自[xjbclz的博客](http://blog.csdn.net/xjbclz/article/details/51477365)

|文件|文件标识符|文件缩写|中文名称|文件作用|
|:--------|:--------:|:--------:|:--------:|:--------:|
|MF|3F00|根目录|备注：所有非ETSI GSM协议中规定的应用文件由各厂家自行定义在根目录下（如：PIN1，PIN2…）||
|EFICCID|2FE2|ICCID|SIM卡唯一的识别号|包含运营商、卡商、发卡时间、省市代码等信息|
|DFGSM|7F20|GSM目录|备注：根据ETSIGSM09.91的规定Phase2(或以上)的SIM卡中应该有7F21并指向7F20,用以兼容Phase1的手机||
|EFLP语言选择|6F05|LP|语言选择文件|包含一种或多种语言编码|
|EFIMSI|6F07|IMSI|国际移动用户识别符|包含SIM卡所对应的号段，比如46000代表135－139号段、46002代表1340－1348|
|EFKC语音加密密钥|6F20|Kc|计算密钥|用于SIM卡的加密、解密|
|EFPLMNsel网络选择表|6F30|PLMNsel|公共陆地网选择|决定SIM卡选择哪种网络，在这里应该选择中国移动的网络|
|EFHPLMN归属地网络选择表|6F31|HPLMN|两次搜索PLMN的时间间隔|两次搜索中国移动的网络的时间间隔|
|EFACMmax最大计费额|6F37|ACMmax|包含累积呼叫表的最大值|全部的ACM数据存在SIM卡中，此处取最大值|
|EFSST SIM卡服务表|6F38|SST|SIM卡服务列表|指出SIM卡可以提供服务的种类，哪些业务被激活哪些业务没有开通|
|EFACM累加计费计数器|6F39|ACM|累计呼叫列表|当前的呼叫和以前的呼叫的单位总和|
|EFGID1分组识别1|6F3E|GID1|1级分组识别文件|包含特定的SIM-ME组合的标识符，可以识别一组特定的SIM卡|
|EFGID2分组识别2|6F3F|GID2|2级分组识别文件|包含特定的SIM-ME组合的标识符，可以识别一组特定的SIM卡|
|EFPUCT单位价格/货币表|6F41|PUCT|呼叫单位的价格和货币表|PUCT是与计费通知有关的信息，ME用这个信息结合EFACM，以用户选择的货币来计算呼叫费用|
|EFCBMI小区广播识别号|6F45|CBMI|小区广播信息标识符|规定了用户希望MS采纳的小区广播消息内容的类型|
|EFSPN服务提供商|6F46|SPN|服务提供商名称|包含服务提供商的名称和ME显示的相应要求|
|EFCBMID|6F48|CBMID|数据下载的小区广播消息识别符|移动台将收到的CBMID传送给SIM卡|
|EFSUME|6F54|SUME|建立菜单单元|建立SIM卡中的菜单|
|EFBCCH广播信道|6F74|BCCH|广播控制信道|由于BCCH的存储，在选择小区时，MS可以缩小对BCCH载波的搜索范围|
|EFACC访问控制级别|6F78|ACC|访问控制级别|SIM卡有15个级别，10个普通级别，5个高级级别|
|EFFPLMN禁止网络号|6F7B|FPLMN|禁用的PLMN|禁止选择除中国移动以外的其他运营商，比如中国联通、中国卫通等|
|EFLOCI位置信息|6F7E|LOCI|位置信息|存储临时移动用户识别符、位置区信息等内容|
|EFAD管理数据|6FAD|AD|管理数据|包括关于不同类型SIM卡操作模式的信息。例如：常规模式（PLMN用户用于GSM网络操作），型号认证模式（允许ME在无线设备的认证期间的特殊应用）；小区测试模式（在小区商用之前，进行小区测试），制造商特定模式（允许ME制造商在维护阶段进行特定的性能自动测试）|
|EFPHASE阶段|6FAE|PHASE|阶段标识|标识SIM卡所处的阶段信息，比如是普通SIM卡还是STK卡等|
|DFTELECOM|7F10|电信目录|||
|EFADN缩位拨号|6F3A|AND|电话簿|用于将电话记录存放在SIM卡中|
|EFFDN固定拨号|6F3B|FDN|固定拨号|包括固定拨号（FDN）和/或补充业务控制字串（SSC），还包括相关网络/承载能力的识别符和扩展记录的识别符，以及有关的α识别符|
|EFSMS短消息|6F3C|SMS|短消息|用于将短消息记录存放在SIM卡中|
|EFCCP能力配置参数|6F3D|CCP|能力配置参数|包括所需要的网络和承载能力的参数，以及当采用一个缩位拨号号码，固定拨号号码，MSISDN、最后拨号号码、服务拨号号码或禁止拨号方式等，建立呼叫时相关的ME配置|
|EFMSISDN电话号码|6F40|MSISDN|移动基站国际综合业务网号|存放用户的手机号|
|EFSMSP短信息参数|6F42|SMSP|短消息业务参数|包括短信中心号码等信息|
|EFSMSS短信息状态|6F43|SMSS|短消息状态|这个标识是用来控制流量的|
|EFLND最后拨号|6F44|LND|最后拨叫号码|存储最后拨叫号码|
|EFExt1扩展文件1|6F4A|EXT1|扩展文件1|包括AND，MSISDN或LND的扩展数据|
|EFExt2扩展文件2|6F4B|EXT2|扩展文件2|包含FDN的扩展数据|

**文件类型说明**

```
MF：The root level of the file system is known as the Master file.
DF：Directories are known as Dedicated files and are of a fixed size.
EF：Individual records (or files) are known as Elementary files.
All files are identified as an address (a DWORD value), rather than a filename.
```
其中`EF`文件，又分为如下三种
```
Transparent： 透明结构的EF 由一个字节序列组成。当文件读或更新，字节序列活动是参照相对地址（OFFSET）进行的，相对地址可表示出起始操作的地址（用字节表示）和读出、更新的字节数。透明EF 的第一个字节有一个相对地址‘0000’。EF 主体的数据长度在EF 的文件头中。
Linear Fixed File： 线性固定EF 文件由一个记录长度固定的记录序列组成。第一个记录记录号是1。记录的长度和记录长度与记录个数的乘积存放在EF 文件头中。
Cyclic： 循环文件用于以时间顺序存储的记录，当所有的记录空间都占用时，新的存储数据将覆盖最旧的信息。　　
访问不同的文件类型，使用的方式也将不同。对于USIM，RUIM等卡基本文件结构应该是一致的，局部存储信息的单元，位置不同而已。
```
# SIM卡交互
`ME`与卡交互的过程其实就是对`EF`数据进行读写的过程，而这些数据的读写主要是由一个`APDU`命令--响应对完成的，`APDU`(应用协议数据单)的格式如下

![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/sim_ef_2.png)

具体指令的格式

![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/sim_ef_3.png)

其中关注一下READ RECORD这个命令，之后在代码上对于EF的读写主要使用这个命令，当p2='04'时，代表模式为绝对/当前模式，记录号在P1 中给出，P1='00'表示当前记录

APDU响应格式如下
![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/sim_ef_4.png)

```
SW1 = '90'  SW2 = '00'   命令正常结束；
SW1 = '9F'  SW2 = 'XX'   长度为'XX'的响应数据；
```




