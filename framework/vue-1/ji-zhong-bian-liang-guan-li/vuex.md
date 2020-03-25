# Vuex

Vuex 主要用于 Vue，和 Flux，Redux 的思想很类似。

![](../../../.gitbook/assets/image%20%28185%29.png)

每一个 Vuex 里面有一个全局的 Store，包含着应用中的状态 State，这个 State 只是需要在组件中共享的数据，不用放所有的 State，没必要。这个 State 是单一的，和 Redux 类似，所以，一个应用仅会包含一个 Store 实例。单一状态树的好处是能够直接地定位任一特定的状态片段，在调试的过程中也能轻易地取得整个当前应用状态的快照。

### Mutation

显而易见，State 不能直接改，需要通过一个约定的方式，这个方式在 Vuex 里面叫做 mutation，更改 Vuex 的 store 中的状态的唯一方法是提交 mutation。Vuex 中的 mutation 非常类似于事件：每个 mutation 都有一个字符串的 事件类型 \(type\) 和 一个 回调函数 \(handler\)。

注意：**mutation 都是同步事务**。

### Action

到这里又该处理**异步**这块儿了。mutation 是必须同步的，这个很好理解，和之前的  reducer 类似，不同步修改的话，会很难调试，不知道改变什么时候发生，也很难确定先后顺序，A、B两个 mutation，调用顺序可能是 A -&gt; B，但是最终改变 State 的结果可能是 B -&gt; A。

对比Redux的中间件，Vuex 加入了 Action 这个东西来处理异步，Vuex的想法是把同步和异步拆分开，异步操作想咋搞咋搞，但是不要干扰了同步操作。View 通过 store.dispatch\('increment'\) 来触发某个 Action，Action 里面不管执行多少异步操作，完事之后都通过 store.commit\('increment'\) 来触发 mutation，一个 Action 里面可以触发多个 mutation。所以 Vuex 的Action 类似于一个灵活好用的中间件。

Vuex 单一状态树并不影响模块化，把 State 拆了，最后组合在一起就行。Vuex 引入了 Module 的概念，每个 Module 有自己的 state、mutation、action、getter，其实就是把一个大的 Store 拆开。

总的来看，Vuex 的方式比较清晰，适合 Vue 的思想，在实际开发中也比较方便。  


### 对比Redux

Redux

*  view——&gt;actions——&gt;reducer——&gt;state变化——&gt;view变化（同步异步一样）

Vuex 

* view——&gt;commit——&gt;mutations——&gt;state变化——&gt;view变化（同步操作）
*  view——&gt;dispatch——&gt;actions——&gt;mutations——&gt;state变化——&gt;view变化（异步操作） 

