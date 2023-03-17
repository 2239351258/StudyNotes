<center><h1>React Hooks的使用</h1></center>

### useState

- 参数: 第一次初始化指定的值在内部作缓存
-  返回值: 包含2个元素的数组, 第1个为内部当前状态值, 第2个为更新状态值的函数

```jsx	
const [xxx,setXxx]=useState(val)
//xxx的初始值为val
```

```
(1). setState(stateChange, [callback])------对象式的setState
        1.stateChange为状态改变对象(该对象可以体现出状态的更改)
        2.callback是可选的回调函数, 它在状态更新完毕、界面也更新后(render调用后)才被调用
				
(2). setState(updater, [callback])------函数式的setState
        1.updater为返回stateChange对象的函数。
        2.updater可以接收到state和props。
        4.callback是可选的回调函数, 它在状态更新、界面也更新后(render调用后)才被调用。
```
总结:

1. 对象式的setState是函数式的setState的简写方式(语法糖)
2. 使用原则：
   1. 如果新状态不依赖于原状态 ===> 使用对象方式
   2. 如果新状态依赖于原状态 ===> 使用函数方式
   3. 如果需要在setState()执行后获取最新的状态数据, 要在第二个callback函数中读取

### useEffect

```
(1). Effect Hook 可以让你在函数组件中执行副作用操作(用于模拟类组件中的生命周期钩子)
(2). React中的副作用操作:
        发ajax请求数据获取
        设置订阅 / 启动定时器
        手动更改真实DOM
(3). 语法和说明: 
        useEffect(() => { 
          // 在此可以执行任何带副作用操作
          return () => { // 在组件卸载前执行
            // 在此做一些收尾工作, 比如清除定时器/取消订阅等
          }
        }, [stateValue]) // 如果指定的是[], 回调函数只会在第一次render()后执行
    
(4). 可以把 useEffect Hook 看做如下三个函数的组合
        componentDidMount()
        componentDidUpdate()
    	  componentWillUnmount() 
```

```
useEffect(callback,[stateValue])
```

- 只传callback时 useEffect相当于 componentDidMount()+componentDidUpdate()
- 传callback+空数组时 useEffect相当于 componentDidMount()
- 传callback+[stateValue] useEffect相当于 componentDidMount() 和stateValue更新时调用
- 当callback返回一个函数时，这个函数相当于 componentWillUnmount() 

### useRef

```
(1). Ref Hook可以在函数组件中存储/查找组件内的标签或任意其它数据
(2). 语法: const refContainer = useRef()
(3). 作用:保存标签对象,功能与React.createRef()一样
```

### useSelector+useDispatch

1. 首先建立一个store

   ```jsx
   import { createStore } from "redux";
   const counterReducer = (state = { counter: 0 }, action) => {
     if (action.type === "increment") {
       return { counter: state.counter + 1 };
     }
     if (action.type === "decrement") {
       return { counter: state.counter - 1 };
     }
     return state;
   };
   const store = createStore(counterReducer);
   export default store;
   
   ```

2. 在使用store的组件里调用这两个钩子(useSelector 用来获取组件需要的state 数据。useDispatch 用来获取 dispatch 函数。)

   ```jsx
   import { useSelector, useDispatch } from "react-redux";
   export default funtion Counter(){
       const dispatch = useDispatch();
       const counter = useSelector((state) => state.counter);
   
       const incrementHandler = () => {
           dispatch({ type: "increment" });
       };
       const decrementHandler = () => {
           dispatch({ type: "decrement" });
       };
   
       const toggleCounterHandler = () => {};
       
       return (
       	<main className={classes.counter}>
             <h1>Redux Counter</h1>
             <div className={classes.value}>{counter}</div>
             <div>
               <button onClick={incrementHandler}>Increment</button>
               <button onClick={decrementHandler}>Decrement</button>
             </div>
             <button onClick={toggleCounterHandler}>Toggle Counter</button>
           </main>
       )
   }
   ```


### useContext

useContext的作用：对它所包含的组件树提供**全局共享数据**的一种技术。可以帮助我们跨越组件层级直接传递变量（向后代传递数据），实现数据共享。

```jsx
import React, { useState, createContext, useContext } from "react";
//创建context
const CountContext = createContext(0);

const Example = () => {
  const [count, setCount] = useState<number>(0);
  return (
    <div>
      <p>父组件点击数量：{count}</p>
      <button onClick={() => setCount(count + 1)}>{"点击+1"}</button>
      {/*使用CountContext.Provider包裹需要接收参数的子组件，并通过value传值*/}
      <CountContext.Provider value={count}>
        <Counter />
      </CountContext.Provider>
    </div>
  );
};

const Counter = () => {
  //通过useContext把刚刚创建好的CountContext作为参数传进去，并读取count值
  const count = useContext(CountContext);
  return <p>子组件获得的点击数量：{count}</p>;
};

export default Example;
```





