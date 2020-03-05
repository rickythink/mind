# useRef warning

> The final argument passed to useEffect changed size between renders. The order and size of this array must remain constant.

为什么设计上有这样的警告呢？其本质原因恐怕还需要探究一下。（大致猜得到，但恐怕准备的还要再查一下）

先谈谈解决方案：

如果我们需要依赖的对象是一个数字或字符串，那么最简单的方式 `Array.join()` 方法:

```text
useEffect(() => {
  animate(pose);
}, [pose.join(",")]);
```

因此，有别于输入

```text
["visible", "hidden"];
```

你现在的输入是

```text
["visible,hidden"];
```

这样你就可以愉快的使用useEffect了

