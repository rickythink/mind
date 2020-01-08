# 为什么 Javascript 是单线程的？

## 基本概念

进程是 CPU 资源分配的最小单位（是能拥有资源和独立运行的最小单位

线程是 CPU 调度的最小单位（是建立在进程基础上的一次程序运行单位）。

## 浏览器多进程架构

Chrome 为例,它由多个进程组成,每个进程都有自己核心的职责,它们相互配合完成浏览器的整体功能,

每个进程中又包含多个线程,一个进程内的多个线程也会协同工作,配合完成所在进程的职责。

Chrome 采用多进程架构,其顶层存在一个 Browser process 用以协调浏览器的其它进程。  


![](../../.gitbook/assets/image%20%286%29.png)

如上图所示有

1. Network Process
2. Browser Process

   负责浏览器界面的显示与交互。各个页面的管理,创建和销毁其他进程。网络的资源管理、下载等。

3. UI Process
4. GPU Process

   用于3D绘制

5. Device Process
6. Plugin Process
7. Renderer Process

   浏览器渲染进程或浏览器内核,内部是多线程的。主要负责页面渲染,脚本执行,事件处理等

![](../../.gitbook/assets/image%20%28141%29.png)

## Renderer Process 渲染进程

渲染进程是多线程的，有如下线程

1. GUI 渲染线程
   * 负责渲染浏览器界面,解析 HTML,CSS,构建 DOM 树和 RenderObject 树,布局和绘制等
   * 当界面需要重绘（Repaint）或由于某种操作引发回流\(reflow\)时,该线程就会执行
   * **GUI 渲染线程与 JS 引擎线程是互斥的**,当 JS 引擎执行时 GUI 线程会被挂起（相当于被冻结了）,GUI 更新会被保存在一个队列中等到 JS 引擎空闲时立即被执行



