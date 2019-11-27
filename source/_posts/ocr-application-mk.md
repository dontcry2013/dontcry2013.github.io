---
title: application-mk
date: 2016-05-18 18:37:59
categories: OCR
tags: [Android]
---

# 生成jni头文件：
* 第一种
比如包的名字是 com.example.test，类的名字是 hellojni，
类文件路径是src/com/example/test/hellojni.class
那么我们需要在src目录下，使用命令
```
javah -jni com.example.test.hellojni
```
<!--more-->

* 第二种

``` bash
#!/bin/sh
javah -classpath build/intermediates/classes/armv7/debug:libs/opencv-sdk.jar -o jni/ocr_jni.h com.polysfactory.opencvocr.jni.NativeMarkerDetector
```
1、编译路径为build/intermediates/classes/armv7/debug，名为com.polysfactory.opencvocr.jni.NativeMarkerDetector的class文件。
2、输出路径为jni，在jni目录下生成名为ocr_jni.h文件。
3、编译依赖libs/opencv-sdk.jar。

因为源文件中使用了opencv的类
``` java
package com.polysfactory.opencvocr.jni;
import org.opencv.core.Mat;
public class NativeMarkerDetector {
    private long mNativeObj;

    public NativeMarkerDetector(String nm1File, String nm2File) {
        mNativeObj = nativeCreateObject(nm1File, nm2File);
    }
    public void doOcr(Mat imageBgra, float scale) {
        Mat transformationsMat = new Mat();
        nativeFindMarkers(mNativeObj, imageBgra.nativeObj, transformationsMat.nativeObj, scale);
    }
    public void release() {
        nativeDestroyObject(mNativeObj);
        mNativeObj = 0;
    }

    private native long nativeCreateObject(String nm1File, String nm2File);

    private native void nativeFindMarkers(long thiz, long imageBgra, long transformations, float scale);

    private native void nativeDestroyObject(long thiz);
}
```

# Application.mk
### 一个实例：
``` bash
APP_STL := gnustl_static
APP_CPPFLAGS := -frtti -fexceptions
APP_ABI := armeabi armeabi-v7a x86 mips arm64-v8a x86_64 mips64
APP_PLATFORM := android-14
APP_OPTIM := release
```
### 简介
Application.mk目的是描述在你的应用程序中所需要的模块(即静态库或动态库)。
Application.mk文件通常被放置在 ```$PROJECT/jni/Application.mk```下，$PROJECT指的是您的项目。
要将C\C++代码编译为so文件，光有Android.mk文件还不行，还需要一个Application.mk文件。

1. APP_PROJECT_PATH  ： 这个变量是强制性的，并且会给出应用程序工程的根目录的一个绝对路径。这是用来复制或者安装一个没有任何版本限制的JNI库，从而给APK生成工具一个详细的路径。

2. APP_MODULES  ：   这个变量是可选的，如果没有定义，这个模块名字被定义在Android.mk文件中的 LOCAL_MODULE 中。NDK将由在Android.mk中声明的默认的模块编译，并且包含所有的子文件（makefile文), NDK会自动计算模块的依赖。
如果APP_MODULES定义了，它必须是一个空格分隔的模块列表( 注意：NDK在R4开始改变了这个变量的行为，在此之前： 在Application.mk中，该变量是强制的必须明确列出所有需要的模块)

3. APP_OPTIM ：   这个变量是可选的，用来定义“release”或"debug"。在编译您的应用程序模块的时候，可以用来改变优先级。“release”模式是默认的，并且会生成高度优化的二进制代码。"debug"模式生成的是未优化的二进制代码，但可以检测出很多的BUG，可以用于调试。
注意：如果你的应用程序是可调试的（即，如果你的清单文件在它的<application>标签中把android:debuggable属性设为true），默认将是debug而非release。把APP_OPTIM设置为release可以覆写它。注意：可以调试release和debug版二进制，但release版构建倾向于在调试会话中提供较少信息：一些变量被优化并且不能被检测，代码重新排序可能致使代码步进变得困难，堆栈跟踪可能不可靠，等等。

4. APP_CFLAGS ： 一个C编译器开关集合，在编译任意模块的任意C或C++源代码时传递。它可以用于改变一个给定的应用程序需要依赖的模块的构建，而不是修改它自身的Android.mk文件。APP_CPPFLAGS在Android NDK-1.5_r1中，这个标志可以应用于C和C++源文件中，现已不建议使用。

5. APP_BUILD_SCRIPT ： 默认，NDK构建系统将在 $(APP_PROJECT_PATH)/jni 下寻找一个名为 Android.mk 的文件。即，对于这个文件$(APP_PROJECT_PATH)/jni/Android.mk 如果你想重载这个行为，你可以定义APP_BUILD_SCRIPT指向一个不同的构建脚本。 一个非绝对路径将总是被解析为相对于NDK顶级目录的路径。

6. APP_ABI ： 默认情况下，NDK的编译系统根据 "armeabi" ABI生成机器代码。可以使用APP_ABI 来选择一个不同的ABI。比如：为了在ARMv7的设备上支持硬件FPU指令。可以使用  APP_ABI := armeabi-v7a
或者为了支持IA-32指令集，可以使用      APP_ABI := x86
或者为了同时支持这三种，可以使用       APP_ABI := armeabi armeabi-v7a x86

7. APP_STL ：默认，NDK构建系统提供由Android系统给出的最小C++运行时库（/system/lib/libstdc++.so）的C++头文件。 然而，NDK带有另一个C++实现，你可以在你自己的应用程序中使用或链接它。
定义APP_STL以选择它们其中的一个：  
APP_STL := stlport_static    -->     static STLport library
APP_STL := stlport_shared    -->     shared STLport library
APP_STL := system            -->      default C++ runtime library





