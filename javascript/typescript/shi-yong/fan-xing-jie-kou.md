# 泛型接口

泛型也可用于接口声明，以上面的函数为例，如果我们将其转化为接口的形式。

```javascript
interface ReturnItemFn<T> {
    (para: T): T
}
```

那么当我们想传入一个number作为参数的时候，就可以这样声明函数:

```javascript
const returnItem: ReturnItemFn<number> = para => para
```

