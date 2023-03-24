### 注释版

```js
class MyPromise{
  // Promise 三种状态
  static PENDING='pending'
  static FULFILLED='fulfilled'
  static REJECTED = 'rejected'
  
  constructor(func) {
    // 初始化promise的状态为pending
    this.promiseStatus = MyPromise.PENDING
    this.promiseResult = undefined
    /**
     * 根据规范2.2.2&2.2.3&2.2.6
     * onFulfilled 和 onRejected 必须在状态变更后再调用,
     * onFulfilled 参数为 promise 的值，onRejected 参数也为 promise 的值。
     * onFulfilled 和 onRejected 不能多次被调用
     * 需要一个队列保存所有的 onFulfilled 和 onRejected，
     * 在 promise 状态真正改变之后，再根据先入先出的顺序进行调用
     */
    this.onFulfilleds = []
    this.onRejecteds = []
    try {
      /**
       * 我们把 this.resolve 和 this.reject 传入 func 中后直接执行，
       * 当 resolve 被执行时，那个时候的 this 会是 undefined。
       * 所以我们需要对传入的 resolve 和 reject 方法做一个绑定。
       */
      func(this.resolve.bind(this), this.reject.bind(this))
    }
    catch(err) {
      this.reject(err)
    }
  }
  resolve(res) {
    // promise状态未改变，则改变状态并保存调用时传入的值
    if (this.promiseStatus === MyPromise.PENDING) {
      this.promiseStatus = MyPromise.FULFILLED
      this.promiseResult = res
      while (this.onFulfilleds.length) {
        this.onFulfilleds.shift()()
      }
    }
  }
  reject(rej) {
    // promise状态未改变，则改变状态并保存调用时传入的值
    if (this.promiseStatus === MyPromise.PENDING) {
      this.promiseStatus = MyPromise.REJECTED
      this.promiseResult = rej
      while (this.onRejecteds.length) {
        this.onRejecteds.shift()()
      }
    }
  }
  then(onFulfilled,onRejected) {
    /**
     * 根据规范2.2.1和2.2.5
     * 当 onFulfilled 或者 onRejected 不是函数的时候，必须被忽略，
     * 同时又 onFulfilled 和 onRejected 一定被作为函数调用，
     * 所以我们遇到非函数的类型的 onFulfilled 时，直接返回 promiseResult，
     * 遇到非函数的类型的 onRejected 时，直接抛出一个异常，异常信息为 result
     */
    onFulfilled = typeof onFulfilled === 'function' ? onFulfilled : promiseResult => promiseResult;
    onRejected = typeof onRejected === 'function' ? onRejected : result => { throw result };
    /**
     * 根据规范2.2.4
     * onFulfilled 和 onRejected 需要在 then 方法被调用的那轮事件循环的末尾执行。
     * 为了达到这个效果，onFulfilled 和 onRejected 可以使用宏任务实现，也可以用微任务实现。
     * 用微任务来实现 onFulfilled 和 onRejected 的异步调用。
     * 在状态为 PENDING 的时候，把函数放入待执行队列的 onFulfilled 和 onRejected也需要加上异步处理。
     */
    /**
     * 根据规范2.2.7
     * then 必须返回一个promise。
     * 对onFulfilled和onRejected进行异常捕获，在有错误的时候抛出异常，并作为 then_promise 的 reject 参数。
     */
    const then_promise = new MyPromise((resolve, reject) => {
      const fulfilledcallback = () => {
        queueMicrotask(() => {
          try {
            let x = onFulfilled(this.promiseResult)
            this.resolvePromise(then_promise, x, resolve, reject)
          } catch (e) {
            reject(e)
          }
        });
      }
      const rejectedcallback = () => {
        queueMicrotask(() => {
          try {
            let x = onRejected(this.promiseResult)
            this.resolvePromise(then_promise, x, resolve, reject)
          } catch (e) {
            reject(e)
          }
        });
      }
      
      if (this.promiseStatus === MyPromise.FULFILLED) {
        fulfilledcallback()
      }
      else if (this.promiseStatus === MyPromise.REJECTED) {
        rejectedcallback()
      }
      else {
        this.onFulfilleds.push(fulfilledcallback)
        this.onRejecteds.push(rejectedcallback)
      }
    })
    return then_promise
  }
  resolvePromise(then_promise, x, resolve, reject) {
    /**
     * 规范2.3.1
     * 如果 promise 和 x 引用同一个对象，用一个 TypeError 作为原因来拒绝 promise
     * 即循环引用的情况（x 是和 then_promise 是同一个对象）
     */
    if (then_promise === x) {
      return reject(new TypeError('不要循环引用'))
    }
    // x(状态改变时返回值)为promise，则采用x的状态
    else if (x instanceof MyPromise) {
      x.then((y) => {
        this.resolvePromise(then_promise, y, resolve, reject)
      }, r => reject(r))
    }
    // x为对象
    else if (x !== null && typeof x === 'object' || typeof x === 'function') {
      // 判断 resolvePromise 和 reject 是否被调用，如果已经被调用，需要以第一次调用为准，后续的调用要被忽略
      let hascalled = false
      try {
        const then = x.then
        if (typeof then === 'function') {
          then.call(x,
            y => {
              if (hascalled) return
              hascalled = true
              this.resolvePromise(then_promise,y,resolve,reject)
            },
            r => {
              if (hascalled) return
              hascalled = true
              reject(r)
            }
          )
        }
        else {
          // then 如果不是一个方法，用 res 解决 then_promise。
          if (hascalled) return
          hascalled = true
          resolve(x)
        }
      }
      catch (err) {
        if (hascalled) return
        hascalled = true
        return reject(err)
      }
    }

    else {
      return resolve(x)
    }
  }
}
// promiseA+ 测试
MyPromise.deferred = function () {
  let result = {};
  result.promise = new MyPromise((resolve, reject) => {
    result.resolve = resolve;
    result.reject = reject;
  });
  return result;
}

module.exports = MyPromise;
```

### 无注释版

```js
class MyPromise{
  static PENDING='pending'
  static FULFILLED='fulfilled'
  static REJECTED = 'rejected'
  
  constructor(func) {

    this.promiseStatus = MyPromise.PENDING
    this.promiseResult = undefined

    this.onFulfilleds = []
    this.onRejecteds = []
    try {
      func(this.resolve.bind(this), this.reject.bind(this))
    }
    catch(err) {
      this.reject(err)
    }
  }
  resolve(res) {
    if (this.promiseStatus === MyPromise.PENDING) {
      this.promiseStatus = MyPromise.FULFILLED
      this.promiseResult = res
      while (this.onFulfilleds.length) {
        this.onFulfilleds.shift()()
      }
    }
  }
  reject(rej) {
    if (this.promiseStatus === MyPromise.PENDING) {
      this.promiseStatus = MyPromise.REJECTED
      this.promiseResult = rej
      while (this.onRejecteds.length) {
        this.onRejecteds.shift()()
      }
    }
  }
  then(onFulfilled,onRejected) {
    onFulfilled = typeof onFulfilled === 'function' ? onFulfilled : promiseResult => promiseResult;
    onRejected = typeof onRejected === 'function' ? onRejected : result => { throw result };
    const then_promise = new MyPromise((resolve, reject) => {
      const fulfilledcallback = () => {
        queueMicrotask(() => {
          try {
            let x = onFulfilled(this.promiseResult)
            this.resolvePromise(then_promise, x, resolve, reject)
          } catch (e) {
            reject(e)
          }
        });
      }
      const rejectedcallback = () => {
        queueMicrotask(() => {
          try {
            let x = onRejected(this.promiseResult)
            this.resolvePromise(then_promise, x, resolve, reject)
          } catch (e) {
            reject(e)
          }
        });
      }
      if (this.promiseStatus === MyPromise.FULFILLED) {
        fulfilledcallback()
      }
      else if (this.promiseStatus === MyPromise.REJECTED) {
        rejectedcallback()
      }
      else {
        this.onFulfilleds.push(fulfilledcallback)
        this.onRejecteds.push(rejectedcallback)
      }
    })
    return then_promise
  }
  resolvePromise(then_promise, x, resolve, reject) {
    if (then_promise === x) {
      return reject(new TypeError('不要循环引用'))
    }
    else if (x instanceof MyPromise) {
      x.then((y) => {
        this.resolvePromise(then_promise, y, resolve, reject)
      }, r => reject(r))
    }
    else if (x !== null && typeof x === 'object' || typeof x === 'function') {
      let hascalled = false
      try {
        const then = x.then
        if (typeof then === 'function') {
          then.call(x,
            y => {
              if (hascalled) return
              hascalled = true
              this.resolvePromise(then_promise,y,resolve,reject)
            },
            r => {
              if (hascalled) return
              hascalled = true
              reject(r)
            }
          )
        }
        else {
          if (hascalled) return
          hascalled = true
          resolve(x)
        }
      }
      catch (err) {
        if (hascalled) return
        hascalled = true
        return reject(err)
      }
    }
    else {
      return resolve(x)
    }
  }
}
// promiseA+ 测试
MyPromise.deferred = function () {
  let result = {};
  result.promise = new MyPromise((resolve, reject) => {
    result.resolve = resolve;
    result.reject = reject;
  });
  return result;
}

module.exports = MyPromise;
```