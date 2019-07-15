---
title: ECMAScript中引用类型
date: 2019-07-15 16:34:56
tags: JavaScript
desc:
categories:
	-JavaScript
---

基本类型值是按值访问的：基本数据类型，为Null，Undefined，Number，Boolean，String，Symbol。值保存在栈内存中。
引用类型值是按引用访问的：复杂数据类型，为Object。一切皆是对象。可以添加属性和方法，也可以改变和删除其属性和方法。值保存在堆内存中。

<!-- 阅读更多 -->

## Array
![Array](https://github.com/FeiYangbuMian/imgUpload/raw/master/forBlog/2019-07-15_163649.jpg)
### 转换方法
- toLocaleString()
- toString()
- valueOf()
- join()

### 栈方法
- push()
- pop()

### 队列方法
- shift()
- unshift()

### 重排序方法
返回值是经过排序之后的数组。
- reverse()
- sort()

### 操作方法
- concat()
- slice()
- splice()

### 位置方法
- indexOf()
- lastIndexOf()

### 迭代方法
- every()
- filter()
- forEach()
- map()
- some()

### 归并方法
- reduce()
- reduceRight()