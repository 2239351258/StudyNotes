<center><h1>ES6学习随笔</h1></center>

### 块级作用域

```javascript
// 浏览器的 ES6 环境
function f() { console.log('I am outside!'); }

(function () {
  if (false) {
    // 重复声明一次函数f
    function f() { console.log('I am inside!'); }
  }

  f();
}());
// Uncaught TypeError: f is not a function
```

上面在 ES6 浏览器中，都会报错。因为如果改变了块级作用域内声明的函数的处理规则，显然会对老代码产生很大影响。为了减轻因此产生的不兼容问题，ES6 在[附录 B](https://www.ecma-international.org/ecma-262/6.0/index.html#sec-block-level-function-declarations-web-legacy-compatibility-semantics)里面规定，浏览器的实现可以不遵守上面的规定，有自己的[行为方式](https://stackoverflow.com/questions/31419897/what-are-the-precise-semantics-of-block-level-functions-in-es6)。

- 允许在块级作用域内声明函数。
- 函数声明类似于`var`，即会提升到全局作用域或函数作用域的头部。
- 同时，函数声明还会提升到所在的块级作用域的头部。

根据这三条规则，浏览器的 ES6 环境中，块级作用域内声明的函数，行为类似于`var`声明的变量。上面的例子实际运行的代码如下。

```javascript
// 浏览器的 ES6 环境
function f() { console.log('I am outside!'); }
(function () {
  var f = undefined;
  if (false) {
    function f() { console.log('I am inside!'); }
  }

  f();
}());
// Uncaught TypeError: f is not a function
```

#### 案例

```javascript
console.log(window.a,a) //undefined,a is not defined
{
    console.log(window.a,a) //undefined,a is not defined
    a=3
    console.log(window.a,a) //3.3
}
console.log(window.a,a) //3,3
```

```
块级作用域中默认声明的变量:
	1、全局变量提升 
	2、执行到声明语句时将块级值映射到全局
```



```javascript
console.log(window.a,a) //undefined undefined
{
    console.log(window.a,a) //undefined function a(){}
    a = 3;
    console.log(window.a,a) //undefined 3
    function a(){}
    console.log(window.a,a) //3 3
}
console.log(window.a,a) //3 3
```

执行a = 3，我们知道，此时，在块级作用域中函数声明已经被提升到顶层，那么此时执行a = 3，就是相当于将函数声明a赋值为3。然后，执行到函数声明语句，块级内函数只有执行到声明的时候才会提升到全局去，此时，虽然这一行代码是函数声明语句，但是a，已经为数字3了，所以，执行function  a(){}之后，a的值3就会被赋值给全局作用域上的a，所以下面打印的window.a,a都为3。块级作用域函数只有执行函数声明语句的时候，才会重写对应的全局作用域上的同名变量。

```
块级内申明函数有以下特点：
	1、块级变量提升 
	2、全局变量提升 
	3、执行到声明语句时将块级值映射到全局
```

### 箭头函数

#### 箭头函数使用注意点

+ 箭头函数没有自己的`this`对象
  + 由于箭头函数没有自己的`this`，所以当然也就不能用`call()`、`apply()`、`bind()`这些方法去改变`this`的指向。
+ 不可以当作构造函数，也就是说，不可以对箭头函数使用`new`命令，否则会抛出一个错误。
+ 不可以使用`arguments`对象，该对象在函数体内不存在。如果要用，可以用```rest```(...扩展运算符)参数代替。
+ 不可以使用`yield`命令，因此箭头函数不能用作 Generator 函数。
+ `arguments`、`super`、`new.target`在箭头函数之中也是不存在的，指向外层函数的对应变量。

对于普通函数来说，内部的this指向函数**运行时**所在的对象，但是这一点对箭头函数不成立。它没有自己的this对象，内部的this就是**定义时**上层作用域中的this。也就是说，箭头函数内部的this指向是固定的，相比之下，普通函数的this指向是可变的。

### Set和Map

#### Map

- Map 的键如果是对象，则键是与**内存地址**绑定的，只要内存地址不一样，就视为两个键。
- Map 的键是一个简单类型的值（数字、字符串、布尔值），则只要两个值严格相等，Map 将其视为一个键

#### WeakSet

- WeakSet 的成员只能是对象，而不能是其他类型的值。
- WeakSet 中的对象都是弱引用，即垃圾回收机制不考虑 WeakSet 对该对象的引用，也就是说，如果其他对象都不再引用该对象，那么垃圾回收机制会自动回收该对象所占用的内存，不考虑该对象还存在于 WeakSet 之中。
- WeakSet 适合临时存放一组对象，以及存放跟对象绑定的信息。只要这些对象在外部消失，它在 WeakSet 里面的引用就会自动消失。
- ES6 规定 WeakSet 不可遍历，也没有size属性。

#### WeakMap

- `WeakMap`只接受对象作为键名（`null`除外），不接受其他类型的值作为键名。
- `WeakMap`的键名所指向的对象，不计入垃圾回收机制。
  + 它的键名所引用的对象都是弱引用，即垃圾回收机制不将该引用考虑在内。因此，只要所引用的对象的其他引用都被清除，垃圾回收机制就会释放该对象所占用的内存。也就是说，一旦不再需要，WeakMap 里面的键名对象和所对应的键值对会自动消失，不用手动删除引用。
- WeakMap 弱引用的**只是键名**，而不是键值。键值依然是正常引用。

### Proxy

#### Proxy 支持的13种拦截操作

- **get(target, propKey, receiver)**：拦截对象属性的读取，比如`proxy.foo`和`proxy['foo']`。
- **set(target, propKey, value, receiver)**：拦截对象属性的设置，比如`proxy.foo = v`或`proxy['foo'] = v`，返回一个布尔值。
- **has(target, propKey)**：拦截`propKey in proxy`的操作（不判断一个属性是对象自身的属性，还是继承的属性。），返回一个布尔值。
- **deleteProperty(target, propKey)**：拦截`delete proxy[propKey]`的操作，返回一个布尔值。
- **ownKeys(target)**：拦截`Object.getOwnPropertyNames(proxy)`、`Object.getOwnPropertySymbols(proxy)`、`Object.keys(proxy)`、`for...in`循环，返回一个数组。该方法返回目标对象所有自身的属性的属性名，而`Object.keys()`的返回结果仅包括目标对象自身的可遍历属性。
- **getOwnPropertyDescriptor(target, propKey)**：拦截`Object.getOwnPropertyDescriptor(proxy, propKey)`，返回属性的描述对象。
- **defineProperty(target, propKey, propDesc)**：拦截`Object.defineProperty(proxy, propKey, propDesc）`、`Object.defineProperties(proxy, propDescs)`，返回一个布尔值。
- **preventExtensions(target)**：拦截`Object.preventExtensions(proxy)`，返回一个布尔值。
- **getPrototypeOf(target)**：拦截`Object.getPrototypeOf(proxy)`，返回一个对象。
- **isExtensible(target)**：拦截`Object.isExtensible(proxy)`，返回一个布尔值。
- **setPrototypeOf(target, proto)**：拦截`Object.setPrototypeOf(proxy, proto)`，返回一个布尔值。如果目标对象是函数，那么还有两种额外操作可以拦截。
- **apply(target, object, args)**：拦截 Proxy 实例作为函数调用的操作，比如`proxy(...args)`、`proxy.call(object, ...args)`、`proxy.apply(...)`。
- **construct(target, args)**：拦截 Proxy 实例作为构造函数调用的操作，比如`new proxy(...args)`。

### Generator 函数

- `yield`表达式本身没有返回值，或者说总是返回`undefined`。`next`方法可以带一个参数，该参数就会被当作上一个`yield`表达式的返回值。

  + 由于`next`方法的参数表示上一个`yield`表达式的返回值，所以在第一次使用`next`方法时，传递参数是无效的。V8 引擎直接忽略第一次使用`next`方法时的参数，只有从第二次使用`next`方法开始，参数才是有效的。

- Generator.prototype.throw()方法抛出的错误要被内部捕获，前提是必须至少执行过一次`next`方法。

  - 因为第一次执行`next`方法，等同于启动执行 Generator 函数的内部代码，否则 Generator 函数还没有开始执行，这时`throw`方法抛错只可能抛出在函数外部。\

  - `throw`方法被捕获以后，会附带执行下一条`yield`表达式。也就是说，会附带执行一次`next`方法。

- Generator.prototype.return()返回给定的值，并且终结遍历 Generator 函数。

  - 如果`return()`方法调用时，不提供参数，则返回值的`value`属性为`undefined`。
  - 如果 Generator 函数内部有`try...finally`代码块，且正在执行`try`代码块，那么`return()`方法会导致立刻进入`finally`代码块，执行完以后，整个函数才会结束。

- 如果在A Generator 函数内部，调用另一个B Generator 函数。需要在A的函数体内部，自己手动完成B的遍历。

  - ES6 提供了`yield*`表达式，作为解决办法，用来在一个 Generator 函数里面执行另一个 Generator 函数。
  - `yield*`后面的 Generator 函数（没有`return`语句时），等同于在 Generator 函数内部，部署一个`for...of`循环。
  - 在有`return`语句时，则需要用`var value = yield* iterator`的形式获取`return`语句的值。
  - 任何数据结构只要有 Iterator 接口，就可以被`yield*`遍历。

### Class关键字

##### 静态方法

类相当于实例的原型，所有在类中定义的方法，都会被实例继承。如果在一个方法前，加上`static`关键字，就表示该方法**不会被实例继承**，而是直接通过类来调用，这就称为“静态方法”。

- 如果静态方法包含`this`关键字，这个`this`指的是类，而不是实例。
- 静态方法可以与非静态方法重名
- 父类的静态方法，可以**被子类继承**。
- 静态方法也可以从`super`对象上调用的。

##### 静态属性

静态属性指的是 Class 本身的属性，即`Class.propName`，而不是定义在实例对象（`this`）上的属性。

- 在实例属性的前面，加上`static`关键字。
- 不能再构造函数中初始化，只能定义时初始化或者再静态块中初始化

##### 私有属性/方法

[ES2022](https://github.com/tc39/proposal-class-fields)正式为`class`添加了私有属性，方法是在属性名之前使用`#`表示。

- 私有属性不限于从`this`引用，只要是在类的内部，实例也可以引用私有属性。

- 私有属性或方法只能在类内部使用，无法被实例对象访问或子类继承。

- 不管在类的内部或外部，读取一个不存在的私有属性，也都会报错。这跟公开属性的行为完全不同，如果读取一个不存在的公开属性，不会报错，只会返回`undefined`。
- 私有属性的属性名必须包括`#`，如果不带`#`，会被当作另一个属性。

在方法前加`#`，则变成私有方法

私有属性和私有方法前面，也可以加上`static`关键字，表示这是一个静态的私有属性或私有方法。

##### in运算符

```javascript
class C {
  #brand;

  static isC(obj) {
    if (#brand in obj) {
      // 私有属性 #brand 存在
      return true;
    } else {
      // 私有属性 #foo 不存在
      return false;
    }
  }
}
```

`in`运算符判断某个对象是否有私有属性`#foo`。它不会报错，而是返回一个布尔值。

- 判断私有属性时，`in`只能用在类的内部。
- 子类从父类继承的私有属性，也可以使用`in`运算符来判断。
- `in`运算符对于`Object.create()`、`Object.setPrototypeOf`形成的继承，是无效的，因为这种继承不会传递私有属性。
  + 对于修改原型链形成的继承，子类都取不到父类的私有属性，所以`in`运算符无效。

##### 静态块

允许在类的内部设置一个代码块，在类生成时运行且只运行一次，主要作用是对静态属性进行初始化。以后，新建类的实例时，这个块就不运行了。

```javascript
class C {
  static x = ...;
  static y;
  static z;

  static {
    try {
      const obj = doSomethingWith(this.x);
      this.y = obj.y;
      this.z = obj.z;
    }
    catch {
      this.y = ...;
      this.z = ...;
    }
  }
}
```

类的内部有一个 `static` 代码块，这就是静态块。它的好处是将静态属性`y`和`z`的初始化逻辑，写入了类的内部，而且只运行一次。

- 每个类允许有多个静态块，每个静态块中只能访问之前声明的静态属性。另外，静态块的内部不能有`return`语句。
- 静态块内部可以使用类名或`this`，指代当前类。

除了静态属性的初始化，静态块还有一个作用，就是将私有属性与类的外部代码分享。

```javascript
let getX;

export class C {
  #x = 1;
  static {
    getX = obj => obj.#x;
  }
}

console.log(getX(new C())); // 1
```

这样写的好处：`#x`是类的私有属性，如果类外部的`getX()`方法希望获取这个属性，以前是要写在类的`constructor()`方法里面，这样的话，每次新建实例都会定义一次`getX()`方法。现在可以写在静态块里面，这样的话，只在类生成时定义一次。

### Class继承

ES5 的继承机制，是先创造一个独立的子类的实例对象，然后再将父类的方法添加到这个对象上面，即“实例在前，继承在后”。ES6 的继承机制，则是先将父类的属性和方法，加到一个空的对象上面，然后再将该对象作为子类的实例，即“继承在前，实例在后”。这就是为什么 ES6 的继承必须**先调用`super()`方法**，因为这一步会生成一个继承父类的`this`对象，没有这一步就**无法继承父类**。

+ `super`虽然代表了父类`A`的构造函数，但是返回的是子类`B`的实例，即`super`内部的`this`指的是`B`的实例，因此`super()`在这里相当于`A.prototype.constructor.call(this)`。

- 子类无法继承父类的私有属性，私有属性只能在定义它的 class 里面使用。
- 父类的静态属性和静态方法，会被子类继承。
  - 静态属性是通过**浅拷贝**实现继承的。

##### super

`super`这个关键字，既可以当作函数使用，也可以当作对象使用。在这两种情况下，它的用法完全不同。

- `super`作为函数调用时，代表父类的构造函数。ES6 要求，子类的构造函数必须执行一次`super`函数。
- `super`作为对象时，在普通方法中，指向父类的原型对象；在静态方法中，指向父类。
  - 在子类普通方法中通过`super`调用父类的方法时，方法内部的`this`指向当前的子类实例。
  - 在子类的静态方法中通过`super`调用父类的方法时，方法内部的`this`指向当前的子类，而不是子类的实例

### new关键字

##### new的过程

1. 帮我们创建了一个空对象，例如：obj；
2. 将空对象原型的内存地址`__proto__`指向构造函数的原型对象；
3. 利用函数的apply方法，将原本指向window的绑定对象this指向了obj。（这样一来，向函数中再传递实参时，对象的属性就会被挂载到obj上。）
4. 如果构造函数执行后返回的是对象类型（null除外），则返回该对象，否则返回 obj(空对象)。

```javascript
function People(name) { 
	this.name = name; 
  return this; 
} 
let obj = {}; 
obj.__proto__ = People.prototype; //People.call(obj, '孙悟空');
let people = People.apply(obj, ['孙悟空']);
console.log(people);
```

##### new的简单实现

```javascript
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

