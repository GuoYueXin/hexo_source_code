---
title: 初探webpack
date: 2019-09-05 15:06:17
tags: webpack
categories: webpack
---
> [webpack](https://webpack.js.org) 是一个大包模块化JavaScript的工具，在webpack里一切文件皆模块，通过Loader转换文件，通过Plugin注入钩子，最后输出有多个模块组成的文件。webpack专注于构建模块化项目。</br>
<!-- more -->

### **webpack 的优点如下：**</br>
> - 专注于处理模块化的项目，能做到开箱即用、一步到位
> - 可通过Plugin扩展，完整好用又不失灵活
> - 使用场景不局限于web开发
> - 社区庞大活跃，经常引入紧跟时代发展的新特性，能为大多数场景找到已有的开源扩展
> - 良好的开发体验

### **webpack基本配置**
1. webpack.config.js基本结构
```js
module.export = {
  entry: {},     // 入口文件相关配置
  output: {},    // 配置如何输出最终的代码
  resolve: {},   // 配置如何寻找模块的规则
  module: {},    // 配置处理模块的规则
  plugins: [],   // 配置扩展插件
  devServer: {}, // 配置DevServer
  ...            // 其他零散配置
}
```
2. **entry**
> ```entry``` 是配置模块的入口，webpack执行构建的第一步将从入口开始，搜寻及地柜解析出所有入口依赖的模块。
> 
> <font color="red">该项为必填项</font>
>
> entry类型
> 
> |类型 |例子 | 含义|
> |:----|:----|:----|
> |string| './app/entry' | 入口木块的文件路径，可以是相对路径 |
> | array | ['./app/entry1', './app/entry2'] | 入口木块的文件路径，可以是相对路径|
> | object| { a: './app/entry-a', b: ['./app/entry-b-1', './app/entry-b-2'] } | 配置多个入口，每个入口生成一个chunk|
>  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;如果是array类型，则搭配output.library配置项使用时，只有数组里的最后一个入口文件的模块会被导出。（<font color="red">可以在入口文件进行polyfill的加载</font>）

test