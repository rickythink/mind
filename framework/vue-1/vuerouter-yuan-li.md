# vue-router原理

参考：

> [https://segmentfault.com/a/1190000018227116](https://segmentfault.com/a/1190000018227116)
>
> [https://zhangmingemma.github.io/2018/10/16/vueRouter-achieve.html](https://zhangmingemma.github.io/2018/10/16/vueRouter-achieve.html)

vue-router是 Vue.js 官方的路由管理器。它和 Vue.js 的核心深度集成，让构建单页面应用变得易如反掌。这篇文章讲述的主要内容便是vue-router的实现原理，相当于[vue-router源码](https://github.com/vuejs/vue-router)的粗浅理解。行文主要包括vue-router的使用方法、实现原理两大部分，使用方法的介绍是为了辅助实现原理的陈述。

#### 使用方法 <a id="&#x4F7F;&#x7528;&#x65B9;&#x6CD5;"></a>

1. 注册VueRouter插件:

   ```text
    import Vue from 'vue'
    import VueRouter from 'vue-router'
    Vue.use(VueRouter)
   ```

2. 定义（路由）组件与路由。

   ```text
    const User = { template: '<div>用户</div>' }
    const Role = { template: '<div>角色</div>' }
    const routes = [
    { path: '/user', component: User },
    { path: '/home', component: Home }
    ]
   ```

3. 创建 router 实例，并传 `routes` 配置 `const router = new VueRouter({routes })`
4. 创建和挂载根实例。

   ```text
    const app = new Vue({
        router,
        template: `
            <div id="app">
            <h1>Basic</h1>
            <ul>
                <li><router-link to="/">/</router-link></li>
                <li><router-link to="/user">用户</router-link></li>
                <li><router-link to="/role">角色</router-link></li>
                <router-link tag="li" to="/user">/用户</router-link>
            </ul>
            <router-view class="view"></router-view>
            </div>
        `
    }).$mount('#app')
   ```

#### 实现原理 <a id="&#x5B9E;&#x73B0;&#x539F;&#x7406;"></a>

每一个工具实现的原理都依赖JS/HTML基本语法，比如Vue的双向数据绑定依赖的是JS的Object.defineProperty。而Vue Router的实现依赖于两种前端路由，即模式history模式和hash模式：

* history模式
  * HTML5中的两个API：**pushState**和**replaceState**，改变url之后页面不会重新刷新，也不会带有\#号，页面地址美观，url的改变会触发**popState**事件，监听该事件也可以实现根据不同的url渲染对应的页面内容
  * 但是因为没有\#会导致用户在刷新页面的时候，还会发送请求到服务端，为避免这种情况，需要每次url改变的时候，都将所有的路由重新定位到跟路由下
* hash模式
  * url hash: http://foo.com/\#help
  * \#后面hash值的改变，并不会重新加载页面，同时hash值的变化会触发**hashchange**事件，该事件可以监听，可根据不同的哈希值渲染不同的页面内容

**1. 插件注册**

Vue.use将VueRouter插件注入Vue中，use方法会调用vue-router的`install`方法，对**VueRouter进行安装**，整个安装过程精简版的源码，全部[源码地址](https://github.com/vuejs/vue-router/blob/dev/src/install.js)：

```javascript
Vue.mixin({
  beforeCreate () { ...},
})
Object.defineProperty(Vue.prototype, '$router', {
  get () { return this._routerRoot._router }
})
Object.defineProperty(Vue.prototype, '$route', {
  get () { return this._routerRoot._route }
})
Vue.component('RouterView', View)
Vue.component('RouterLink', Link)
```

install这个过程主要做了这样的三件事：

* 对Vue实例混入beforeCreate/destroyed两个钩子操作，在Vue实例创建生命周期中钩子函数执行前被调用，这个在创建Vue实例部分详细介绍；
* 设置Vue.prototype代理，当访问this.$router和this.$route的时候,返回this.\_routerRoot.\_router和\_route，方便所有组件可以获取到这两个属性
* 注册RouterLink和RouterView两个组件
  * router-view: 获取到匹配的组件，在非keepalive模式下，每次都要设置钩子进而更新匹配了的实例元素
  * router-link: 绑定click事件，在其点击的时候根据设置的 to 的值去调用 router 的 push 或者 replace 来更新路由的，同时呢，会检查自身是否和当前路由匹配（严格匹配和包含匹配）来决定自身的 activeClass 是否添加

**2. 创建router实例**

全部[源码地址](https://github.com/vuejs/vue-router/blob/dev/src/index.js),核心源码：

```javascript
this.matcher = createMatcher(options.routes || [], this)
let mode = options.mode || 'hash'
this.fallback = mode === 'history' && !supportsPushState && options.fallback !== false
if (this.fallback) {
  mode = 'hash'
}
if (!inBrowser) {
  mode = 'abstract'
}
this.mode = mode
switch (mode) {
  case 'history':
    this.history = new HTML5History(this, options.base)
    break
  case 'hash':
    this.history = new HashHistory(this, options.base, this.fallback)
    break
  case 'abstract':
    this.history = new AbstractHistory(this, options.base)
    break
  default:
    if (process.env.NODE_ENV !== 'production') {
      assert(false, `invalid mode: ${mode}`)
    }
}
init(){....}
```

可以看到这个阶段，Vue Router主要做了两件主要的事情：

* 创建 match 匹配函数：根据用户路由配置对象生成普通的根据 path 来对应的路由记录以及根据 name 来对应的路由记录的 map，方便后续匹配对应
* 根据配置的`mode`创建路由实现的实例：
  * 如果当前环境不支持history模式，会强制切换到hash模式；默认hash模式
  * 如果当前环境不是浏览器环境，会切换到abstract模式下。

**3. 创建Vue实例**

在这里创建Vue实例，首先会执行install时候混入的钩子函数，这一部分的源码：

```javascript
Vue.mixin({
  beforeCreate () { 
    if (isDef(this.$options.router)) {
      this._routerRoot = this
      this._router = this.$options.router
      this._router.init(this)
      Vue.util.defineReactive(this, '_route', this._router.history.current)      //为vue实例定义数据劫持
    } else {

      this._routerRoot = (this.$parent && this.$parent._routerRoot) || this      //非跟组件则直接从父组件中取
    }
    registerInstance(this, this)
  },
  destroyed () {
    registerInstance(this)
  }
})
```

这一部分，主要做了两件事情

* 对\_router赋值，此时this.$options.router包含着传入的路由参数
* 执行创建vue router实例中的init方法，根据不同模式生成监控路由变化的History对象

```javascript
init (app: any /* Vue component instance */) {
    ...
    const history = this.history
    if (history instanceof HTML5History) {
      history.transitionTo(history.getCurrentLocation())
    } else if (history instanceof HashHistory) {
      const setupHashListener = () => {
        history.setupListeners()
      }
      history.transitionTo(
        history.getCurrentLocation(),
        setupHashListener,
        setupHashListener
      )
    }
    history.listen(route => {
      this.apps.forEach((app) => {
        app._route = route
      })
    })
}
```

这一部分，主要做了两件事情：

* 通过history的模式来确定不同路由的切换动作`history.transitionTo`，更新浏览器地址
  * 对于hash模式，更新hash的值
  * 对于history模式，利用pushState/replaceState更新浏览器地址
  * 对于Abstract模式下，所做的仅仅是用一个数组当做栈来模拟浏览器历史记录，拿一个变量来标示当前处于哪个位置。
* 通过`history.listen`来注册路由变化的响应回调

一旦this.\_router.history.curren，即history实例当前的路由对象有变化，就会触发更新机制，继而调用应用实例的 render 重新渲染

* hash模式的更新机制：触发onHashChange的事件，调用transitionTo
* history模式的更新机制：触发popState的事件，调用transitionTo

**为什么不直接在初始化 HashHistory 的时候监听 hashchange 事件呢？**

这个是为了修复https://github.com/vuejs/vue-router/issues/725 这个 bug 而这样做的，简要来说就是说如果在 beforeEnter 这样的钩子函数中是异步的话，beforeEnter 钩子就会被触发两次，原因是因为在初始化的时候如果此时的 hash 值不是以 / 开头的话就会补上 \#/，这个过程会触发 hashchange 事件，所以会再走一次生命周期钩子，也就意味着会再次调用 beforeEnter 钩子函数。

