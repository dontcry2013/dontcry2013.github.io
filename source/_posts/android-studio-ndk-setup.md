---
title: Android Studio NDK Development Environment Setup
date: 2016-04-01 18:56:39
categories: Android
tags: [NDK]
---

最近在做的文字识别采用了卷积神经网络技术，这种方式需要大量使用循环计算，无疑c++是高效处理这种密集运算的语言。在测试识别mnist样本库10K个测试图片时，识别率正确率达到了92%以上。实际使用中，在识别手写和印刷体字符时还是存在一些很复杂的问题，比如字体粗细、偏移极大影响识别正确率，实际上卷积神经网络的特征提取是位置不相关的，这也是以后要着重优化的地方。回到主题，c++工程要通过ndk编译成so库才能在Android端使用，以前主要使用eclipse开发，现在Android studio成为了行业主流，在配置使用过程中碰到了很多的坑，用了一天的时间才使项目正常运行。
<!-- more -->
# 生成头文件

* 新建Load类
``` java
package myself.exercise.ndktest;

public class Load {
    static { //载入名为“lb”的C++库
        System.loadLibrary("lb");
    }
    public native int addInt(int a, int b); //调用库里的方法“addInt”，这是计算a和b两个整数相加
}
```
* 走一遍主菜单“Build > Make Project”，这一步有没有效果.
* 点界面左下角的“Terminal”标签，打开命令行窗口,末尾输入：cd app/build/intermediates/classes/debug 按回车键，进入“debug”目录
* 接着输入：javah -jni myself.exercise.ndktest.Load 按回车键。注意：“myself.exercise.ndktest”是包名，“Load”是调用C++库的类的名称，这时“debug”目录下会有一个myself.exercise.ndktest.Load.h头文件生成。
* 右键单击此文件，在弹出菜单中选择“Cut”,然后粘贴到“app/src/main/jni”目录。

# 编写c文件

``` c
#include "myself_exercise_ndktest_Load.h"

JNIEXPORT jint JNICALL Java_myself_exercise_ndktest_Load_addInt (JNIEnv *, jobject, jint a, jint b) {
    return a + b;
}
```
恭喜，现在正式进入调试阶段...


# Error:Unable to load class 'com.android.build.gradle.managed.NdkConfig_Impl'.
大boss，在使用日志输出的时候，配置出现了这个错误。[Issue Tracker](https://code.google.com/p/android/issues/detail?id=192928&q=label%3Acomponent-tools&colspec=ID%20Type%20Status%20Owner%20Summary%20Stars)这样讲：
There has been a change in the DSL.  Due to some technical limitations, += no longer works, but it can be fixed by replacing with .add or .addAll.

然而老版本大多使用ldLibs += ["android", "log"]这种方式， 这种+= 的方式已经废弃,现在换成了如下方式。 [Android Tools Project Site](http://tools.android.com/tech-docs/new-build-system/gradle-experimental)上面有介绍。这里多说一句，作为开发人员科学上网非常重要，浪费了不少时间在这个问题上面，在百度里相信这个问题两天我也解决不了。

``` java
    android.ndk {
        moduleName = "lb"
        ldLibs.add("log")
    }
```

# Bad class file magic or version
增加compileOptions.with{} 需要选择JavaVersion.VERSION_1_7，否则会报这种错误。即使你使用的jdk版本并不是这个版本也要这样设置，我使用的jdk就是1.8的，尝试了多次都以失败告终。

``` java
    compileOptions.with {
        sourceCompatibility = JavaVersion.VERSION_1_7
        targetCompatibility = JavaVersion.VERSION_1_7
    }
```
查看完整配置文件请点击[这里](https://gist.github.com/dontcry2013/5ceac51f15d5afb8b3f7457f26999906)
前提是你能翻墙，真是不明白为什么墙gist这样的域名。




# 有用的资源
[Google ndk sample](https://github.com/googlesamples/android-ndk)

