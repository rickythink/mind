# 赋值断言

变量后追加`!` 表示变量已经确定被赋值了

```typescript
let x: number;
initialize();
console.log(x! + x!); //ok

function initialize() {
    x = 10;
}
```



