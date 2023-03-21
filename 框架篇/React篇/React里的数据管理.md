<center><h1>React里的数据管理</h1></center>

### setState()/useState()的使用

#### setState()两种写法：

- ```javascript
  setState(updater,[callback])
    updater为返回stateChange对象的函数: (state, props) => stateChange
    接收的state和props被保证为最新的
  ```

- ```javascript
  setState(stateChange, [callback])
      stateChange为对象,
      callback是可选的回调函数, 在状态更新且界面更新后才执行
  ```

#### useState:

```jsx
const [xxx,setXxx]=useState(val)
//xxx的初始值为val
```

- 参数: 第一次初始化指定的值在内部作缓存
- 返回值: 包含2个元素的数组, 第1个为内部当前状态值, 第2个为更新状态值的函数

```
(1). setXxx(stateChange)------对象式的setState
        1.stateChange为状态改变对象(该对象可以体现出状态的更改)
				
(2). setXxx(updater)------函数式的setState
        1.updater为返回stateChange对象的函数。
        2.updater可以接收到state和props。
```

#### 对useState扩展

react hooks默认是不支持useState的回调的，但有时候因为异步问题，不能及时获取到修改后的值，所以我们可以自定义回调函数

```jsx
function useCallbackState(od) {
  const cbRef = useRef();
  const [data, setData] = useState(od);

  useEffect(() => {
    cbRef.current && cbRef.current(data);
  }, [data]);

  return [
    data,
    function (d, callback) {
      cbRef.current = callback;
      setData(d);
    }
  ];
}
```

#### 总结

    对象方式是函数方式的简写方式
        如果新状态不依赖于原状态 ===> 使用对象方式
        如果新状态依赖于原状态 ===> 使用函数方式
    如果需要在setState()后获取最新的状态数据, 在第二个callback函数中读取（useState返回的setXxx没有第二个参数）

### setState()的同步or异步

#### 结论

##### React 18之前

与执行setState()的位置有关

- 在react控制的回调函数中: 生命周期勾子 / react事件监听回调  => 异步
- 非react控制的异步回调函数中: 定时器回调 / 原生事件监听回调 / promise回调 /... => 同步

##### React 18

与执行setState()的位置无关，全部为异步执行

但可以使用 ` flushSync`方法使之变同步

```jsx
import React, { Component } from 'react'
// flushSync它方法就可以让里面的操作为同步
import { flushSync } from 'react-dom'

export default class App extends Component {
  state = {
    count: 1
  }

  addCount = () => {
    // react18中，让setState它为同步
    flushSync(() => {
      this.setState(state => ({ count: this.state.count + 1 }))
    })
    // 因为setState放在flushSync方法里面了，则它现在是一个同步的，所以在此处可以得到最新的数据
    console.log(this.state.count)
  }

  render() {
    return (
      <div>
        <h3>{this.state.count}</h3>
        <button onClick={this.addCount}>count++</button>
      </div>
    )
  }
}
```

##### setState()与render

- 同步setState():

  > 每执行一次setState(),render执行一次

- 异步setState():

  > 根据js的事件循环机制来确定什么时候把setState()任务队列清空，清空后在执行一次render

### 异步setState()多次调用

- `setState(stateChange, [callback])`:合并跟新一次状态，只调用一次render跟新界面

  > 状态跟新和界面跟新都合并了

- `setState(updater,[callback])`:跟新多次状态，但只调用一次render跟新界面

  > 状态跟新没有合并，但界面跟新合并了

### 测试dome

```jsx
export default class StateTest extends React.Component {

  state = {
    count: 0,
  }

  componentDidMount() {
    this.setState({ count: this.state.count + 1 })
    this.setState({ count: this.state.count + 1 })
    console.log('[1]', this.state.count) 

    this.setState(state => ({ count: state.count + 1 }))
    this.setState(state => ({ count: state.count + 1 }))
    console.log('[2]', this.state.count) 

    setTimeout(() => {
      this.setState({ count: this.state.count + 1 })
      console.log('timeout1', this.state.count) 

      this.setState({ count: this.state.count + 1 })
      console.log('timeout2', this.state.count) 
    }, 0)

    Promise.resolve().then(value => {
      this.setState({ count: this.state.count + 1 })
      console.log('promise1', this.state.count)  

      this.setState({ count: this.state.count + 1 })
      console.log('promise2', this.state.count) 
    })
  }

  render() {
    const count = this.state.count
    console.log('render', count)  
    return (
      <div>
        <p>{count}</p>
      </div>
    )
  }
}
```