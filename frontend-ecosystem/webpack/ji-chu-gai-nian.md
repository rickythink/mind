# 基础概念

## 整体流程

1. 利用babel完成代码转换,并生成单个文件的依赖
2. 从入口开始递归分析，并生成依赖图谱
3. 将各个引用模块打包为一个立即执行函数
4. 将最终的bundle文件写入bundle.js中

## Loader

Loader 就像一个翻译员，能将源文件经过转化后输出新的结果，并且一个文件还可以链式地经过多个翻译员翻译。

**概念：**

* 一个Loader 的职责是单一的，只需要完成一种转换
* 一个Loader 其实就是一个Node.js 模块，这个模块需要导出一个函数

## Plugin

专注处理 webpack 在编译过程中的某个特定的任务的功能模块，可以称为插件

**概念：**

* 是一个独立的模块
* 模块对外暴露一个 js 函数
* 函数的原型 (prototype) 上定义了一个注入 compiler 对象的 apply 方法 apply 函数中需要有通过 compiler 对象挂载的 webpack 事件钩子，钩子的回调中能拿到当前编译的 compilation 对象，如果是异步编译插件的话可以拿到回调 callback
* 完成自定义子编译流程并处理 complition 对象的内部数据
* 如果异步编译插件的话，数据处理完成后执行 callback 回调。

## **Tapable & Tapable 实例**

webpack 的插件架构主要基于 Tapable 实现的，Tapable 是 webpack 项目组的一个内部库，主要是抽象了一套插件机制。它类似于 NodeJS 的 EventEmitter 类，专注于自定义事件的触发和操作。 除此之外, Tapable 允许你通过回调函数的参数访问事件的生产者。\


