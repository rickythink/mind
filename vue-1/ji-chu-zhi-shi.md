# 基础知识

## MVC

![](../.gitbook/assets/image%20%28134%29.png)

MVC允许在不改变视图的情况下改变视图对用户输入的响应方式，用户把对View的操作交给了Controller处理，在Controller中响应View的事件调用Model的接口对数据进行操作，一旦Model发生变化便通知相关视图进行更新。

如果前端没有框架，只使用原生的html+js，MVC模式可以这样理解。将html看成view;js看成controller，负责处理用户与应用的交互，响应对view的操作（对事件的监听），调用Model对数据进行操作，完成model与view的同步（根据model的改变，通过选择器对view进行操作）;将js的ajax当做Model，也就是数据层，通过ajax从服务器获取数据。  


## MVVM

![](../.gitbook/assets/image%20%2810%29.png)

MVVM与MVC两者之间最大的区别就是：MVVM实现了对View和Model的自动同步，也就是当Model的属性改变时，我们不用再自己手动操作Dom元素来改变View的变化，而是改变其属性后，该属性对应的View层数据会自动改变。 

## vue 源码目录

![](../.gitbook/assets/image%20%2894%29.png)

