# 9991.Layou****tInflater.from(this).inflate()**

\1. 如果root为null，attachToRoot将失去作用，设置任何值都没有意义。

\2. 如果root不为null，attachToRoot设为true，则会给加载的布局文件的指定一个父布局，即root。

\3. 如果root不为null，attachToRoot设为false，则会将布局文件最外层的所有layout属性进行设置，当该view被添加到父view当中时，这些layout属性会自动生效。

\4. 在不设置attachToRoot参数的情况下，如果root不为null，attachToRoot参数默认为true。



# **9992.java的四种引用**

https://www.cnblogs.com/pascall/p/10281775.html

## **1.强引用**

不管内存是否足够都不会被垃圾回收器(JVM)回收，如果内存溢出只会抛出OutOfMemoryError，使用程序异常终止如果想中断强引用和对象之间的关联，可以显式地将引用赋值为null,这样的话jvm就会在合适的时间回收



# **9993.动画**

## **1.逐帧动画**

逐帧动画的原理是让一系列静态图片依次播放，利用人眼“视觉暂留”的原理，

## **2.补间动画**

补间动画就是指开发者指定动画的开始、结束的“关键帧”，而动画变化的“中间帧”由系统计算，并补齐

补间动画有四种：

alpha：淡入淡出 

translate：位移

scale：缩放

rotate：旋转



# **9994.框架**

http://www.jcodecraeer.com/a/anzhuokaifa/2017/1020/8625.html

## **1.MVP**

Activity  -> Presenter -> Model -(callback) ->  Presenter -(View)-> Activity

View：Activity/Fragment

Presenter：业务处理层，既能调用UI逻辑，又能请求数据，该层为纯java类，不涉及任何Android API

Model：包含具体的数据请求，数据源

## **2.MVC**

VIew  ->  Controller  -> Model  -> View



# **9994.View和ViewGroup的区别**

## **1.触摸事件(MotionEvent)**

**dispatchTouchEvent(event:MotionEvent):boolean**  通过该方法来分发的，返回true表示事件被当前视图消费掉，不再继续分发事件；返回super.dispatchTouchEvent表示继续分发该事件。

**onInterceptTouchEvent(event:MotionEvent):boolean**只存在ViewGroup及其子类，在View和Activity中不存在，返回true表示拦截该事件；返回false表示不对事件进行拦截，需要继续传递给子视图。

**onTouchEvent(event:MotionEvent):boolean**事件的消费，返回true表示当前的视图可处理对应的事件，事件不会向上传递给父视图；返回false表示不处理该事件，事件会被传递给父视图的onTouchEvent方法进行处理



## **2.UI(MotionEvent)**

**2.1.UI管理系统的层级关系**

Activity->PhoneWindow->DecorView->TitleView,ContentView

每个Activity会创建一个PhoneWindow，它是Android系统中最基本的窗口系统，它是Activity和View系统交互的接口。DecorView本质上是一个FrameLayout，是Activity中所有View的组先。



## **2.2.绘制流程**



# **9995.ADB(Android Debug Bridge)命令**

[https://github.com/mzlogin/awesome-adb#%E6%9F%A5%E7%9C%8B%E6%97%A5%E5%BF%97](https://github.com/mzlogin/awesome-adb#查看日志)

叫android调试器，通过它可以在电脑上对Android手机进行操作和执行，在sdk/platform-tools/目录中

1.adb devices:查看android设备

2.adb -s  设备命令，如果有多个设备，可以通过命令对指定的设备进行操作

3.查看安装的应用

```
adb shell pm list package
```



# **9996.网络的使用**

## **1.Httpclient**

在新版本中被移除

## **2.HttpConnection**

它的api简单，体积较小

## **3.OkHttp框架**



# **9997.尺寸问题**

px：不推荐使用

sp：字体使用，它会根据用户的字体大小偏好来缩放

dp(Device independant pixle,Density independent pixel)：与设备/密度无关像素或独立像素，除字体外



# **9998.安全问题**

## **1.反编译**

https://www.jianshu.com/p/1a28e6439c6a

需要下载：

apktool.jar: https://ibotpeaches.github.io/Apktool/

### **1.1.apk->small->dex**

apktool: 反编译apk，获取class文件和资源文件，

apk->small: java -jar apktool.jar d xxx.apk  -o  app

apk直接转dex: java -jar apktool.jar d -s xxx.apk  -o  app

### **1.2.dex2jar**

dex2jar:将dex转成jar文件

sh ./dex2jar-2.0/d2j-dex2jar.sh ./app/classes.dex

注意：要给这些文件授权chmod +x file

### **1.3.class->java**

jd-gui-osx-1.6.3直接打开jar



## **2.动态调试**

smallIdea.zip: https://bitbucket.org/JesusFreke/smali/downloads/

用android studio添加插件smallIdea.zip，重启android studio



### **2.1.修改项目为可调试状态**

#### **2.1.1.修改源码**

因为apk release都不能调试，所以在AndroidManifest.xml的application加入android:debuggable="true"

#### **2.1.2.系统认证**

/default.prop中ro.debuggable的值为1

default.prop是保存在img的ramdisk中/default.prop中ro.debuggable

https://bbs.pediy.com/thread-188870.htm



### **2.2.打包项目**

执行下面的命令，会在/dist下面生成打包好的apk，安装会有问题，没有证书，然后我用的是360加固软件来签名

打包：java -jar apptool.jar b xxx.apk

安装：adb install xxx.apk



### **2.3.连接终端和android studio**

\1. 用android studio打开项目目录，配置运行配置的端口(这里假如是5005)

\2. 打开手机上的app，然后运行adb shell ps | grep 项目包名,  找到apk进程，假如是11111

\3. 打通android studio和手机，adb forward tcp:5005 jdwp:1111

\4. 运行android studio上面的调试，就可以进行调试了

帮助：

查看所有进程：adb shell ps tencent 或 adb shell ps | findstr tencent

查看apk进程：adb shell dumpsys activity | grep mFocusedActivity

端口印射：adb forward tcp:5005 jdwp:9022



### **2.4.防动态调试**

既然知道怎么动态调试，那我们该怎么防止别人来动态调试了

**if**(!BuildConfig.**DEBUG**){

​    **if**(Debug.*isDebuggerConnected*()){

​        *//进程自杀*

​        **int** myPid = android.os.Process.*myPid*();

​        android.os.Process.*killProcess*(myPid);

​        *//异常退出虚拟机*

​        System.*exit*(1);

​    }

}

## **3.混淆代码**

https://blog.csdn.net/fine1938768839/article/details/75529260

android代码在反编译后，如果没有对代码进行任何的保护，就很容易看到简单的源码，这样是很不安全的，还好android studio有ProGuard，ProGuard对代码进行压缩，优化，混淆，预检。

压缩(Shrink)：检测和删除没有使用的类，字段，方法和属性

优化(Optimize)：对字节码进行优化，并且移除无用指令

混淆(Obfuscate)：使用a,b,c等无意义的名称，对类，字段和方法进行重命名

预检(Preveirfy)：主要是在java平台上对处理后的代码进行预检

配置：

buildTypes {

​    release {

​        minifyEnabled **true** *//true表示对代码加密，false不加密*

​        *//代码加密原则 有两个文件对，getDefaultProguardFile(‘proguard-android.txt’)表示默认文件，这个文件是sdk自带的，*

​        *// 有一些通用的配置；但是如果apk需要更加严格的加密，我们可以在’proguard-rules.pro’文件中进行更加详尽的配置。*

​        proguardFiles getDefaultProguardFile(**'proguard-android.txt'**), **'proguard-rules.pro'**

​    }

}



## **4.日志输出风险**

有时我们的日志输出会泄露一些敏感的数据，这时我们就需要对处理一下输出的日志了

### **4.1.有做代码混淆处理**

1.去掉log日志

-assumenosideeffects class android.util.Log {

public static *** d(...);

public static *** v(...);

public static *** i(...);

}

2.去掉System.out.println 和System.out.print输出： 

 -assumenosideeffects class java.io.PrintStream {

   public *** println(...);

   public *** print(...);

 }

### **4.2.未做代码混淆处理**

自宝义日志类，分开发模式和发布模式



# **9999.Fragment生命周期**

**onAttach:** *onAttach()在fragment与Activity关联之后调调查用。需要注意的是，初始化fragment参数可以从getArguments()获得，但是，当Fragment附加到Activity之后，就无法再调用setArguments()。所以除了在最开始时，其它时间都无法向初始化参数添加内容。*

**onCreate:** *fragment初次创建时调用。尽管它看起来像是Activity的OnCreate()函数，但这个只是用来创建Fragment的。此时的Activity还没有创建完成，因为我们的Fragment也是Activity创建的一部分。所以如果你想在这里使用Activity中的一些资源，将会获取不到.*

**onCreateView**:*在这个fragment构造它的用户接口视图(即布局)时调用*

**onActivityCreated**:*在Activity的OnCreate()结束后，会调用此方法。所以到这里的时候，Activity已经创建完成！在这个函数中才可以使用*

 

**onStart**:*当到OnStart()时，Fragment对用户就是可见的了。但用户还未开始与Fragment交互。在生命周期中也可以看到Fragment的OnStart()过程与Activity的OnStart()过程是绑定的。意义即是一样的*

**onResume**:*当这个fragment对用户可见并且正在运行时调用。这是Fragment与用户交互之前的最后一个回调*

**onPause**:

**onStop**:

**onDestroyView**: *如果Fragment即将被结束或保存，那么撤销方向上的下一个回调将是onDestoryView()。会将在onCreateView创建的视图与这个fragment分离。下次这个fragment若要显示，那么将会创建新视图。这会在onStop之后和onDestroy之前调用。这个方法的调用同onCreateView是否返回非null视图无关。它会潜在的在这个视图状态被保存之后以及它被它的父视图回收之前调用*

 

**onDestroy**:*当这个fragment不再使用时调用。需要注意的是，它即使经过了onDestroy()阶段，但仍然能从Activity中找到，因为它还没有Detach*

**onDetach**: *Fragment生命周期中最后一个回调是onDetach()。调用它以后，Fragment就不再与Activity相绑定，它也不再拥有视图层次结构，它的所有资源都将被释放*

 



# 10000.遇见的问题

## 1.包引入重复的问题

```
Multiple dex files define Luk/co/senab/photov
```

解决方法：

先查看所依赖的包在哪里重复：gradlew -q 模块名：dependencies

![202009250001](.\images\202009250001.jpg)

在引入的地方排除：

```
implementation('com.journeyapps:zxing-android-embedded:3.4.0'){
    exclude group: 'com.google.zxing', module:'core'
}
```