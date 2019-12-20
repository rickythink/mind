# 内存

## shadow size

对象自身占用的内存大小，不包括它引用的对象。  
针对非数组类型的对象，它的大小就是对象与它所有的成员变量大小的总和。当然这里面还会包括一些java语言特性的数据存储单元。  
针对数组类型的对象，它的大小是数组元素对象的大小总和。

## retained size

Retained Size=当前对象大小+当前对象可直接或间接引用到的对象的大小总和。\(间接引用的含义：A-&gt;B-&gt;C, C就是间接引用\) 

换句话说，Retained Size就是当前对象被GC后，从Heap上总共能释放掉的内存。 不过，释放的时候还要排除被GC Roots直接或间接引用的对象。他们暂时不会被当做Garbage。

## 看图理解

![](../.gitbook/assets/image%20%2821%29.png)

上图中，GC Roots直接引用了A和B两个对象。  
  
A对象的Retained Size=A对象的Shallow Size  
B对象的Retained Size=B对象的Shallow Size + C对象的Shallow Size  
  
**这里不包括D对象，因为D对象被GC Roots直接引用。**  
如果GC Roots不引用D对象呢？  


![](../.gitbook/assets/image%20%2819%29.png)

此时, B对象的Retained Size=B对象的Shallow Size + C对象的Shallow Size + D对象的Shallow Size

