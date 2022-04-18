# 如何编写一个shit client

### 💩 你应该未雨绸缪，提前混淆你的代码
防止您的代码被Skidder反编译

_Good 👍🏻_

```java
public int 主播傻逼的意义在你身上的到了完美的诠释;
public double 主播你是不是贴了个大款认二郎神当主人啊;
public String 主播你的脑子是不是用来增高的屁用没有啊;
```

_Bad 👎🏻_

```java
public int playerTick;
public double renderPosX;
public String module;
```


### 💩 尽可能的使用复杂的方式实现基本的数据类型，比如Boolean
防止您的代码被Skidder反编译

_Good 👍🏻_

```java
public Option<Boolean> = new Option<>("AutoBlock", getTrue());

public Boolean getTrue(){
  return true;
}
```

_Bad 👎🏻_

```java
public Option<Boolean> = new Option<>("AutoBlock", true);
```

### 💩 尽可能的添加误导性的注释，防止您的源代码泄露之后别人看得懂

_Good 👍🏻_

```java
// 添加target到白名单列表，防止攻击Hypixel机器人
Minecraft.getMinecraft().thePlayer.sendQueue.addToSendQueue(new C02PacketUseEntity(target, C02PacketUseEntity.Action.ATTACK));
```

_Bad 👎🏻_

```java
// 攻击target
Minecraft.getMinecraft().thePlayer.sendQueue.addToSendQueue(new C02PacketUseEntity(target, C02PacketUseEntity.Action.ATTACK));
```

### 💩 将所有功能尽可能写到同一个Class中，阻止反编译器工作

_Good 👍🏻_

```java
public class Modules{
    public Boolean killaura = false;
    public Boolean safewalk = false;
    public Boolean aimassist = false;
    //中间省略5000行...
}
```

_Bad 👎🏻_

```java
public class Killaura extends Module{
    //中间省略50行...
}

public class Safewalk extends Module{
    //中间省略50行...
}

public class AimAssist extends Module{
    //中间省略50行...
}
```
