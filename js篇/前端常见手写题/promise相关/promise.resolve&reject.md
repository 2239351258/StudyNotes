### promise.resolve

```js
Promise._resolve = function (data) {
  // 如果传入一个promise则直接返回这个promise
  if (data instanceof Promise) {
    return data
  }
  // 如果传入一个 thenable对象，则包装为promise对象并返回
  if (typeof data?.then === 'function') {
    return new Promise((res, rej) => {
      data.then(res,rej)
    })
  }
  return new Promise((res,rej)=>{res(data)})
}

// 测试
!(function test() {
  // thenable
  Promise.resolve({ then: (resolve) => resolve('xx') })
    .then(res => console.log(res))
  Promise._resolve({ then: (resolve) => resolve('x_x') })
    .then(res => console.log(res))
  // 普通值
  Promise.resolve(1)
    .then(res => console.log(res))
  Promise._resolve(1)
    .then(res => console.log(res))
  // promise对象
  Promise.resolve(new Promise((res,rej)=>{res('promise')}))
    .then(res => console.log(res))
  Promise._resolve(new Promise((res,rej)=>{res('_promise')}))
    .then(res => console.log(res))
})()
```

### promise.reject

```js
/**
 * @param {*} data 
 * @returns 一个被拒绝的 Promise 对象，其理由（即被拒绝的原因）被设置为传入的参数。
 */
Promise._reject = function (data) { 
  return new Promise((res,rej)=>rej(data))
}
// test
!(() => {
  // thenable
  Promise.reject({ then: (resolve,rej) => rej('xx') })
    .catch(res => console.log(res))
  Promise._reject({ then: (resolve,rej) => rej('x_x') })
    .catch(res => console.log(res))
  // 普通值
  Promise.reject(1)
    .catch(res => console.log(res))
  Promise._reject(1)
    .catch(res => console.log(res))
  // promise对象
  Promise.reject(new Promise((res, rej) => { rej('promise') }))
    .then(res=>console.log(res))
    .catch(res => {
      console.error(res)
      res.catch(e => console.info(e))
    })
  Promise._reject(new Promise((res, rej) => { rej('_promise') }))
    .then(res=>console.log(res))
    .catch(res => {
      console.error(res)
      res.catch(e=>console.info(e))
    })
})()
```

