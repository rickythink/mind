# 设计工具类型\(重要）

```javascript
interface Part {
    id: number;
    name: string;
    subparts: Part[];
    updatePart(newName: string): void;
}

type R = FunctionPropertyNames<Part>;
```

设计结果：

```javascript
type FunctionPropertyNames<T> = { [K in keyof T]: T[K] extends Function ? K : never }[keyof T]
```

结题思路

1. 假设我们把`Part`代入泛型`T`,`[K in keyof T]`相当于遍历整个interface
2. 这时`K`相当于interface的key,`T[K]`相当于interface的value
3. 接下来,用条件类型验证value的类型,如果是`Function`那么将value作为新interface的key保留下来,否则为`never`
4. 到这里我们得到了遍历修改后的**新**interface即:

```javascript
type R = {
    id: never;
    name: never;
    subparts: never;
    updatePart: "updatePart";
}
```

{% hint style="info" %}
特别注意: 这里产生的新interface R中的value是老interface Part的key,取出新interface R的value就是取出了对应老interface Part的key
{% endhint %}

但是我们的的要求是取出老interface Part的key,这个时候再次用`[keyof T]`作为key依次取出新interface的value,但是由于`id` `name`和`subparts`的value为`never`就不会返回任何类型了,所以只返回了`'updatePart'`.



