# 工厂模式

## 定义

定义一个创建对象的接口，但让实现这个接口的类来决定实例化哪个类。

工厂方法让类的实例化推迟到子类中进行。

## 场景

### 优点&#x20;

隐藏原始类，方便之后的代码迁移

### 缺点

多了层封装，会造成类的数目过多，系统复杂度增加。

## JavaScript实现

```javascript
/**
 * 实体类：Dog、Cat
 */

class Dog {
  run() {
    console.log("狗");
  }
}

class Cat {
  run() {
    console.log("猫");
  }
}

/**
 * 工厂类：Animal
 */

class Animal {
  constructor(name) {
    name = name.toLocaleLowerCase();
    switch (name) {
      case "dog":
        return new Dog();
      case "cat":
        return new Cat();
      default:
        throw TypeError("class name wrong");
    }
  }
}

/**
 * 以下是测试代码
 */

const cat = new Animal("cat");
cat.run();
const dog = new Animal("dog");
dog.run();
```
