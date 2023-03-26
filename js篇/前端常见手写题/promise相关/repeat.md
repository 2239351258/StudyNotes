### 题意：

```js
function repeat(func, times, wait) {
  // TODO
}
const repeatFunc = repeat(alert, 4, 3000);
// 调用这个 repeatFunc ("hellworld")，会alert4次 helloworld, 每次间隔3秒
```

### 实现

#### setTimeout实现

```js
function repeat(func, times, wait) {
  // TODO
  return function cb(...args){
    setTimeout(() => {
      if(times<=0) return
      func(...args)
      times -= 1
      cb(...args)
    }, wait);
  }
}
const repeatFunc = repeat(console.log, 4, 3000);
repeatFunc("hellworld")
```

#### Promise实现

```js
function once(func,wait,args) {
  return new Promise((res, rej) => {
    setTimeout(() => {
      func.apply(this, args)
      res()
    },wait)
  })
}
function repeat(func, times, wait) {
  return async (...args) => {
    while (times--) {
      await once(func,wait,args)
    }
  }
}
const repeatFunc = repeat(console.log, 4, 3000);
repeatFunc("hellworld")
```

