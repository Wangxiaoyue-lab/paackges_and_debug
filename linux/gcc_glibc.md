# 编译器

## gcc编译器

GNU C Compiler

你写的C代码.c文件通过gcc首先转化为汇编.S文件，之后汇编器as将.S文件转化为机器代码.o文件，生成的.o文件再与其它.o文件，或者之前提到的libc.so.6库文件通过ld链接器链接在一块生成可执行文件。当然，在你编译代码使用gcc的时候，gcc命令已经帮你把这些细节全部做好了。

## g++编译器

不要以为gcc只能编译C代码，g++只能编译c++代码。 后缀为.c的，gcc把它当作是C程序，而g++当作是c++程序，后缀为.cpp的，两者都会认为是c++程序，注意，虽然c++是c的超集，但是两者对语法的要求是有区别的。

在编译阶段，g++会调用gcc,对于c++代码，两者是等价的，但是因为gcc命令不能自动和C++程序使用的库联接，需要这样，gcc -lstdc++, 所以如果你的Makefile文件并没有手动加上libstdc++库，一般就会提示错误，要求你安装g++编译器了。

# 重要库

## libc

libc是Linux下原来的标准C库，也就是当初写hello world时包含的头文件#include < stdio.h> 定义的地方，后来逐渐被glibc取代，也就是传说中的GNU C Library,在此之前除了有libc，还有klibc,uclibc。现在只要知道用的最多的是glibc就行了，主流的一些linux操作系统如 Debian, Ubuntu，Redhat等用的都是glibc或者其变种

原文链接：https://blog.csdn.net/qq_41854911/article/details/122017552

### glibc linux最底层库

glibc是Linux系统中最底层的API，几乎其它任何的运行库都要依赖glibc。

glibc最主要的功能就是对系统调用的封装

除了封装系统调用，glibc自身也提供了一些上层应用函数必要的功能,如string,malloc,stdlib,linuxthreads,locale,signal等等。

提供应用程序和Linux内核之间的接口。

尽管它的名称是C库（用“ C”语言编写的程序的库），但实际上所有动态链接的程序二进制文件都依赖它——它是几乎所有Linux操作系统中的实际系统库。

Glibc 是系统的两大核心之一(还有一个是内核)。

Glibc具有允许向后兼容的版本控制系统（即在较旧版本的glibc上运行构建的旧程序可以继续在新的glibc上运行）；但是，依赖较新版本的glibc的程序通常将无法在旧版本glibc的系统上运行。

原文链接：https://blog.csdn.net/lalaxumelala/article/details/103178680



### elibc

e是Embedded的意思，也就是前面说到的变种glibc

eglibc的主要特性是为了更好的支持嵌入式架构，可以支持不同的shell(包括嵌入式)，但它是二进制兼容glibc的，就是说如果你的代码之前依赖eglibc库，那么换成glibc后也不需要重新编译。ubuntu系统用的就是eglibc（而不是glibc）

## glib

glib也是个c程序库，不过比较轻量级，glib将C语言中的数据类型统一封装成自己的数据类型，提供了C语言常用的数据结构的定义以及处理函数，有趣的宏以及可移植的封装等(注：glib是可移植的，说明你可以在linux下，也可以在windows下使用它）。

它跟glibc有什么关系吗？

其实并没有，除非你的程序代码会用到glib库中的数据结构或者函数

GLib 为 C 语言编写的库和程序提供了核心应用程序组件。它提供了 GNOME 中使用的核心对象系统， main 循环的实现以及操作字符串和常用数据结构的一整套工具函数。

## libstdc++   c++标准库

libstdc++与gcc是捆绑在一起的，也就是说安装gcc的时候会把libstdc++装上。 那为什么glibc和gcc没有捆绑在一起呢？相比glibc，libstdc++虽然提供了c++程序的标准库，但它并不与内核打交道。**对于系统级别的事件，libstdc++首先是会与glibc交互，才能和内核通信。**相比glibc来说，libstdc++就显得没那么基础了。
