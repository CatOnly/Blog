[TOC]


<p><h1><center>平台常用知识体系梳理</center></h1></p>
> 宗旨，不要浪费时间在已有的工具/文章上，复用别人的工具/文章，让自己更专注于自己的领域



## 一、Android 平台

### 1. Android Studio 使用

1. [全局搜索](https://jingyan.baidu.com/article/ca2d939d7943e3eb6c31cebc.html)



### 2. Android 项目工程文件

1. [Android 程序入口以及项目文件夹的含义](https://www.cnblogs.com/mingjiatang/p/5978538.html)
2. [gradle 配置](https://www.cnblogs.com/wxishang1991/p/5457878.html)
    [gradle 参数解释](https://www.jianshu.com/p/c11862136abf)
  [gradle 项目构建](https://www.cnblogs.com/smyhvae/p/4456420.html)




### 3. 相机

1. [Android四大组件：这是一份全面 & 详细的Activity学习指南](https://blog.csdn.net/carson_ho/article/details/82848840)
2. [Camera2 简介](https://www.jianshu.com/p/23e8789fbc10)
3. [手机旋转角度](https://www.jianshu.com/p/f2cce22280df)
4. [SensorManager](https://developer.android.google.cn/reference/android/hardware/SensorManager)




### 4. 存储

1. [SharedPreferences](https://www.jianshu.com/p/13f26d68b02e)
2. [Android 文件外/内部存储的获取各种存储目录路径](https://www.jianshu.com/p/2de0113b3164)
   [Android中读取assets目录下的文件详细介绍](https://blog.csdn.net/greathfs/article/details/52123984)
3. [NDK 下 AAssetManager](https://blog.csdn.net/u012098794/article/details/53405142)




### 5. 编译 JNI

1. [如何将Android studio 的项目变成Lib工程，供项目使用](https://blog.csdn.net/qq_33373648/article/details/75671402)
2. [Android 调用so库全过程](https://blog.csdn.net/liujian8654562/article/details/78717149)
3. [JNI 接口编写](https://www.cnblogs.com/rocomp/p/4892866.html)
   [JNI 手册](https://blog.csdn.net/eydwyz/article/details/77800867)
   [JNI API 介绍](https://blog.csdn.net/yuzhou_zang/article/details/78410632)
4. [aar 介绍](https://blog.csdn.net/xiexiangyu92/article/details/75200091)
   [aar = jar + so 介绍](https://blog.csdn.net/mq2856992713/article/details/77619299)
5. [JNI 数据类型转换和 JNIEnv 的介绍、操作 jobject，以及 jstring 的介绍](https://blog.csdn.net/u010947098/article/details/71079380)
6. [GetObjectClass、FindClass和GetMethodID](https://www.jianshu.com/p/dddb0ccd476a)
7. [string 、 char* 和 jstring 的互相转换](https://blog.csdn.net/xlxxcc/article/details/51106721)
   [jni 获取对象 string 类型属性](https://blog.csdn.net/xiuye2015/article/details/90111908)
8. [NDK 开发](https://blog.csdn.net/afei__/article/details/81290711)
9. [Java 成员变量为 C++ 的指针对象 / 智能指针对象](https://www.studiofuga.com/2017/03/10/a-c-smart-pointer-wrapper-for-use-with-jni/)
10. [三方库管理存储工具 Maven](https://www.cnblogs.com/zachary7/p/7975632.html)
11. [C++ Hook Java 代码](https://zhuanlan.zhihu.com/p/109157321)



### 6. 语法

1. [android 不建议使用枚举类](https://www.cnblogs.com/zgz345/p/5871351.html)
   [为什么推荐使用注解代替枚举](https://www.jianshu.com/p/80180e1728d0)
   [什么是注解](https://www.cnblogs.com/liaojie970/p/7879917.html)
2. [Java 的反射和动态代理](https://www.cnblogs.com/jingmoxukong/p/12049112.html)



### 7. 调试

1. [logcat 抓取 app 日志](https://blog.csdn.net/tscying/article/details/79317537)
2. [adb logcat 的妙用（输出 log 到指定文件中）](https://blog.csdn.net/qq_34801506/article/details/81014994)
3. `adb push path_compute path_mobile`：从电脑上传送文件到手机
   `adb pull path_mobile path_compute`：从手机传送文件到电脑上
4. [系统自带的 GPU 呈现分析](https://www.jianshu.com/p/ac2d58666106)
5. [高通骁龙 Adreno GPU Profiler 调试工具（建议在 windows 下使用，mac 下测试无用）](https://gameinstitute.qq.com/community/detail/123051)
6. [GAPID 调试 Android 应用，需要 Android stuido 停用 adb 的使用](http://www.geeks3d.com/20171214/google-gapid-capture-vulkan-and-opengl-es-calls-on-android-windows-macos-and-linux/)



## 二、iOS 平台

### 1. Xcode 使用

1. [iOS 和 Xcode 的真机调试 版本匹配问题](https://www.jianshu.com/p/9e72bec04c06)
2. [Storybroad 的使用](https://www.jianshu.com/p/aaa4b89dbba1)
3. [Storybroad 比例布局](https://segmentfault.com/a/1190000002997979)
4. [Xcode 不升级添加 deivce support 文件](https://github.com/iGhibli/iOS-DeviceSupport/tree/master/DeviceSupport)
5. [Could not launch app 解决办法](https://blog.csdn.net/u012581760/article/details/78528480?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task)
6. [Xcode 垃圾清理](https://www.jianshu.com/p/225a4b3dd88e)
7. [App Installation failed, No code signature found](https://blog.macrowolf.cn/31.html)
8. [CocoaPods的常见使用方式](https://www.cnblogs.com/hs-funky/p/6759977.html)
9. [自定义cocoapods库](https://www.jianshu.com/p/oZfb8s)
10. 装新的描述文件时，需要删掉旧的描述文件，位置在：
    `cd ~/Library/MobileDevice/Provisioning Profiles/`




### 2. Xcode 项目工程文件

1. [Xcode添加头文件路径和链接库文件](https://blog.csdn.net/tintinr/article/details/50936313)
2. [Xcode中的环境变量](https://www.jianshu.com/p/74b2a1a46179)



### 3. 相机

1. [UIDevice](https://www.jianshu.com/p/aaa4b89dbba1)

2. [自定义相机](https://blog.csdn.net/qq_30513483/article/details/51198464)

3. [iOS  官方 Demo AVCamManual 相机的曝光，白平衡](https://github.com/sri-omg/AVCamManual)

4. [相机曝光、白平衡](https://docs.microsoft.com/zh-cn/xamarin/ios/user-interface/controls/intro-to-manual-camera-controls)

5. [屏幕旋转 UIDeviceOrientation](https://www.jianshu.com/p/36bf3f9f2141)

6. [iOS 视频采集编码前后显示汇总](https://www.jianshu.com/p/1a241674a075)

7. [AVFoundation](https://www.jianshu.com/p/0537388ef8ef)

8. [Drawing to Other Rendering Destinations](https://developer.apple.com/library/archive/documentation/3DDrawing/Conceptual/OpenGLES_ProgrammingGuide/WorkingwithEAGLContexts/WorkingwithEAGLContexts.html#//apple_ref/doc/uid/TP40008793-CH103-SW1)

9. [iOS 传感器与 CMMotionManager](https://blog.csdn.net/boring_cat/article/details/51153476)

10. [iOS camera: manual exposure duration but auto ISO?](https://stackoverflow.com/questions/29819515/ios-camera-manual-exposure-duration-but-auto-iso)



### 4. 存储

1. [iOS 目录结构](https://my.oschina.net/liuchuanfeng/blog/388338)
2. [资源路径获取](https://blog.csdn.net/xuhuan_wh/article/details/7474310)



### 5. 编译

1. [LLVM 文档](https://releases.llvm.org/)
2. [iOS开发中BitCode功能](https://www.jianshu.com/p/8eff48e5c010)



### 6. 语法

1. [NSString / NSData / char* 类型之间的转换](https://www.cnblogs.com/pengyingh/articles/2341880.html)
2. [iOS BOOL 和 bool](https://www.jianshu.com/p/efe976b53560)
3. [iOS9 的几个新关键字（nonnull、nullable、null_resettable、__null_unspecified）](https://www.cnblogs.com/alan12138/p/5620021.html)
4. [iOS 动画 —— transform](https://www.jianshu.com/p/3bc427f0dd56)



### 7. 调试

1. [iOS lldb 调试命令](https://blog.csdn.net/ios_xumin/article/details/42711759)
2. [获取其他 APP 的 ipa](https://imazing.com/guides/how-to-manage-apps-without-itunes)
3. [Xcode GPU debug](https://www.cnblogs.com/TracePlus/p/4093830.html)
4. [获取 app ipa 资源](https://www.jianshu.com/p/009018d11648)




## 三、QT 平台

### 1. QCreator 使用

### 2. QCreator 项目工程文件

### 3. 相机

### 4. 存储

- [Qt程序关于路径、用户目录路径、临时文件夹位置获取方法](https://www.cnblogs.com/icmzn/p/6953948.html)

  

### 5. 编译

### 6. 语法

- 事件使用了一个事件队列来维护，如果事件的处理中又产生了新的事件，那么新的事件会加入到队列尾，直到当前事件处理完毕后， QApplication再去队列头取下一个事件来处理

- 信号的处理方式有些不同，信号处理是立即回调的，也就是一个信号产生后，他上面所注册的所有槽都会立即被回调
  这样就会产生一个递归调用的问题，比如某个信号处理器中又产生了一个信号，会使得信号的处理像一棵树一样的展开

  

### 7. 调试

