### 1. 组件间通信

#### 1. Props:

 父组件可以通过props将数据传递给子组件。子组件可以通过props获取父组件传递过来的数据。这是React中最常用的一种组件间通信方式。

```tsx
function ParentComponent() {
  const name = "Tom";
  return <ChildComponent name={name} />;
}

function ChildComponent(props) {
  return <div>Hello {props.name}</div>;
}
```

#### 2. Context:

 Context提供了一种跨层级组件传递数据的方式。使用Context可以避免通过props一层层传递数据的麻烦。

```jsx
const ThemeContext = React.createContext('light');

function App() {
  return (
    <ThemeContext.Provider value="dark">
      <Toolbar />
    </ThemeContext.Provider>
  );
}

function Toolbar() {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

function ThemedButton() {
  const theme = useContext(ThemeContext);
  return <button style={{ background: theme }}>I am styled by theme context!</button>;
}
```

#### 3. 事件总线：

事件总线是一个全局的事件系统，可以让不同组件之间通过**订阅和发布**事件来进行通信。

```jsx
import EventEmitter from 'events';

const emitter = new EventEmitter();

function ComponentA() {
  function handleClick() {
    // 触发事件
    emitter.emit('event', 'hello from A');
  }
  return <button onClick={handleClick}>Click me!</button>;
}

function ComponentB() {
  const [message, setMessage] = useState('');
  useEffect(() => {
    // 订阅事件
    emitter.on('event', message => {
      setMessage(message);
    });
    return () => {
      // 销毁事件
      emitter.removeListener('event');
    };
  }, []);
  return <div>{message}</div>;
}
```

#### 4. Redux：

Redux是一种状态管理库，可以让不同组件之间共享状态。Redux可以在React应用中使用，它提供了一种中心化的状态管理方案。

```jsx
import { createStore } from 'redux';
import { Provider, connect } from 'react-redux';

// 定义初始状态和修改规则
const initialState = { count: 0 };
function reducer(state = initialState, action) {
  switch (action.type) {
    case 'INCREMENT':
      return { count: state.count + 1 };
    case 'DECREMENT':
      return { count: state.count - 1 };
    default:
      return state;
  }
}

// 创建store
const store = createStore(reducer);

// 连接React组件和Redux store
function Counter(props) {
  return (
    <div>
      <div>Count: {props.count}</div>
      <button onClick={props.increment}>+</button>
      <button onClick={props.decrement}>-</button>
    </div>
  );
}
const mapStateToProps = state => ({ count: state.count });
const mapDispatchToProps = { increment: () => ({ type: 'INCREMENT' }), decrement: () => ({ type: 'DECREMENT' }) };
const ConnectedCounter = connect(mapStateToProps, mapDispatchToProps)(Counter);

// 渲染组件
/*
渲染组件时，需要将<Provider>组件包裹在应用程序的最外层，以便将Redux store传递给所有的组件。然后，可以使用<ConnectedCounter>组件渲染<Counter>组件，这样<Counter>组件就可以访问Redux store中的状态并通过派发Action来更新状态。
*/
function App() {
  return (
    <Provider store={store}>
      <ConnectedCounter />
    </Provider>
  );
}
```

在上述代码中，通过`connect`函数将`<Counter>`组件连接到`Redux store`。`mapStateToProps`函数将`Redux store`中的状态映射到`<Counter>`组件的`props`中，`mapDispatchToProps`函数将`<Counter>`组件的事件处理程序映射到`Action`中。`connect`函数返回一个新的组件，这个新的组件会将`mapStateToProps`和`mapDispatchToProps`的结果作为`props`传递给`<Counter>`组件。

最后，使用`<ConnectedCounter>`组件渲染`<Counter>`组件，这样`<Counter>`组件就可以访问`Redux store`中的状态并通过派发`Action`来更新状态。

#### 5. Ref:

Ref是React提供的一种机制，可以在父组件中通过Refs获取子组件的实例，从而实现父组件向子组件传递数据或调用子组件的方法

```jsx
import React, { useRef } from 'react';

function ChildComponent(props, ref) {
  const handleClick = () => {
    props.onClick();
  };

  return <button ref={ref} onClick={handleClick}>Click me!</button>;
}

ChildComponent = React.forwardRef(ChildComponent);

function ParentComponent() {
  const buttonRef = useRef(null);

  const handleClick = () => {
    console.log('Button clicked!');
  };

  const handleButtonClick = () => {
    buttonRef.current.click();
  };

  return (
    <div>
      <ChildComponent ref={buttonRef} onClick={handleClick} />
      <button onClick={handleButtonClick}>Trigger button click!</button>
    </div>
  );
}
```

在上述代码中，`ChildComponent`组件将自己的`button`元素暴露给父组件，父组件可以使用`Ref`获取这个`button`元素，并调用它的`click`方法触发点击事件。

首先，在`ChildComponent`组件中，**使用`forwardRef`函数将组件转换成一个可以接收ref属性的组件**，并将ref传递给内部的`button`元素。

然后，在`ParentComponent`组件中，使用`useRef Hook`创建一个Ref对象，并将它传递给`ChildComponent`组件的ref属性，这样就可以获取到`ChildComponent`组件中的`button`元素。同时，`ParentComponent`组件中也定义了一个`handleButtonClick`函数，它会在触发点击事件时调用`ChildComponent`组件中`button`元素的click方法。

最后，在`ParentComponent`组件中渲染`ChildComponent`组件和一个按钮，当点击“Trigger button click!”按钮时，就会调用`ChildComponent`组件中的`button`元素的`click`方法，并触发点击事件。

总的来说，通过Refs实现组件间通信可以实现非常灵活的组件交互方式，但需要注意Refs的使用时机和作用范围，避免出现不必要的代码复杂性和性能问题。

### 2. 路由懒加载

路由懒加载是一种在React应用中优化性能的技术，它可以将应用的代码分割成多个小块，只在需要时加载，从而加速应用的启动和响应速度。在使用路由懒加载时，可以将每个路由组件单独打包成一个文件，并在需要时按需加载。

React Router库支持使用React.lazy和import()函数来实现路由懒加载。具体步骤如下：

1. 定义要进行懒加载的组件，并使用React.lazy函数包装它们：

   ```jsx
   const Home = React.lazy(() => import('./Home'));
   const About = React.lazy(() => import('./About'));
   const Contact = React.lazy(() => import('./Contact'));
   ```

2. 在Route组件中使用懒加载的组件：

   ```jsx
   <Route exact path="/" component={Home} />
   <Route path="/about" component={About} />
   <Route path="/contact" component={Contact} />
   ```

3. 在应用的入口处，使用`Suspense`组件包裹`<Route>`组件，用于在加载懒加载组件时显示一个`loading`状态：

   ```jsx
   function App() {
     return (
       <Router>
         <div>
           <Suspense fallback={<div>Loading...</div>}>
             <Route exact path="/" component={Home} />
             <Route path="/about" component={About} />
             <Route path="/contact" component={Contact} />
           </Suspense>
         </div>
       </Router>
     );
   }
   ```

   在上述代码中，使用`<Suspense>`组件包裹`<Route>`组件，并将一个`loading`状态的组件作为`fallback`属性的值传递进去。当懒加载的组件还未加载完成时，会显示`fallback`属性中定义的`loading`状态，直到组件加载完成后再渲染出来。

