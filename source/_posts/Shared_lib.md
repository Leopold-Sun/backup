---
title: How to compile shared library
date: 2018-12-12 14:02
tags: [Compiler]
---

- `gcc -fPIC -shared ${target source file}.c -o ${target name}.so`

生成.so文件，其中，命令中 

-shared该选项指定生成动态连接库（让连接器生成T类型的导出符号表，有时候也生成弱连
接W类型的导出符号），不用该标志外部程序无法连接。相当于一个可执行文件
-fPIC：表示编译为位置独立的代码，不用此选项的话编译后的代码是位置相关的所以动态<font color="#729FCF">&gt;</font>载入时是通过代码拷贝的方式来满足不同进程的需要，而不能达到真正代码段共享的目的。

- `gcc ${source}.c -L. -ltest -o ${target file name}`

编译生成应用程序main，其中命令中

-L.：表示要连接的库在当前目录中
-ltest：编译器查找动态连接库时有隐含的命名规则，即在给出的名字前面加上lib，后面<font color="#729FCF">&gt;</font>加上.so来确定库的名称

Linux共享库的搜索路径先后顺序：
> 1、编译目标代码时指定的动态库搜索路径：在编译的时候指定-Wl,-rpath=路径
> 2、环境变量LD-PRELOAD指定的动态库
> 3、LD-LIBRARY-PATH指定的动态库搜索路径
> 4、配置文件/etc/ld.so.conf中指定的动态库搜索路径
> 5、默认的动态库搜索路径/lib
> 6、默认的动态库搜索路径 /usr/lib

