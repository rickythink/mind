# infer

条件类型提供了 infer 关键字来推断类型，用于替代手动获取类型。TypeScript 为此提供了一个示例，他们创建了一个叫作 Flatten 的类型，用于将数组转成他们需要的元素类型：

```javascript
type Flatten<T> = T extends any[] ? T[number] : T;
```

如果使用关键字 infer 就可以将上面的代码简化成：

```javascript
type Flatten<T> = T extends Array<infer U> ? U : T;
```

## 示例

```javascript
type ParamType<T> = T extends (param: infer P) => any ? P : T;
```

在这个条件语句 `T extends (param: infer P) => any ? P : T` 中，`infer P` 表示待推断的函数参数。

整句表示为：如果 T 能赋值给 (param: infer P) => any，则结果是 (param: infer P) => any 类型中的参数 P，否则返回为 T。

```javascript
interface User {
  name: string;
  age: number;
}

type Func = (user: User) => void

type Param = ParamType<Func>;   // Param = User
type AA = ParamType<string>;    // string
```
