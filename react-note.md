## React
* className
* props & super(props)
* setState是异步的
```javascript
class Square extends React.componet {
    constructor(props) {
        super(props);
        this.state = {
            value: null
        }
    }
    render() {
        return (
            <button 
                className="square"
                onClick={()=>{
                    this.setState({value: 'X'})
                }}
            >
                {this.state.value}
            </button>
        )
    }
}
```
1. 如果你想写的组件只包含一个 render 方法，并且不包含 state，那么使用函数组件就会更简单。我们不需要定义一个继承于 React.Component 的类，我们可以定义一个函数，这个函数接收 props 作为参数，然后返回需要渲染的元素。
```javascript
class Square extends React.Component {
    render() {
      return (
        <button 
            className="square"
            onClick={()=>this.props.onClick()}
        >
          {this.props.value}
        </button>
      );
    }
  }
  //修改后
function Square(props) {
  return (
    <button className="square" onClick={props.onClick}>
      {props.value}
    </button>
  );
}
```