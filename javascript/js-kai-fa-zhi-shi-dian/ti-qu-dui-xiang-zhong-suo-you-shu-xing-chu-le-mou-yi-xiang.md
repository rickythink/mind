# 提取对象中所有属性除了某一项

```javascript
const obj  = { A: 1, B: 2, C: 3, D: 4 };
const { C, ...objWithoutC } = obj;
console.log(objWithoutC);
```
