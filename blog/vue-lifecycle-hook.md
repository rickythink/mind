# Vue lifecycle hook little trick

我们都知道 React Hook 刮起了函数式编程的旋风，与之对应的，Vue 也在 3.0 借鉴了这种思想。不过Vue 3.0 hook 不在本文的讨论范畴，本文将重点总结一下Vue 2.0的生命周期 Hook。

## 组件上的 hook 小 trick

比如有父组件 Parent 和子组件 Child，如果父组件监听到子组件挂载 mounted 就做一些逻辑处理，常规的写法可能如下：

{% code title="Parent.vue" %}
```markup
<Child @mounted="doSomething"/>
```
{% endcode %}

{% code title="Child.vue" %}
```javascript
mounted() {
  this.$emit("mounted");
}
```
{% endcode %}

此外，还有一种特别简单的方式，子组件不需要任何处理，只需要在父组件引用的时候通过@hook 来监听即可，代码如下：

```markup
<Child @hook:mounted="doSomething" />
<Child @hook:updated="doSomething" />
```

## 方法中的 hook 小trick

可以通过一个程序化的侦听器来注册生命周期的绑定

```javascript
mounted: function () {
  var picker = new Pikaday({
    field: this.$refs.input,
    format: 'YYYY-MM-DD'
  })

  this.$once('hook:beforeDestroy', function () {
    picker.destroy()
  })
}
```

