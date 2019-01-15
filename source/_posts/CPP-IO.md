---
title: CPP IO
tags: [CPP]
categories: [CPP,C++收录]
date: 2019-01-15 12:33:47
description:
---

# How do we enable I/O function using C plus plus?

## Examples

### E.g.1

```c++
#include <cstdio>
#include<string>
#include <vector>
#include<cassert>
#include<fstream>
#include<iostream>
using namespace std; // 由于所有的c++库名字是在名字空间std中被定义的，因此在程序文本文件中它们是不可见的，除非使用下面的using指示符显示地使其可见

#define DEBUG=10;

int main(){
#ifdef DEBUG
#ifdef __cplusplus // 在编译c++程序时，编译器自动定义了一个预处理器名字"__cplusplus"(前面两个下划线)
//另外几个比较有用的预定义名字
//1."__LINE__"记录文件已经被编译的行数
//2."__FILE__"包含正在被编译文件的名字
//可以这么使用他们：if(element_count==0)
//cerr<<"Error: "<<__FILE__<<" : line "<<__LINE__<<"element_count must be non-zero."<<endl;
//3."__TIME__" and "__DATE__"分别表示当前被编译文件的编译时间和日期
cout<<"Yes,we will compile C++"<<endl;
/*c++的输入输出功能由输入输出流（iostream）库提供，输入输出流库是c++中的一个面向对象的类层次结构，也是标准库的一部分。
* 终端输入cin
* 终端输出cout
* 标准错误cerr
* 操纵符endl执行一个动作而不是简单地提供数据。例如，endl在输入流中插入一个换行符‘\n’，然后刷新输出缓冲区。
*/

#endif
cout<<"Beginning execution of main ()\n";
#endif
string word;
vector <string> text;
while(cin>>word&&word!="q"){
#ifdef DEBUG
cout<<"word read : "<<word<<endl;
#endif
text.push_back(word);
}
cout<<"Time & Date : "<<__TIME__<<"\t"<<__DATE__<<endl;

//use assert() : assert()是C语台的一个通用预处理宏。在代码中常用assert()来判断一个前提条件，以使得程序正确运行。
//For example : assert(filename != 0); 将测试filename不等于0的条件是否满足。

system("pause");
return 0;
}
```

<!-- more -->

### E.g.2

A simple program : 从一个名为in_file的文件中读取单词，然后把每个单词写到一个名为out_file的输出文件中，并且每个单词用空格分开。

```c++
/*
* 1.5.1 文件输入和输出
* iostreama 库也支持文件的输入和输出，所有能用于标准输入和标准输出的操作符也都能用于已被打开的输入或输出的文件上。
* 为了打开一个文件供输入输出，除了 #include<iostream>还要 ： #include<fstream>
* 为了打开一个输出文件，必须声明一个ofstream 类型的对象 ： ofstream outfile("name-of-file");
* //如果文件不能打开，值为false
* if(! outfile)
* cerr<<"Sorry ! We were unable to open the file !\n";
*/
#include<iostream>
#include<fstream>
#include<string>
using namespace std;

int main(){
 ofstream outfile("out_file");
 ifstream infile("in_file");
 if(!infile){
 cerr<<"error : unable to open input file !"<<endl;
 return -1;
 }
 if(!outfile){
 cerr<<"error : unable to open output file !"<<endl;
 return -2;
 }
 string word;
 while(infile>>word){
 outfile<<word<<' ';
 }
 return 0;
}
```
