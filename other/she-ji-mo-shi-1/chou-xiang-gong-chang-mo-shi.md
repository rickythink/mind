# 抽象工厂模式

## 直接上代码

```javascript
// 具体工厂继承自抽象工厂
class FakeStarFactory extends MobilePhoneFactory {
    createOS() {
        // 提供安卓系统实例
        return new AndroidOS()
    }
    createHardWare() {
        // 提供高通硬件实例
        return new QualcommHardWare()
    }
}

// 定义操作系统这类产品的抽象产品类
class OS {
    controlHardWare() {
        throw new Error('抽象产品方法不允许直接调用，你需要将我重写！');
    }
}

// 定义具体操作系统的具体产品类
class AndroidOS extends OS {
    controlHardWare() {
        console.log('我会用安卓的方式去操作硬件')
    }
}

class AppleOS extends OS {
    controlHardWare() {
        console.log('我会用🍎的方式去操作硬件')
    }
}
```

使用的时候代码

```javascript
// 这是我的手机
const myPhone = new FakeStarFactory()
// 让它拥有操作系统
const myOS = myPhone.createOS()
```

假如有一天，FakeStar过气了，我们需要产出一款新机投入市场，这时候怎么办？我们是不是**不需要对抽象工厂MobilePhoneFactory做任何修改**，只需要拓展它的种类：

```javascript
class newStarFactory extends MobilePhoneFactory {
    createOS() {
        // 操作系统实现代码
    }
    createHardWare() {
        // 硬件实现代码
    }
}
```

这么个操作，**对原有的系统不会造成任何潜在影响** 所谓的“对拓展开放，对修改封闭”就这么圆满实现了。前面我们之所以要实现**抽象产品类**，也是同样的道理。
