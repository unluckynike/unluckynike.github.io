---
title: Leetcode
date: 2020-09-17 12:57:40
img: https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/pictures/code-2620118_1920.jpg
tags:
    - C语言
    - Leetcode 简单
categories: 算法    
summary: Leetcode简单题目
---

## Leetcode9回文数

判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

**示例 1:**

```
输入: 121
输出: true
```


**示例 2:**

```
输入: -121
输出: false
解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。
```


**示例 3:**

```
输入: 10
输出: false
解释: 从右向左读, 为 01 。因此它不是一个回文数。
```


**进阶:**

你能不将整数转为字符串来解决这个问题吗？

[链接](https://leetcode-cn.com/problems/palindrome-number)

**题解**

利用从末位每位乘十相加等于原数来判断，负数直接排除，注意溢出。

```c
bool isPalindrome(int x){
    if(x<0){
        return false;
    }
    long init=0,temp=x;
    while(temp!=0){
        init=init*10+temp%10;
        temp/=10;
    }
return init==x;
}
```

## Leetcode189旋转数组

给定一个数组，将数组中的元素向右移动 k 个位置，其中 k 是非负数。

示例 1:

```
输入: [1,2,3,4,5,6,7] 和 k = 3
输出: [5,6,7,1,2,3,4]
解释:
```

```
向右旋转 1 步: [7,1,2,3,4,5,6]
向右旋转 2 步: [6,7,1,2,3,4,5]
向右旋转 3 步: [5,6,7,1,2,3,4]
```

示例 2:

```
输入: [-1,-100,3,99] 和 k = 2
输出: [3,99,-1,-100]
解释: 
向右旋转 1 步: [99,-1,-100,3]
向右旋转 2 步: [3,99,-1,-100]
```


说明:

尽可能想出更多的解决方案，至少有三种不同的方法可以解决这个问题。
要求使用空间复杂度为 O(1) 的 原地 算法。
[链接](https://leetcode-cn.com/problems/rotate-array)

**题解**

第一能想到的就是循环k次，每次将数组的最后一位记录，再后一位用前一位代替，两个循环求解。运行超时。

```c
void rotate(int* nums, int numsSize, int k){	
    k = k % numsSize;
	for (int i = 0; i < k; i++)
	{
		int tmp = nums[numsSize - 1];//记录最后一位
		for (int j = numsSize; j > 0; j--)
		{
			nums[j] = nums[j - 1];
		}
		nums[0] = tmp;//放在开头
	}
	return;
}
```

三次旋转求解，利用k，分别将k的前部分逆置，后部分逆置，最后将数组整体逆置。

```c
void rotate(int* nums, int numsSize, int k){
	k = k % numsSize;
	for (int i = 0,j=numsSize-k-1; i <j ; i++,j--)//注意j的值
	{//前部分逆置
		int tmp;
		tmp = nums[i];
		nums[i] = nums[j];
		nums[j] = tmp;
	}
	for (int i = numsSize-k,j=numsSize-1; i < j; i++,j--)
	{//后部分逆置
		int tmp;
		tmp = nums[i];
		nums[i] = nums[j];
		nums[j] = tmp;c
	}
	for (int i = 0,j=numsSize-1; i < j; i++,j--)
	{//全部逆置
		int tmp;
		tmp = nums[i];
		nums[i] = nums[j];
		nums[j] = tmp;
	}
	return;
}
```

