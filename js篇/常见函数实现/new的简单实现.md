##### new的过程

1. 帮我们创建了一个空对象，例如：obj；
2. 将空对象原型的内存地址`__proto__`指向构造函数的原型对象；
3. 利用函数的apply方法，将原本指向window的绑定对象this指向了obj。（这样一来，向函数中再传递实参时，对象的属性就会被挂载到obj上。）
4. 如果构造函数执行后返回的是对象类型（null除外），则返回该对象，否则返回 obj(空对象)。

```js
const _new = function(constructor, ...args) {
  //创建一个空对象
	//let obj = {}
  //将空对象的隐式原型(__proto__)指向构造函数的显式原型(prototype)
  //obj.__proto__ = constructor.prototype
  //Es6新写法，合并第一二步
  let obj = Object.create(constructor.prototype)
  //将构造函数的this指向空对象，并接收构造函数返回值
  const res = constructor.apply(obj, args)
  //若返回的是对像就直接返回，否则返回空对象
  return res instanceof Object ? res : obj
}
```

