---
title: struct & unio
tags: [CPP]
categories: [CPP,C++收录]
date: 2019-01-15 13:31:58
description:
---

# Array and Struct

`array`: elements of one array share one same type.
`struct`: one struct could be implemented with data possessing distinct types and defined by users with `struct definition containing its features`.

<!-- more -->

## Creat struct

1. Define struct description

This description describes and marks all types capable of being stored in struct, and declares features of new structs.

2. Narrowing convertions forbidden

```c++
#include<iostream>
using namespace std;
struct inflatable{
	char name[20];
	float volume;
	double price;
};

int main(){
	inflatable guest={ //NOTE：some implementation require using static inflatable guest
	"Glorious Gloria",
	1.88,
	29.99,
	};

	inflatable pal{//initialize pal without "="
	"Audacious Arthur",
	3.12,
	32.99,
	};

	cout<<"Expand the guest list with "<<guest.name
	<<" and "<<pal.name<<" !"<<endl;
	cout<<"You can have both for $"
	<<guest.price+pal.price<<" !"<<endl;

	return 0;
}

//execution:
//Expand the guest list with Glorious Gloria and Audacious Arthur !
//You can have both for $62.98 !
```

# Union

共用体union：能够存储不同的数据类型，但只能同时存储其中一种类型。比如，只能存储int、long或double。其用途之一是当数据项使用两种及以上的数据类型，但不同时使用时，可以节省空间。

## Example

```c++
#include<iostream>
#include<string>
using namespace std;
struct widget{
	char brand[20];
	int type;
	union id{ //format depends on widget type
	long id_num; //type=1
	char id_char[20]; //type=others
	}id_val;
// union{ //匿名共用体:没有名称，其成员将成为位于相同地址处的变量
// long id_num; //type=1
// char id_char[20];
// };
};

int main(){
	widget price;
	cout<<" Go on ? ('1' for yes , '0' for no) >> "<<endl;
	int wetherToGoOn;
	(cin>>wetherToGoOn).get();
	while(wetherToGoOn==1){
		// cin.get(); //如果“cin>>wetherToGoOn;”语句改成“(cin>>wetherToGoOn).get();”,则不必在使用“cin.get();”
		cout<<"choose the type : \n\ta)type=1 \n\tothers)type=others "<<endl;
		string choiceForType;
		getline(cin,choiceForType);
		if(choiceForType=="a"){
		cout<<" Enter price_ID(long) "<<endl;
		cin>>price.id_val.id_num;
		//cin>>price.id_num; //匿名共用体
		cout<<"The type is "<<wetherToGoOn<<" , and its price is "<<price.id_val.id_num<<" !"<<endl;
		}
		else{
			cout<<" Enter price_ID(string) "<<endl;
			cin.get(price.id_val.id_char,20);
			//cin>>price.id_char; //匿名共用体
			cout<<"The type is "<<wetherToGoOn<<" , and its price is "<<price.id_val.id_char<<" !"<<endl;
		}
	cout<<" Go on ? ('1' for yes , '0' for no) >> "<<endl;
	(cin>>wetherToGoOn).get();
	// system("pause");
	}

	return 0;
}
```
