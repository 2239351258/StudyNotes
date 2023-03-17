<center><h1>HTML备忘录</h1></center>

### 1. src和href的区别

- src 是 source 的缩写，指向外部资源的位置，指向的内容将会嵌入到文档中当前标签所在位置；在请求 src 资源时会将其指向的资源**下载并应用**到文档内，例如 js 脚本，img 图片和 frame 等元素。`<script src =”xx.js”></script>`当浏览器解析到该元素时，会**暂停其他资源的下载和处理，直到将该资源加载、编译、执行完毕**，图片和框架等元素也如此，类似于将所指向资源嵌入当前标签内。这也是为什么将js 脚本放在底部而不是头部。 

- href 是 Hypertext Reference 的缩写，指向网络资源所在位置，建立和当前元素（锚点）或当前文档（链接）之间的链接，如果在文档中添加`<link href=”common.css” rel=”stylesheet”/>`那么浏览器会识别该文档为 css 文件，就会**并行下载资源并且不会停止对当前文档的处理**。 这也是为什么建议使用 link 方式来加载 css，而不是使用@import 方式

### 2. script标签中defer和async的区别

如果没有defer或async属性，浏览器会**立即加载并执行**相应的脚本。它不会等待后续加载的文档元素，读取到就会开始加载和执行，这样就**阻塞**了后续文档的加载。

async:异步加载js脚本，**加载完成后立即执行**js脚本（这时会阻碍html解析），并且多个标记async的js脚本可能**不会按照顺序执行**。（一般用于加载第三方库）

defer:异步加载js脚本，在html解析完成之后`DOMContentLoaded`事件触发执行之前再执行js脚本文件，而且多个defer的脚本**会按照顺序执行**。(一般用于与dom有关联的脚本加载)

![](https://my-learning-data.oss-cn-hongkong.aliyuncs.com/imgs/defer%E5%92%8Casync%E7%9A%84%E5%8C%BA%E5%88%AB.png)

> 蓝色代表js脚本网络加载时间，红色代表js脚本执行时间，绿色代表html解析。

### 3. 常用的meta标签

`<meta>`是描述HTML文档的**元数据**，例如网页的描述、关键字、作者等

**元数据(Metadata)**,可以理解为描述数据的数据信息。

元数据**不会显示在客户端**，但是**会被浏览器解析**，或其他 Web 服务调用。这样通过这些描述信息就知道如何去解析此网页数据。

常用的meta标签:

```html
<!-- 设定页面使用的字符集。  -->
<meta http-equiv="content-Type" content="text/html; charset=gb2312">
<!--可以用于设定网页的到期时间。一旦网页过期，必须到服务器上重新传输。 -->
<meta http-equiv="expires" content="Fri, 12 Jan 2018 18:18:18 GMT">
<!--禁止浏览器从本地计算机的缓存中访问页面内容。 -->
<meta http-equiv="Pragma" content="no-cache">
<!--自动刷新并指向新页面。 -->
<meta http-equiv="Refresh" content="2; URL=http://www.baidu.com">
<!-- 设置cookie, 如果网页过期，那么存盘的cookie将被删除。-->
<meta http-equiv="Set-Cookie" content="cookievalue=xxx; expires=Friday, 12-Jan-2001 18:18:18 GMT； path=/">
<!--强制页面在当前窗口以独立页面显示,用来防止别人在框架里调用自己的页面。 -->
<meta http-equiv="Window-target" content="_top">
```

|                             语法                             |                       含义                       |
| :----------------------------------------------------------: | :----------------------------------------------: |
|                  `<meta charset="UTF-8" >`                   |      `charset`，用来描述HTML文档的编码类型       |
|         `<meta name="keywords" content="关键词" />`          |              `keywords`，页面关键词              |
|     `<meta name="description" content="页面描述内容" />`     |             `description`，页面描述              |
|       `<meta http-equiv="refresh" content="0;url=" />`       |           `refresh`，页面重定向和刷新            |
| `<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1">` | `viewport`，适配移动端，可以控制视口的大小和比例 |
|       `<meta name="robots" content="index,follow" />`        |                 搜索引擎索引方式                 |

> `name=viewport`中的`content` 参数有以下几种：
>
> - `width viewport` ：宽度(数值/device-width)
> - `height viewport` ：高度(数值/device-height)
> - `initial-scale` ：初始缩放比例
> - `maximum-scale` ：最大缩放比例
> - `minimum-scale` ：最小缩放比例
> - `user-scalable` ：是否允许用户缩放(yes/no）

> `name=robots`中的`content`参数有以下几种：
>
> - `all`：文件将被检索，且页面上的链接可以被查询；
> - `none`：文件将不被检索，且页面上的链接不可以被查询；
> - `index`：文件将被检索；
> - `follow`：页面上的链接可以被查询；
> - `noindex`：文件将不被检索；
> - `nofollow`：页面上的链接不可以被查询。

### 4. img的srcset属性的作用

`<img/> `元素的 `srcset` 属性用于浏览器根据宽度和像素密度来加载相应的图片资源。

语法：`<img srcset="候选地址1 [限制条件：宽度/像素密度],[候选地址2 [限制条件2]],... />`

如果图片地址未加限定条件，则该地址为回退方案，即在未命中其他方案的情况下，使用该图片，否则使用命中的图片。

```html
<img srcset="image/pic_300.jpg 300w,
	image/pic_800.jpg 800w,
	image/pic_1300.jpg 1300w"
/>
```

在不同的设备像素DPR下，每张图片显示所需的视口宽度（viewport）条件：

| DRP(设备像素) |     pic_300     |        pic_800        |        pic_1300        |
| :-----------: | :-------------: | :-------------------: | :--------------------: |
|       1       | viewport<=300px | 300px<viewport<=800px | 800px<viewport<=1300px |
|       2       | viewport<=150px | 150px<viewport<=400px | 400px<viewport<=650px  |
|       3       | viewport<=100px | 100px<viewport<=267px | 267px<viewport<=433px  |

因为没有专门的条件，所以浏览器会选择最靠近1300px的条件，即，还是会显示pic_1300。

```html
<img srcset="image/pic_300.jpg 300w,
	image/pic_800.jpg 800w,
	image/pic_1300.jpg 1300w,
	image/pic_default.jpg"
/>
```

这里有多设置一条pic_default，即在未命中其他条件时，显示pic_default的图片。（视口≥  1300px时）

##### sizes语法

```html
sizes="媒体条件1 [槽的宽度1],[媒体条件2 [槽的宽度2]],..."
```

`sizes="(max-width: 360px) 340px, 128px"`

sizes就是指默认显示128px, 如果视区宽度<=360px, 则显示340px。

对于槽的宽度，可以使用像素或rem等长度单位，但是不可使用百分比。

在定义了sizes属性后，浏览器会依次执行以下操作：

1、查看当前设备宽度；

2、依次检查sizes属性列表中第一个为真的媒体条件；

3、查看该媒体条件对应的槽的大小；

4、通过槽的大小，加载对应的srcset列表中与槽的大小最接近的图像。

这也是为什么，sizes属性必须配合srcset属性，而srcset却可以脱离sizes属性使用的原因了。

##### chrome浏览器调试无效的原因

```html
<img srcset="image/pic_300.jpg 300w,
	image/pic_800.jpg 800w,
	image/pic_1300.jpg 1300w,
	image/pic_default.jpg"
/>
```

现象：实际测试过程中，火狐浏览器正常，但是谷歌Chrome浏览器，在将窗口从大拖小的过程中无变化，将窗口置于最小，刷新后，逐渐拖宽，这个过程中图片会变化，但是再将浏览器拖窄的过程中，图片不再变化，在调试工具中，点击对应图片，清除缓存后，又会恢复效果。

原因：Chrome浏览器在缓存中有高分辨率图片时，不会加载低分辨率的图片去替换高分辨率图片

解决方案：使用picture，可以很好的规避chrome这个烦人的缓存问题，代码如下：

```html
<picture>
	<source media="(max-width:500px)" srcset="image/pic_300.jpg" />
	<source media="(max-width:800px)" srcset="image/pic_800.jpg" />
	<source media="(min-width:800px)" srcset="image/pic_1300.jpg" />
  <img src="image/pic_300.jpg" />
</picture>
```

### 5. web worker的理解

### 6. head标签的作用，以及必须元素

`<head> `标签用于定义文档的头部，它是所有头部元素的容器。`<head> `中的元素可以引用脚本、指示浏览器在哪里找到样式表、提供元信息等。

文档的头部描述了文档的各种属性和信息，包括文档的标题、在 Web 中的位置以及和其他文档的关系等。绝大多数文档头部包含的数据都不会真正作为内容显示给读者。

下这些标签可用在 head 部分：`<base>, <link>, <meta>, <script>, <style>, <title>`。 

其中` <title>` 定义文档的标题，它是 head 部分中**唯一必需**的元素。

### 7. 文档声明（Doctype）和<!Doctype html>有何作用? 严格模式与混杂模式如何区分？它们有何意义?

**文档声明的作用：**文档声明是为了告诉浏览器（解析器）应该以什么样（html或xhtml）的模式来渲染文档，不同的渲染模式会影响浏览器对 CSS 代码甚⾄ JavaScript 脚本的解析。它必须声明在HTML⽂档的第⼀行。当前`HTML`文档使用什么版本的`HTML`来写的，这样浏览器才能按照声明的版本来正确的解析。

**<!Doctype html>的作用：**`<!doctype html>` 的作用就是让浏览器进入标准模式，使用最新的 `HTML5` 标准来解析渲染页面；如果不写，浏览器就会进入混杂模式，我们需要避免此类情况发生。

浏览器渲染页面的两种模式（可通过document.compatMode获取）：

- **CSS1Compat：标准模式(严格模式)(Strick mode)**，默认模式，浏览器使用W3C的标准解析渲染页面。在标准模式中，浏览器以其支持的最高标准呈现页面，浏览器按照`W3C`标准解析代码。
- **BackCompat：怪异模式(混杂模式)(Quick mode)**，浏览器使用自己的怪异模式解析渲染页面。在怪异模式中，页面以一种比较宽松的向后兼容的方式显示。混杂模式通常模拟老式浏览器的行为，以防止老站点无法工作。

**区分**：网页中的`DTD`，直接影响到使用的是严格模式还是浏览模式，可以说`DTD`的使用与这两种方式的区别息息相关。

- 如果文档包含严格的`DOCTYPE` ，那么它一般以严格模式呈现（**严格 DTD ——严格模式**）；
- 包含过渡 `DTD` 和 `URI` 的 `DOCTYPE` ，也以严格模式呈现，但有过渡 `DTD` 而没有 `URI` （统一资源标识符，就是声明最后的地址）会导致页面以混杂模式呈现（**有 URI 的过渡 DTD ——严格模式；没有 URI 的过渡 DTD ——混杂模式**）；
- `DOCTYPE` 不存在或形式不正确会导致文档以混杂模式呈现（**DTD不存在或者格式不正确——混杂模式**）；
- `HTML5` 没有 `DTD` ，因此也就没有严格模式与混杂模式的区别，`HTML5` 有相对宽松的语法，实现时，已经尽可能大的实现了向后兼容(**HTML5 没有严格和混杂之分**)。

> DTD(Document Type Definition) 是一套关于标记符的语法规则。DTD 是一种保证XML文档格式正确的有效方法，可通过比较XML文档和DTD文件来看文档是否符合规范，元素和标签使用是否正确。 可被成行地声明于 XML 文档中，也可作为一个外部引用。

**总而言之，严格模式让各个浏览器统一执行一套规范，混杂模式保证了旧网站的正常运行。**

