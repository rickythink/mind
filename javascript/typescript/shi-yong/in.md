# in

in 则可以遍历枚举类型

```typescript
type Keys = "a" | "b"
type Obj =  {
  [p in Keys]: any
} // -> { a: any, b: any }
```

