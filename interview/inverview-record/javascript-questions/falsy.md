# falsy

**下面哪些值是 falsy?**

```javascript
0
new Number(0)
('')
(' ')
new Boolean(false)
undefined
```

{% tabs %}
{% tab title="选项" %}
* A: `0`, `''`, `undefined`
* B: `0`, `new Number(0)`, `''`, `new Boolean(false)`, `undefined`
* C: `0`, `''`, `new Boolean(false)`, `undefined`
* D: All of them are falsy
{% endtab %}

{% tab title="答案" %}
**答案: A**

只有 6 种 [falsy](https://developer.mozilla.org/zh-CN/docs/Glossary/Falsy) 值:

* `undefined`
* `null`
* `NaN`
* `0`
* `''` (empty string)
* `false`

`Function` 构造函数, 比如 `new Number` 和 `new Boolean`，是 [truthy](https://developer.mozilla.org/zh-CN/docs/Glossary/Truthy)。
{% endtab %}
{% endtabs %}
