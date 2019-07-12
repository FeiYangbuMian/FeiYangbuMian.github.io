---
title: Vue学习笔记二：组件、自定义指令
date: 2019-07-06 17:58:01
tags: Vue
desc: 结合vue官网内容与梁灏编著的vue.js实战
categories: 
	- Vue
---

## 组件
- component
- 组件名推荐使用小写加减号分割形式命名
- 组件注册必须在Vue实例申明之前
- 每个组件必须只有一个根元素

<!-- 阅读更多 -->
```
Vue.component('my-component',{
    // 选项
});
```
- 局部注册组件使用components选项。只在实例作用域下有效。
```
let vm = new Vue({
    el:'#vm',
    components:{
        template:''
    }
});
```
### 选项
- name选项，可用来递归
- data选项，必须是函数，返回对象数据
- computed
- methods
- props选项，声明需要从父级接收的数据
    - 父组件向子组件传数据
	- 值为字符串数组或对象
	- 驼峰命名的props名称，DOM上改为短横分隔命名
    - 值为对象时，k为名称，v为类型
    - v-bind动态绑定props值

### 父子通信
props
> 在组件中，使用选项props来声明需要从父级接收的数据，props的值可以是两种，一种是字符串数组，一种是对象。

> props中声明的数据与组件data函数return的数据的主要区别就是props的来自父级，而data中的是组件自己的数据，作用域是组件本身，这两种数据都可以在模板template及计算属性computed和方法methods中使用。

> 由于HTML特性不区分大小写，当使用DOM模板时，驼峰命名（）的props名称要转为短横分割命名（kebab-case）。

```
<div id="vm">
	<huahua say="hello"></huahua>
</div>
Vue.component('huahua',{
	props:['say'],
	template:`<div>huahua:{{say}}</div>`,
});
```
### 子父通信
.$emit(自定义的事件名,携带的数据)
.$on(自定义的事件名,回调函数)
> 子组件用$emit()来触发事件，父组件用$on()来监听子组件的事件。

```
<div id="vm">
	<for-show @show="show_num"></for-show>
	<p v-if="iftrue">余额：55元</p>
</div>

Vue.component('for-show',{
	template:`<div><button @click="on_click">显示余额</button></div>`,
	methods:{
		on_click:function () {
			this.$emit('show',{a:2})
		}
	}
});
let vm = new Vue({
	el:'#vm',
	data:{
		iftrue:false
	},
	methods:{
		show_num:function (data) {
			console.log(data);
			this.iftrue = true;
		}
	}
});

```
### 非父子通信
父子、兄弟、跨级通信
1. 创建一个空的Vue实例，作为中央事件总线bus
2. 全局定义组件
3. 创建Vue实例，在mounted钩子函数里监听来自bus的事件

```
// 例子1
<div id="vm">
	<comp-txt></comp-txt>
	{{message}}
</div>

let bus = new Vue();
Vue.component('comp-txt',{
	template:`<div>
		<button @click="on_click" >传递事件</button>
	</div>`,
	methods:{
		on_click: function () {
			bus.$emit('on_message',this.txt);
		}
	},
	data: function () {
		return {
			txt:'hello world'
		}
	}
});
let vm = new Vue({
	el:'#vm',
	data:{
		message:''
	},
	mounted: function () {
		let _this = this;
		bus.$on('on_message',function(msg){
			_this.message=msg;
		})
	}
});
```
```
// 例子2
<div id="vm">
	<say></say>
	<saied></saied>
</div>
	
let bus = new Vue();
Vue.component('say',{
	template:`<div>
		他在说:<input type="text" @keyup="saying" v-model="he_say" />
	</div>`,
	methods:{
		saying:function () {
			bus.$emit('say_what',this.he_say);
		}
	},
	data: function () {
		return {
			he_say:''
		}
	}
});
Vue.component('saied',{
	template:`<div>
		他说了:{{msg}}
	</div>`,
	data: function () {
		return {
			msg:''
		}
	},
	mounted:function () {
		let _this = this;
		bus.$on('say_what',function (data) {
			// this指向bus
			_this.msg = data;
		})
	}
});
let vm = new Vue({
	el:'#vm'
});
```
### 插槽
内容分发，用到slot
> Vue组件3个API来源：props传递数据、events触发事件、slot内容分发
- 插槽内可以包含任何模板代码，包括 HTML
- 插槽内也可以包含其它的组件

> 父级模板里的所有内容都是在父级作用域中编译的；子模板里的所有内容都是在子作用域中编译的。

```
<div id="vm">
	<comp-txt><p>父组件的内容</p></comp-txt>
</div>

Vue.component('comp-txt',{
    template:`<div><slot><p>父组件没有内容</p></slot></div>`
});
let vm = new Vue({
    el:'#vm',
});
```
#### 具名插槽
<slot> 元素有一个特殊的特性：name。这个特性可以用来定义额外的插槽。

在向具名插槽提供内容的时候，可以在一个 <template> 元素上使用 v-slot 指令，并以 v-slot 的参数的形式提供其名称。

`v-slot:` 有参数时可缩写为 `#`
```
<div id="vm">
	<comp-child>
		<template #header>
            <h1>标题</h1>
        </template>
        <!-- <h2 slot="header">标题</h2> -->
		<p>正文</p>
		<p>还是正文</p>
	    <template v-slot:footer>
            <p>底部</p>
        </template>
        <!-- <p slot="footer">底部</p> -->
	</comp-child>
</div>
		
Vue.component('comp-child', {
	template: `<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>`
});
let vm = new Vue({
	el: '#vm',
});
```

### 作用域插槽
绑定在 <slot> 元素上的特性被称为插槽 prop。现在在父级作用域中，我们可以给 v-slot 带一个值来定义我们提供的插槽 prop 的名字
```
<div id="vm">
	<comp-child>
		<template v-slot:header="demo">
            <h1>标题<span>{{demo.msg}}</span></h1>
        </template>
		<p>正文</p>
	    <template v-slot:footer>
            <p>底部</p>
        </template>
	</comp-child>
</div>
		
Vue.component('comp-child', {
	template: `<div class="container">
  <header>
    <slot name="header" msg="副标题"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>`
});
let vm = new Vue({
	el: '#vm',
});
```
### 动态组件
- is特性
- <keep-alive> 元素
    - <keep-alive>要求被切换到的组件都有自己的名字，不论是通过组件的 name 选项还是局部/全局注册。
```
<!-- 失活的组件将会被缓存！-->
<keep-alive>
  <component v-bind:is="currentTabComponent"></component>
</keep-alive>
```
### 异步组件
> Vue 允许以一个工厂函数的方式定义组件，这个工厂函数会异步解析组件定义。Vue只有在这个组件需要被渲染的时候才会触发该工厂函数，且会把结果缓存起来供未来重渲染。

```
Vue.component('async-example', function (resolve, reject) {
  setTimeout(function () {
    // 向 `resolve` 回调传递组件定义
    resolve({
      template: '<div>I am async!</div>'
    })
  }, 1000)
})
```
## 自定义指令
directive
```
// 注册一个全局自定义指令 `v-focus`
Vue.directive('focus',{
    // 当被绑定的元素插入到 DOM 中时……
    inserted: function (el) {
        // 聚焦元素
        el.focus()
    }
});
```
局部指令 directives选项
```
directives: {
    focus: {
    // 指令的定义
    inserted: function (el) {
      el.focus()
    }
  }
}
```
### 钩子函数
- `bind`：只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置。
- `inserted`：被绑定元素插入父节点时调用 (父节点存在即可调用，不必存在于document中)。
- `update`：被绑定元素插入父节点时调用，而不论绑定值是否变化。通过比较更新前后的值来忽略不必要的模板更新。
- `componentUpdated`：被绑定元素所在模板完成一次更新周期时调用。
- `unbind`：只调用一次，指令与元素解绑时调用。
### 钩子函数参数
- `el`指令所绑定的元素，可以用来直接操作 DOM 。
- `binding`
    - name：指令名，不包括 v- 前缀。
    - value：指令的绑定值，例如：v-my-directive="1 + 1" 中，绑定值为 2。
    - oldValue：指令绑定的前一个值，仅在 update 和 componentUpdated 钩子中可用。无论值是否改变都可用。
    - expression：绑定值的字符串形式。例如 v-my-directive="1 + 1" 中，表达式为 "1 + 1"。
    - arg：传给指令的参数。例如 v-my-directive:foo 中，arg的值为 "foo"。
    - modifiers：一个包含修饰符的对象。例如：v-my-directive.foo.bar 中，修饰符对象modifiers的值为 { foo: true, bar: true }。
- `vnode`Vue 编译生成的虚拟节点。
- `oldVnode`上一个虚拟节点，仅在 update 和 componentUpdated 钩子中可用。

> 除了 el 之外，其它参数都应该是只读的，切勿进行修改。如果需要在钩子之间共享数据，建议通过元素的 dataset 来进行。

```
<div id="baseexample">
  <p>Scroll down the page</p>
  <p v-pin="200">Stick me 200px from the top of the page</p>
</div>

Vue.directive('pin', {
  bind: function (el, binding, vnode) {
    el.style.position = 'fixed'
    el.style.top = binding.value + 'px'
  }
})
new Vue({
  el: '#baseexample'
})
```
### 动态参数
v-mydirective:[argument]="value"
```
<div id="dynamicexample">
  <h3>Scroll down inside this section ↓</h3>
  <p v-pin:[direction]="200">I am pinned onto the page at 200px to the left.</p>
</div>

Vue.directive('pin', {
  bind: function (el, binding, vnode) {
    el.style.position = 'fixed'
    var s = (binding.arg == 'left' ? 'left' : 'top')
    el.style[s] = binding.value + 'px'
  }
})
new Vue({
  el: '#dynamicexample',
  data: function () {
    return {
      direction: 'left'
    }
  }
})
```