





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