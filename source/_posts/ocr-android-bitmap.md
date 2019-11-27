---
title: 图像识别之jni返回图片到java
date: 2016-04-22 14:51:58
categories: OCR
tags: [Android, JNI]
---

在jni中操作图像并返回给java层有两种方式，第一种直接传递空图片到jni，在jni层锁定后操作像素再返回给java层，类似指针操作。第二种直接在jni层新建图片返回。
<!-- more -->
# 传递输出图片到jni
``` c
JNIEXPORT jint JNICALL Java_com_example_fonter_nncrdemo_Load_ocrPrepare (JNIEnv* env, jobject obj, jobject bmpSource, jobject bmpReturn) {
    int ret = 0;
    AndroidBitmapInfo bmpInfo, bmpReturnInfo;
    if((ret = AndroidBitmap_getInfo(env, bmpSource, &bmpInfo)) < 0) {
        LOGE("AndroidBitmap_getInfo() failed ! error=%d", ret);
        return -1;
    }
    if(ret = (AndroidBitmap_getInfo(env, bmpReturn, &bmpReturnInfo)) < 0) {
        LOGE("AndroidBitmap_getInfo() failed ! error=%d", ret);
        return -1;
    }
    if(bmpInfo.width <= 0 || bmpInfo.height <= 0 ||
       (bmpInfo.format != ANDROID_BITMAP_FORMAT_RGB_565 && bmpInfo.format != ANDROID_BITMAP_FORMAT_RGBA_8888)) {
        LOGE("invalid bitmap\n");
        return -1;
    }
    if(bmpReturnInfo.width <= 0 || bmpReturnInfo.height <= 0 ||
       (bmpReturnInfo.format != ANDROID_BITMAP_FORMAT_RGB_565 && bmpReturnInfo.format != ANDROID_BITMAP_FORMAT_RGBA_8888)) {
        LOGE("invalid bitmap\n");
        return -1;
    }

    LOGE("prepare|in(%d,%d)|out(%d,%d)",
         bmpInfo.width, bmpInfo.height, bmpReturnInfo.width, bmpReturnInfo.height);

    //截取后的图片的像素
    unsigned char* buffer = NULL;
    unsigned char* bufferRet = NULL;

    if(AndroidBitmap_lockPixels(env, bmpSource, (void**)&buffer)) {
        return -1;
    }
    if(AndroidBitmap_lockPixels(env, bmpReturn, (void**)&bufferRet)) {
        return -2;
    }
    //做灰度、二值化处理
    IMAGE imge = prepare(buffer, bmpInfo.width, bmpInfo.height);
    unsigned char *b = imge.buffer;
    for(int i = 0; i < bmpReturnInfo.width * bmpReturnInfo.height; i++){
        unsigned char g = b[i];
        bufferRet[i*4] = g;
        bufferRet[i*4+1] = g;
        bufferRet[i*4+2] = g;
        bufferRet[i*4+3] = 255;
    }
    free(b);
    AndroidBitmap_unlockPixels(env, bmpReturn);
    AndroidBitmap_unlockPixels(env, bmpSource);
    return 1;
}
```

其中java层传入两张图片。
``` java
	bmSource = CameraManager.get().buildBitmap(data, width, height);
	grayBitmap = Bitmap.createBitmap(bmSource.getWidth(), bmSource.getHeight(), Bitmap.Config.ARGB_8888);
	waveBitmap = Bitmap.createBitmap(bmSource.getHeight(), bmSource.getHeight(), Bitmap.Config.ARGB_8888);
	//grayBitmap即为需返回的图片
    new Load().ocrPrepare(bmSource, grayBitmap);
```

# 在jni中新建图片返回给java。
参考了http://stackoverflow.com/questions/14398670/rotating-a-bitmap-using-jni-ndk 但是不能够正常显示。做以下调整后运行正常。
``` c
JNIEXPORT jobject JNICALL Java_com_example_fonter_nncrdemo_Load_rowStatistic2 (JNIEnv* env, jobject obj, jobject bitmap) {
    //
    //getting bitmap info:
    //
    LOGE("reading bitmap info...");
    AndroidBitmapInfo info;
    int ret;
    if ((ret = AndroidBitmap_getInfo(env, bitmap, &info)) < 0)
    {
        LOGE("AndroidBitmap_getInfo() failed ! error=%d", ret);
        return NULL;
    }
    LOGE("width:%d height:%d stride:%d", info.width, info.height, info.stride);
    if (info.format != ANDROID_BITMAP_FORMAT_RGBA_8888)
    {
        LOGE("Bitmap format is not RGBA_8888!");
        return NULL;
    }
    //
    //read pixels of bitmap into native memory :
    //
    LOGE("reading bitmap pixels...");
    unsigned char* bitmapPixels;
    unsigned char* newBitmapPixels;
    if ((ret = AndroidBitmap_lockPixels(env, bitmap, (void**)&bitmapPixels)) < 0)
    {
        LOGE("AndroidBitmap_lockPixels() failed ! error=%d", ret);
        return NULL;
    }

    LOGE("creating new bitmap...");
    jclass bitmapCls = env->GetObjectClass(bitmap);
    jmethodID createBitmapFunction = env->GetStaticMethodID(bitmapCls, "createBitmap", "(IILandroid/graphics/Bitmap$Config;)Landroid/graphics/Bitmap;");
    jstring configName = env->NewStringUTF("ARGB_8888");
    jclass bitmapConfigClass = env->FindClass("android/graphics/Bitmap$Config");
    jmethodID valueOfBitmapConfigFunction = env->GetStaticMethodID(bitmapConfigClass, "valueOf", "(Ljava/lang/String;)Landroid/graphics/Bitmap$Config;");
    jobject bitmapConfig = env->CallStaticObjectMethod(bitmapConfigClass, valueOfBitmapConfigFunction, configName);
    jobject newBitmap = env->CallStaticObjectMethod(bitmapCls, createBitmapFunction, info.height, info.height, bitmapConfig);
    //
    // putting the pixels into the new bitmap:
    //
    if ((ret = AndroidBitmap_lockPixels(env, newBitmap, (void**)&newBitmapPixels)) < 0)
    {
        LOGE("AndroidBitmap_lockPixels() failed ! error=%d", ret);
        return NULL;
    }

    IMAGE imge = rowStatistic(bitmapPixels, info.width, info.height);
    //将黑白单色道图转化为RGBA8888格式
    unsigned char *b = imge.buffer;
    for(int i = 0; i < info.height*info.height; i++){
        unsigned char g = b[i];
        newBitmapPixels[i*4] = g;
        newBitmapPixels[i*4+1] = g;
        newBitmapPixels[i*4+2] = g;
        newBitmapPixels[i*4+3] = 255;
    }
    free(b);

    AndroidBitmap_unlockPixels(env, newBitmap);
    AndroidBitmap_unlockPixels(env, bitmap);
    return newBitmap;
}
```


# 返回数组的情况
``` c
jobject createBitmap(JNIEnv* env, jclass bitmapCls, IMAGE image){
    LOGE("creating new bitmap...");
    int ret;
    unsigned char* newBitmapPixels;
    jmethodID createBitmapFunction = env->GetStaticMethodID(bitmapCls, "createBitmap", "(IILandroid/graphics/Bitmap$Config;)Landroid/graphics/Bitmap;");
    jstring configName = env->NewStringUTF("ARGB_8888");
    jclass bitmapConfigClass = env->FindClass("android/graphics/Bitmap$Config");
    jmethodID valueOfBitmapConfigFunction = env->GetStaticMethodID(bitmapConfigClass, "valueOf", "(Ljava/lang/String;)Landroid/graphics/Bitmap$Config;");
    jobject bitmapConfig = env->CallStaticObjectMethod(bitmapConfigClass, valueOfBitmapConfigFunction, configName);
    jobject newBitmap = env->CallStaticObjectMethod(bitmapCls, createBitmapFunction, NN_RECT_SIZE*MAX_LETTER_COUNT, NN_RECT_SIZE, bitmapConfig);
    if ((ret = AndroidBitmap_lockPixels(env, newBitmap, (void**)&newBitmapPixels)) < 0){
        LOGE("AndroidBitmap_lockPixels() failed ! error=%d", ret);
    }

    unsigned char *b = image.buffer;
    for(int i = 0; i < image.width * image.height; i++){
        unsigned char g = b[i];
        newBitmapPixels[i*4] = g;
        newBitmapPixels[i*4+1] = g;
        newBitmapPixels[i*4+2] = g;
        newBitmapPixels[i*4+3] = 255;
    }
    free(b);

    AndroidBitmap_unlockPixels(env, newBitmap);
    return newBitmap;
}

JNIEXPORT jobjectArray JNICALL Java_com_example_fonter_nncrdemo_Load_returnBitmapArray (JNIEnv* env, jobject obj, jobject bitmap) {
    LOGE("reading bitmap info...");
    AndroidBitmapInfo info;
    int ret;
    if ((ret = AndroidBitmap_getInfo(env, bitmap, &info)) < 0){
        LOGE("AndroidBitmap_getInfo() failed ! error=%d", ret);
        return NULL;
    }
    LOGE("width:%d height:%d stride:%d", info.width, info.height, info.stride);
    if (info.format != ANDROID_BITMAP_FORMAT_RGBA_8888)
    {
        LOGE("Bitmap format is not RGBA_8888!");
        return NULL;
    }

    LOGE("reading bitmap pixels...");
    unsigned char* bitmapPixels;
    if ((ret = AndroidBitmap_lockPixels(env, bitmap, (void**)&bitmapPixels)) < 0) {
        LOGE("AndroidBitmap_lockPixels() failed ! error=%d", ret);
        return NULL;
    }

    LOGE("creating new array...");
    jclass bitmapCls = env->GetObjectClass(bitmap);
    jobjectArray infos = env->NewObjectArray(BITMAPS_SIZE, bitmapCls, NULL);
    IMAGE images[BITMAPS_SIZE];
    int s = parseWaveToArray(bitmapPixels, info.width, info.height, images);
    LOGE("s = %d", s);
    int i;
    for(i = 0; i < s; i++) {
        jobject bp = createBitmap(env,  bitmapCls, images[i]);
        env->SetObjectArrayElement(infos, i, bp);
    }

    AndroidBitmap_unlockPixels(env, bitmap);
    return infos;
}
```
native函数的声明
``` java
 public native Bitmap[] returnBitmapArray(Bitmap bitmap);
```