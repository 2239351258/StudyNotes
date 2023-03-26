`instanceof`是JavaScript中的一个操作符，用于判断一个对象是否为某个构造函数的实例。它的语法是 `obj instanceof constructor`，其中 `obj` 是要检查的对象，`constructor` 是构造函数。

`instanceof` 的工作原理是检查 `obj` 的原型链是否包含 `constructor` 的原型。如果包含，返回 `true`，否则返回 `false`。通常，`constructor` 可以是任何函数，包括内置函数（如 `Array` 和 `Object`）和自定义函数。

```js
function myInstanceof(left = null, right) {
  // 对于左侧参数如果是非对象直接返回false
  if (Object(left) !== left) return false
  // 对于右侧参数可以认为只能为构造函数且不能没有Prototype属性
  if (typeof right !== 'function' || !right.prototype) throw new TypeError('Right-hand side of ' instanceof ' is not an object')
  // 声明一个变量获取对象的__proto__
  let link = left.__proto__
  // 做循环（当link最终指向null，如果指向null的情况下都找不到那就返回false）
  while (link !== null) {
    // 如果找到说明R.prototype在L的原型链上，即返回true
    if (link === right.prototype) return true
    // 继续向原型链上查找
    link = link.__proto__
  }
  return false
}

!(() => {
  function Person() { };
  var p = new Person();
  console.log(p)
  console.log(myInstanceof(p, Person));
})()
```

