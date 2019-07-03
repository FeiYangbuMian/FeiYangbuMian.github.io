---
title: Vue学习笔记 一
date: 2019-07-02 15:16:29
tags: Vue
desc: 结合vue官网内容与梁灏编著的vue.js实战
categories: 
	- Vue
---

## 1. 初识Vue.js
### 高级功能
- 解耦视图与数据
- 可复用的组件
- 前端路由
- 状态管理
- 虚拟DOM
<!--阅读更多-->
### MVVM模式
- Model-View-ViewModel
- 视图-模型-视图模型

## 2. 数据绑定
- v-model
- el
- data
- filters 过滤器
- v-pre 跳过编译
### 生命周期钩子
- created 实例创建完成后调用
- mounted el挂到实例上后调用
- beforeDestroy 实例销毁之前调用。主要用于解绑监听事件
![生命周期钩子图](https://cn.vuejs.org/images/lifecycle.png)
### 插值与表达式
- Mustache语法
   - 也可使用简单的JavaScript表达式
### 指令
- 指令Directives，前缀v，当表达式的值
> 当其表达式的值改变时，相应地将某些行为应用到DOM上
- v-bind 动态更新HTML元素上的属性
- v-on 绑定事件监听器
- methods
### 语法糖
- v-bind简写为 :
- v-on简写 为@

## 3. 计算属性
> 以函数形式写在Vue实例内的computed选项内，最终返回计算后的结果
- 每一个计算属性都包含一个getter和一个setter
    - getter 默认，用于读取
    - setter 写入时触发
- computed依赖缓存，数据变化时才会重新取值
- methods重新渲染时就会被调用
## 4. 指令

- :class设置一个对象，动态切换class
    - 表达式每项为真时，对应类名就会加载
    - 表达式过长或逻辑复杂时可绑定计算属性
- :class绑定一个数组，应用一个class列表
    - 可以在数组语法中使用对象语法
- v-cloak
    - 在Vue实例结束编译时从绑定的HTML上移除
- v-once
    - 只渲染一次，之后不再随数据的变化重新渲染
- v-if v-else-if v-else
    - v-else-if要紧跟着v-if
    - v-else要紧跟着v-else-if或v-if
- v-show
    - 简单的CSS属性切换
    - 表达式值为false时，元素会隐藏
    - v-show不能在<template>上使用
- v-for
    - 表达式结合in使用
    - 遍历数组时，支持一个可选参数作为索引
    - 遍历对象时，支持两个可选参数 键名 索引
    - push,pop,shift,unshift,splice,sort,reverse
    - filter,concat,slice

> 修饰符：
stop 阻止事件冒泡 prevent capture self once

> 按键修饰符：
enter tab delete esc

## 5. 表单与v-model
> v-model 表单类元素的双向绑定数据
> v-model 会忽略所有表单元素的 value、checked、selected 特性的初始值而总是将 Vue 实例的数据作为数据来源。

v-model 在内部为不同的输入元素使用不同的属性并抛出不同的事件：
- text 和 textarea 元素使用 value 属性和 input 事件；
- checkbox 和 radio 使用 checked 属性和 change 事件；
- select 字段将 value 作为 prop 并将 change 作为事件


