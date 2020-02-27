# vue 渲染过程

![](../.gitbook/assets/image%20%28190%29.png)

![](../.gitbook/assets/image%20%28129%29.png)

1. new Vue，执行初始化，将传入的data数据绑定到当前实例，就可以通过this.message的形式访问传入的数据。这个过程是执行initData\(\)函数完成的。
2. 挂载$mount方法，通过自定义Render方法、template、el等生成Render函数。如果传入了模版\(template\)就将模版里面的内容编译成render函数，否则将传入的el对应的元素的内容编译成render函数。编译是调用compileToFunctions函数完成的。也可以自己手写render函数，可以减少编译这一环节。其中render渲染函数的优先级最高，template次之且需编译成渲染函数，而挂载点el属性对应的元素若存在，则在前两者均不存在时，其outerHTML才会用于编译与渲染。
3. 生成render函数后，调用\_createElement函数生成vnode。
4. 将虚拟DOM映射为真实DOM页面上。

非常有用的参考：

> [https://juejin.im/post/5ce263bf518825645c34cd4e](https://juejin.im/post/5ce263bf518825645c34cd4e)



