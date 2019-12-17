[TOC]

<p><h1> <center>问题总结 - 常见平台环境配置问题</center> </h1></p>
## 一、Android 平台问题

### 1. Android Studio 的问题
1. Android Studio 有时候 Run 产生的异常信息要比 Build 要清晰
2. 确定 Android Studio 的 build Varians 是否正确，在不同 build Varians 来回切换可以解决一些莫名其妙的问题
3. 确定 local.properties 中的 NDK 和 SDK 路径是否正确（为了维持项目的整体稳定，做到最小修改，有些 module 在 gradle 配置里会定义自己使用的 NDK 和 SDK 路径）
   `.mk` 和 cmake 文件里也会重新配置新的 NDK 路径，也需要核实一遍
4. 在编写 JNI 代码时，如果有多个 JNI 文件要使用 Android Studio 会报一些
   jni.h 找不到，C++ 文件找不到等 IDE 代码拼写检查错误，在这种情况下，JNI 部分没有代码提醒，**即使编译运行都没有问题**，Studio 还是会报错
   建议找个文本编辑器编写，忽略 Studio 的拼写检查



### 2. Android Studio 缓存更新不及时问题
**Android Studio 编译操作介绍**

- Make Project
  编译 Project 下所有 Module，一般是自上次编译后 Project 下有更新的文件
- Make Selected Modules
  编译指定的Module，一般是自上次编译后 Module 下有更新的文件
- Clean Project
  删除之前编译后的编译文件，并重新编译整个 Project，比较花费时间
- Rebuild Project
  先执行 Clean 操作，删除之前编译的编译文件和可执行文件，然后重新编译新的编译文件
  效果跟 Clean Project 一致
- Build APK
  前面 4 个选项都是编译，没有生成 apk 文件，如果想生成 apk，需要点击 Build APK
- Generate Signed APK
  生成有签名的 apk



**Android studio 有时找不到 R 文件、 导入的资源、布局、 jar 包 等可能是 studio 缓存造成的**

1. gradle sync
   build / Clean Project
   build / Refresh Linked C++ Projects
   build / Rebuild Project
2. build / Clean Project
   然后选择工具栏  File => Invalidate Caches /Restart… => Invalidate and Restart
3. 删除 项目目录下的 .idea 和 .gradle 缓存和配置文件夹后
   重启 Android Studio，然后 Make Project



### 3. NDK 版本自身 BUG
1. NDK 版本与当前项目不一致（有些项目及其依赖可能使用不同的 NDK 版本，这是首先要排除的问题）
2. [NDK 官方解释文档](https://developer.android.google.cn/ndk/guides)
3. [Android NDK 编译器由 GCC 切换到 Clang 不同 NDK 版本带来的问题](https://zhuanlan.zhihu.com/p/27470060)
4. 如果 NDK 版本较新（大于 r12？）却使用了 `.mk` 来编译 C++ / C 文件
   由于 Google 正在推进使用 cmake 而非原来自家的 `.mk` 文件
   需要修改 gradle.properties 下的 `android.deprecatedNdkCompileLease=1558259721076`
   这为了确保使用 `.mk` 而不是 cmake 的具体时间截止日期（首先使用 `android.useDeprecatedNdk=true` 当 studio 在命令行报错时会给出一个允许的时间戳，然后将 `android.deprecatedNdkCompileLease` 时间设为获取到的时间）



### 4. JNI 层编译连接问题
1. 从 aar 解压出来的 so，刚添加却一直无法链接到项目里
   从 aar 解压出来的 so 创建时间太早，且最近修改时间也较早
   需要在命令行执行 `touch XX.so` 将其修改时间设置为当前，Android studio 才能识别这个 so 为刚更新的 so 并重新链接



### 5. Java 层问题
1.  Java 代码的混淆
这时要将三方库集成到该项目，需要在名称类似 proguard.pro 的文件里添加三方库的 java 包名，便于 java 代码混淆后的链接



### 6. 包管理问题

1. Java 语言的项目一般采用 maven 进行包的存储和管理，[关于 maven 的介绍看这里](https://www.cnblogs.com/zachary7/p/7975632.html)



### 7. 硬件适配问题

1. [CPU 32 位 和 64 位适配](https://blog.csdn.net/lyabc123456/article/details/81557220)
2. [Neon 指令集 ARMv7/v8 对比](https://blog.csdn.net/zsc09_leaf/article/details/45825015)



### 8. 其他

1. 单独编译项目中的一个 model 查看的错误信息更详细



## 二、iOS 平台问题

### 1. Xcode 的问题

1. 证书问题
   证书运行发现过期或者没有时（本来是有的），需要先 build 下在运行，可消除错误
2. 代码错误提示一直存在
   Xcode 卡顿了，需要等一段时间错误会自动消失
3. Xcode 断点共享
   A 工程编译 B 工程提供的 .a 库时，可在 B 工程的 Xcode 中，选中断点右键把断点改为**共享断点**
   并且将 B 工程的断点 move to User，这样在 A 工程中也能使用该断点



### 2. Xcode 缓存更新不及时问题





### 3. clang 编译、连接问题





### 4. 资源读取问题

1. 图片资源优化问题
   Xcode 工程直接放入图片文件，会将图片优化导致 其他三方库（例如 C++ 平台上的）无法直接读取图片资源
   解决方法，将图片放入一个 bundle 中



### 5. 包管理问题

1. [cocoapods 快速更新](https://www.jianshu.com/p/e979f3398653)

