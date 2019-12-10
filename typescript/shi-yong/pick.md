# Pick

```javascript
type Pick<T, K extends keyof T> = { [P in K]: T[P] };
```

从 T 中取出 一系列 K 的属性

