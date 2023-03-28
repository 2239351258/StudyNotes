### 用setTimeout模拟setInterval

```js
let timeWorker = {}
let _setInterval = function (fn, time,...args) {
  // 定义一个key，来标识此定时器
  let key = Symbol();
  // 定义一个递归函数，持续调用定时器
  let execute = function (fn, time) {
    timeWorker[key] = setTimeout(function () {
      fn(...args);
      execute(fn, time);
    }, time)
  }
  execute(fn, time);
  // 返回key
  return key;
}
let _clearInterval = function (key) {
  if (key in timeWorker) {
    clearTimeout(timeWorker[key]);
    delete timeWorker[key];
  }
}

!(() => {
  let timer = _setInterval(() => console.log(1), 1000)
  setTimeout(() => {
    _clearInterval(timer)
  }, 10000)
})()
```

### 用setInterval模拟setTimeout

```js
function _setTimeout(fn, delay, ...args) {
  const timer = setInterval(() => {
    fn.apply(this, args);
    clearInterval(timer);
  },delay)
}
```

