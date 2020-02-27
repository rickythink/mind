# single-spa重要概念

> [https://juejin.im/post/5db4253451882556a035ae5d\#comment](https://juejin.im/post/5db4253451882556a035ae5d#comment)
>
> [https://github.com/YataoZhang/my-single-spa](https://github.com/YataoZhang/my-single-spa)

## app状态

![](../.gitbook/assets/image%20%2861%29.png)

**状态说明（app和service在下表统称为app）：**

| 状态 | 说明 | 下一个状态 |
| :--- | :--- | :--- |
| NOT\_LOADED | app还未加载，默认状态 | LOAD\_SOURCE\_CODE |
| LOAD\_SOURCE\_CODE | 加载app模块中 | NOT\_BOOTSTRAPPED、SKIP\_BECAUSE\_BROKEN、LOAD\_ERROR |
| NOT\_BOOTSTRAPPED | app模块加载完成，但是还未启动（未执行app的`bootstrap`生命周期函数） | BOOTSTRAPPING |
| BOOTSTRAPPING | 执行app的`bootstrap`生命周期函数中（只执行一次） | SKIP\_BECAUSE\_BROKEN |
| NOT\_MOUNTED | app的`bootstrap`或`unmount`生命周期函数执行成功，等待执行`mount`生命周期函数（可多次执行） | MOUNTING |
| MOUNTING | 执行app的`mount`生命周期函数中 | SKIP\_BECAUSE\_BROKEN |
| MOUNTED | app的`mount`或`update(service独有)`生命周期函数执行成功，意味着此app已挂载成功，可执行Vue的$mount\(\)或ReactDOM的render\(\) | UNMOUNTING、UPDATEING |
| UNMOUNTING | app的`unmount`生命周期函数执行中，意味着此app正在卸载中，可执行Vue的$destory\(\)或ReactDOM的unmountComponentAtNode\(\) | SKIP\_BECAUSE\_BROKEN、NOT\_MOUNTED |
| UPDATEING | service更新中，`只有service才会有此状态，app则没有` | SKIP\_BECAUSE\_BROKEN、MOUNTED |
| SKIP\_BECAUSE\_BROKEN | app变更状态时遇见错误，如果app的状态变为了`SKIP_BECAUSE_BROKEN`，那么app就会`blocking`，不会往下个状态变更 | 无 |
| LOAD\_ERROR | 加载错误，意味着app将无法被使用 | 无 |

**load、mount、unmount条件** 判断需要被加载\(load\)的App：

![](../.gitbook/assets/image%20%2815%29.png)

判断需要被挂载\(mount\)的App：

![](../.gitbook/assets/image%20%28128%29.png)

判断需要被卸载\(unmount\)的App：

![](../.gitbook/assets/image%20%2898%29.png)

## app生命周期递归调用

app的生命周期函数可以传入数组或函数，但是它们都必须返回一个Promise，为了方便处理，所以我们会判断：如果传入的不是Array，就会用数组将传入的函数包裹起来。

![](../.gitbook/assets/image%20%28162%29.png)

## 路由拦截

微前端中app分为两种：一种是根据Location进行变化的，称之为app。另一种是纯功能\(Feature\)级别的，称之为service。

如果要实现随Location的变化动态进行mount和unmount那些符合条件的app，我们就需要对浏览器的Location相关操作做统一的拦截。另外，为了在使用Vue、React等视图框架时降低冲突，我们需要保证微前端必须是第一个处理Location的相关事件，然后才是Vue或React等框架的Router处理。

## 执行流程

![](../.gitbook/assets/image%20%28137%29.png)

**触发时机**

整个系统的触发时机分为两类：

1. 浏览器触发：浏览器Location发生改变，拦截onhashchange和onpopstate事件，并mock浏览器history的pushState\(\)和replaceState\(\)方法。
2. 手动触发：手动调用框架的`registerApplication()`或`start()`方法。

**修改队列\(changesQueue\)**

每通过触发时机进行一次触发操作，都会被存放到changesQueue队列中，它就像事件循环的事件队列一样，静静地等待被处理。如果changesQueue为空，则停止循环直至下一次触发时机到来。

> 和js事件循环队列不同的是，`changesQueue`是当前循环内的所有修改\(changes\)会绑成一批（batch）同时执行，而js事件循环是一个一个地执行。

**"事件"循环**

在每一次循环的开始阶段，会先判断整个微前端的框架是否已经启动。

**未启动：** 根据规则（见上文的『判断需要被加载\(load\)的App』）加载需要被加载的app，加载完成之后调用内部的finish方法。

**已启动：** 根据规则获取当前因为不满足条件而需要被卸载\(unmount\)的app、需要被加载\(load\)的app以及需要被挂载\(mount\)的app，将load和mount的app先合并在一起进行去重，等unmout完成之后再统一进行mount。然后再等到mount执行完成之后就会调用内部的finish方法。

**location事件**

另外在每次循环终止时都会将已拦截的location事件进行触发，这样就可以保证上文说的微前端框架的location触发时机总是首先被执行，而Vue或React的Router总是在后面执行。

