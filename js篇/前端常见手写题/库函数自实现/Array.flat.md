`Array.flat()`方法可以接收一个可选的参数，用于指定需要“扁平化”的嵌套层数。默认情况下，该参数值为1，表示只将一层嵌套的数组“扁平化”，如果需要将多层嵌套的数组“扁平化”，可以将该参数值设置为大于等于2的整数。

```js
/**
 * 数组扁平化
 */

const arr = [1, [2, 3, [4, 5]], 6, 7, [8, 9]]
Array.prototype._flat = function (deep=1) {
  let res = []
  deep--
  for (let item of this) {
    if (Array.isArray(item) && deep >= 0) {
      res=[...res,...item._flat(deep)]
    }
    else {
      res.push(item)
    }
  }
  return res
}

!(() => {
  const _flat = (arr,deep=1) => {
    let res = []
    deep--
    for (let item of arr) {
      if (Array.isArray(item)&&deep>=0) {
        res=[...res,..._flat(item,deep)]
      }
      else {
        res.push(item)
      }
    }
    return res
  }
  /**
   * 如果参数是数组，那么 concat() 方法会将数组中的所有元素添加到新数组中；
   * 如果参数是值，则直接将值添加到新数组中。
   * concat() 方法并不会修改原数组，而是返回一个新的数组。
   * @param {*} arr 原数组
   * @param {*} deep 展开深度
   * @returns 展开后的新数组
   */
  function flatten(arr, deep=1) {
    while (arr.some(item => Array.isArray(item)) && deep) {
      arr = [].concat(...arr);
      deep--;
    }
    return arr;
  }
  console.log(flatten(arr))
  console.log(_flat(arr,Infinity))
  console.log(arr._flat())
})()
```

