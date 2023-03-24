```js
Promise._all = (iterObj) => {
  // 1. iterObj 必须是一个可迭代对象, 否则, 无法正常进行则抛出错误
  if (!(typeof iterObj === "object" && iterObj !== null && typeof iterObj[Symbol.iterator] === "function")) {
    throw new TypeError(`${iterObj} is not iterable`);
  }
  iterObj = [...iterObj];
  /*
   *  2. 函数返回值为 `<Promise>` 对象, 当参数 `iterObj` 内所有的 `Promise` 成功, 
   *     该 `<Promise>` 对象成功, 成功数据为所有成功的 `Promise` 结果数组,
   *     有一个不成功, 则该 `<Promise>` 不成功, 失败数据为失败原因字符串
   */
  return new Promise((resolve, reject) => {
    // 获取可迭代对象的长度
    const len = iterObj.length;
    // 记录以及完成的promise数量
    let count = 0;
    // 长度为0，直接返回成功的promise，数据为空数组
    if (len === 0) return resolve([]);
    // 记录成功的promise
    const res = new Array(len);
    // 遍历每一个对象
    iterObj.forEach(async (item, index) => {
      // 包裹一下对象，处理直接传值的情况
      const newItem = Promise.resolve(item);
      try {
        // 等待promise回调
        const result = await newItem;
        // 记录回调结果
        res[index] = result;
        // 全部完成，返回一个包含全部数据的成功promise
        if (++count === len) {
          resolve(res)
        }
      } catch (err) {
        // 捕获到一个错误，直接返回一个失败的promise
        reject(err);
      }
    })
  })
}

// 验证:
function test() {
  console.time('promise.all');
  Promise._all([
    new Promise((res, rej) => {
      setTimeout(() => {
        res(1)
      },4000)
    }),
    new Promise((res, rej) => {
      setTimeout(() => {
        res(2)
      }, 3000)
    }),
    new Promise((res, rej) => {
      setTimeout(() => {
        rej(3)
      },5000)
    }),
    4,
  ]).then(res => console.timeEnd('promise.all'), rej => console.timeEnd('promise.all'))
  // [1, 2, 3, 4]
}
test();
```

