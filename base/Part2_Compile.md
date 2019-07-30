[TOC]

# 一、确定编译的依赖关系

**确定编译的依赖关系：确定了编译时需要哪些头文件，这是个在编译时拼接资源文件的过程**



## 1. 头文件的作用

- 只暴露接口，隐藏实现细节
- 加强类型安全检查
  某个接口被实现或被使用时，其方式与头文件中的声明不一致，编译器就会指出错误
- 防止重复声明，便于修改声明
  编译器在编译代码时对代码的后缀名并不敏感

  > **早期编译器只认识 .c / .cpp 文件，而没有 .h 文件**。多个 .c / .cpp 文件中存在大量的重复声明。其中有一个声明变更时，就需要修改其他重复声明的 .c / .cpp 文件。为了防止某个声明发生了变更到处寻找与修改相关文件，人们将重复的部分提取出来放在一个后缀名叫 .h 的新文件里，并在 .c / .cpp 文件内首行添加引入。



## 2. 明确依赖关系

> .c / .cpp和 .h 文件名称即便相同也没有任何直接关系，很多编译器都可以接受其他扩展名

假定 A 文件依赖于 B 文件，编译器应该保证：

- 先编译依赖的文件 B 在编译 A 文件
- 当依赖文件发生变化时，A 文件也会重新编译

- .h 文件中的所有内容会被写到包含它的 .c / .cpp 文件中
  **在 .c / .cpp 文件中的 `#include` 对应的 .h 文件来防止 .c / .cpp 文件中函数的相互调用时，调用的函数没有声明的现象**
- **先在预处理阶段**找到 main 函数所在的 .c / .cpp 文件，然后**在链接阶段**通过 .c / .cpp 文件引入的头文件找到相应的其他文件编译
  所有的 .c / .cpp 文件以一个共同的 main 函数作为可执行程序的入口



## 3. 预编译头文件

采用**预处理头文件**，能将公共且不常改变的头文件放在公共的 PCH 文件里，减少不必要的代码重新编译工作，**提高编译效率**

预编译头文件的使用方法：

- 创建预编译头文件 .h（如果是 gcc 编译器还要创建 .cpp 文件）
  预编译头文件中应该 include 很多长期不易变动的文件，这样才能只在第一次编译的时候编译预编译头文件，在以后的编译中不断调用已经生成的 .pch 预编译头文件（后缀名可以是任意的）
- 在所有依赖预编译头文件的代码（.h / .hpp / .c / .cpp）中的第一行引入预编译头文件




# 二、编译步骤

> GCC：GNU Compiler Collection，GNU编译器套件
>
> 包括 C、C++、Objective-C、Fortran、Java、Ada 和 Go语言 的前端，也包括了这些语言的库（如 libstdc++、libgcj 等等）

**编译器工作之前需要配置**

- 在编译之前，需要对编译器进行配置：确定编译参数，配置标准函数库和头文件的位置

- 一般编译器的配置工作写在配置文件里，或者由 IDE 通过在 UI 界面里进行配置来代劳

以下，以 hello.c 文件为例介绍 GCC 编译器的工作流程。其中 hello.c 内容如下

```c
#include <stdio.h>

/** 编译器先从 main 函数所在的源文件开始编译 */
int main()
{
    printf("Hello World\n");
    return 0; 	// 不要忘了 main 函数的 return 值
}
```

以 GCC 编译器为例，在编译源代码时都会先后经历：预处理，编译，汇编，链接这 4 个步骤

```shell
$gcc hello.c	# GCC 编译器经过预处理，编译，汇编，链接，得到文件 a.out
$./a.out 		  # 运行 a.out 文件
Hello World		# 得到 a.out 文件的运行结果
```



## 1. 预处理（Prepressing）

```shell
$cpp hello.c > hello.i		  # CPP 预编译器 把 hello.c 文件生成 .i 文件
$gcc -E hello.c -o hello.i	# GCC 编译器通过 -E 进行预编译，由 .c 文件生成 .i 文件
```

| 预编译       | 输入                     | 输出 |
| ------------ | ------------------------ | ---- |
| C 程序代码   | .h、.c                   | .i   |
| C++ 程序代码 | .h、.c、.hpp、.cpp、.cxx | .ii  |

预处理主要处理以 `#` 开始的预编译指令，**检测依赖关系，进行宏替换**，比如：`#include`、`#define` 

处理规则：

- 递归处理所有预编译指令 `#include` 将被包含的文件插入到该预编译指令位置
- 删除所有注释
- 删除所有预编译指令 `#define`，展开宏定义
- 处理所有条件预编译指令，比如 `#if`、`#ifdef`、`#elif`、`#else`、`#endif`

- 添加行号和文件名标识，供编译器调试
- 保留所有 `#pragma` 编译器指令，供编译器使用



## 2. 编译（Compilation）

```shell
$gcc -S hello.i -o hello.s	# GCC 编译器通过 -S 将 预处理文件 .i 文件 编译为 .s 文件
$gcc -S hello.c -o hello.s	# GCC 编译器通过 -S 将 源代码文件 .c 文件 预处理后再编译为 .s 文件
```

> **根据不同的编译器**，可以在编译阶段加入不同的参数控制编译行为

gcc 会根据不同类型的源文件调用相应的编译程序进行 词法分析，语法分析，语义分析及优化后生成相应的汇编代码文件

- C 语言代码调用 ccl 程序编译
- C++ 语言代码调用 cclplus 程序编译
- Objective-C 语言代码调用 cclobj 程序编译
- Java 语言代码调用 jcl 程序编译



处理规则：

- 现代编译器有很多层次的优化，但在不同的编译器中会有不同的差异
- 在编译期间就能确定的表达式可以优化，比如 `a = 2 + 6;` 会被优化为 `a = 8;`



## 3. 汇编（Assembly）

```shell
$as hello.s -o hello.o		  #  as 汇编器将 .s 文件汇编为 .o 二进制文件
$gcc -c hello.s -o hello.o	# GCC 编译器将 .s 文件汇编为 .o 二进制文件
$gcc -c hello.c -o hello.o	# GCC 编译器将 .c 文件编译为 .o 二进制文件
```

处理规则：

- 根据汇编指令和机器指令对照表一一翻译
- 每个汇编语句几乎都对应一条机器指令，操作简单，不需要优化



## 4. 链接（Linking）

```shell
# ld 链接器将 hello.o 和其使用的相关库链接起来，一起编译出 a.out 可执行文件
$ld -static crtl.o crti.o crtbeginT.o hello.o -start-group -lgcc -lgcc_eh -lc -end-group crtend.o crtn.o
```

> **根据不同的编译器**，可以在链接阶段加入不同的参数控制链接行为
>
> **编译器会在每个.o / .obj 文件中都去找一下所需要的符号，而不是只在某个文件中找或者说找到一个就不找了**
>
> 因此，如果在几个不同文件中实现了同一个函数，或者定义了同一个全局变量，链接的时候就会提示 "redefined"

这里 `-lgcc -lgcc_eh -lc` 为链接器在链接期间需要的 3 个参数

处理规则：

- 地址和空间分配，定义在**其他模块或者本模块**的全局变量、函数等地址的分配
- 符号绑定（也叫：符号决议、地址绑定、名称绑定、名称决议）
  **决议（Resolution）偏静态，绑定（Binding）偏动态**
- 重定位，在链接时重新计算在本模块中调用的其他模块函数或变量的地址
  （链接使用的 .o 文件准确的说只能叫中间目标文件）
- 将中间目标文件和相关库（.a、.so / .dll / .dylib）一起链接



静态链接：

动态链接：



# 三、编译后数据的存储

## 内存布局



草稿：

交叉编译： build、host、systroot

make-standalone-tools.py



# 参考

- [Linkers & Loaders](https://book.douban.com/subject/1436811/)
- [Recursive Make Considered Harmful](http://aegis.sourceforge.net/auug97.pdf)
- [Building C Projects](http://nethack4.org/blog/building-c.html)
- [Precompiled headers](http://itscompiling.eu/2017/01/12/precompiled-headers-cpp-compilation/)
- [C 语言中 .h 和 .c 文件解析](https://www.cnblogs.com/laojie4321/archive/2012/03/30/2425015.html)