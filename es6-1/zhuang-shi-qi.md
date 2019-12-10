# 装饰器

## 类的装饰

装饰器\(Decorator\)是一个函数，用来修改类的行为。

```javascript
@decortator
class A {}

// 等同于
class A {}
A = decorator(A) || A
```

```javascript
// minxins.js
export function mixins(...list) {
    return function (target) {
        Object.assign(targe.prototype, ...list)
    }
}

// main.js
import { mixins } from './mixins'

const Foo = {
    foo () { console.log('foo') }
}

@mixins(Foo)
class MyClass {}

let obj = new MyClass()
obj.foo() // 'foo'
```

## 方法的装饰

装饰器不仅可以装饰类，还可以装饰类的属性。

```javascript
class Person {
    @readonly
    name() { return `${this.first} ${this.last}`
}
```

其中装饰器 readonly 用来修饰”类“的name方法。此时装饰器一共可以接受3个参数，第一个是所要装饰的目标对象，第二个参数是修饰的属性名，第三个是属性的描述对象。

```javascript
function readonly(target, name, descriptor){
    descriptor.writable = false
    return descriptor
}
```

## 装饰器不能用于函数

装饰器只能用于类和类的方法，不能用于函数，因为存在函数提升。

