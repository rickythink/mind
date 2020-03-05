# 定义固定长度的数组

来源: [https://stackoverflow.com/questions/52489261/typescript-can-i-define-an-n-length-tuple-type](https://stackoverflow.com/questions/52489261/typescript-can-i-define-an-n-length-tuple-type)

Typescript 3 introduces [rest elements in tuple types](https://github.com/Microsoft/TypeScript/wiki/What%27s-new-in-TypeScript#rest-elements-in-tuple-types)

> The last element of a tuple type can be a rest element of the form ...X, where X is an array type

To restrict the length of a tuple we can use intersection with `{ length: N }`

定义类型

```typescript
type Tuple<TItem, TLength extends number> = [TItem, ...TItem[]] & { length: TLength };
```

那么定长为6的数组就是

```typescript
let matrix: Tuple<number, 6>
matrix = [1,2,3,4,5,6]
```

