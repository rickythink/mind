# 模块

AMD 与 CMD：

* AMD是 RequireJS 在推广过程中对模块定义的规范化产出。
* CMD是 SeaJS 在推广过程中对模块定义的规范化产出。
* CMD推崇依赖就近，AMD推崇依赖前置。

ES Module与CommonJS:

* CommonJS模块是对象，是运行时加载，运行时才把模块挂载在exports之上（加载整个模块的所有），加载模块其实就是查找对象属性。
* ES Module不是对象，是使用export显示指定输出，再通过import输入。此法为编译时加载，编译时遇到import就会生成一个只读引用。等到运行时就会根据此引用去被加载的模块取值。所以不会加载模块所有方法，仅取所需。
* CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用。
* CommonJS 模块是运行时加载，ES6 模块是编译时输出接口

CommonJS与AMD/CMD:

* AMD/CMD是CommonJS在浏览器端的解决方案。
* CommonJS是同步加载（代码在本地，加载时间基本等于硬盘读取时间）。
* AMD/CMD是异步加载（浏览器必须这么做，代码在服务端）

UMD与AMD/CMD

* UMD（Universal Module Definition）是AMD和CommonJS的糅合，跨平台的解决方案。
* AMD模块以浏览器第一的原则发展，异步加载模块。
* CommonJS模块以服务器第一原则发展，选择同步加载，它的模块无需包装\(unwrapped modules\)。
* UMD先判断是否支持Node.js的模块（exports）是否存在，存在则使用Node.js模块模式。再判断是否支持AMD（define是否存在），存在则使用AMD方式加载模块。

## 用法

### umd 模块（通用模块） <a id="articleHeader0"></a>

```text
(function (global, factory) {
    typeof exports === 'object' && typeof module !== 'undefined' ? module.exports = factory() :
    typeof define === 'function' && define.amd ? define(factory) :
    (global.libName = factory());
}(this, (function () { 'use strict';})));
```

如果你在`js`文件头部看到这样的代码，那么这个文件使用的就是 `UMD` 规范  
实际上就是 **amd** + **commonjs** + **全局变量** 这三种风格的结合  
这段代码就是对当前运行环境的判断，如果是 `Node` 环境 就是使用 `CommonJs` 规范， 如果不是就判断是否为 `AMD` 环境， 最后导出全局变量  
有了 `UMD` 后我们的代码和同时运行在 `Node` 和 `浏览器上`  
所以现在前端大多数的库最后打包都使用的是 `UMD` 规范

### CommonJS

* 导出使用module.exports，也可以exports。就是在此对象上挂属性。exports指向module.exports，即exports= module.exports
* 加载模块使用require\('xxx'\)。相对、绝对路径均可。默认引用js，可以不写.js后缀

```javascript
// commonjs
module.exports.add = function add(params) {
    return ++params;
}
exports.sub = function sub(params) {
    return --params;
}
// index.js
var common = require('./commonjs');
console.log(common.sub(1));
console.log(common.add(1));
```

### AMD/RequireJS

* 定义模块：define\(id?, dependencies?, factory\)
  * 依赖有三个默认的，即"require", "exports", "module"。顺序个数均可视情况
  * 如果忽略则factory默认此三个传入参数
  * id一般是不传的，默认是文件名
* 加载模块：require\(\[module\], factory\)

```javascript
// a.js
define(["b", "require", "exports"], function(b, require, exports) {
    console.log("a.js执行");
    console.log(b);
    // 暴露api可以使用exports、module.exports、return
    exports.a = function() {
        return require("b");
    }
})
// b.js
define(function() {
    console.log('b.js执行');
    console.log(require);
    console.log(exports);
    console.log(module);
    return 'b';
})
// index.js
// 支持Modules/Wrappings写法，注意dependencies得是空的，且factory参数不可空
define(function(require, exports, module) {
    console.log('index.js执行');
    var a = require('a');
    var b = require('b');
})
// index.js
require(['a', 'b'], function(a, b) {
    console.log('index.js执行');
})
```

### CMD/SeaJS

* 定义模块：define\(factory\)
  * require, exports, module参数顺序不可乱
  * 暴露api方法可以使用exports、module.exports、return
  * 与requirejs不同的是，若是未暴露，则返回{}，requirejs返回undefined
* 加载模块：require
* 定义模块无需列依赖，它会调用factory的toString方法对其进行正则匹配以此分析依赖
* 预先下载，延迟执行

```text
// a.js
define(function(require, exports, module) {
    console.log('a.js执行');
    console.log(require);
    console.log(exports);
    console.log(module);
})
// b.js
define(function(require, module, exports) {
    console.log('b.js执行');
    console.log(require);
    console.log(exports);
    console.log(module);
})
// index.js
define(function(require) {
    var a = require('a');
    var b = require('b');
    console.log(a);
    console.log(b);
})
复制代码
```

* ES Module
  * 输出/export
  * 输入/import
  * 输入的模块变量是不可重新赋值的，它只是个可读引用，不过却可以改写属性

```javascript
// 报错1
export 1;
// 报错2
const m = 1;
export m;

// 接口名与模块内部变量之间，建立了一一对应的关系
// 写法1
export const m = 1;
// 写法2
const m = 1；
export { m };
// 写法3
const m = 1；
export { m as module };
// 类似于对象解构
```

```javascript
// module.js
export const m = 1;
// index.js
// 注意，这里的m得和被加载的模块输出的接口名对应
import { m } from './module';
// 若是想为输入的变量取名
import { m as m1 }  './module';
// 值得注意的是，import是编译阶段，所以不能动态加载，比如下面写法是错误的。因为'a' + 'b'在运行阶段才能取到值，运行阶段在编译阶段之后
import { 'a' + 'b' } from './module';
// 若是只是想运行被加载的模块，如下
// 值得注意的是，即使加载两次也只是运行一次
import './module';
// 整体加载
import * as module from './module';
```

* 总结:

| 区别项 | es模块化 | commonJS | AMD |
| :--- | :--- | :--- | :--- |
| 可用于服务端还是浏览器 | 服务端和浏览器 | 服务端 | 浏览器 |
| 模块依赖关系何时确定\(即：何时加载模块\) | 编译时 | 运行时 | 运行时 |
| 设计思想 | 尽量的静态化 |  |  |
| 模块是不是对象 | 不是 | 是 |  |
| 是否整体加载模块\(即加载的所有方法\) | 否 | 是 |  |
| 是否是动态更新\(即通过接口,可以取到模块内部实时的值\) | 是。es module输出的是值的引用 | 不是。commonJS模块输出的是值的拷贝，不存在动态更新 |  |
| 模块变量是否是只读的 | v是。原因：ES6 输入的模块变量，只是一个“符号连接”，所以这个变量是只读的，对它进行重新赋值会报错。 |  |  |

* commonJS模块就是对象，整体加载模块（即加载的所有方法）
* ES6 模块不是对象，而是通过export命令显式指定输出的代码，再通过import命令输入。
* export命令规定的是对外的接口，必须与模块内部的变量建立一一对应关系
* export语句输出的接口，与其对应的值是动态绑定关系，即通过该接口，可以取到模块内部实时
* export命令和import命令可以出现在模块的任何位置，只要处于模块顶层就可以。 如果处于块级作用域内，就会报错，这是因为处于条件代码块之中，就没法做静态优化了，违背了ES6模块的设计初衷。
* import命令具有提升效果，会提升到整个模块的头部，首先执行。

### CommonJs 和 ES6 Module 的区别 <a id="articleHeader11"></a>

其实上面我们已经说到了一些区别

* `CommonJs`导出的是变量的一份拷贝，`ES6 Module`导出的是变量的绑定（`export default` 是特殊的）
* `CommonJs`是单个值导出，`ES6 Module`可以导出多个
* `CommonJs`是动态语法可以写在判断里，`ES6 Module`静态语法只能写在顶层
* `CommonJs`的 `this` 是当前模块，`ES6 Module`的 `this` 是 `undefined`

### ES6 import 的顺序？

{% embed url="https://stackoverflow.com/questions/35551366/what-is-the-defined-execution-order-of-es6-imports" %}







