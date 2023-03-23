学习网站：https://codejiaonang.com/#/course/regex_chapter1/0/0

### 正则表达式常用快捷方式

| 快捷方式       | 描述                                                         |
| -------------- | ------------------------------------------------------------ |
| \w             | 与任意单词字符匹配，任意单词字符表示 `[A-Z]`、 `[a-z]`、`[0-9]`、`_` |
| \d             | 与任意数字匹配                                               |
| \\s            | 匹配空白字符，比如空格，tab、换行等。                        |
| \b             | 匹配的是单词的边界                                           |
| .              | 匹配任何单个字符，它**只能出现在方括号以外**，不能匹配\n(换行符) |
| ?              | 出现0次或1次(将前面的量词设为非贪婪模式)                     |
| {n,m}          | 出现n到m次，{n}出现n次，{n,}出现n次以上                      |
| +等价{1,}      | 出现1次及以上                                                |
| *等价{0,}      | 出现0次及以上                                                |
| *?             | 匹配前一个字符的 0 次或多次，并且匹配尽量少的字符            |
| (正则表达式)   | 捕获分组，用来捕获符合正则表达式的数据                       |
| (?:正则表达式) | 非捕获分组，能使用捕获分组的功能，而**不捕获数据**           |
| \N             | **分组的回溯引用**，使用`\N`可以引用编号为`N`的分组          |
| (?=表达式)     | 正向先行断言：指在某个位置向右看，表示所在位置右侧必须能匹配表达式 |
| (?!=表达式)    | 反向先行断言：作用是保证右边不能出现某字符。                 |
| (?<=表达式)    | 正向后行断言：指在某个位置向左看，表示所在位置左侧必须能匹配表达式 |
| (?<!表达式)    | 反向后行断言：指在某个位置向左看，表示所在位置左侧不能匹配表达式 |

> 快捷方式也可以取反，例如对于`\w`的取反为`\W`，将小写改写成大写即可，其他快捷方式也遵循这个规则。

![image-20230320170308570](https://my-learning-data.oss-cn-hongkong.aliyuncs.com/imgs/image-20230320170308570.png)

> `\bmaster\b` 就仅匹配有边界的`master`单词。

![image-20230320181748791](https://my-learning-data.oss-cn-hongkong.aliyuncs.com/imgs/image-20230320181748791.png)

> 先行断言和后行断言只有一个区别，即**先行断言从左往右看，后行断言从右往左看。**

### 正则表达式修饰符

| 修饰符 | 作用         |
| ------ | ------------ |
| g      | 全局模式     |
| i      | 不区分大小写 |
| m      | 多行模式     |

> 多行模式：即在到达一行文本末尾时还会继续查找下一行中是否存在与模式匹配的项。

### 正则相关的函数

1. `RegExp()`：用于创建一个正则表达式对象。
2. `test()`：用于测试一个字符串是否符合某个正则表达式，返回布尔值。
3. `exec()`：用于在一个字符串中搜索匹配正则表达式的结果，返回匹配结果数组。
4. `match()`：用于在一个字符串中搜索匹配正则表达式的结果，返回匹配结果数组。
5. `search()`：用于在一个字符串中搜索匹配正则表达式的结果，返回匹配结果的位置。
6. `replace()`：用于在一个字符串中替换匹配正则表达式的结果。
7. `split()`：用于将一个字符串按照正则表达式进行分割，返回分割后的数组。

### test()

#### 语法

```
regexObj.test(str)
```

#### 参数

- `str`

  ​    用来与正则表达式匹配的字符串  

#### 返回值

如果正则表达式与指定的字符串匹配，返回`true`；否则`false`。

> ### [当设置全局标志的正则使用`test()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp/test#当设置全局标志的正则使用test)
>
> 如果正则表达式设置了全局标志，`test()` 的执行会改变正则表达式 [`lastIndex`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp/lastIndex)属性。连续的执行`test()`方法，后续的执行将会从 lastIndex 处开始匹配字符串，([`exec()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp/exec) 同样改变正则本身的 `lastIndex 属性值`).

### exec()

#### 语法

```
regexp.exec(str)
```

#### 参数

- `str`

  ​    要匹配正则表达式的字符串。  

#### 返回值

如果匹配失败，`exec()` 方法返回 [`null`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/null)，并将正则表达式的 [`lastIndex`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp/lastIndex) 重置为 0。

如果匹配成功，`exec()` 方法返回一个数组，并更新正则表达式对象的 [`lastIndex`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp/lastIndex) 属性。完全匹配成功的文本将作为返回数组的第一项，从第二项起，后续每项都对应一个匹配的捕获组。数组还具有以下额外的属性：

- `index`

  ​    匹配到的字符位于原始字符串的基于 0 的索引值。  

- `input`

  ​    匹配的原始字符串。  

- `groups`

  ​    一个命名捕获组对象，其键是名称，值是捕获组。若没有定义命名捕获组，则 `groups` 的值为 [`undefined`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined)。参阅[捕获组](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions/Groups_and_Backreferences)以了解更多信息。  

- `indices` 可选

  ​    此属性仅在设置了 [`d`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp/hasIndices) 标志位时存在。它是一个数组，其中每一个元素表示一个子字符串的边界。每个子字符串匹配本身就是一个数组，其中第一个元素表示起始索引，第二个元素表示结束索引。  

### match()

#### 语法

```js
str.match(regexp)
```

#### 参数

- `regexp`

  ​    一个正则表达式对象或者任何具有 [`Symbol.match`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol/match) 方法的对象。如果 `regexp` 不是 `RegExp` 对象并且对象上无 `Symbol.match` 方法，则会使用 `new RegExp(regexp)` 将其隐式地转换为 [`RegExp`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp)。    如果你没有给出任何参数并直接使用 `match()` 方法，你将会得到一个包含空字符串的[数组](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array)：`[""]`，因为这等价于 `match(/(?:)/)`。  

#### 返回值

- 如果使用 g 标志，则将返回与完整正则表达式匹配的所有结果，但不会返回捕获组。
- 如果未使用 g 标志，则仅返回第一个完整匹配及其相关的捕获组（`Array`）。在这种情况下，返回的项目将具有如下所述的其他属性。

#### 附加属性

如上所述，匹配的结果包含如下所述的附加特性。

- `groups`: 一个命名捕获组对象，其键是捕获组名称，值是捕获组，如果未定义命名捕获组，则为 [`undefined`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined)。有关详细信息，请参阅[组和范围](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions/Groups_and_Backreferences)。
- `index`: 匹配的结果的开始位置
- `input`: 搜索的字符串。

### search()

#### 语法

```
str.search(regexp)
```

#### 参数

- `regexp`

  ​    一个[`正则表达式（regular expression）`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp)对象。如果传入一个非正则表达式对象 `regexp`，则会使用 `new RegExp(regexp)` 隐式地将其转换为正则表达式对象。  

#### 返回值

如果匹配成功，则 `search()` 返回正则表达式在字符串中首次匹配项的索引;否则，返回 `-1`。

### replace()

#### 语法

```js
str.replace(regexp|substr, newSubStr|function)
```

#### 参数

- `regexp` (pattern)

  ​    一个[`RegExp`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp) 对象或者其字面量。该正则所匹配的内容会被第二个参数的返回值替换掉。  

- `substr` (pattern)

  ​    一个将被 `newSubStr` 替换的 [`字符串`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String)。其被视为一整个字符串，而不是一个正则表达式。仅第一个匹配项会被替换。  

- `newSubStr` (replacement)

  ​    用于替换掉第一个参数在原字符串中的匹配部分的[`字符串`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String)。该字符串中可以内插一些特殊的变量名。参考下面的[使用字符串作为参数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/replace#使用字符串作为参数)。  

- `function` (replacement)

  ​    一个用来创建新子字符串的函数，该函数的返回值将替换掉第一个参数匹配到的结果。参考下面的[指定一个函数作为参数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/replace#指定一个函数作为参数)。  

#### 返回值

一个部分或全部匹配由替代模式所取代的新的字符串。

### split()

#### 语法

```
str.split([separator[, limit]])
```

#### 参数

- `separator`

  ​    指定表示每个拆分应发生的点的字符串。`separator` 可以是一个字符串或[正则表达式](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp)。如果纯文本分隔符包含多个字符，则必须找到整个字符串来表示分割点。如果在 str 中省略或不出现分隔符，则返回的数组包含一个由整个字符串组成的元素。如果分隔符为空字符串，则将 str 原字符串中每个字符的数组形式返回。  

- `limit`

  ​    一个整数，限定返回的分割片段数量。当提供此参数时，split 方法会在指定分隔符的每次出现时分割该字符串，但在限制条目已放入数组时停止。如果在达到指定限制之前达到字符串的末尾，它可能仍然包含少于限制的条目。新数组中不返回剩下的文本。  

#### 返回值

返回源字符串以分隔符出现位置分隔而成的一个 Array

### 百度二面正则题

实现模板字符串渲染render函数

```js
const data = {
  user:{
    name: "小明",
    age: 16,
  },
  school: "第三中学",
  classroom: "教室2"
}
// 百度面试题
var str=`这是一个模板字符串。我是{{user.name }}，{{user.age}}岁，{{user.a}}`
// 自己新增包含js语句的情况
var str1`{{user.name}}今年 {{ user.age }} 岁，就读于 {{ school }} 今天在 {{ classroom }} 上课，{{ user.name }} {{ #data.age >= 18 ? '成年了' : '未成年' }}`
console.log(render(str,data))
//这是一个模板字符串。我是小明，16岁，undefined
console.log(render(str1,data))
//小明今年 16 岁，就读于 第三中学 今天在 教室2 上课，小明 未成年
```

```js
// 普通版
function render(str,data){
  return str.replace(/\{\{\s*([a-zA-Z.]*)\s*\}\}/g,(match,p1,offset,str)=>{
    let arr=p1.split(".")
    let res=data[arr[0]]
		for(let i=1;i<arr.length;i++){
      res=res[arr[i]]
    }
    return res
  })
}
// 升级版
function render(str,data){
  return str.replace(/{{\s*([\S\s]*?)\s*}}/g,(match,p1,offset,str)=>{
		// 处理{{}} 这种情况
    if(p1==="") return ""
    // 处理包含js语句的情况
    else if(p1.startsWith('#')){
      return eval(p1.substr(1, p1.length - 1));
    }
    else{
      let arr=p1.split(".")
      let res=data[arr[0]]
      for(let i=1;i<arr.length;i++){
        res=res[arr[i]]
      }
      return res
    }
  })
}
```

