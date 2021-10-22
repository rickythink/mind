# 组件间通信

## 方法1 props/$emit

父组件A通过props的方式向子组件B传递，B to A 通过在 B 组件中 $emit, A 组件中 v-on 的方式实现。

## 方法2 空组件实例的$emit/$on

这种方法通过一个空的Vue实例作为中央事件总线（事件中心），用它来触发事件和监听事件,巧妙而轻量地实现了任何组件间的通信，包括父子、兄弟、跨级。

## 方法3 vuex

## 方法4 $attrs/$listeners

`$attrs`：包含了父作用域中不被 prop 所识别 (且获取) 的特性绑定 (class 和 style 除外)。当一个组件没有声明任何 prop 时，这里会包含所有父作用域的绑定 (class 和 style 除外)，并且可以通过 v-bind="$attrs" 传入内部组件。通常配合 inheritAttrs 选项一起使用。

`$listeners`：包含了父作用域中的 (不含 .native 修饰器的) v-on 事件监听器。它可以通过 v-on="$listeners" 传入内部组件

实现 A 传递数据 到 C

A组件

```javascript
<template>
<div>
  <h2>组件A 数据项:{{myData}}</h2>
  <B @changeMyData="changeMyData" :myData="myData"></B>
</div>
</template>
<script>
import B from "./B";
export default {
  data() {
    return {
      myData: "100"
    };
  },
  components: { B },
  methods: {
    changeMyData(val) {
      this.myData = val;
    }
  }
};
</script>
复制代码
```

B组件

```javascript
<template>
  <div>
    <h3>组件B</h3>
    <C v-bind="$attrs" v-on="$listeners"></C>
  </div>
</template>
<script>
import C from "./C";
export default {
  components: { C },
};
</script>
```

C组件

```javascript
<template>
  <div>
    <h5>组件C</h5>
    <input v-model="myc" @input="hInput" />
  </div>
</template>
<script>
export default {
  props: { myData: { String } },
  created() {
    this.myc = this.myData;  // 在组件A中传递过来的属性
    console.info(this.$attrs, this.$listeners);
  },
  methods: {
    hInput() {
      this.$emit("changeMyData", this.myc); // // 在组件A中传递过来的事件
    }
  }
};
</script>
```

## 方法5 provide/inject

{% hint style="info" %}
`provide` 和 `inject` 主要在开发高阶插件/组件库时使用。并不推荐用于普通应用程序代码中。
{% endhint %}

假设有两个组件： A.vue 和 B.vue，B 是 A 的子组件

```javascript
// A.vue
export default {
  provide: {
    name: 'Mike'
  }
}
```

```javascript
// B.vue
export default {
  inject: ['name'],
  mounted () {
    console.log(this.name);  // Mike
  }
}
```

provide 和 inject 绑定并不是可响应的。这是刻意为之的。然而，如果你传入了一个可监听的对象，那么其对象的属性还是可响应的。比如使用`Vue.obserbable` 方法响应式provide

## 方法6 $parent/$children 与 ref

* `ref`：如果在普通的 DOM 元素上使用，引用指向的就是 DOM 元素；如果用在子组件上，引用就指向组件实例
* `$parent` / `$children`：访问父 / 子实例

需要注意的是：这两种都是直接得到组件实例，使用后可以直接调用组件的方法或访问数据。

不过，**这两种方法的弊端是，无法在跨级或兄弟间通信**。

```javascript
// component-a 子组件
export default {
  data () {
    return {
      title: 'Vue.js'
    }
  },
  methods: {
    sayHello () {
      window.alert('Hello');
    }
  }
}
复制代码
```

```javascript
// 父组件
<template>
  <component-a ref="comA"></component-a>
</template>
<script>
  export default {
    mounted () {
      const comA = this.$refs.comA;
      console.log(comA.title);  // Vue.js
      comA.sayHello();  // 弹窗
    }
  }
</script>
```
