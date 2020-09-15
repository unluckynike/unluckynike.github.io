---
title: 算法之数列求值1.1
date: 2020-04-16 13:31:49
categories: 算法
tags: 
   - C
   - 数列
img: https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/code-1839406_640.jpg
summary: 给定数列 1, 1, 1, 3, 5, 9, 17, …，从第 4 项开始，每项都是前 3 项的和。求第 20190324 项的最后 4 位数字。
---

### 题目描述

给定数列 1, 1, 1, 3, 5, 9, 17, …，从第 4 项开始，每项都是前 3 项的和。求第 20190324 项的最后 4 位数字。

### 思路

类似斐波那契数列，不断循环。

### 代码

```C
#include  stdio.h

int main() {
	int a = 1, b = 1, c = 1;
	int result = 0;
	for (int i=4 ;  i<=20190324 ; i++)
	{
		result = (a + b + c) %10000;
		a = b;
		b = c;
		c = result;
	}
	printf("%d\n",result);
	return 0;
}
```

### 结果

```
4659
```

