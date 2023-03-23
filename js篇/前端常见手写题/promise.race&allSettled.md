### promise.race

```js
/**
 * 返回的Promise与第一个有结果的一致
 * @param {iterator} proms
 */
Promise._race = function (proms) {
  if (!(typeof proms === 'object' && proms !== null && typeof proms[Symbol.iterator] === 'function')) {
    throw new TypeError(`${proms} is not iterator`)
  }
  proms=[...proms]
  return new Promise((resolve, reject) => {
    for (const prom of proms) {
      Promise.resolve(prom).then(resolve,reject)
    }
  })
}

!(() => {
  let p1 = new Promise(resolve => {
    setTimeout(() => {
      resolve(1)
    }, 100)
  })
  let p2 = new Promise((resolve, reject) => {
    setTimeout(() => {
      reject(2)
    })
  })
  let p3 = new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(3)
    })
  })

  Promise._race([p1, p2, p3]).then(res => {
    console.log(res);
  }).catch(err => {
    console.error(err);
  })
})()
```

### promise.allSettled

```js
/**
 * 等待所有的Promise有结果后
 * 该方法返回的Promise完成
 * 并且按照顺序将所有结果汇总
 * @param {Iterable} proms
 */
Promise._allSettled = function (proms) {
  if (!(typeof proms === 'object' && proms !== null && typeof proms[Symbol.iterator] === 'function')) {
    throw new TypeError(`${proms} is not iterator`)
  }
  // 简单处理一下可迭代对象，如果是Map就会有问题
  proms = [...proms]
  return Promise.all(proms.map(p => Promise.resolve(p).then(
    value => ({ status: 'fulfilled', value }),
    reason => ({ status: 'rejected', reason })
  )))
}

!(() => {
  const promise1 = Promise.resolve(3);
  const promise2 = new Promise((resolve, reject) =>
    setTimeout(reject, 100, "foo")
  );
  const promises = [promise1, promise2];
  Promise._allSettled(promises).then((results) =>
    results.forEach((result) => console.log(result))
  );
})()
```

