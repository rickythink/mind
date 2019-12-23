# Redux

Flux 有一些缺点（特点），比如一个应用可以拥有多个 Store，多个Store之间可能有依赖关系；Store 封装了数据还有处理数据的逻辑。

所以大家在使用的时候，一般会用 Redux，他和 Flux 思想比较类似，也有差别。

![](../../.gitbook/assets/image%20%2847%29.png)

1、用户通过 View 发出 Action：

```javascript
store.dispatch(action);
```

2、然后 Store 自动调用 Reducer，并且传入两个参数：当前 State 和收到的 Action。 Reducer 会返回新的 State 。

```javascript
let nextState = xxxReducer(previousState, action);
```

3、State 一旦有变化，Store 就会调用监听函数。

```javascript
store.subscribe(listener);
```

4、listener可以通过  store.getState\(\)  得到当前状态。如果使用的是 React，这时可以触发重新渲染 View。

```javascript
function listerner() {
  let newState = store.getState();
  component.setState(newState);   
}
```

## Reducer

Redux 有很多的 Reducer，对于大型应用来说，State 必然十分庞大，导致 Reducer 函数也十分庞大，所以需要做拆分。Redux 里每一个 Reducer 负责维护 State 树里面的一部分数据，多个 Reducer 可以通过 combineReducers 方法合成一个根 Reducer，这个根 Reducer 负责维护整个 State。

## 对比 Flux

和  Flux  比较一下：Flux 中 Store 是各自为战的，每个 Store 只对对应的 View 负责，每次更新都只通知对应的View：

![](https://user-gold-cdn.xitu.io/2018/12/18/167c11c13f902c34?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

Redux 中各子 Reducer 都是由根 Reducer 统一管理的，每个子 Reducer 的变化都要经过根 Reducer 的整合：

![image.png \| left \| 827x395](https://user-gold-cdn.xitu.io/2018/12/18/167c11c13fb657b0?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

简单来说，Redux有三大原则：

* 单一数据源：Flux 的数据源可以是多个。
* State 是只读的：Flux 的 State 可以随便改。
* 使用纯函数来执行修改：Flux 执行修改的不一定是纯函数。







