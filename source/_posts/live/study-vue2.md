---
title:  Vue学习笔记三：Render函数
date: 2019-07-10 17:37:48
tags: Vue
desc: 结合vue官网内容与梁灏编著的vue.js实战
categories: 
	- Vue
---

## Render函数
渲染函数
### 虚拟DOM
- Vue 通过建立一个虚拟 DOM 来追踪自己要如何改变真实 DOM，提升渲染性能。
- Render函数用于实现虚拟DOM
- Virtual Dom是一个轻量级的JavaScript对象，状态变化时进行Diff算法
- Virtual Dom基于JavaScript计算
- “虚拟节点 (virtual node)”，简写为“VNode”。“虚拟 DOM”是由 Vue 组件树建立起来的整个 VNode 树的称呼。

<!-- 阅读更多 -->


```
render: function (createElement) {
  return createElement('h1', this.blogTitle)
}
```

### createElement
Render函数通过createElement参数来创建Virtual Dom
- 参数一必选，HTML标签、组件或函数
    - {String | Object | Function}
- 参数二可选，对应属性的数据对象
    - {Object}
- 参数三可选，子节点
    - {String | Array}
