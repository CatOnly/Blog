[TOC]

# 一、基础

## 1. shell 简介
- shell 是用户和 Linux 内核交互的一种统一接口协议
- bash 是实现和执行 shell 这种接口协议的程序，用户通过在命令行上使用 bash 命令和 Linux 内核交互
  bash 是 Linux/Unix 系统默认的 shell 执行程序，类似于 bash 的程序有很多，比如：zsh

zsh

- [zsh 介绍 池建强](http://macshuo.com/?p=676)

- [oh-my-zsh 具体使用](https://github.com/robbyrussell/oh-my-zsh)



## 2. 切换 shell

1. 通过注释设置 shell 的路径
   `#! ` 是一个特殊的标志符，其后，跟解释此脚本的 shell 路径
   **只有第一行出现的 `#!` 有效，其余的会被判作注释**

   ```shell
   #!/bin/bash
   # shell code ...
   ```

2. 每次执行时说明用哪个 shell 调用
    本方法优先级大于 `#!` 可使 `#!` 指定使用的 shell 无效

    ```shell
    sh ./shellScriptFile.sh
    ```

3. 用命令直接切换默认调用 shell 命令的程序

    ```shell
    chsh -s /bin/bash
    chsh -s /bin/zsh
    ```



## 3. 交互 shell 和 非交互 shell

**Interactive**（交互 shell）：表示 shell 命令运行在可以和用户交互的命令行界面上（非 ui系统 UI 界面），可以实时接收用户的输入

**Non-interactive**（非交互 shell）：表示 shell 命令以脚本的形式运行，不能实时接收用户的输入



## 4. login shell 和 non-login shell

判断当前执行的是不是 login shell：在命令行执行 `echo $0` 查看进程名

- login shell 输出 `-bash`，有前缀 `-`
- non-login shell 输出 `bash` ，没有前缀 `-`



**login shell**

- 通过传入参数 0 代表 login shell
- 表示在用户登入操作系统后运行的 shell
  **使用命令行 或 [SSH](<https://en.wikipedia.org/wiki/Secure_Shell>) 或 `su -` 登入属于用户登入操作，而通过[系统UI 界面](<https://en.wikipedia.org/wiki/X_display_manager_%28program_type%29>)登入则不属于用户登入**

**non-login shell**

- 通过传入参数 1 代表 non-login shell
- 表示在用户登入操作系统后运行的其他不与当前用户绑定的 shell
  **部分系统 UI 界面在登入时会执行 non-login shell**
  执行 shell 文件属于 non-login shell
  在命令行上不登入为 non-login shell 状态，但是 mac 上的命令行一开启便是登入状态



# 二、系统环境配置

## 1. 系统读取配置文件的环境

> **不同的 shell 状态，读取不同的文件，以下以 bash 为例**
>
> - 在 zsh 中，profile 对应 zprofile，.bash_profile 对应 .zsh_profile，.bashrc 对应 .zshrc
> - 由于大部分脚本执行在 non-login 状态下，所以一般我们总是修改 ～/.bashrc 和 /etc/bashrc 文件



**login shell**

1. login shell
2. 读取并执行  `/etc/bashrc`  文件（所有用户的公共配置）
3. 读取并执行 `/etc/profile` 文件（所有用户的公共配置）
4. **如果是 linux 系统而非 mac 系统**
   读取并执行 ` /etc/profile.d/*.sh`
5. 查找  `~/.bash_profile`  文件，如果存在执行  `~/.bash_profile` ，如果不存在
    查找  `~/.bash_login`  文件，如果存在执行  `~/.bash_login` ，如果还不存在
    查找  `~/.profile`  文件，如果存在执行  `~/.profile` 
    （当前用户的配置，**如果是 zsh 会先查找** `~/.zshrc`）

**non-login shell**

1. 不 login shell
2. **如果是 zsh**，读取并执行  `/etc/zshrc`  文件（所有用户的公关配置）
3. 读取并执行 `~/.bashrc` 文件
4. **如果是 linux 系统而非 mac 系统**
   读取并执行 ` /etc/profile.d/*.sh`



## 2. shell 作用域的变化

shell 中声明的变量都是全局的，同一个 shell 中没有局部变量 

执行 login shell 会清空执行前的环境变量，重新读取新的环境变量

执行 non-login shell 一般为 子 shell，子 shell 会继承 父 shell 的环境变量，而自己退出时自己定义的变量也会消失，例：

- 执行 `./test.sh` 会在当前 shell 的环境变量下开启子 shell，test.sh 中的变量不会被保留下来
- **执行 `source ./test.sh` 不会开启 子 shell，test.sh 的变量会被保留下来**



## 3. su 和 sudo

**su 用于切换不同的用户，需要输入对应切换到用户的密码**

切换登入和非登入用户，通过读取不同配置文件，来提前预设不同的环境变量

- 切换到 login shell root 权限：`su -`
  切换到 non-login shell root 权限：`su`
- 切换到 login shell 普通用户：`su -l 用户名 ` 或 `su --login 用户名`
  切换到 non-login shell 普通用户：`su 用户名`



**sudo 用于切换到 sudoer 用户，需要输入当前用户自己的密码**

- 运行 sudo 是以 root 的身份来执行命令
- 运行 sudo 时，系统会在文件  `/etc/sudoers` 查找改用户是否有运行 sudo 的权限，如果有，让该用户输入自己的密码
- sudoers 文件的修改只能用 vim 



# 参考

1. [Difference between Login Shell and Non-Login Shell ?](https://unix.stackexchange.com/questions/38175/difference-between-login-shell-and-non-login-shell)

2. [WHY a **login** shell over a **non-login** shell ?](https://unix.stackexchange.com/questions/324359/why-a-login-shell-over-a-non-login-shell)

3. [深入浅出理解交互式shell和非交互式shell、登录shell和非登录shell的区别](<https://blog.csdn.net/gui951753/article/details/79154496>)

4. [linux权限之su和sudo的差别](https://www.cnblogs.com/slgkaifa/p/6852884.html)
