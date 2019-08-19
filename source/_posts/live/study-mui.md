---
title: mui的一些实战
date: 2019-08-19 14:54:46
tags: mui
desc:
categories:
	- mui
---
最近在用mui做一款app,在此总结遇见的问题 解决方案 
所用工具: HBuilderX H5+App 底部选项卡模板

<!-- 阅读更多 -->

## 底部选项卡
创建成功后js文件夹下有util.js文件
```
//util.js
var util = {
	options: {
		<!-- 选项卡被选中的颜色 -->
		ACTIVE_COLOR: "#007aff",
		<!-- 选项卡未被选中的颜色 -->
		NORMAL_COLOR: "#000",
		<!-- 选项卡对应的页面 -->
		subpages: ["html/tab-webview-subpage-chat.html", "html/tab-webview-subpage-contact.html"]
	},
	...
	initSubpage: function(aniShow) {
		var subpage_style = {
				top: 0,
				<!-- 底部选项卡高度，建议改为50，51会页面穿透 -->
				bottom: 51
			},
			subpages = util.options.subpages,
			self = plus.webview.currentWebview(),
			temp = {};
	...
	toggleNview: function(currIndex) {
		currIndex = currIndex * 2;
		// 重绘当前tag 包括icon和text，所以执行两个重绘操作
		util.updateSubNView(currIndex, util.options.ACTIVE_COLOR);
		util.updateSubNView(currIndex + 1, util.options.ACTIVE_COLOR);
		// 重绘兄弟tag 反之排除当前点击的icon和text
		<!-- 默认底部选项卡是4个选项，于是为8，若选项卡数目变化，相应i最大值也要修改 -->
		for(var i = 0; i < 8; i++) {
			if(i !== currIndex && i !== currIndex + 1) {
				util.updateSubNView(i, util.options.NORMAL_COLOR);
			}
		}
	},
```

```
// manifest.json
"plus": {
	"launchwebview": {
		"bottom": "0px",
		<!-- 底部选项卡背景颜色 -->
		"background": "#fff",
		"subNViews": [
			{
				"id": "tabBar",
				"styles": {
					"bottom": "0px",
					"left": "0",
					"height": "50px",
					"width": "100%",
					"backgroundColor": "#fff"
				},
				"tags": [
					{
						"tag": "font",
						"id": "indexIcon",
						<!-- text放图标，图标可在官网查，然后再mui.css中查找对应的字符，替换掉\u后的字符 -->
						"text": "\ue500",
						"position": {
							"top": "4px",
							"left": "0",
							<!-- 25%是四个选项卡 -->
							"width": "25%",
							"height": "24px"
						},
						"textStyles": {
							"fontSrc": "_www/fonts/mui.ttf",
							"align": "center",
							"size": "24px"
						}, {
							"tag": "font",
							"id": "indexText",
							"text": "首页",
							"position": {
								"top": "23px",
								"left": "0",
								"width": "25%",
								"height": "24px"
							},
							"textStyles": {
								"align": "center",
								"size": "10px"
							}
						},
```
## 弹出框
```
<li class="mui-table-view-cell">
	<!-- href锚点 点击显示弹出框 -->
	<a class="mui-navigate-right" href="#namePopover">
		昵称
		<span class="mui-pull-right gray-big mr20">一个测试的账号</span>
	</a>
</li>
```

```
<!-- 弹出框内容 -->
<div id="namePopover" class="mui-popover">
	<div class="mui-popover-arrow mui-hidden"></div>
	<form class="mui-input-group input-group">
		<label class="dark-big">昵称编辑</label>
		<div class="mui-input-row">
			<input type="text" class="mui-input-clear gray-big" placeholder="请输入新昵称" value="一个测试的账号" autofocus="autofocus" />
		</div>
		<div class="mui-button-row" style="text-align: right;margin-top: 10px;">
			<a href="" class="green-big cancel">取消</a>
			<a href="" class="green-big confirm">确认</a>
		</div>
	</form>
</div>
```
## 图片浏览
## 图片裁剪
