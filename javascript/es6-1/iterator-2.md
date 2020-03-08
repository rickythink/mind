---
description: 遍历器
---

# Iterator

## 概念

{% hint style="success" %}
遍历器为各种不同的数据类型提供统一的访问机制。任何数据接口，只要部署了 Iterator 接口，就可以完成遍历操作\( 即依次处理该数据结构的所有成员 \)
{% endhint %}

## 遍历过程

1. 创建一个指针对象，指向当前数据结构的起始位置。\( 遍历器对象本质上就是一个指针对象\)
2. 第一次调用指针对象的 next 方法，可以将指针指向数据结构的第一个成员
3. 第二次调用指针对象的 next 方法，指针就指向数据结构的第二个成员
4. 不断调用指针对象的 next 方法，直到它指向数据结构的结束位置

每次调用 next 方法都返回数据结构的当前成员的信息。具体来说，就是返回一个包含 value 和 done 两个属性的对象。

Iterator 接口的目的是为所有的数据结构提供统一的访问机制，即 for ... of 循环。数据结构只要部署了 Iterator 接口，我们就称这种数据结构为” 可遍历 ”。ES6 规定，默认的 Iterator 接口部署在数据结构的 Symbol.iterator 属性，或者说，一个数据结构只要具有 Symbol.iterator 属性，就可以认为是“ 可遍历的 ”。调用 Symbol.iterator 方法，我们就会得到当前数据结构默认的遍历器生成函数。

```javascript
const obj = {
    [Symbol.iterator]: function () {
        return {
            next: function () {
                return {
                    value: 1,
                    done: true
                }
            }
        }
    }
}
```

该对象的根本特征就是具有 next 方法。每次调用 next 方法都会返回一个代表当前成员的信息对象，具有 value 和 done 两个属性。

## 调用 Iterator 接口的场合

1. 解构赋值
2. 扩展运算符
3. yield \*
4. 其他场合\( for...of / Array.from\(\) / Promise.all\(\) \)





