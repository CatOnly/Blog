# 环境配置问题的一般性调试流程列表

## Android Studio

1. 有时候 Run 产生的异常信息要比 Build 要清晰

2. 确定 build Varians 是否正确

3. 确定 local.properties 中的 ndk 和 sdk 路径是否正确
   `.mk` 和 cmake 文件里也会重新配置新的 ndk 路径，也需要核实一遍

4. gradle sync
   build / Clean Project
   build / Refresh Linked C++ Projects
   build / Rebuild Project

5. 重启 Android Studio

6. 删除 项目目录下的 .idea 和 .gradle 文件夹后
   重启 Android Studio，然后 Make Project

7. 如果 ndk 版本较新（大于 r12？）却使用了 `.mk` 来编译 C++ / C 文件
   由于 Google 正在推进使用 cmake 而非原来自家的 `.mk` 文件
   需要修改 gradle.properties 下的 `android.deprecatedNdkCompileLease=1558259721076`
   这个数字

   - 确保使用 `.mk` 而不是 cmake 的具体时间截止日期
   - 需要固定的增长，如 `1556259721076 => 1558259721076 `

8. 如果刚添加的 so 一直无法链接到项目里
   原因：从 aar 解压出来的 so 创建时间太早，且最近修改时间也较早
   需要在命令行执行 `touch XX.so` 将其修改时间设置为当前，Android studio 才能识别这个 so 为刚更新的 so 并重新链接