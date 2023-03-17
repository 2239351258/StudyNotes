<center><h1>call、apply、bind</h1></center>

### call()

**`call()`** 方法使用一个指定的 `this` 值和单独给出的一个或多个参数来调用一个函数。

#### 语法

```javascript
function.call(thisArg, arg1, arg2, ...)
```

#### 参数

- thisArg
  + 可选的。在 *`function`* 函数运行时使用的 `this` 值。请注意，`this`可能不是该方法看到的实际值：如果这个函数处于非严格模式下，则指定为 `null` 或 `undefined` 时会自动替换为指向全局对象，原始值会被包装。

- arg1,arg2,···
  + 指定的参数列表。

#### 返回值

使用调用者提供的 `this` 值和参数调用该函数的返回值。若该方法没有返回值，则返回 `undefined`。

#### 示例

```javascript
function Product(name, price) {
  this.name = name;
  this.price = price;
}

function Food(name, price) {
  Product.call(this, name, price);
  this.category = 'food';
}

console.log(new Food('cheese', 5).name);
// expected output: "cheese"
```

#### 手撕call

```javascript
Function.prototype.myCall = function(target,...args){
  //传入的参数为null或undefine时，自动替换为指向全局对象
  target = target || window
  //避免对象键名冲突，用Symbol作为键  
  const symbolKey = Symbol()
  //保存被借用的函数
  //将被借用的函数添加到target对象上，target在调用这个函数时，被借用的函数里的this自然就指向了target
  target[symbolKey] = this
  //args本身是rest参数，搭配的变量是一个数组，数组解构后就可以一个个传入函数中
  //执行被借用的函数
  const res = target[symbolKey](...args) 
  //执行完借用的函数后，销毁掉
  delete target[symbolKey]
  //返回借用函数的结果
  return res
}
```

### apply()

`apply()` 方法调用一个具有给定 `this` 值的函数，以及以一个数组（或一个类数组对象）的形式提供的参数。

`apply` 与 `call()` 非常相似，不同之处在于提供参数的方式。`apply` 使用**参数数组**而不是一组**参数列表**。`apply` 可以使用数组字面量（array literal），如 `fun.apply(this, ['eat', 'bananas'])`，或数组对象，如 `fun.apply(this, new Array('eat', 'bananas'))`。

#### 示例

```javascript
const numbers = [5, 6, 2, 3, 7];

const max = Math.max.apply(null, numbers);

console.log(max);
// expected output: 7

const min = Math.min.apply(null, numbers);

console.log(min);
// expected output: 2
```

#### 手撕apply

```javascript
// 区别就是这里第二个参数直接就是个数组
Function.prototype.myApply = function(target,args){ 
  target = target || window
  const symbolKey = Symbol()
  target[symbolKey] = this
  // args本身是个数组，所以我们需要解构后一个个传入函数中
  const res = target[symbolKey](...args)
  // 执行完借用的函数后，销毁掉
  delete target[symbolKey]
  return res
}
```

### bind()

#### 语法

```javascript
function.bind(thisArg[, arg1[, arg2[, ...]]])
```

#### 参数

- thisArg
  + 调用绑定函数时作为 `this` 参数传递给目标函数的值。如果使用`new`运算符构造绑定函数，则忽略该值。当使用 `bind` 在 `setTimeout` 中创建一个函数（作为回调提供）时，作为 `thisArg` 传递的任何原始值都将转换为 `object`。如果 `bind` 函数的参数列表为空，或者`thisArg`是`null`或`undefined`，执行作用域的 `this` 将被视为新函数的 `thisArg`。
- arg1,arg2,···
  - 当目标函数被调用时，被预置入绑定函数的参数列表中的参数。

#### 返回值

返回一个原函数的拷贝，并拥有指定的 **`this`** 值和初始参数。

#### 示例

```javascript
this.x = 9;    // 在浏览器中，this 指向全局的 "window" 对象
var module = {
  x: 81,
  getX: function() { return this.x; }
};

module.getX(); // 81

var retrieveX = module.getX;
retrieveX();
// 返回 9 - 因为函数是在全局作用域中调用的

// 创建一个新函数，把 'this' 绑定到 module 对象
var boundGetX = retrieveX.bind(module);
boundGetX(); // 81
```

#### 手撕bind()

```javascript
Function.prototype.myBind = function (target,...outArgs) {
  // 处理边界条件
  target = target || {}
  const symbolKey = Symbol()
  //将被借用的函数添加到target对象上，target在调用这个函数时，被借用的函数里的this自然就指向了target
  target[symbolKey] = this
  //返回一个改变this指向的函数
  //在调用函数时，也可能传参，将调用bind()传的参合并在一起
  return function (...innerArgs) {
    // outArgs和innerArgs都是一个数组，解构后传入函数
    const res = target[symbolKey](...outArgs, ...innerArgs)
    return res
  } 
}
```

