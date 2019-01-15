---
title: enum
tags: [CPP]
categories: [CPP,C++收录]
date: 2019-01-15 12:54:42
description:
---

`enum`: 创建符号常量的一种方式，可代替`const`。
使用事需定义以下两点：
1. enum类型名称
2. 新类型内部的符号常量
比如：
```c++
enum colorkinds {red,orange,yello,green,blue,violet,indigo,ultraviolet}
/*enum name: colorkinds
symbolic constants: red……*/
```
<!-- more -->

> 枚举类型是整型，可以提升为int类型，但是int类型不能自动转换到枚举类型`缩窄操作`。但是，如果int值是有效的，则可以通过强制类型转换，将其赋给枚举变量。

- Example

```c++
int color = blue ; //valid
band=3; //invalid , int not converted to spectrum
color=3+red; //valid
```

1. 设置枚举量的值

（后一位未定义的枚举常量对应整数值是前一位常量的值+1，不论前一位常量是否被赋值）

```c++
enum bits {one=1,two=2,four=4,eight=8};//指定的值必须是整数
enum bigstep {first , second=100,third };//first=0,second=100,third=101
enum {zero,null=0,one,number_one=1};//可以创建多个、值相同的枚举量
```

2. 枚举的取值范围

```c++
enum bits {one=1,two=2,four=4,eight=8};
bits myflag;
myflag=6;//valid，赋值枚举类型bits的枚举量myflag
```
