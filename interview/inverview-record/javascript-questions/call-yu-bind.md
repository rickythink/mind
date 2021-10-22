# call与bind

输出是什么？

```javascript
const person = { name: 'Lydia' }

function sayHi(age) {
  console.log(`${this.name} is ${age}`)
}

sayHi.call(person, 21)
sayHi.bind(person, 21)
```

{% tabs %}
{% tab title="选项" %}
* A: `undefined is 21` `Lydia is 21`
* B: `function` `function`
* C: `Lydia is 21` `Lydia is 21`
* D: `Lydia is 21` `function`
{% endtab %}

{% tab title="答案" %}
**答案: D**

使用这两种方法，我们都可以传递我们希望 `this` 关键字引用的对象。但是，`.call` 是**立即执行**的。

`.bind` 返回函数的**副本**，但带有绑定上下文！它不是立即执行的。
{% endtab %}
{% endtabs %}
