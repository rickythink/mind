# 改变数组的length

## 引子

```
const clothes = ['jacket', 't-shirt']; 
clothes.length = 0;

clothes[0]; // => ???
```

## 解析

数组对象的 `length` 属性具有一些[特殊的行为](http://www.ecma-international.org/ecma-262/6.0/#sec-properties-of-array-instances-length)：

> 减少 length 属性的值的副作用是删除 自己的 数组元素，这些元素的数组索引位于新旧长度值之间。

由于 `length` 属性行为，当 JS 执行 `clothes.length = 0` 时，删除所有的 `clothes` 项。 所以 `clothes[0]` 的值为 `undefined`，因为 `clothes` 数组已被清空。
