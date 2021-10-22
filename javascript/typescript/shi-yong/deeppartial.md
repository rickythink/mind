# DeepPartial

`Partial`,它的作用是将属性全部变为可选.

```javascript
type Partial<T> = { [P in keyof T]?: T[P] };
```

但是`Partial`无法把深层属性变成可选,只能处理外层属性。

想处理深层属性,就必须用到类型递归:

```javascript
type DeepPartial<T> = {
    [U in keyof T]?: T[U] extends object
    ? DeepPartial<T[U]>
    : T[U]
};

type R2 = DeepPartial<Person>
```

