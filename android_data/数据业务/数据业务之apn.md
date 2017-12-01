---
title: 数据业务之APN相关
date: 2017-04-27 22:13:30
tags: android
categories:
    - apn
    - 
password: 
---


# 相关类、属性

## NetworkConfig

这个类主要是定义各种`APN`的基本信息，比如彩信、上网、wifi等。包括名称，类型，分值等

```java
public NetworkConfig(String init) {
    String fragments[] = init.split(",");
    name = fragments[0].trim().toLowerCase(Locale.ROOT);
    type = Integer.parseInt(fragments[1]);
    radio = Integer.parseInt(fragments[2]);
    priority = Integer.parseInt(fragments[3]);
    restoreTime = Integer.parseInt(fragments[4]);
    dependencyMet = Boolean.parseBoolean(fragments[5]);
}

```
## ApnSetting
这个类包含的是详细`APN`信息，针对某个运营商的。包含的信息就比较多了，我们可以通过初始化方法了解下

```java
public ApnSetting(int id, String numeric, String carrier, String apn,
        String proxy, String port,
        String mmsc, String mmsProxy, String mmsPort,
        String user, String password, int authType, String[] types,
        String protocol, String roamingProtocol, boolean carrierEnabled, int bearer,
        int bearerBitmask, int profileId, boolean modemCognitive, int maxConns, int waitTime,
        int maxConnsTime, int mtu, String mvnoType, String mvnoMatchData) {
    this.id = id;
    this.numeric = numeric;
    this.carrier = carrier;
    this.apn = apn;
    this.proxy = proxy;
    this.port = port;
    this.mmsc = mmsc;
    this.mmsProxy = mmsProxy;
    this.mmsPort = mmsPort;
    this.user = user;
    this.password = password;
    this.authType = authType;
    this.types = new String[types.length];
    for (int i = 0; i < types.length; i++) {
        this.types[i] = types[i].toLowerCase(Locale.ROOT);
    }
    this.protocol = protocol;
    this.roamingProtocol = roamingProtocol;
    this.carrierEnabled = carrierEnabled;
    this.bearer = bearer;
    this.bearerBitmask = (bearerBitmask | ServiceState.getBitmaskForTech(bearer));
    this.profileId = profileId;
    this.modemCognitive = modemCognitive;
    this.maxConns = maxConns;
    this.waitTime = waitTime;
    this.maxConnsTime = maxConnsTime;
    this.mtu = mtu;
    this.mvnoType = mvnoType;
    this.mvnoMatchData = mvnoMatchData;

}
```

## ApnContext
这个类包含了上面提到的两个类，除此以后，还增加了`APN`状态，状态变化原因，`APN`是否可用等属性。

```java

public ApnContext(Context context, String apnType, String logTag, NetworkConfig config,
        DcTrackerBase tracker) {
    mContext = context;
    mApnType = apnType;
    mState = DctConstants.State.IDLE;
    setReason(Phone.REASON_DATA_ENABLED);
    mDataEnabled = new AtomicBoolean(false);
    mDependencyMet = new AtomicBoolean(config.dependencyMet);
    mWaitingApnsPermanentFailureCountDown = new AtomicInteger(0);
    priority = config.priority;
    LOG_TAG = logTag;
    mDcTracker = tracker;
}
```
这个类里面有一个关键的属性，就是`mWaitingApns`,它是一个`List`列表，在第一次使用某种类型的`APN`去激活`PDP`的时候，都会先去查看本运营商是否支持此
`APN`,如果支持，就会创建`ApnSetting`,然后保存到这个列表中。所以，如果这个列表为空，说明是不支持此`APN`
这个类还有一个属性，就是`DcAsyncChannel`，当建立数据连接的时候，会设置这个属性

## DcAsyncChannel

```java
public DcAsyncChannel(DataConnection dc, String logTag) {
    mDc = dc;
    mDcThreadId = mDc.getHandler().getLooper().getThread().getId();
    mLogTag = logTag;
}
```
这个类是用来建立数据链接的,当尝试建立数据链接的时候，会先从`ApnContext`中获取它的值，而每次建立之后，会存储这个值，方便下次直接使用。而每次
清除数据链接的时候，都会清空此值

## DataConnection
这个类是用来创建数据链接的，它也会保存一个`ApnSetting`，用来记录创建数据链接的`APN`

## mAllApnSettings
这个也是一个`List`，里面存储的是ApnSetting对象，它里面保存的是当前`SIM`卡的全部`APN`，包括全球化参数中的`APN`

## mApnContexts

这个属性是在类`DcTracker.java`中定义的，它是一个`HashMap`,保存的就是各种`APN`类型`ApnContext`类

## mPrioritySortedApnContexts
这个类跟`mApnContexts`的唯一区别就是它是进过`APN`类型排序的


## 关键的方法

`ApnSetting.java` | `canHandleType`

这个方法是用来判断当前的`APN`是否支持此类型

`notifyOffApnsOfAvailability`
具体实现如下

# APN的各种使用场景
