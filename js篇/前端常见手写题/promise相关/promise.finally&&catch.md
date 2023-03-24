### promise.catch

```js
/**
 * 本质就是then，只是少传了一个onFulfilled
 * 所以仅处理失败的场景
 * @param {*} callback 失败时回调函数
 */
Promise.prototype._catch = function (callback) {
  return this.then(null,callback)
}

!(() => {
  let pro = new Promise((res, rej) => {
    throw 456
    rej(123)
  })
  pro._catch((rej)=>{console.log(rej)})
  pro.catch((rej)=>{console.log(rej)})
})()
```

### promise.finally

```js
/**
 * 无论成功还是失败都会执行回调
 * finally 回调的结果将会被忽略，因此不能修改 promise 的值，只能用来进行一些清理操作。
 * @param {*} callback 回调函数
 */
Promise.prototype._finally = function (callback) {
  // this 指向Promise实例自身
  return this.then(async (res) => {
    // finally 回调的结果将会被忽略
    await Promise.resolve(callback())
    // 返回原本的 promise 的结果
    return res
  },
    async (rej) => {
    // 如果该回调函数抛出了一个错误，则该错误会被传递到下一个 catch 方法。
    await Promise.reject(callback())
    throw rej
  })
}

!(() => {
  const pro = new Promise((resolve, reject) => {
    resolve(1);
  });
  const pro2 = pro._finally((d) => {
    console.log("finally", d); // 收不到d参数
    // 本身不改变状态，但是抛出一个错误，数据就会变成它的错误
    // throw 123;
    return 123; //不起作用
  });
  setTimeout(() => {
    console.log(pro2);
  });
})()
```

