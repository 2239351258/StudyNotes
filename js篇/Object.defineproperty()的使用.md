<center><h1>Object.defineproperty()的使用</h1></center>

###  Object.defineproperty()的语法

```javascript
Object.defineProperty(obj, prop, descriptor)
```

#### 参数

```javascript
obj----------->要定义属性的对象。
prop---------->要定义或修改的属性的名称或 Symbol。
descriptor---->要定义或修改的属性描述符。
```

#### 返回值

被传递给函数的对象。

### 描述

该方法允许精确地添加或修改对象的属性。

通过对象赋值操作添加的普通属性是可枚举的，可以改变这些属性的值，也可以删除（如：delete）这些属性。

这个方法允许修改默认的额外选项（或配置）。默认情况下，使用 `Object.defineProperty()` 添加的属性值是不可修改（immutable）的。

对象里目前存在的属性描述符有两种主要形式：*数据描述符*和*存取描述符*。*数据描述符*是一个具有值的属性，该值可以是可写的，也可以是不可写的。*存取描述符*是由 getter 函数和 setter 函数所描述的属性。一个描述符只能是这两者其中之一；不能同时是两者。

#### 这两种描述符都是对象。它们共享以下可选键值：

```
enumerable------->控制属性是否可以枚举，默认值是false
configurable----->控制属性是否可以被删除，默认值是false
```

#### 数据描述符还具有以下可选键值：

```
value------------>该属性对应的值（数值，对象，函数等），默认值是undefined
writable--------->控制属性是否可以被修改，默认值是false
```

#### 存取描述符还具有以下可选键值：

```
get():属性的 getter 函数，如果没有 getter，则为 undefined。当访问该属性时，会调用此函数。执行时不传入任何参数，但是会传入 this 对象（由于继承关系，这里的this并不一定是定义该属性的对象）。该函数的返回值会被用作属性的值。默认值是：undefined
set():属性的 setter 函数，如果没有 setter，则为 undefined。当属性值被修改时，会调用此函数。该方法接受一个参数（也就是被赋予的新值），会传入赋值时的 this 对象。默认值是：undefined
```

### 定义多个属性

```javascript
var student = {}
var sex = "男"
Object.defineProperties(student,{
    name:{
        value:"张三"，
        writable:false
    },
    age : {
        value : 16,
        writable:true
    },
    sex:{
        get(){
            return sex
        },
        set(v){
            sex = v
        }
    }
})
```

### 实现双向绑定

```HTML
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Document</title>
  </head>
  <body>
    <input id="myInput" type="text" />
    <div id="contain"></div>

    <script>
      var text
      window.data = {}
      var Input = document.getElementById("myInput")
      var Div = document.getElementById("contain")

      Input.addEventListener("input", function (e) {
        text = e.target.value
        console.log(text)
        window.data.value = text
      })
      Object.defineProperty(window.data, "value", {
        get() {
          return text
        },
        set(v) {
          Div.innerHTML = v
        }
      })
    </script>
  </body>
</html>
```

