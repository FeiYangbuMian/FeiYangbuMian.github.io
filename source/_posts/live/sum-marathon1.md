---
title: Vue2项目实战总结一
date: 2019-08-31 14:50:01
tags: Vue
desc: marathon项目实战总结一
categories:
	- Vue
---
## 最近遇见的问题与解决方案

### axios
[vue2之axios](https://segmentfault.com/a/1190000013071458)
> 因为本项目用的是json格式，所以未使用插件qs

### wangEditor
[wangEditor官方文档](https://www.kancloud.cn/wangfupeng/wangeditor3/332599)

[vue2+wangeditor牛刀小试](https://segmentfault.com/a/1190000016010354)

<!-- read more -->

### vue-amap
[vue2调用高德地图(Amap)及其UI组件](https://elemefe.github.io/vue-amap/#/zh-cn/introduction/install)

### 部署
[Vue项目打包部署到apache服务器](https://www.cnblogs.com/ykCoder/p/11022572.html)

1. 后端foldername项目
2. router index.js
```
base: '/foldername/',
```
3. config index.js
```
assetsPublicPath: '/foldername/'
```
4. foldername项目下新建.htaccess