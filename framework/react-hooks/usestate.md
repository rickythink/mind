# useState

## 基本

```javascript
const [n, setN] = React.useState(0)
const [user, setUser] = React.useState({name: 'Jack', age: 18})
```

{% hint style="info" %}
如果state是一个对象，能否部分setState？ 

答案是不行，因为setState不会帮我们合并属性

那么useReducer会合并属性吗？也不会！ 因为React认为这应该是你自己要做的事情
{% endhint %}

示例

```javascript
function App(){
    const [user, setUser] = React.useState({name: 'Jack', age: 18})
    const onClick = () =>{
        //setUser不可以局部更新，如果只改变其中一个，那么整个数据都会被覆盖
        // setUser({
        //  name: 'Frank'
        // })
        setUser({
            ...user, //拷贝之前的所有属性
            name: 'Frank' //这里的name覆盖之前的name
        })
    }
    return (
        <div className='App'>
            <h1>{user.name}</h1>
            <h2>{user.age}</h2>
            <button onClick={onClick}>Click</button>
        </div>
    )
}
```

## 函数式更新

useState 和 setState 都可以传入函数参数。

```javascript
// 初始化 state 为 initialState。这也叫作 惰性初始 state
const [state, setSate] = useState(() => initialState) 
// 取到最新的 state 并加一后进行 setState
setState(state => state + 1)
```

