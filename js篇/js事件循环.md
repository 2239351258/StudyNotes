<center><h1>JavaScript事件循环</h1></center>

### JavaScript执行上下文

当一段 JavaScript 代码在运行的时候，它实际上是运行在**执行上下文**中。下面 3 种类型的代码会创建一个新的执行上下文：

- 全局上下文是为运行代码主体而创建的执行上下文，它是为那些存在于 JavaScript 函数之外的任何代码而创建的。
- 每个函数会在执行的时候创建自己的执行上下文。这个上下文就是通常说的“本地上下文”。
- 使用 `eval()`函数也会创建一个新的执行上下文。

每一个上下文在本质上都是一种作用域层级。每个代码段开始执行的时候都会创建一个新的上下文来运行它，并且在代码退出的时候销毁掉。

### 事件循环(Event loops)

事件循环负责**收集事件**（包括用户事件以及其他非用户事件等）、对任务进行排队以便在合适的时候执行回调。然后它执行所有处于等待中的 JavaScript 任务（宏任务），然后是微任务，然后在开始下一次循环之前执行一些必要的渲染和绘制操作。

网页或者 app 的代码和浏览器本身的用户界面程序运行在相同的**线程**中，共享相同的 **事件循环**。该线程就是 **主线程**，它除了运行网页本身的代码之外，还负责收集和派发用户和其它事件，以及渲染和绘制网页内容等。

#### 事件循环流程

- 同步任务和异步任务分别进入不同的执行"场所"; 同步任务进入主线程，异步任务进入Event Table并注册回调函数。
- 当指定的事情完成时，Event Table会将这个函数移入任务队列(task quene)，等待主线程的任务执行完毕;
- 当栈中的代码执行完毕，执行栈(call stack)中的任务为空时，就会读取任务队列(task quene)中的任务，去执行对应的回调;
-  如此循环，就形成js的事件循环机制(Event Loop)。

#### 事件表格(Event Table)

一张事件及其对应回调函数的列表

当指定的事件完成(如ajax请求响应返回, setTimeout延迟到指定时间)时，Event Table会将这个回调函数移入Event Queue, 即macro-task(宏任务)队列 或 micro-task(微任务)队列。

#### 宏任务VS微任务

**macro-task（宏任务）:**

- script(整体代码)
- setTimeout
- setInterval
- setImmediate
- I/O (比如Ajax操作从网络读取数据)
- UI render

**micro-task（微任务）:**

- process.nextTick
  + 这个函数其实是独立于事件循环之外的，它有一个自己的队列。当每个阶段完成后，如果存在 nextTick 队列，就会清空队列中的所有回调函数，并且优先于其他 microtask 执行。

- Promise
  + new Promise时传的函数是同步执行，promise.then为微任务
  + Promise.then 也是独立于事件循环之外的，有一个自己的队列，但是优先级要比 process.nextTick 要低，所以当微任务中同时存在 process.nextTick 和 Promise.then 时，会优先执行前者。
- Async/Await(实际就是promise)
- MutationObserver(html5新特性)

#### 任务队列VS微任务队列

- 当执行来自任务队列中的任务时，在每一次新的事件循环开始迭代的时候运行时都会执行队列中的每个任务。在每次迭代开始之后加入到队列中的任务需要**在下一次迭代开始之后才会被执行**.
- **每次当一个任务退出且执行上下文为空**的时候，**微任务队列中的每一个微任务会依次被执行**。不同的是它会等到微任务队列为空才会停止执行——即使中途有微任务加入。换句话说，微任务可以添加新的微任务到队列中，并在下一个任务开始执行之前且当前事件循环结束之前执行完所有的微任务。

#### 浏览器EventLoop执行过程

1. js引擎将所有代码放入执行栈，并依次弹出并执行，这些任务有的是同步有的是异步(宏任务或微任务)。
2. 如果在执行 栈中代码时发现宏任务则交个浏览器相应的线程去处理，浏览器线程在正确的时机(比如定时器最短延迟时间)将宏任务的消息(或称之为回调函数)推入宏任务队列。而宏任务队列中的任务只有执行栈为空时才会执行。
3. 如果执行 栈中的代码时发现微任务则推入微任务队列，和宏任务队列一样，微任务队列的任务也在执行栈为空时才会执行，但是微任务始终比宏任务先执行。
4. 当执行栈为空时，eventLoop转到微任务队列处，依次弹出首个任务放入执行栈并执行，如果在执行的过程中又有微任务产生则推入队列末尾，这样循环直到微任务队列为空。
5. 当执行栈和微任务队列都为空时，eventLoop转到宏任务队列，并取出队首的任务放入执行栈执行。需要注意的是宏任务每次循环只执行一个。
6. 重复1-5过程
7. 直到栈和队列都为空时，代码执行结束。引擎休眠等待直至下次任务出现。

值得注意的是：

- **宏任务每次只取一个，执行之后马上执行微任务。**
- **微任务会依次执行，直到微任务队列为空。**

#### Node与浏览器执行顺序不完全一致

##### node事件循环的6个阶段：

```
timers 阶段：这个阶段执行 timer（ setTimeout、setInterval ）的回调
	timers 阶段会执行 setTimeout 和 setInterval 回调，并且是由 poll 阶段控制的。同样，在 Node.js 中定时器指定的时间也不是准确时间，只能是尽快执行。
	
I/O callbacks 阶段：处理一些上一轮循环中的少数未执行的 I/O 回调

idle、prepare 阶段：仅 Node.js 内部使用

poll 阶段：获取新的 I/O 事件, 适当的条件下 Node.js 将阻塞在这里
	poll 是一个至关重要的阶段，这一阶段中，系统会做两件事情：
	      ● 回到 timers 阶段执行回调
	      ● 执行 I/O 回调
	并且在进入该阶段时如果没有设定了 timer 的话，会发生以下两件事情：
	      ● 如果 poll 队列不为空，会遍历回调队列并同步执行，直到队列为空或者达到系统限制
	      ● 如果 poll 队列为空时，会有两件事发生：
	        ○ 如果有 setImmediate 回调需要执行，poll 阶段会停止并且进入到 check 阶段执行回调
	        ○ 如果没有 setImmediate 回调需要执行，会等待回调被加入到队列中并立即执行回调，这里同样会有个超时时间设置防止一直等待下去
	当然设定了 timer 的话且 poll 队列为空，则会判断是否有 timer 超时，如果有的话会回到 timer 阶段执行回调。
	假设 poll 被堵塞，那么即使 timer 已经到时间了也只能等着，这也是为什么上面说定时器指定的时间并不是准确的时间。
	
check 阶段：执行 setImmediate( ) 的回调
	setImmediate( ) 的回调会被加入 check 队列中，从事件循环的阶段图可以知道，check 阶段的执行顺序在 poll 阶段之后。
	
close callbacks 阶段：执行 socket 的 close 事件回调
```

##### Node EventLoop执行流程

**v11.x 之前：**

1. 执行全局Script的同步代码
2. 执行microtask微任务，先执行所有Next Tick Queue中的所有任务，再执行Other Microtask Queue中的所有任务。
3. 执行macrotask宏任务，共6个阶段，从第一阶段开始，依次执行到第6个阶段，每个阶段执行完，统一执行所有的微任务，再执行下一阶段。
4. 重复1 - 3过程。

**v11.x之后（与浏览器保持一致）：**

Node 11及其之后，针对事件循环的每一个阶段，微任务的执行顺序进行了统一，在每次调用回调之后，就执行相应微任务，不会等到所有回调执行完毕后才执行。

每执行一个宏任务后，就去执行所有的微任务（如果有），再去执行下一个宏任务，如此反复。

#### 案例

```javascript
console.log('start');
setTimeout(function() {
  console.log(2);
  new Promise((resolve) => {
    console.log('promise');
    resolve();
  }).then(() => {
    console.log('promise then');
  })
});
setTimeout(() => {
  console.log(3);
})
console.log('end');

// node v11.x之前: start, end, 2, promise, 3, promise then
// 浏览器js和node v11.x之后: start, end, 2, promise, promise then, 3
```

**nodev11.x之前：**任务进入执行栈，在主线程执行同步代码即打印`start`和`end`，遇到异步任务进入Event Table注册回调函数即将两个`setTimeout`的回调函数入宏任务队，主线程执行完毕。开始执行微任务，微任务队列为空，则开始执行macrotask宏任务，第一阶段执行timers,即两个`setTimeout`,首先执行第一个`setTimeout`,打印`2`,`new Promise`里的函数是同步任务，即打印`promise`,遇到`resolve`将其压入微任务队列，第一个`setTimeout`执行完毕。开始执行第二个`setTimeout`，打印`3`，第二个`setTimeout`执行完毕。至此`timers`阶段结束，开始执行所有微任务，微任务队列里只有`resolve`，即打印`promise then`。至此程序执行完毕。

**浏览器和node v11.x之后：**任务进入执行栈，在主线程执行同步代码即打印`start`和`end`，遇到异步任务进入Event Table注册回调函数即将两个`setTimeout`的回调函数入宏任务队，主线程执行完毕。开始执行微任务，微任务队列为空，则开始执行macrotask宏任务，宏任务队列里有两个`setTimeout`先执行第一个，即打印`2`，`new Promise`里的函数是同步任务，即打印`promise`,遇到`resolve`将其压入微任务队列。第一个宏任务执行完毕，查询微任务队列，微任务队列有任务，则执行所有的微任务，即打印`promise then`。所有微任务执行完毕后，执行第二个宏任务，即打印`3`。宏任务微任务全部执行完，至此程序执行完毕。

### 总结

#### Node v11.x之前的Event Loop

![未命名文件](https://my-learning-data.oss-cn-hongkong.aliyuncs.com/imgs/%E6%9C%AA%E5%91%BD%E5%90%8D%E6%96%87%E4%BB%B6.png)

 #### 浏览器的Event Loop（Node v11.x之后与浏览器保持一致）

![浏览器的eventLoop](https://my-learning-data.oss-cn-hongkong.aliyuncs.com/imgs/%E6%B5%8F%E8%A7%88%E5%99%A8%E7%9A%84eventLoop.png)