# 如何编写一个shit client

### 💩 你应把i18n国际化变成l10n本地化
防止外国skidder反编译看得懂

_Good 👍🏻_

```java
displayName="这是个模块";
```

_Bad 👎🏻_

```java
displayName=I18n.get("this_is_a_module");
```
```json
zh_cn.json
{
    "this_is_a_module":"这是个模块"
}
en_us.json
{
    "this_is_a_module":"This Is A Module"
}
```

### 💩 你应避免缩进
防止您的代码被Skidder反编译后看得懂

_Good 👍🏻_

```kotlin
class AutoVelocity : Module() {
    @EventTarget
    fun onUpdate(event: UpdateEvent?) {
        val velocity = moduleManager.getModule(Velocity::class.java) as Velocity

if (mc.thePlayer!!.hurtTime <= 0 && mc.thePlayer!!.onGround){
    velocity.state = true
}else{
    velocity.state = false
}
    }
    }
```

_Bad 👎🏻_

```kotlin
class AutoVelocity : Module() {
    @EventTarget
    fun onUpdate(event: UpdateEvent?) {
        val velocity = moduleManager.getModule(Velocity::class.java) as Velocity

        if (mc.thePlayer!!.hurtTime <= 0 && mc.thePlayer!!.onGround){
            velocity.state = true
        }else{
            velocity.state = false
        }
    }
}
```

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
[以下代码引用自BapeClient Killaura](https://github.com/cubk/BapeClient/blob/main/src/main/java/mc/bape/modules/blatant/Killaura.java)

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
package fuck.you.wuyutong;
package ExploreSurvival.Game.Load;
```

_Bad 👎🏻_

```java
package today.getvapu;
package com.client;
package studios.mcmodule;
package just.monika.literaturemc.lucky;
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

### 💩 大量使用kotlin让反编译器永远达不到反编译的真实

_Good 👍🏻_

```kotlin
@JvmField
    val altPanels = AltPanels()
```

_Bad 👎🏻_

```java
AltPanels altPanels = new AltPanels();
```

### 💩 使用高效的if而不是低效的swtich

_Good 👍🏻_

```java
if(a==1){
    xxx1()
}else if(a==2){
    xxx2()
}else if .....
```

_Bad 👎🏻_

```java
swtich(a){
case 1:xxx1();break;
case 2:xxx2();break;
case ...:...;
```

### 💩 使用高效的coremode而不是低效的mixin 

_Good 👍🏻_

```java
public class ExampleTweaker implements ITweaker {

    private String[] args;

    public void acceptOptions(List<String> args, File gameDir, File assetsDir, String profile) {
        String[] additionArgs = {"--gameDir", gameDir.getAbsolutePath(), "--assetsDir", assetsDir.getAbsolutePath(), "--version", profile};
        List<String> fullArgs =  new ArrayList<String>();
        fullArgs.addAll(args);
        fullArgs.addAll(Arrays.asList(additionArgs));
        this.args = fullArgs.toArray(new String[fullArgs.size()]);
    }

    public void injectIntoClassLoader(LaunchClassLoader classLoader) {
        classLoader.registerTransformer("com.example.ClassTransformer");
    }

    public String getLaunchTarget() {
        return "net.minecraft.client.main.Main";
    }

    public String[] getLaunchArguments() {
        return args;
    }
}
```

_Bad 👎🏻_

```java
package com.example.mixins;

import net.minecraft.client.Minecraft;
import org.spongepowered.asm.mixin.Mixin;

@Mixin(Minecraft.class)
public abstract class MixinMinecraft {
    ......
}
```
