# 如何编写一个shit client
### 💩 你应该手动管理Value，而不是依赖低效的反射

_Good 👍🏻_

```java
public Flight() {
    /***/
    addValues(new Value[] { mode, timeboost, spoof, prey, mark});
}
```

_Bad 👎🏻_

```java
for (final Field field : createdModule.getClass().getDeclaredFields()) {
    try {
            field.setAccessible(true);
            final Object obj = field.get(createdModule);
            if (obj instanceof Value) createdModule.getValues().add((Value) obj);
    } catch (IllegalAccessException e) {
        e.printStackTrace();
    }
}
```


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
### 💩 你应该删除任何可能会被反作弊注意到的内容
![以下代码引用自BapeClient Killaura](https://github.com/cubk/BapeClient/blob/main/src/main/java/mc/bape/modules/blatant/Killaura.java)

_Good 👍🏻_

```java
            if(ModuleManager.getModule("Criticals").getState() && Criticals.canJump() && mc.thePlayer.onGround)
                mc.thePlayer.jump();
            if(this.swing.getValue()){
                mc.thePlayer.swingItem();
            }
//            mc.getNetHandler().addToSendQueue(new C02PacketUseEntity(target, C02PacketUseEntity.Action.ATTACK));
//            if ((Boolean) this.autoblock.getValue()){
//                if (mc.thePlayer.getCurrentEquippedItem() == null) {
//                    return;
//                }
//                if (!(mc.thePlayer.getCurrentEquippedItem().getItem() instanceof ItemSword)) {
//                    return;
//                }
//                if (target != null
//                        && (mc.thePlayer.getHeldItem() != null
//                        && mc.thePlayer.getHeldItem().getItem() instanceof ItemSword
//                        && this.autoblock.getValue() || mc.thePlayer.isBlocking())) {
//                    mc.getNetHandler().addToSendQueue(new C08PacketPlayerBlockPlacement(mc.thePlayer.inventory.getCurrentItem()));
//                    mc.thePlayer.getCurrentEquippedItem().useItemRightClick(mc.theWorld, mc.thePlayer);
//                }
//            }
}
```

_Bad 👎🏻_

```java
            if(ModuleManager.getModule("Criticals").getState() && Criticals.canJump() && mc.thePlayer.onGround)
                mc.thePlayer.jump();
            if(this.swing.getValue()){
                mc.thePlayer.swingItem();
            }
            mc.getNetHandler().addToSendQueue(new C02PacketUseEntity(target, C02PacketUseEntity.Action.ATTACK));
```

### 💩 你应该使用非传统的Name space体现你的客户端走在时代前沿
_Good 👍🏻_

```java
package mc.bape.Gui;
package Fuck.You.Loser;
package 傻逼.草拟吗.你妈死了;
package dont.Hurt.me
```

_Bad 👎🏻_

```java
package today.getvapu;
package com.cubk;
package ml.mckuhei;
```

### 💩 尽可能的在注释留下“为什么”而不是“是什么”和“问题”

_Good 👍🏻_

```java
// 为什么会NullPointerException
```

_Bad 👎🏻_

```java
// 这里有问题，需要修改，不然没法进服务器
// 字体渲染必须修复才能打开指南针
```

