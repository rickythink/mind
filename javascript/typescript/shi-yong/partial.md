# Partial

```typescript
type Partial<T> = { [P in keyof T]?: T[P] };
```

Partial 就是将传入的属性全部变为可选项.
