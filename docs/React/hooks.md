## React Hooks

一个简单的有状态组件
```js
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  render() {
    return (
      <div>
        <p>You clicked {this.state.count} times</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
}
```

使用hooks后的版本
```js
import { useState } from 'react';

function Example() {
  const [count, setCount] = useState(0);
  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

```js
const [count, setCount] = useState(0);
```
声明了一个状态变量count，把它的初始值设为0，同时提供了一个可以更改count的函数setCount。

### react native

TextInput是一个允许用户输入文本的基础组件

ScrollView是一个通用的可滚动的容器

FlatList组件用于显示一个垂直的滚动列表，其中的元素之间结构近似而仅数据不同。

FlatList组件必须的两个属性是data和renderItem。data是列表的数据源，而renderItem则从数据源中逐个解析数据，然后返回一个设定好格式的组件来渲染。

SectionList渲染的是一组需要分组的数据，也许还带有分组标签的

使用 Fetch 请求数据


## react

Props（属性）
State（状态）

在子类的`constructor()`中必须先调用`super()`才能引用`this`
在子类的`constructor()`中调用`super(props)`，可以使用`this.props`

