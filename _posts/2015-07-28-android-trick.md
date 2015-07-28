---
layout: post
title: 'Android开发及使用技巧'
date: '2015-07-28'
header-img: "img/post-bg-android.jpg"
tags:
     - android
author: 'Codeboy'
---

### 1. 查看wifi密码

查询连接的wifi密码，没问题，前提是手机已经root了，可以查看系统文件，android的wifi密码明文保存在一下文件中，使用root explorer查看即可。

	/data/misc/wifi/wpa_supplicant.conf


### 2. 查看activity堆栈情况

	adb shell dumpsys activity ---------------查看ActvityManagerService 所有信息
	adb shell dumpsys activity activities ----------查看Activity组件信息
	adb shell dumpsys activity services -----------查看Service组件信息
	adb shell dumpsys activity providers ----------产看ContentProvider组件信息
	adb shell dumpsys activity broadcasts --------查看BraodcastReceiver信息
	adb shell dumpsys activity intents --------------查看Intent信息
	adb shell dumpsys activity processes ---------查看进程信息


### 3. 查看apk中的AndroidManifest.xml文件

可以使用apktool对apk进行反编译，不过现在很多的apk都进行了加固，可以防止apktool进行反编译，我们可以使用google提供的aapt(Android Asset Packaging Tool)进行查看:

	aapt dump xmltree target.apk AndroidManifest.xml

>其中target.apk为需要查看AndroidManifest.xml的apk包。


### 4. 查看android的log

	adb logcat
	
	"-s"选项 : 设置输出日志的标签, 只显示该标签的日志;

	"-f"选项 : 将日志输出到文件, 默认输出到标准输出流中, -f 参数执行不成功;

	"-r"选项 : 按照每千字节输出日志, 需要 -f 参数, 不过这个命令没有执行成功;

	"-n"选项 : 设置日志输出的最大数目, 需要 -r 参数, 这个执行 感觉 跟 adb logcat 效果一样;

	"-v"选项 : 设置日志的输出格式, 注意只能设置一项;

	"-c"选项 : 清空所有的日志缓存信息;

	"-d"选项 : 将缓存的日志输出到屏幕上, 并且不会阻塞;

	"-t"选项 : 输出最近的几行日志, 输出完退出, 不阻塞;

	"-g"选项 : 查看日志缓冲区信息;

	"-b"选项 : 加载一个日志缓冲区, 默认是 main, 下面详解;

	"-B"选项 : 以二进制形式输出日志;



持续更新...

> 如有任何知识产权、版权问题或理论错误，还请指正。
>
> 转载请注明原作者及以上信息。
