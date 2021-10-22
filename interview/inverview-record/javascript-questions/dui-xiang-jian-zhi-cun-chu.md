# 对象键值存储

输出是什么？

```javascript
const obj = { 1: 'a', 2: 'b', 3: 'c' }
const set = new Set([1, 2, 3, 4, 5])

obj.hasOwnProperty('1')
obj.hasOwnProperty(1)
set.has('1')
set.has(1)
```

{% tabs %}
{% tab title="选项" %}
* A: `false` `true` `false` `true`
* B: `false` `true` `true` `true`
* C: `true` `true` `false` `true`
* D: `true` `true` `true` `true`
{% endtab %}

{% tab title="答案" %}
**答案: C**

**所有对象的键（不包括 Symbol）在底层都是字符串，即使你自己没有将其作为字符串输入**。这就是为什么 `obj.hasOwnProperty('1')` 也返回 `true`。

对于集合，它不是这样工作的。在我们的集合中没有 `'1'`：`set.has('1')` 返回 `false`。它有数字类型为 `1`，`set.has(1)` 返回 `true`。
{% endtab %}
{% endtabs %}

输出是什么？

```javascript
const a = {}
const b = { key: 'b' }
const c = { key: 'c' }

a[b] = 123
a[c] = 456

console.log(a[b])
```

{% tabs %}
{% tab title="选项" %}
* A: `123`
* B: `456`
* C: `undefined`
* D: `ReferenceError`
{% endtab %}

{% tab title="答案" %}
**答案: B**

对象的键被自动转换为字符串。我们试图将一个对象 `b` 设置为对象 `a` 的键，且相应的值为 `123`。

然而，当字符串化一个对象时，它会变成 `"[object Object]"`。因此这里说的是，`a["[object Object]"] = 123`。然后，我们再一次做了同样的事情，`c` 是另外一个对象，这里也有隐式字符串化，于是，`a["[object Object]"] = 456`。

然后，我们打印 `a[b]`，也就是 `a["[object Object]"]`。之前刚设置为 `456`，因此返回的是 `456`。
{% endtab %}
{% endtabs %}
