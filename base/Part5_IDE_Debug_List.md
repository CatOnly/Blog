# 环境配置问题的一般性调试流程列表

## Android Studio

1. 确定 build Varians 是否正确

2. 确定 local.properties 中的 ndk 和 sdk 路径是否正确

3. build / Clean Project
   build / Refresh Linked C++ Projects
   build / Rebuild Project

4. 重启 Android Studio

5. 删除 项目目录下的 .idea 和 .gradle 文件夹后
   重启 Android Studio，然后 Make Project

6. 如果 ndk 版本较新（大于 r12？）却使用了 `.mk` 来编译 C++ / C 文件
   由于 Google 正在推进使用 cmake 而非原来自家的 `.mk` 文件
   需要修改 gradle.properties 下的 `android.deprecatedNdkCompileLease=1558259721076`
   这个数字

   - 确保使用 `.mk` 而不是 cmake 的具体时间截止日期
   - 需要固定的增长，如 `1556259721076 => 1558259721076 `

   