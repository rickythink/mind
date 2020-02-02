# Record

将 K 中所有的属性的值转化为 T 类型

```javascript
type Record<K extends keyof any, T> = { [P in K]: T };
```

```javascript
type Car = 'Audi' | 'BMW' | 'MercedesBenz'
type CarList = Record<Car, {age: number}>

const cars: CarList = {
    Audi: { age: 119 },
    BMW: { age: 113 },
    MercedesBenz: { age: 133 },
}
```

