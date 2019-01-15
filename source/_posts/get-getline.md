---
title: get & getline
tags: [CPP]
categories: [CPP,C++收录]
date: 2019-01-15 12:49:29
description:
---

`getline()`:成员函数在读取到指定数目的字符或遇到换行符时停止读取，随后getline()将丢弃换行符
`get()`:读取一行输入，直到换行符，但是get()将换行符留在输入序列中

<!-- more -->

- Example

```c++
#include <iostream>
#include <string>
using namespace std;
int main(){
    const auto ArSize=20;//“auto”自动检测类型
    char name[ArSize];//字符数组保存字符串
    char dessert[ArSize];
    int year=0;
    cout<<"Enter the year"<<endl;
    (cin>>year).get();//利用表达式cin>>year返回cin对象将调用拼接起来
    cout<<"This year is "<<year<<endl;
    
    cout<<"Enter your name : "<<endl;
    cin.get(name,ArSize).get();//read string ,new line
    cout<<"Enter your favourate dessert : "<<endl;
    cin.getline(dessert,20);//valid将一行读取到数组对象中
    // get(cin,dessert); //invalid
    // getline(cin,dessert);//invalid , because varable 'dessert' is an char array other than a string
    cout<<"Your name is "<<name<<endl;
    cout<<"You like "<<dessert<<endl;
    
    string trial;
    cout<<"This is a test : "<<endl;
    getline(cin,trial);//只能cin到sting类型变量中去，将一行读取到string对象中
    cout<<"Test works "<<trial<<endl;
    
    // cout<<"This is a test as well : "<<endl;
    // get(cin,name);
    // cout<<"Test works "<<name<<endl;
    cout<<"This is a test as well : "<<endl;
    cin.get(name,20);//换行符被留在输入序列
    cout<<"Test works "<<name<<endl;
    getline(cin,trial);//因为换行符被留在输入序列，导致getline读入的是换行符，以为此行没有输入，故没有下一次输入的机会
    cout<<"will the test work? "<<trial<<endl;
    
    return 0;
}
```
