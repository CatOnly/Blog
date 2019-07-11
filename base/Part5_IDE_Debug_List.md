[TOC]

# 环境配置问题总结

## 一、Android 的通用 BUG

### 1. Android Studio IDE 的问题
1. Android Studio
2. 有时候 Run 产生的异常信息要比 Build 要清晰
3. 确定 Android Studio 的 build Varians 是否正确，在不同 build Varians 来回切换可以解决一些莫名其妙的问题
4. 确定 local.properties 中的 NDK 和 SDK 路径是否正确（为了维持项目的整体稳定，做到最小修改，有些 module 在 gradle 配置里会定义自己使用的 NDK 和 SDK 路径）
   `.mk` 和 cmake 文件里也会重新配置新的 NDK 路径，也需要核实一遍



### 2. Android Studio IDE 缓存更新不及时问题
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
1. [NDK 官方解释文档](https://developer.android.google.cn/ndk/guides)
2. [Android NDK 编译器由 GCC 切换到 Clang 不同 NDK 版本带来的问题](https://zhuanlan.zhihu.com/p/27470060)
3. 如果 NDK 版本较新（大于 r12？）却使用了 `.mk` 来编译 C++ / C 文件
   由于 Google 正在推进使用 cmake 而非原来自家的 `.mk` 文件
   需要修改 gradle.properties 下的 `android.deprecatedNdkCompileLease=1558259721076`
   这为了确保使用 `.mk` 而不是 cmake 的具体时间截止日期，而且这个数字需要固定的增长，如 `1556259721076 => 1558259721076 `



### 4. JNI 层编译连接问题
1. 从 aar 解压出来的 so，刚添加却一直无法链接到项目里
   从 aar 解压出来的 so 创建时间太早，且最近修改时间也较早
   需要在命令行执行 `touch XX.so` 将其修改时间设置为当前，Android studio 才能识别这个 so 为刚更新的 so 并重新链接



### 5. Java 层问题
1.  Java 代码的混淆
这时要将三方库集成到该项目，需要在名称类似 proguard.pro 的文件里添加三方库的 java 包名，便于 java 代码混淆后的链接