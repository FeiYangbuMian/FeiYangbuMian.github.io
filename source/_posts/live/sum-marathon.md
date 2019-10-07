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

1. 
```
npm install axios
```
2. main.js中引入
```
import Axios from 'axios'
Vue.prototype.$axios = Axios;  //在Vue的原型上添加$axios方法
```
3. main.js中配置
```
// 设置baseURL
Axios.defaults.baseURL = 'http://192.168.20.56:8089'
// 设置token值
// Axios.defaults.headers.common['Authorization'] = AUTH_TOKEN
// 请求头
Axios.defaults.headers.post['Content-Type'] = 'application/json;charset=UTF-8'

// 添加请求拦截器
Axios.interceptors.request.use(function (config) {
  // 在发送请求之前做些什么
  console.log(config)
  return config
}, function (error) {
  // 对请求错误做些什么
  return Promise.reject(error)
})

// 添加响应拦截器
Axios.interceptors.response.use(function (response) {
  // 对响应数据做点什么
  return response
}, function (error) {
  // 对响应错误做点什么
  return Promise.reject(error)
})
```
4. 页面中使用
```
// post
this.$axios.post('/insertNews', JSON.stringify(news))
    .then(rsp => {
        console.log('insertNews success')
    })
    .catch(error => { console.log(error) })

// get
this.$axios.get('/selectnews', {
    params: {
        'newsType': '1'
    }
}).then(rsp => {
        _this.tableData = rsp.data.list
        console.log(JSON.stringify(rsp.data))
}).catch(error => {
        console.log(error)
    })
```

### wangEditor
[wangEditor官方文档](https://www.kancloud.cn/wangfupeng/wangeditor3/332599)

[vue2+wangeditor牛刀小试](https://segmentfault.com/a/1190000016010354)
1. 
```
npm install wangeditor
```
2. 新增富文本组件
```
<template>
  <div id="wangeditor">
    <div ref="editorElem" style="text-align:left;width: 600px"></div>
  </div>
</template>

<script>
import WE from 'wangeditor'
export default {
  name: 'WangEditor',
  data () {
    return {
      editor: null,
      editorContent: ''
    }
  },
  // catchData是一个类似回调函数，来自父组件，当然也可以自己写一个函数，主要是用来获取富文本编辑器中的html内容用来传递给服务端
  // content接收初始化数据
  props: ['catchData', 'content'], // 接收父组件的方法
  watch: {
    content () {
      this.editor.txt.html(this.content)
    }
  },
  mounted () {
    this.editor = new WE(this.$refs.editorElem)
    // 编辑器的事件，每次改变会获取其html内容
    this.editor.customConfig.onchange = html => {
      this.editorContent = html
      this.catchData(this.editorContent) // 把这个html通过catchData的方法传入父组件
    }
    this.editor.customConfig.zIndex = 2 // 修改编辑器的z-index
    this.editor.customConfig.pasteFilterStyle = false // 关闭粘贴样式的过滤
    this.editor.customConfig.pasteIgnoreImg = true // 粘贴时忽视图片
    // 自定义处理粘贴的文本内容
    this.editor.customConfig.pasteTextHandle = function (content) {
      // content 即粘贴过来的内容（html 或 纯文本），可进行自定义处理然后返回
      if (content === '' && !content) { return '' }
      let str = content
      str = str.replace(/<xml>[\s\S]*?<\/xml>/ig, '')
      str = str.replace(/<style>[\s\S]*?<\/style>/ig, '')
      str = str.replace(/<\/?[^>]*>/g, '')
      str = str.replace(/[ | ]*\n/g, '\n')
      str = str.replace(/&nbsp;/ig, '')
      return str
    }
    this.editor.customConfig.uploadImgMaxLength = 5 // 限制一次最多上传图片个数
    this.editor.customConfig.uploadImgShowBase64 = true // 允许上传本地图片，转为bash64或如下接口上传
    this.editor.customConfig.uploadImgServer = 'http://192.168.20.56:8089/uploadImg' // 你的上传图片的接口
    this.editor.customConfig.uploadFileName = 'newsImages' // 你自定义的文件名，默认名字
    this.editor.customConfig.uploadImgTimeout = 6000000 // 超时时间
    this.editor.customConfig.menus = [
      // 菜单配置
      'head', // 标题
      'bold', // 粗体
      'fontSize', // 字号
      // 'fontName', // 字体
      'italic', // 斜体
      'underline', // 下划线
      'strikeThrough', // 删除线
      'foreColor', // 文字颜色
      // 'backColor', // 背景颜色
      'link', // 插入链接
      'list', // 列表
      'justify', // 对齐方式
      // 'quote', // 引用
      // 'emoticon', // 表情
      'image', // 插入图片
      'table', // 表格
      // 'code', // 插入代码
      'undo', // 撤销
      'redo' // 重复
    ]

    // 下面是最重要的的方法
    this.editor.customConfig.uploadImgHooks = {
      before: function (xhr, editor, files) {
        // 图片上传之前触发
        // xhr 是 XMLHttpRequst 对象，editor 是编辑器对象，files 是选择的图片文件
        console.log(files)
        // 如果返回的结果是 {prevent: true, msg: 'xxxx'} 则表示用户放弃上传
        // return {
        //     prevent: true,
        //     msg: '放弃上传'
        // }
      },
      success: function (xhr, editor, result) {
        // 图片上传并返回结果，图片插入成功之后触发
        // xhr 是 XMLHttpRequst 对象，editor 是编辑器对象，result 是服务器端返回的结果
        console.log(result)
        this.imgUrl = result.pathmap
      },
      fail: function (xhr, editor, result) {
        // 图片上传并返回结果，但图片插入错误时触发
        // xhr 是 XMLHttpRequst 对象，editor 是编辑器对象，result 是服务器端返回的结果
      },
      error: function (xhr, editor) {
        // 图片上传出错时触发
        // xhr 是 XMLHttpRequst 对象，editor 是编辑器对象
      },
      timeout: function (xhr, editor) {
        // 图片上传超时时触发
        // xhr 是 XMLHttpRequst 对象，editor 是编辑器对象
      },

      // 如果服务器端返回的不是 {errno:0, data: [...]} 这种格式，可使用该配置
      // （但是，服务器端返回的必须是一个 JSON 格式字符串！！！否则会报错）
      customInsert: function (insertImg, result, editor) {
        // 图片上传并返回结果，自定义插入图片的事件（而不是编辑器自动插入图片！！！）
        // insertImg 是插入图片的函数，editor 是编辑器对象，result 是服务器端返回的结果
        // result 必须是一个 JSON 格式字符串！！！否则报错

        // 举例：假如上传图片成功后，服务器端返回的是 {url:'....'} 这种格式，即可这样插入图片：
        // let url = Object.values(result.path) // result.data就是服务器返回的图片名字和链接
        // JSON.stringify(url) // 在这里转成JSON格式

        let url = result.pathmap
        let len = url.length
        for (let i = 0; i < len; i++) {
          insertImg(url[i].path)
        }
      }
    }
    this.editor.create() // 创建富文本实例
  }
}
</script>

<style scoped>
#wangeditor {
  width: 600px;
}
</style>

```
3. 页面使用
```
<wang-editor :catchData="catchData" :content="form.newsContent"></wang-editor>

 methods: {
    catchData (value) {
      console.log(value)
      this.content = value // 在这里接受子组件传过来的参数，赋值给data里的参数
    },
}
```

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