### 演示代码

```html
<!DOCTYPE HTML>
<html>

<head>
  <meta charset="utf-8">
  <title>Html页面的生命周期,DOMContentLoaded,load,beforeunload,unload</title>
</head>

<body>
  <div id="main"></div>
  <iframe src="http://smart-pig.top/index" height="400" width="700" onload="append('iframe onload')"></iframe>
  <div><img src="http://cdn.smart-pig.top/imgs/categoriesBackgroundImgs/0.png" onload="append('image onload')"></div>
  <script>
    var output = "";
    function print() {
      document.getElementById("main").innerHTML += output;
    }
    function append(data) {
      output += data + " <br />";
    }
    document.addEventListener('readystatechange', function () {
      append("readyState : " + document.readyState);
    });
    document.addEventListener('DOMContentLoaded', function () {
      append("DOMContentLoaded");
    });
    window.onload = function () {
      append("window onload");
      print();
    };
    append("script executed");
  </script>
</body>
</html>
```



​            

[![img](http://img.souche.com/test/9edf6abc08619a90cc82b3078c28f215.png)](https://s.souche.com/_S5nA3)

在文章任何区域双击击即可给文章添加【评注】！浮到评注点上可以查看详情。

隐藏标注

## 简言

理解WEB页面的生命周期，文档加载事件及顺序对WEB开发有十分的重要意义。如果不理解，在元素未加载就提前操作元素，则得不到想要的结果。而如果页面完全加载完成后，再进行操作，则又会影响用户体验。

一般来说，大多数的操作我们都应该放在DOMContentLoaded事件中执行。采用如下形式：

```javascript
document.addEventListener('DOMContentLoaded', function () {
    alert("DOMContentLoaded");
});
```

当初始的 HTML 文档被完全加载和解析完成之后，DOMContentLoaded 事件被触发，而无需等待样式表、图像和子框架的完成加载。

## 说明

理解WEB页面的生命周期，文档加载事件及顺序对WEB开发有十分的重要意义。用过JQuery都知道，我们大多将代码写在$(document).ready()中，当文档加载完成后，就会执行该函数。如果不这样做，在文档未加载完成之前操作DOM，就得不到想要的结果。

#### 演示程序

```html
<!DOCTYPE HTML>
<html>
<head>
    <meta charset="utf-8">
    <title>42度空间-Html页面的生命周期,DOMContentLoaded,load,beforeunload,unload</title>
</head>
<body>
  <div id="main"></div>
  <iframe src="http://res.42du.cn/static/html/iframe.html" height="35" onload="append('iframe onload')"></iframe>
  <div><img src="http://res.42du.cn/up/201802/cbbtiga6.jpg?cache=none" onload="append('image onload')"></div>
  <script>
      var output = "";
      function print() {
          document.getElementById("main").innerHTML += output;
      }
      function append(data) {
          output += data+" <br />";
      }
      document.addEventListener('readystatechange', function () {
          append("readyState : "+document.readyState);
      });
      document.addEventListener('DOMContentLoaded', function () {
          append("DOMContentLoaded");
      });
      window.onload = function () {
          append("window onload");
          print();
      };
      append("script executed");
  </script>
</body>
</html>
```

从演示程序运行结果，得出一般浏览器的加载顺序如下：

- script executed   
- readyState : interactive
- DOMContentLoaded
- image onload
- iframe onload
- readyState : complete
- window onload 

​            

[![img](http://img.souche.com/test/9edf6abc08619a90cc82b3078c28f215.png)](https://s.souche.com/_S5nA3)

在文章任何区域双击击即可给文章添加【评注】！浮到评注点上可以查看详情。

隐藏标注

## 简言

理解WEB页面的生命周期，文档加载事件及顺序对WEB开发有十分的重要意义。如果不理解，在元素未加载就提前操作元素，则得不到想要的结果。而如果页面完全加载完成后，再进行操作，则又会影响用户体验。

一般来说，大多数的操作我们都应该放在DOMContentLoaded事件中执行。采用如下形式：

```javascript
document.addEventListener('DOMContentLoaded', function () {
    alert("DOMContentLoaded");
});
```

当初始的 HTML 文档被完全加载和解析完成之后，DOMContentLoaded 事件被触发，而无需等待样式表、图像和子框架的完成加载。

## 说明

理解WEB页面的生命周期，文档加载事件及顺序对WEB开发有十分的重要意义。用过JQuery都知道，我们大多将代码写在$(document).ready()中，当文档加载完成后，就会执行该函数。如果不这样做，在文档未加载完成之前操作DOM，就得不到想要的结果。

#### 演示程序

```html
<!DOCTYPE HTML>
<html>
<head>
    <meta charset="utf-8">
    <title>42度空间-Html页面的生命周期,DOMContentLoaded,load,beforeunload,unload</title>
</head>
<body>
<div id="main"></div>
<iframe src="http://res.42du.cn/static/html/iframe.html" height="35" onload="append('iframe onload')"></iframe>
<div><img src="http://res.42du.cn/up/201802/cbbtiga6.jpg?cache=none" onload="append('image onload')"></div>
<script>
    var output = "";
    function print() {
        document.getElementById("main").innerHTML += output;
    }
    function append(data) {
        output += data+" <br />";
    }
    document.addEventListener('readystatechange', function () {
        append("readyState : "+document.readyState);
    });
    document.addEventListener('DOMContentLoaded', function () {
        append("DOMContentLoaded");
    });
    window.onload = function () {
        append("window onload");
        print();
    };
    append("script executed");
</script>
</body>
</html>
```

从演示程序运行结果，得出一般浏览器的加载顺序如下：

- script executed   
- readyState : interactive
- DOMContentLoaded
- image onload
- iframe onload
- readyState : complete
- window onload 

从以上可以看出同步的Script总是先于其它事件执行，而window.onload事件总是最后执行。而image onload和iframe onload的先后顺序并不确定。

### 结论

关于WEB页面的生命周期，应该记住如下这些观点，一些观点从运行结果中也可以得出。

- **同步的JavaScript脚本最先执行**，它**先于DOMContentLoaded事件执行**。
- 当**DOM准备就绪时**，**DOMContentLoaded事件在document上触发**。 我们可以在这个阶段利用JavaScript来操作DOM元素。
  - 所有脚本都执行完毕，除了那些外部使用异步（async）或延迟（defer）加载的脚本
  - 图片和其他资源可能仍在载入过程中。
- **window上的onload事件，在页面加载完所有资源后触发**。 我们很少使用它，因为通常的操作不用等到最后才执行。
- **window上的beforeunload事件，该事件在用户准备离开页面，在unload事件之前触发**。 如果beforeunload返回一个字符串，浏览器会给出 用户是否真的想离开的提示。
- **window上的unload事件，当用户最终离开时会触发该事件**。在unload的事件处理程序中，我们只能做简单的事情，不涉及延迟或询问用户。 由于这个限制，它很少使用。
- **document.readyState表示文档的当前状态，可以在readystatechange事件中跟踪文档状态的变更**。
  - loading – 文档正在载入。
  - interactive – document已经解析完毕时触发，几乎与DOMContentLoaded同时发生，但在DOMContentLoaded事件之前触发。
  - complete – 文档和资源加载完成时触发，几乎与window.onload同时发生，但在onload事件之前触发。

一般来说，大多数的操作我们都应该放在DOMContentLoaded事件中执行，而不要放在window.onload中执行。