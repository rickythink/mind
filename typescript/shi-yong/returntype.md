# ReturnType

```javascript
type ReturnType<T> = T extends (...args: any[]) => infer R ? R : any;
```

示例

```javascript
function getInt(a: string) {
  return parseInt(a);
}

type A = ReturnType<typeof getInt>; // => number
```

