### 1. splice,slice,split三者区别

##### splice

splice() 方法通过删除或替换现有元素或者原地添加新的元素来修改数组,并以数组形式返回被修改的内容。(`此方法会改变原数组,获取的是新的数组`)

语法：

```javascript
arrayObject.splice(index,howmany,item1,.....,itemX)
```

> 参数：
>  index —— 必需。整数，规定添加/删除项目的位置，使用负数可从数组结尾处规定位置。
>  howmany ——必需。要删除的项目数量。如果设置为 0，则不会删除项目。
>  item1, …, itemX —— 可选。向数组添加的新项目。

总结：

- 如果splice() 方法中存在一个或两个参数，表示删除操作。如果存在三个参数或三个以上参数的时候要着重记住第二个元素，若第二个元素为0就是添加操作，添加第三个及后面的元素，如果第二个元素不为0则是替换操作，替换第三个及后面的元素。
- splice() 方法可删除从 index 处开始的零个或多个元素，并且用参数列表中声明的一个或多个值来替换那些被删除的元素。
- 如果从 arrayObject 中删除了元素，则返回的是含有被删除的元素的数组。

##### slice

用于截取数组，并返回截取到的新的数组，数组与字符串对象都使用（`此方法对原数组不会改变`）

语法：

```javascript
array.slice(start, end)
```

>参数：
>start:：必需，规定从何处开始选取。如是负数，规定从数组尾部开始算起，也就是说，-1指的是最后一个元素，以此类推；
>end：可选，规定从何处结束选取。该参数是数组片段结束处的数组下标。如果没有指定该参数，那么切分的数组包含从star到数组结束的所有元素。如果这个参数是负数，那么它规定的是从数组尾部开始算起的元素。

返回值：一个新的数组，包含从start 到end（`不包括该元素`）的ArrayObject 中的元素。

##### split

split() 方法用于把一个字符串分割成字符串数组。 (`此方法不改变原始字符串。`)

> 注： 如果把空字符串 ("") 用作 separator，那么 stringObject 中的每个字符之间都会被分割。

语法：

```
string.split(separator,limit)
```

>参数：
> separator ：`可选`。字符串或正则表达式，从该参数指定的地方分割 string Object。
> limit ：可选。该参数可指定返回的数组的最大长度。如果设置了该参数，返回的子串不会多于这个参数指定的数组。如果没有设置该参数，整个字符串都会被分割，不考虑它的长度。

### 2. isNaN与Number.isNaN的区别

函数 isNaN 接收参数后，会先尝试将这个参数**转换为数值**，**任何不能被转换为数值的的值都会返回 true**，因此非数字值传入也会返回 true ，会影响 NaN 的判断。

函数 Number.isNaN 会首先**判断传入参数是否为数字**，如果是数字再继续判断是否为 NaN ，不会进行数据类型的转换，这种方法对于 NaN 的判断更为准确。（只有NaN才返回true）

### 3. 其他值到字符串的转换规则

- Null 和 undefined 类型 ，null 转换为 "null"，undefined 转换为 "undefined"。
- Boolean 类型，true 转换为 "true"，false 转换为 "false"。
- Number 类型的值直接转换，不过那些极小和极大的数字会使用指数形式。
- Symbol 类型的值直接转换，但是只允许显式强制类型转换，使用隐式强制类型转换会产生错误。
- 对普通对象来说，**除非自行定义 toString() 方法**，否则会调用 toString()（Object.prototype.toString()）来返回内部属性 [[Class]] 的值，如"[object Object]"。如果对象有自己的 toString() 方法，字符串化时就会调用该方法并使用其返回值。

### 4. 其他值到数字值的转换规则

- undefined 类型的值转换为 NaN。
- Null 类型的值转换为 0。
- Boolean 类型的值，true 转换为 1，false 转换为 0。
- String 类型的值转换如同使用 Number() 函数进行转换，如果包含非数字值则转换为 NaN，空字符串为 0。
- Symbol 类型的值不能转换为数字，会报错。
- 对象（包括数组）会首先被转换为相应的基本类型值，如果返回的是非数字的基本类型值，则再遵循以上规则将其强制转换为数字。

为了将值转换为相应的基本类型值，抽象操作 ToPrimitive 会首先（通过内部操作 DefaultValue）检查该值是否有valueOf()方法。如果有并且返回基本类型值，就使用该值进行强制类型转换。如果没有就使用 toString() 的返回值（如果存在）来进行强制类型转换。

> 如果 valueOf() 和 toString() 均不返回基本类型值，会产生 TypeError 错误。

### 5. 其他值到布尔类型的值的转换规则

以下这些是假值：

- undefined
- null
- false
- +0、-0 和 NaN
- ""

假值的布尔强制类型转换结果为 false。从逻辑上说，假值列表以外的都应该是真值。

### 6. ||与&&操作符的返回值

- ||返回第一个为true的操作数值
- &&返回第一个为false的操作数值

例如：

```javascript
console.log(0||1||2)		//打印1
console.log(1&&0&&false)		//打印0
```

|| 和 && 返回它们其中一个**操作数的值**，而非条件判断的结果

### 7. Object.is() 与比较操作符 “===”、“==” 的区别

- 使用双等号（==）进行相等判断时，如果两边的类型不一致，则会进行强制类型转换后再进行比较。
- 使用三等号（===）进行相等判断时，如果两边的类型不一致时，不会做强制类型转换，直接返回 false。
- 使用 Object.is 来进行相等判断时，一般情况下和三等号的判断相同，它处理了一些特殊的情况，比如 -0 和 +0 不再相等，两个 NaN 是相等的。

### 8. JavaScript 中的隐式类型转换

首先要介绍`ToPrimitive`方法，这是 JavaScript 中每个值隐含的自带的方法，用来将值 （无论是基本类型值还是对象）转换为基本类型值。如果值为基本类型，则直接返回值本身；如果值为对象，其看起来大概是这样：

```javascript
/**
* @obj 需要转换的对象
* @type 期望的结果类型
*/
ToPrimitive(obj,type)
```

`type`的值为`number`或者`string`。

（1）当`type`为`number`时规则如下：

- 调用`obj`的`valueOf`方法，如果为原始值，则返回，否则下一步；
- 调用`obj`的`toString`方法，后续同上；
- 抛出`TypeError` 异常。

（2）当`type`为`string`时规则如下：

- 调用`obj`的`toString`方法，如果为原始值，则返回，否则下一步；
- 调用`obj`的`valueOf`方法，后续同上；
- 抛出`TypeError` 异常。

可以看出两者的主要区别在于调用`toString`和`valueOf`的先后顺序。默认情况下：

- 如果对象为 `Date` 对象，则`type`默认为`string`；
- 其他情况下，`type`默认为`numbe`r。

总结上面的规则，对于 `Date` 以外的对象，转换为基本类型的大概规则可以概括为一个函数：

```javascript
var objToNumber = value => Number(value.valueOf().toString())
objToNumber([]) === 0
objToNumber({}) === NaN
```

而 JavaScript 中的隐式类型转换主要发生在`+、-、*、/`以及`==、>、<`这些运算符之间。而这些运算符只能操作基本类型值，所以在进行这些运算前的第一步就是将两边的值用`ToPrimitive`转换成基本类型，再进行操作。

以下是基本类型的值在不同操作符的情况下隐式转换的规则 （对于对象，其会被`ToPrimitive`转换成基本类型，所以最终还是要应用基本类型转换规则）

1. **`+`操作符**：`+`操作符的两边有至少一个string类型变量时，两边的变量都会被隐式转换为字符串；其他情况下两边的变量都会被转换为数字。

```javascript
1 + '23' // '123'
1 + false // 1 
1 + Symbol() // Uncaught TypeError: Cannot convert a Symbol value to a number
'1' + false // '1false'
false + true // 1
```

2. **`-、*、\`操作符**：`NaN`也是一个数字

```javascript
1 * '23' // 23
1 * false // 0
1 / 'aa' // NaN
```

3. **`==`操作符：**

```javascript
3 == true // false, 3 转为number为3，true转为number为1
'0' == false //true, '0'转为number为0，false转为number为0
'0' == 0 //true '0'转为number为0
```

4. **对于`<`和`>`比较符：**

如果两边都是字符串，则比较字母表顺序：

```javascript
'ca' < 'bd' // false
'a' < 'b' // true
```

其他情况下，转换为数字再比较：

```javascript
'12' < 13 // true
false > -1 // true
```

以上说的是基本类型的隐式转换，而对象会被ToPrimitive转换为基本类型再进行转换：

```javascript
let a = {}
a > 2 // false
```

其对比过程如下：

```javascript
a.valueOf() // {}, 上面提到过，ToPrimitive默认type为number，所以先valueOf，结果还是个对象，下一步
a.toString() // "[object Object]"，现在是一个字符串了
Number(a.toString()) // NaN，根据上面 < 和 > 操作符的规则，要转换成数字
NaN > 2 //false，得出比较结果
```

> `NaN`与任何值作比较都返回`false`(除Object.is(NaN,NaN)返回`true`外)

又比如：

```javascript
let a = {name:'Jack'}
let b = {age: 18}
a + b // "[object Object][object Object]"
```

运算过程如下：

```javascript
a.valueOf() // {}，上面提到过，ToPrimitive默认type为number，所以先valueOf，结果还是个对象，下一步
a.toString() // "[object Object]"
b.valueOf() // 同理
b.toString() // "[object Object]"
a + b // "[object Object][object Object]"
```

### 9. ` +` 操作符什么时候用于字符串的拼接

根据 ES5 规范，如果某个操作数是字符串或者能够通过以下步骤转换为字符串的话，+ 将进行拼接操作。如果其中一个操作数是对象（包括数组），则首先对其调用 `ToPrimitive` 抽象操作，该抽象操作再调用 [[DefaultValue]]，以数字作为上下文。**如果不能转换为字符串，则会将其转换为数字类型来进行计算**。

简单来说就是，如果 + 的其中一个操作数是字符串（或者通过以上步骤最终得到字符串），则执行字符串拼接，否则执行数字加法。

那么对于除了加法的运算符来说，只要其中一方是数字，那么另一方就会被转为数字。

例如：

```javascript
console.log([1,2]+1)		//"1,21"
//运算过程
[1,2].valueOf() // [1,2]，上面提到过，ToPrimitive(除Date对象)默认type为number，所以先valueOf，结果还是个对象，下一步
[1,2].valueOf().toString()	//"1,2"，此时[1,2]转换为字符串，则进行字符串拼接
console.log("1,2"+1)		//"1,21"
```

### 10. Math.round()注意细节

 如果参数的小数部分大于 0.5，则舍入到下一个绝对值更大的整数; 

 如果参数的小数部分小于 0.5，则舍入到下一个绝对值更小的整数; 

如果参数的小数部分恰好等于0.5，则舍入到下一个在正无穷（+∞）方向上的整数

```js
Math.round(11.5)->12
Math.round(11.6)->12
Math.round(11.4)->11
Math.round(-11.4)->-11
Math.round(-11.6)->-12
Math.round(-11.5)->-11
```

### 11. `==`比较步骤

== 判断是按一下步骤执行的：

1. 如果是NaN和NaN，返回false
2. 如果类型相同，判断内容是否相同
3. 如果是undefined和null，返回true
4. 如果其中一方是undefined或null，返回false
5. 如果是String和Number，String转Number，然后重新到第1步执行
6. 如果其中一方是Boolean，Boolean转Number，然后重新到第1步执行
7. 如果其中一方是对象类型，对象类型转基本类型，然后重新到第1步执行

> 如果一个值是对象，另一个值是数字或字符串，则将对象转换为原始值，再进行比较。

就是除了 null undefined NaN ，其他的都可以粗暴地看成转成数字类型做对比就可以了

https://i.waimai.meituan.com/cfeplay/playcenter/batchgrabred/acceptRed

 https://i.waimai.meituan.com/cfeplay/playcenter/batchgrabred/corepage

### 12. js中的取整方法

在 JavaScript 中，取整的方法有以下几种：

1. `Math.trunc(x)`：去掉一个数字的小数部分，返回整数部分。
2. Math.floor：向下取整，即小于等于该数的最大整数。
3. Math.ceil：向上取整，即大于等于该数的最小整数。
4. Math.round：四舍五入取整。
5. parseInt：将字符串转换为整数，会自动去除字符串前面的空格，并且只返回整数部分。
6. `~~`运算符：双取反运算符，可以快速向下取整。例如：~~3.14 等价于 Math.floor(3.14)，结果为 3。

需要注意的是，以上方法对于负数的处理方式不同，具体如下：

1. Math.floor：对于负数，向下取整后绝对值会变小。
2. Math.ceil：对于负数，向上取整后绝对值会变大。
3. Math.round：对于负数，四舍五入后绝对值会变大或者不变。
4. parseInt：对于负数，会返回其绝对值的整数部分，并将该整数的符号赋值给结果。
5. `~~`运算符：对于负数，双取反运算符会将小数部分去掉，并使绝对值变小，结果为负整数。
