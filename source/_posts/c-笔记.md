---
title: c++笔记
date: 2019-08-09 11:00:13
tags:
---

符数组不可以用“=”赋值，而是需要strcpy()函数；

```
#include <string.h>
class Test{
	private:
	 char* name;
	public:
	 Test(char cname []){
	 	name = new char[strlen(cname)+1];
	 	strcpy(name,cname)
	 }
}
```

在C/C++中一个指针占4个字节

cout<<"哈哈哈"<<endl endl 的作用是换行 

假定A为一个类，则执行“A a[3]，b(3)；”语句时调用该类构造函数的次数为4次 a[3]调用3次，b(3)调用一次

