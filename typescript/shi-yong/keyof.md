# Keyof

在JavaScript中属性名称作为参数的API是相当普遍的，但是到目前为止还没有表达在那些API中出现的类型关系。

输入索引类型查询或`keyof`，索引类型查询`keyof T`产生的类型是`T`的属性名称。`keyof T`的类型被认为是`string`的子类型。

**示例**

```javascript
interface Person {
    name: string;
    age: number;
    location: string;
}

type K1 = keyof Person; // "name" | "age" | "location"
type K2 = keyof Person[];  // "length" | "push" | "pop" | "concat" | ...
type K3 = keyof { [x: string]: Person };  // string
```

与之相对应的是_索引访问类型_，也称为_查找类型_。在语法上，它们看起来像元素访问，但是写成类型：

**示例**

```javascript
type P1 = Person["name"];  // string
type P2 = Person["name" | "age"];  // string | number
type P3 = string["charAt"];  // (pos: number) => string
type P4 = string[]["push"];  // (...items: string[]) => number
type P5 = string[][0];  // string
```

你可以将这种模式和类型系统的其它部分一起使用，以获取类型安全的查找。

```javascript
function getProperty<T, K extends keyof T>(obj: T, key: K) {
    return obj[key];  // 推断类型是T[K]
}

function setProperty<T, K extends keyof T>(obj: T, key: K, value: T[K]) {
    obj[key] = value;
}

let x = { foo: 10, bar: "hello!" };

let foo = getProperty(x, "foo"); // number
let bar = getProperty(x, "bar"); // string

let oops = getProperty(x, "wargarbl"); // 错误！"wargarbl"不存在"foo" | "bar"中

setProperty(x, "foo", "string"); // 错误！, 类型是number而非string
```

