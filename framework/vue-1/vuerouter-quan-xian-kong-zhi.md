# vue-router权限控制

### 路由元信息(meta)+全局导航守卫

```javascript
router.beforeEach((to,from,next)=>{
	if(to.meta.roles.includes(role)){
		next()	//放行
	}esle{
		next({path:"/404"})	//跳到404页面
	}
})
```

## 动态添加路由控制

`vue-router`提供了`addRoutes()`方法，可以动态注册路由，**需要注意的是，动态添加路由是在路由表中`push`路由，由于路由是按顺序匹配的，因此需要将诸如404页面这样的路由放在动态添加的最后。**

### 填坑

vue-router并没有删除路由的api, 这在某些需要删除的场景下会出现问题。可以这样尝试重置vue-router配置

```javascript
// 定义一个函数来创建router
export const createRouter = routes => new VueRouter({
  mode: 'history',
  base: process.env.BASE_URL,
  routes
});

router.match = createRouter(constantRoutes).match;
```
