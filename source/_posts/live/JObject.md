---
title: ECMAScript中引用类型
date: 2019-07-15 16:34:56
tags: JavaScript
desc:
categories:
	- JavaScript
---

基本类型值是按值访问的：基本数据类型，为Null，Undefined，Number，Boolean，String，Symbol。值保存在栈内存中。
引用类型值是按引用访问的：复杂数据类型，为Object。一切皆是对象。可以添加属性和方法，也可以改变和删除其属性和方法。值保存在堆内存中。

<!-- 阅读更多 -->

## Array
基本类型值是按值访问的：基本数据类型，为Null，Undefined，Number，Boolean，String，Symbol。值保存在栈内存中。 引用类型值是按引用访问的：复杂数据类型，为Object。一切皆是对象。可以添加属性和方法，也可以改变和删除其属性和方法。值保存在堆内存中。
### 转换方法
- toLocaleString()
- toString()
- valueOf()
- join() 数组元素用某个字符连接成字符串

### 栈方法
- push() 在数组最后增加元素，可多参 
- pop() 删除数组最后面一个元素，无参

### 队列方法
- shift() 删除数组最前面一个元素，无参
- unshift() 在数组前面增加元素，可多参 

### 重排序方法
> 返回值是经过排序之后的数组。

- reverse() 反转
- sort() 排序

### 操作方法
- concat() 把当前数组与另一个数组连接起来 返回新数组
- slice()  截取数组索引区域[l,r) 返回新数组 
- splice() 增/删数组中元素
    - arr.splice(index,howmany,item1...itemX)
    - 删除从 index 处开始的零个或多个元素，
    - 并且用参数列表中声明的一个或多个值来替换那些被删除的元素。
    - 规定增删项目的位置，要删除的项目数量，向数组添加的新项目
    - arr为最后结果，arr.splice(...)为删除的结果

### 位置方法
- indexOf() 从后往前搜索指定元素的索引号
- lastIndexOf() 从后往前搜索指定元素的索引号

### 迭代方法
> 对数组中的每一项运行给定函数。

- every()
    - 对数组中的每一项运行给定函数。每一项返回true，就返回true
- some()
    - 对数组中的每一项运行给定函数。任一项返回true，就返回true
- filter()
    - 对数组中的每一项运行给定函数。返回true的项组成的数组
- forEach()
    - 对数组中的每一项运行给定函数。无返回值
- map()
    - 对数组中的每一项运行给定函数。返回每次函数调用的结果组成的数组

``` 
function(item,index,arr){}
// item 当前元素值（必选），index当前元素索引，arr此数组
```

### 归并方法
- reduce()
- reduceRight()