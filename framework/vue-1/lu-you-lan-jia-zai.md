# 路由懒加载

Vue 项目中实现路由按需加载（路由懒加载）有 3 种方式：

```javascript
// 1、Vue异步组件技术：
 {
    path: '/home',
    name: 'Home',
    component: resolve => reqire(['path路径'], resolve)
  }

// 2、es6提案的import()
const Home = () => import('path路径')

// 3、webpack提供的require.ensure()
{
  path: '/home',
  name: 'Home',
  component: r => require.ensure([],() =>  r(require('path路径')), 'demo')
 }
```

## require.context\(\)

> require.context\(directory,useSubdirectories,regExp\)

> * directory：说明需要检索的目录
> * useSubdirectories：是否检索子目录
> * regExp: 匹配文件的正则表达式,一般是文件名

```javascript
import titleCom from '@/components/home/titleCom'
import bannerCom from '@/components/home/bannerCom'
import cellCom from '@/components/home/cellCom'
components: {
  titleCom, bannerCom, cellCom
}
```

这样就写了大量重复的代码,利用 require.context 可以写成

```javascript
const path = require('path')
const files = require.context('@/components/home', false, /\.vue$/)
const modules = {}
files.keys().forEach(key => {
  const name = path.basename(key, '.vue')
  modules[name] = files(key).default || files(key)
})
components: modules
```

