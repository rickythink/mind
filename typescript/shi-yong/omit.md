# Omit

```javascript
type Omit<T, K extends keyof any> = Pick<T, Exclude<keyof T, K>>;
```

用之前的 Pick 和 Exclude 进行组合, 实现忽略对象某些属性功能

`Omit` = `Exclude` + `Pick`

`Omit<T, K>`的作用是忽略`T`中的某些属性.

在Typescript 3.4之前没有`Omit` 时，实现同样的功能必须写更多的代码

```typescript
type Person = {
    name: string;
    age: number;
    location: string;
};

type RemainingKeys = Exclude<keyof Person, "location">;

type QuantumPerson = Pick<Person, RemainingKeys>;

// equivalent to
type QuantumPerson = {
    name: string;
    age: number;
};
```

使用Omit

```javascript
type QuantumPersion = Omit<Person, "location">
```



