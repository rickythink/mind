# reduce

## 定义

**`reduce()`** 方法对数组中的每个元素执行一个由您提供的**reducer**函数\(升序执行\)，将其结果汇总为单个返回值。  


## 语法

```javascript
arr.reduce(callback(accumulator, currentValue[, index[, array]])[, initialValue])
```

**reducer** 函数接收4个参数:

1. Accumulator \(acc\) \(累计器\)
2. Current Value \(cur\) \(当前值\)
3. Current Index \(idx\) \(当前索引\)
4. Source Array \(src\) \(源数组\)

**reducer** 函数的返回值分配给累计器，该返回值在数组的每个迭代中被记住，并最后成为最终的单个结果值。

`callback`执行数组中每个值 \(如果没有提供 `initialValue则第一个值除外`\)的函数，包含四个参数：

`accumulator`累计器累计回调的返回值; 它是上一次调用回调时返回的累积值，或`initialValue`（见于下方）。

`currentValue`数组中正在处理的元素。

`index` 可选数组中正在处理的当前元素的索引。 如果提供了`initialValue`，则起始索引号为0，否则从索引1起始。

`array`可选调用`reduce()`的数组。

`initialValue`可选作为第一次调用 `callback`函数时的第一个参数的值。 如果没有提供初始值，则将使用数组中的第一个元素。 在没有初始值的空数组上调用 reduce 将报错。

## 例子

```typescript
var maxCallback = ( acc, cur ) => Math.max( acc.x, cur.x );
var maxCallback2 = ( max, cur ) => Math.max( max, cur );

// reduce() 没有初始值
[ { x: 22 }, { x: 42 } ].reduce( maxCallback ); // 42
[ { x: 22 }            ].reduce( maxCallback ); // { x: 22 }
[                      ].reduce( maxCallback ); // TypeError

// map/reduce; 这是更好的方案，即使传入空数组或更大数组也可正常执行
[ { x: 22 }, { x: 42 } ].map( el => el.x )
                        .reduce( maxCallback2, -Infinity );
```

```javascript
let total = [ 0, 1, 2, 3 ].reduce(
  ( acc, cur ) => acc + cur,
  0
); // 和我 6

var initialValue = 0;
var sum = [{x: 1}, {x:2}, {x:3}].reduce(
    (accumulator, currentValue) => accumulator + currentValue.x
    ,initialValue
);

console.log(sum) // logs 6
```

```javascript
var flattened = [[0, 1], [2, 3], [4, 5]].reduce(
  function(a, b) {
    return a.concat(b);
  },
  []
);
// flattened is [0, 1, 2, 3, 4, 5]
```

## 理解

reduce 是一个函数式编程的概念，经常和 map 放在一起说，简单来说，map 就是映射，reduce 就是归纳。映射就是把一个列表按照一定规则映射成另一个列表，而 reduce 是把一个列表通过一定规则进行合并，也可以理解为对初始值进行一系列的操作，返回一个新的值。



