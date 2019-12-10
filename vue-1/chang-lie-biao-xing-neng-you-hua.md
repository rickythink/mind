---
description: 使用Object.freeze优化Vue中的不需要响应式监听的数据
---

# 长列表性能优化

在2.x版本中`Vue`会通过`Object.defineProperty`对数据进行劫持, 以实现双向数据绑定. 但在一些特定的业务场景, 组件只需要进行纯数据展示, 不会有任何变化, 此时我们可能不需要`Vue`对来数据进行劫持. 在大量数据需要进行呈现时, 如果禁止`Vue`对数据进行劫持, 会明显减少组件初始化的时间.  


```javascript
export default {
  data: () => ({
    userList: []
  }),
  async created() {
    const userList = await this.$service.get("/getuserList");
    this.userList = Object.freeze(userList);
  }
};
```

使用`Object.freeze`后，Vue的双向数据绑定实效，这也就是带来性能提升的原因。

