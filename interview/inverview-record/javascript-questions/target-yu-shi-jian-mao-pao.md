# target与事件冒泡

输出是什么？

```javascript
<div onclick="console.log('first div')">
  <div onclick="console.log('second div')">
    <button onclick="console.log('button')">
      Click!
    </button>
  </div>
</div>
```

{% tabs %}
{% tab title="选项" %}
* A: Outer `div`
* B: Inner `div`
* C: `button`
* D: 一个包含所有嵌套元素的数组。
{% endtab %}

{% tab title="答案" %}
**答案: C**

导致事件的最深嵌套的元素是事件的 target。你可以通过 `event.stopPropagation` 来停止冒泡。

见笔记 事件委托 那一页。**currentTarget始终是监听事件者，而target是事件的真正发出者**。
{% endtab %}
{% endtabs %}

