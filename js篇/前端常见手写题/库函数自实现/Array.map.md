`Array.prototype.map()` 方法创建一个新数组，其结果是该数组中的每个元素都调用一个提供的函数后返回的结果。

该方法的语法如下：

```js
array.map(callback(currentValue[, index[, array]])[, thisArg])
```

其中，`callback` 函数用于处理数组中的每个元素，参数如下：

- `currentValue`：必需。当前元素的值。
- `index`：可选。当前元素的索引。
- `array`：可选。当前正在处理的数组。
- `thisArg`：可选。执行 `callback` 函数时，用于 `this` 的值。

该方法返回一个新的数组，其中每个元素都是回调函数执行的结果。

```js
const arr = [1, 2, 3, 4, 5, 6]
Array.prototype._map = function (cb, thisBinding) {
  // 排除回调非函数情况
  if (typeof cb !== "function") {
    throw new TypeError(`${cb} is not a function`);
  }
  // 排除this为非可迭代对象情况
  if (this == null || typeof this[Symbol.iterator] !== "function") {
    throw new TypeError(`${this} is not a iterable`);
  }
  // 将可迭代对象转换成数组
  const array = [...this];
  const result = [];
  // 执行遍历并回调
  for (let i = 0; i < array.length; i++) {
    result.push(cb.call(thisBinding, array[i], i, this));
  }
  return result;
};

!(() => {
  console.log(arr._map(a=>a**2))
})()
```

