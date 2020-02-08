# Array.some

**`some()`** 方法测试数组中是不是至少有1个元素通过了被提供的函数测试。它返回的是一个Boolean类型的值。

只要有一个元素通过测试，`some()` 即会立即返回true。

**`some()`** 被调用时不会改变数组。

```javascript
const array = [1, 2, 3, 4, 5];

// checks whether an element is even
const even = (element) => element % 2 === 0;

console.log(array.some(even));
// expected output: true
```

{% hint style="info" %}
如果用一个空数组进行测试，在任何情况下它返回的都是`false`。
{% endhint %}

