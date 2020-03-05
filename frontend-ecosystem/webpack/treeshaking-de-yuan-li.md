# Tree-shaking的原理

CommonJS的动态特性模块意味着tree shaking不适用。因为它是不可能确定哪些模块实际运行之前是需要的或者是不需要的。

因为ES6模块的出现，ES6模块依赖关系是确定的，`和运行时的状态无关`，可以进行可靠的静态分析， 这就是Tree shaking的基础。

webpack自身的Tree-shaking不能分析副作用的模块。不纯的函数即为有副作用的函数

