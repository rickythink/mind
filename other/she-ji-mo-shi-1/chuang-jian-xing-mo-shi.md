# 单例模式

## 定义

保证一个类仅有一个实例，并提供访问此实例的全局访问点。

单例模式属于创建型模式。

## 使用场景

如果一个类负责连接数据库的线程池、日志记录逻辑等等，此时需要单例模式来保证对象不被重复创建，以达到降低开销的目的。

## JavaScript代码实现

```text
const Singleton = function() {};

Singleton.getInstance = (function() {
  // 由于es6没有静态类型,故闭包: 函数外部无法访问 instance
  let instance = null;
  return function() {
    // 检查是否存在实例
    if (!instance) {
      instance = new Singleton();
    }
    return instance;
  };
})();

let s1 = Singleton.getInstance();
let s2 = Singleton.getInstance();

console.log(s1 === s2);
```

