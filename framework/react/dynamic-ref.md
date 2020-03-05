# Dynamic Ref

在dom结构不固定的情况下，需要使用`Dynamic Ref` 

> [https://stackoverflow.com/questions/52448143/dealing-with-ref-within-a-loop-in-react](https://stackoverflow.com/questions/52448143/dealing-with-ref-within-a-loop-in-react)

## 示例

```javascript
const Hello = React.forwardRef((props,  ref) => <input ref={ref} />);

class Button extends React.Component {
  onClick = () => this.props.onClick(this.props.id);

  render() {
    return (
      <button onClick={this.onClick}>{this.props.children}</button>
    );
  }
}

class TestRef extends React.Component {
  state = {
    data: [
      {
        name: "abc"
      },
      { name: "def" }
    ]
  };
  
  inputRefs = [];
  
  setRef = (ref) => {
    this.inputRefs.push(ref);
  };
  
  focusInput = (id) => this.inputRefs[id].focus();
  
  render() {
    return (
      <div>
        {this.state.data.map(({ name }) => (
          <Hello 
            placeholder={name} 
            ref={this.setRef} 
            key={name} />
        ))}
        <Button onClick={this.focusInput} id={0}>focus input 1</Button>
        <Button onClick={this.focusInput} id={1}>focus input 2</Button>
      </div>
    );
  }
}

ReactDOM.render(<TestRef />, document.getElementById("root"));
```

