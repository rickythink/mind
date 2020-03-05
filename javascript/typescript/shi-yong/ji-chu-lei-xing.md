# 基础类型

## boolean

```javascript
let isDone: boolean = false;
```

## number

```javascript
let decLiteral: number = 6;
let hexLiteral: number = 0xf00d;
let binaryLiteral: number = 0b1010;
let octalLiteral: number = 0o744;
```

## string

```javascript
let name: string = "bob";
name = "smith";
```

## \[ \] 与数组泛型

```javascript
let list: number[] = [1, 2, 3];
let list: Array<number> = [1, 2, 3];
```

## 元组 Tuple

```typescript
let x: [string, number];
```

## 枚举 enum

```typescript
enum Color {Red, Green, Blue}
let c: Color = Color.Green;
```

## Any

不希望类型检查器对这些值进行检查而是直接让它们通过编译阶段的检查。 那么我们可以使用 `any`类型来标记这些变量

```javascript
let notSure: any = 4;
notSure = "maybe a string instead";
```

## Void

`void`表示没有任何类型。 当一个函数没有返回值时，你通常会见到其返回值类型是 `void`

```javascript
function warnUser(): void {
    console.log("This is my warning message");
}
```

声明一个`void`类型的变量没有什么大用，因为你只能为它赋予`undefined`和`null`：

```text
let unusable: void = undefined;
```

## undefined & null

TypeScript里，`undefined`和`null`两者各自有自己的类型分别叫做`undefined`和`null`。 和 `void`相似，它们的本身的类型用处不是很大：

```javascript
let u: undefined = undefined;
let n: null = null;
```

## never

`never`类型表示的是那些永不存在的值的类型。 例如， `never`类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型； 变量也可能是 `never`类型，当它们被永不为真的类型保护所约束时。

```javascript
// 返回never的函数必须存在无法达到的终点
function error(message: string): never {
    throw new Error(message);
}

// 推断的返回值类型为never
function fail() {
    return error("Something failed");
}

// 返回never的函数必须存在无法达到的终点
function infiniteLoop(): never {
    while (true) {
    }
}
```

## object

```javascript
declare function create(o: object | null): void;

create({ prop: 0 }); // OK
create(null); // OK

create(42); // Error
```

