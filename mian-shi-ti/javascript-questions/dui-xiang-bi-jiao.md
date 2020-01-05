# 对象引用

输出是什么？

```javascript
function checkAge(data) {
  if (data === { age: 18 }) {
    console.log('You are an adult!')
  } else if (data == { age: 18 }) {
    console.log('You are still an adult.')
  } else {
    console.log(`Hmm.. You don't have an age I guess`)
  }
}

checkAge({ age: 18 })
```

{% tabs %}
{% tab title="选项" %}
* A: `You are an adult!`
* B: `You are still an adult.`
* C: `Hmm.. You don't have an age I guess`
{% endtab %}

{% tab title="答案" %}
**答案: C**

在测试相等性时，基本类型通过它们的值（value）进行比较，而对象通过它们的引用（reference）进行比较。JavaScript 检查对象是否具有对内存中相同位置的引用。

题目中我们正在比较的两个对象不是同一个引用：作为参数传递的对象引用的内存位置，与用于判断相等的对象所引用的内存位置并不同。

这也是 `{ age: 18 } === { age: 18 }` 和 `{ age: 18 } == { age: 18 }` 都返回 `false` 的原因。
{% endtab %}
{% endtabs %}

输出是什么？

```javascript
let person = { name: "Lydia" };
const members = [person];
person = null;

console.log(members);
```

{% tabs %}
{% tab title="选项" %}
* A: `null`
* B: `[null]`
* C: `[{}]`
* D: `[{ name: "Lydia" }]`
{% endtab %}

{% tab title="答案" %}
**答案: D**

首先我们声明了一个拥有`name`属性的对象 `person`。[![](https://camo.githubusercontent.com/18ec11730a406fb1ea8fe7e4f0cea77d8fe33dd0/68747470733a2f2f692e696d6775722e636f6d2f544d4c314d62532e706e67)](https://camo.githubusercontent.com/18ec11730a406fb1ea8fe7e4f0cea77d8fe33dd0/68747470733a2f2f692e696d6775722e636f6d2f544d4c314d62532e706e67)

然后我们又声明了一个变量`members`. 将首个元素赋值为变量`person`。 当设置两个对象彼此相等时，它们会通过 _引用_ 进行交互。**但是当你将引用从一个变量分配至另一个变量时，其实只是执行了一个** _**复制**_ **操作。**（注意一点，他们的引用 _并不相同_!）[![](https://camo.githubusercontent.com/a775ede4cce9b143614b05f06e4afac49ee4aab0/68747470733a2f2f692e696d6775722e636f6d2f465347354b33462e706e67)](https://camo.githubusercontent.com/a775ede4cce9b143614b05f06e4afac49ee4aab0/68747470733a2f2f692e696d6775722e636f6d2f465347354b33462e706e67)

接下来我们让`person`等于`null`。[![](https://camo.githubusercontent.com/a13998111c61325f557b40db37d77a054fa76407/68747470733a2f2f692e696d6775722e636f6d2f73596a63734d542e706e67)](https://camo.githubusercontent.com/a13998111c61325f557b40db37d77a054fa76407/68747470733a2f2f692e696d6775722e636f6d2f73596a63734d542e706e67)

我们没有修改数组第一个元素的值，而只是修改了变量`person`的值,因为元素（复制而来）的引用与`person`不同。`members`的第一个元素仍然保持着对原始对象的引用。当我们输出`members`数组时，第一个元素会将引用的对象打印出来。
{% endtab %}
{% endtabs %}

