# Parent控制Child(Lifting state up)

React的设计哲学就是各自负责自己的state，当parent需要控制child时，这时要用到的设计概念叫`Lifting state up`&#x20;

Demo: [https://codesandbox.io/s/z3431proxl?from-embed](https://codesandbox.io/s/z3431proxl?from-embed)

## 核心代码

```javascript
function Counter(props) {
  const {
    count: [count, setCount]
  } = {
    count: useState(0),
    ...(props.state || {})
  };

  return (
    <div>
      <h3>{count}</h3>
      <button onClick={() => setCount(count - 1)}>Decrement</button>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}

function App() {
  const [count, setCount] = useState(0);

  return (
    <div className="App">
      <h2 onClick={() => setCount(count - 1)}>Lifted state</h2>
      <Counter state={{ count: [count, setCount] }} />
      <Counter state={{ count: [count, setCount] }} />
      <h2>Isolated state</h2>
      <Counter />
    </div>
  );
}
```
