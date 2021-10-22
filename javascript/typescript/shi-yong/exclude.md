# Exclude

```javascript
type Exclude<T, U> = T extends U ? never : T;
```

结合实例

```javascript
type T = Exclude<1 | 2, 1 | 3> // -> 2
```

很轻松地得出结果 `2`

根 据代码和示例我们可以推断出&#x20;

**Exclude 的作用是从 T 中找出 U 中没有的元素, 换种更加贴近语义的说法其实就是从T 中排除 U**
