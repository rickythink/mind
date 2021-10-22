# OCP | The Open Closed Principle

软件实体应该是可扩展，而不可修改的。也就是说，对扩展是开放的，而对修改是封闭的。

我的理解是，我们应该以插件式的方式来编写类和模块。如果我们需要额外的功能，我们不应该修改类，而是能够嵌入一个提供这个额外功能的不同类。

我将使用一个经典的计算器示例。这个计算器在最开始只能执行两种运算：加法和减法。计算器类看起来像下面这样（本文中的代码不是用特定语言编写的）：

```javascript
class Calculator {
  public float add(float a, float b) {
    return a + b
  }
  public float subtract(float a, float b) {
    return a — b
  }
}
```

我们像下面这样使用这个类：

```javascript
Calculator calculator = new Calculator()

float sum = calculator.add(10, 2) //the value of sum is 12
float diff = calculator.subtract(10, 2) //the value of diff is 8
```

现在，我们假设客户希望为这个计算器添加乘法功能。为了添加这个额外的功能，我们必须编辑计算器类并添加乘法方法：

```javascript
public float multiply(float a, float b) {
  return a * b
}
```

如果需求又一次改变，客户又需要除法，`sin`，`cos`，`pow`以及众多的其他数学函数，我们不得不一次又一次编辑这个类来添加这些需求。根据开闭原则，这并不是一个明智的做法。因为这意味着我们的类可以修改。我们需要让它屏蔽修改，而对扩展开放，那么我们该怎么做呢？

首先，我们定义一个名为 `Operation` 的接口，这个接口只有一个名为 `compute` 的方法：

```javascript
interface Operation {
  float compute(float a, float b)
}
```

之后，我们可以通过实现 `Operation` 接口来创建操作类（本文中提供的大多数示例也可以通过继承和抽象类来完成，但我更喜欢使用接口）。为了重建简单的计算器示例，我们将编写加法和减法类：

```javascript
class Addition implements Operation {
  public float compute(float a, float b) {
    return a + b
  }
}
class Subtraction implements Operation {
  public float compute(float a, float b) {
    return a — b
  }
}
```

我们的新计算器类只有一个名叫 `calculate` 的方法，在这个方法中，我们可以传递操作数与操作类：

```javascript
class Calculator {
  public float calculate(float a, float b, Operation operation) {
    return operation.compute(a, b)
  }
}
```

我们将像下面这样使用我们的新类：

```javascript
Calculator calculator = new Calculator()

Addition addition = new Addition()
Subtraction subtraction = new Subtraction()

float sum = calculator.calculate(10, 2, addition) //the value of sum is 12
float diff = calculator.calculate(10, 2, subtraction) //the value of diff is 8
```

现在如果我们需要添加乘法，我们将创建这样的一个乘法运算类：

```javascript
class Multiplication implements Operation {
  public float compute(float a, float b) {
    return a * b
  }
}
```

然后通过添加以下内容在上面的示例中使用它：

```javascript
Multiplication multiplication = new Multiplication()
float prod = calculator.calculate(10, 2, multiplication) // the value of prod is 20
```
