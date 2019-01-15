---
title: C++ 数组
tags: [CPP]
categories: [C++收录，CPP]
date: 2019-01-15 12:41:26
description:
---

# C++ 数组

数组是一种顺序容器，它包含单一类型的元素

<!-- more -->

```c++
#include <cstdio>
#include<string>
#include <vector>
#include<cassert>
#include<fstream>
#include<iostream>
using namespace std;

//int main(){
// int fibon[9]={0,1,1,2,3,5,8,13,21}; //fibon是一个包含9个整型一维（dimension）数组
//}

//虽然c++对数组类型提供了内置支持，但是这种支持仅限于“用来读写单个元素”的机制。C++不支持数组的abstraction，也不支持对整个数组的操作。
//在将数组作为参数传递给一个函数时将会出现问题。
//int main(){
// int ia[10]; //ia is an array as well as fibon .
// int index;
// for(index=0;index<10;++index) //关键字for后面三个语句控制循环
// ia[index]=index;
// for(index=9;index>=0;--index)
// cout<<ia[index]<<" ";
// cout<<endl;
// return 0;
//}

/*
* 动态分配内存和指针
* 静态分配《编译时》+动态分配《程序执行时》
* 静态与动态分配内存的两个主要区别：
* 1.静态对象是有名字的变量，直接对其操作。而动态对象是没有名字的变量，通过指针间接地对其进行操作。
* 2.静态对象的分配与释放由编译器自动处理。动态对象的分配和释放，显示地由程序员进行处理，通过new+delete两个表达式来完成。
* 访问和存储内存地址：C++支持用指针类型来存放对象的内存地址值。
* C++主要使用指针管理和操纵动态分配的内存。
*/
int main(){
int ival=1024;
int *new_pointer1=new int(1024); //new的第一个版本：分配一个没有名字的int类型的对象；
int *new_pointer2=new int[4]; //new的第二个版本：用于分配特定类型和维数的数组。但没有办法给动态分配的数组每个元素显示地指定一个初始值。
delete new_pointer1; //用完后显示地释放。否则会在程序结束时出现memory leak 问题。
delete new_pointer2; //内存泄漏：是指不再拥有指向一块动态分配内存的指针，无法回收。

int *ping; //一个只想int类型的指针,"*"是解引用操作符（dereference）；
ping=&ival; //c++预定义 “&”操作符（用来专门取地址）；当我们把它用在一个对象上时，返回该对象的地址值。
cout<<ping<<endl;
*ping=*ping+1;
cout<<ival<<endl;
ping=ping+1;
cout<<ping<<endl;

int a=1;
int b(a); //use constructor
if(b==a)
cout<<"That's how constructor works !"<<endl;

return 0;
}

/*支持基于对象设计的类的一般形式
* class classname{ //类头，本身也用做类的声明
* public:
* //公有操作集合
* private:
* //私有实现代码
* }; //类体：花括号括起来的部分。
*/
```


