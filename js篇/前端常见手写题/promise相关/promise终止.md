### 中断promise链式调用

利用了promise.then()返回的新 promise 的结果状态是由 then()指定的回调函数执行的结果决定。也就是说：

- 如果抛出异常, 新 promise 变为 rejected, reason 为抛出的异常
- 如果返回的是非 promise 的任意值, 新 promise 变为 resolved, value 为返回的值
- 如果返回的是另一个新 promise, 此 promise 的结果就会成为新 promise 的结果

所以我们在`promise.then()`返回了一个`pending（进行中）` 状态的Promise 对象，`promise.then()`返回的新 promise 的结果状态就一直是`pending（进行中）`的，后面的`then`方法也不会执行了。

```js
let p = new Promise((resolve, reject) => {setTimeout(() => { resolve('OK')}, 1000)})
p.then(value => {return new Promise(() => {})})
  .then(value => { console.log(222)})
  .then(value => { console.log(333)})
  .catch(reason => {console.warn(reason)})
```

### 主动取消promise

```js
/**
 * 自定义的可以主动取消的 Promise
 */
function myCoolPromise({ signal }) {
  return new Promise((resolve, reject) => {
    // 如果刚开始 signal 存在并且是终止的状态可以直接抛出异常
    signal?.throwIfAborted();

    // 异步的操作，这里使用 setTimeout 模拟
    setTimeout(() => {
      Math.random() > 0.5 ? resolve('ok') : reject(new Error('not good'));
    }, 1000);

    // 添加 abort 事件监听，一旦 signal 状态改变就将 Promise 的状态改变为 rejected
    signal?.addEventListener('abort', () => reject(signal?.reason));
  });
}

/**
 * 使用自定义可取消的 Promise
 */
const ac = new AbortController();
const { signal } = ac;

myCoolPromise({ signal }).then((res) => console.log(res), err => {
  console.warn(err)
  return 111
})
  .then(res=>console.log(res))
  .catch(err=>console.log(err))

setTimeout(() => {
  ac.abort();
}, 100);
```

