---
layout: post
title: 'Android Ndk的应用'
date: '2015-05-04'
header-img: "img/post-bg-android.jpg"
tags:
     - android
author: 'Codeboy'
---

做android开发，或多或少应该对ndk有些了解。大家都知道，开发android应用很多部分是使用java完成的，但是java语言使用起来虽然简单，但是也比较容易进行反编译，尽管现在网络上有很多的加密工具。那怎么保护应用的一些隐私逻辑模块(加解密)的，ndk是一个很好的选择。

ndk使用c或者cpp完成代码的编写，使用c或者cpp可以将一些模块编译为链接库(so文件)，这些文件反编译起来则非常的困难，同时使用c和cpp写出的代码在执行效率上会有所提升。本文将展示使用ndk技术将字符串的简单加解密方法写进so文件中。

考虑到编码等的原因，本文中的加密解密算法方式为：java中将字符串转为为byte数组，然后通过jni调用c语言加解密函数，同时将byte数据传递给c(中间有一部类型转化，将byte数组转化为char数组)，c语言对char数组进行加解密后返回。有关ndk的一些简单使用，大家可以看一些麦子学院的[教程](http://www.maiziedu.com/lesson/3805/),本文中只对使用的一些例子进行解释。对于字符串的加解密，按照本文的方式加解密应该是一种不错的方式，使用中您可能需要修改一下c语言中的加解密函数即可。

本文中c语言对char数组的加解密很简单，对每一个char进行拆分，char占8位，高4位与低4位拆成2个数值，然后根据数值从一个长度为16的钥匙串中拿出对应字符，这些字符对应的数组记为加密后的字符串。反向解密原理相似，只需要将字符数组中每2个字符抽取，计算出加密前的数值即可。下面看一下整体ndk的使用。

使用ndk前，需要从android官网上下载ndk组件，解压后大概3G左右，建议一个新项目(AndroidNDK)，将新项目目录指定在ndk解压后的根目录(这样比较方便，不这样做也可以)，我们需要在新项目中额外添加的只有一个jni目录，jni目录与src，res等是同一层的。目录内需要含有的文件如下所示:

	mac:AndroidNDK YD$ tree  jni
	jni
	├── Android.mk
	├── Application.mk
	└── encrypt.c

其中encrypt.c即是我们的加解密函数所在的位置，Android.mk与Application.mk为配置文件，内容很固定。可以看下各个文件的内容:

**Application.mk**

	APP_ABI := armeabi,armeabi-v7a
	

Application.mk中还有其他的一些配置，大家可以去官网或者google一下，常用的配置是App_ABI,指定生成对应cpu架构的库文件。

**Android.mk**

	LOCAL_PATH := $(call my-dir)
	include $(CLEAR_VARS)
	LOCAL_MODULE    := codeboy_encrypt
	LOCAL_SRC_FILES := encrypt.c
	LOCAL_LDLIBS := -L$(SYSROOT)/usr/lib -llog
	include $(BUILD_SHARED_LIBRARY)

Android.mk中需要修改的内容只有LOCAL_MODULE和LOCAL_SRC_FILES,前者指定生成的连接库名称，后者指定要编译的c语言或者cpp的文件名字，其他的保持不变即可。

**encrypt.c**

	#include<string.h>
	#include<jni.h>
	#include<android/log.h>

	//宏定义打印函数,使用方法 LOGI("hello") 或者 LOGI("money %d",15)
	#define LOGI(...) ((void)__android_log_print(ANDROID_LOG_INFO, "native", __VA_ARGS__))
	#define LOGW(...) ((void)__android_log_print(ANDROID_LOG_WARN, "native", __VA_ARGS__))

	const char key[] = "abcdefghijklmnop"; //16个字符
	int len = 0;

	//计算字符对应的byte值
	unsigned char getByteNumber(unsigned char first, unsigned char end) {
	    int firstPosition = 0, endPosition = 0;
	    int position = 0;
	    for (; position < 16; position++) {
	        if (key[position] == first) {
	            firstPosition = position;
	        }
	        if (key[position] == end) {
	            endPosition = position;
	        }
	    }
	    return (firstPosition << 4) | (endPosition);
	}

	//加密函数
	void encrypt(unsigned char p[], unsigned char res[]) {
	    int i = 0;
	    for (; i < len; i++) {
	        res[2 * i] = key[p[i] / 16];
	        res[2 * i + 1] = key[p[i] % 16];
	    }
	}

	//解密函数
	void decrypt(unsigned char p[], char res[]) {
	    int i;
	    for (i = 0; i < len; i++) {
	        res[i] = getByteNumber(p[i * 2], p[i * 2 + 1]);
	    }
	}

	//java中生命的native函数，函数名称格式Java_包名(点换下划线)_类名_函数名
	//前两个参数JNIEnv *env, jclass this比较固定，其中第二个参数jclass代表方法是静态的，仅仅是个表示，如果方法不是静态的话，jclass换成jobject
	//后续的参数是函数要传进来的参数
	//java中的byte数组对应jni中的jbyteArray,jni中的jbyteArray可以通过jni中的函数转换为char数组
	jstring Java_me_codeboy_encrypt_EncryptUtil_encrypt(JNIEnv *env, jclass this,
	        jbyteArray src) {
	    unsigned char *buff = (char*) (*env)->GetByteArrayElements(env, src, NULL);
	    len = (*env)->GetArrayLength(env, src);
	    //加密后长度变为原先的2倍
	    unsigned char res[len * 2];
	    encrypt(buff, res);
	    //此步骤很重要，标志结束
	    res[len * 2] = '\0';

	    //使用完毕释放src数组，因为src数组的存在jvm中
	    (*env)->ReleaseByteArrayElements(env, src, buff, 0);

	    //jni中函数将char数组转变为字符串，jni中字符串为jstring，对应java中的String
	    jstring resStr = (*env)->NewStringUTF(env, res);
	    return resStr;
	}

	//和加密类似
	jstring Java_me_codeboy_encrypt_EncryptUtil_decrypt(JNIEnv *env, jclass this,
	        jbyteArray src) {
	    unsigned char *buff = (char*) (*env)->GetByteArrayElements(env, src, NULL);
	    len = (*env)->GetArrayLength(env, src);
	    //解密后长度变为原先的1/2
	    len = len / 2;
	    signed char res[len];
	    decrypt(buff, res);
	    //此步骤很重要，标志结束
	    res[len] = '\0';

	    //使用完毕释放src数组，因为src数组的存在jvm中
	    (*env)->ReleaseByteArrayElements(env, src, buff, 0);

	    jstring resStr = (*env)->NewStringUTF(env, res);
	    return resStr;
	}

这样我们的ndk相关的文件就写好了，下面在终端下切换到AndroidNDK目录下,运行命令即可:

	../ndk-build
	
运行结果如下:

	mac:AndroidNDK YD$ ../ndk-build
	Android NDK: WARNING: APP_PLATFORM android-21 is larger than android:minSdkVersion 14 in ./AndroidManifest.xml    
	[armeabi] Compile thumb  : codeboy_encrypt <= encrypt.c
	[armeabi] SharedLibrary  : libcodeboy_encrypt.so
	[armeabi] Install        : libcodeboy_encrypt.so => libs/armeabi/libcodeboy_encrypt.so
	[armeabi-v7a] Compile thumb  : codeboy_encrypt <= encrypt.c
	[armeabi-v7a] SharedLibrary  : libcodeboy_encrypt.so
	[armeabi-v7a] Install        : libcodeboy_encrypt.so => libs/armeabi-v7a/libcodeboy_encrypt.so

执行完成后，我们可以看一下libs文件夹,多了一些so文件,如下:

	mac:AndroidNDK YD$ tree libs
	libs
	├── android-support-v4.jar
	├── armeabi
	│   └── libcodeboy_encrypt.so
	└── armeabi-v7a
	    └── libcodeboy_encrypt.so

	2 directories, 3 files


下面我们就开始写对应的java代码了，将调用c函数的加解密函数抽象到一个类中即可

**EncryptUtil.java**

	package me.codeboy.encrypt;

	public class EncryptUtil {
	    public native static String encrypt(byte[] src); // 加密函数

	    public native static String decrypt(byte[] src); // 解密函数

	    static {
	        System.loadLibrary("codeboy_encrypt");
	    }

	    /**
	     * 加密函数
	     * 
	     * @param src
	     * @return
	     */
	    public static String encrypt(String src) {
	        return encrypt(src.getBytes());
	    }

	    /**
	     * 解密函数
	     * 
	     * @param src
	     * @return
	     */
	    public static String decrypt(String src) {
	        return decrypt(src.getBytes());
	    }
	}

注意C语言中的函数名称中的包名类名函数名要与该类统一，还有对应的链接库名称。

做好了这些以后，我们就可以使用了。在我们的Activity中简单的定义一个按钮，点击后对一个字符串进行加密打印，之后进行解密打印，Activity中的代码如下:

	package me.codeboy.ndk.ui;

	import me.codeboy.encrypt.EncryptUtil;
	import me.codeboy.ndk.R;
	import android.app.Activity;
	import android.os.Bundle;
	import android.view.View;
	import android.view.View.OnClickListener;
	import android.widget.Button;

	public class MainActivity extends Activity {
	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
	        setContentView(R.layout.main_ui);
	        super.onCreate(savedInstanceState);

	        Button btn = (Button) findViewById(R.id.btn);
	        btn.setOnClickListener(new OnClickListener() {

	            @Override
	            public void onClick(View v) {
	                String src = "我是玄恒，欢迎访问我的网站codeboy.me";
	                String res = EncryptUtil.encrypt(src);
	                System.out.println("------>" + res);
	                res = EncryptUtil.decrypt(res);
	                System.out.println("--->" + res);
	            }
	        });
	    }
	}


点击按钮后可以看到点击button打印的结果:

	--->ogiijbogjikpohioieogibjcoplmimogkmkcoilpiooikolpojjhkoogiijbohjkieohlnjbohkljjgdgpgegfgcgphjcogngf
	--->我是玄恒，欢迎访问我的网站codeboy.me

这样下来我们就完成了简单的加解密操作,也对ndk有了一个初步的了解。


> 如有任何知识产权、版权问题或理论错误，还请指正。
>
> 转载请注明原作者及以上信息。
