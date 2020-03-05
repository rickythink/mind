# function.length

`length` 是函数对象的一个属性值，指该函数有多少个必须要传入的参数，即形参的个数。

形参的数量不包括剩余参数个数，仅包括第一个具有默认值之前的参数个数。

与之对比的是，  [`arguments.length`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions_and_function_scope/arguments/length) 是函数被调用时实际传参的个数。

```javascript
console.log(Function.length); /* 1 */

console.log((function()        {}).length); /* 0 */
console.log((function(a)       {}).length); /* 1 */
console.log((function(a, b)    {}).length); /* 2 etc. */

console.log((function(...args) {}).length); 
// 0, rest parameter is not counted

console.log((function(a, b = 1, c) {}).length);
// 1, only parameters before the first one with 
// a default value is counted

((a, b, c = 3) => {}).length; // 2
((a, b = 2, c) => {}).length; // 1
((a = 1, b, c) => {}).length; // 0
((...args) => {}).length; // 0
```

