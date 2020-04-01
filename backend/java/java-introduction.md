# 简介

Java介于编译型语言和解释型语言之间。

* 编译型语言如C、C++，代码是直接编译成机器码执行，但是不同的平台（x86、ARM等）CPU的指令集不同，因此，需要编译出每一种平台的对应机器码。
* 解释型语言如Python、Ruby没有这个问题，可以由解释器直接加载源码然后运行，代价是运行效率太低。

Java是将代码编译成一种“字节码”，它类似于抽象的CPU指令，然后，针对不同平台编写虚拟机，不同平台的虚拟机负责加载字节码并执行，这样就实现了“一次编写，到处运行”的效果。

对于虚拟机，需要为每个平台分别开发。为了保证不同平台、不同公司开发的虚拟机都能正确执行Java字节码，SUN公司制定了一系列的Java虚拟机规范。

## 版本

SUN给Java又分出了三个不同版本：

* Java SE：Standard Edition
* Java EE：Enterprise Edition
* Java ME：Micro Edition

![](../../.gitbook/assets/image%20%28195%29.png)

Java SE就是标准版，包含标准的JVM和标准库，而Java EE是企业版，它只是在Java SE的基础上加上了大量的API和库，以便方便开发Web应用、数据库、消息服务等，Java EE的应用使用的虚拟机和Java SE完全相同。

Java SE是整个Java平台的核心，而Java EE是进一步学习Web应用所必须的。

## 基础类型

Java定义了以下几种基本数据类型：

* 整数类型：byte，short，int，long
* 浮点数类型：float，double
* 字符类型：char
* 布尔类型：boolean

