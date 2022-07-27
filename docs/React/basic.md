## 生命周期
`constructor()`数据初始化  
`componentDidMount()`DOM渲染完成，调用请求，返回数据后会重新渲染  
`componentWillUnmount()`完成组件的卸载和数据的销毁  
`render()`生成虚拟DOM，重新渲染DOM树  
`componentDidUpdate()`更新DOM以响应props或state更改

```js
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {
    this.setState({
      date: new Date()
    });
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

```
## 构造函数调用 super

Props（属性）
State（状态）

在子类的`constructor()`中必须先调用`super()`才能引用`this`  
在子类的`constructor()`中调用`super(props)`，可以使用`this.props`

```js
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    console.log(this.props);  // { name: 'sudheer',age: 30 }
  }
}
```

## 类组件和函数组件
类组件有this，状态 state 和生命周期钩子  
函数组件都没有 性能比类组件的性能要高
```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}
```
## 事件处理
```js
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // 为了在回调中使用 `this`，这个绑定是必不可少的
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(state => ({
      isToggleOn: !state.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}

传参
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```

## React 中 refs
Refs 提供了一种访问在render方法中创建的 DOM 节点或者 React 元素的方法
```js
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.myRef = React.createRef();
  }
  render() {
    return <div ref={this.myRef} />;
  }
}


```

## state 和 props 区别
state 是组件自己管理数据，控制自己的状态，可变  
props 是外部传入的数据参数，不可变  
没有state的叫做无状态组件，有state的叫做有状态组件  
多用 props，少用 state，也就是多写无状态组件  

