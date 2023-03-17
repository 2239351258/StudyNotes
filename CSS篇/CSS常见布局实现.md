### 1. 两栏布局

两栏布局指的是左边一栏宽度固定，右边一栏宽度自适应，两栏布局的具体实现：

#### 1. 利用浮动+margin-left

将左边元素宽度设置为200px，并且设置向左浮动。将右边元素的margin-left设置为200px，宽度设置为auto（默认为auto，撑满整个父元素）。

```html
<style>
  .outer{
    height: 100vh;;
  }
  .left{
    float: left;
    width: 200px;
    height: 100vh;
    background-color: aqua;
  }
  .right{
    margin-left: 200px;
    height: 100vh;
    background-color: antiquewhite;
  }
</style>
<div class="outer">
  <div class="left"></div>
  <div class="right"></div>
</div>
```

#### 2. 利用浮动+BFC

利用浮动，左侧元素设置固定大小，并左浮动，**右侧元素设置overflow: hidden; 这样右边就触发了BFC**，BFC的区域不会与浮动元素发生重叠，所以两侧就不会发生重叠。

```html
<style>
  .left{
    float: left;
    height: 100vh;
    width: 200px;
    background-color: antiquewhite;
  }
  .right{
    height: 100vh;
    overflow: hidden;
    background-color: aqua;
  }
</style>
<div class="left"></div>
<div class="right"></div>
```

#### 3. flex布局

利用flex布局，将左边元素设置为固定宽度200px，将右边的元素设置为flex:1。

```html
<style>
  .outer{
    display: flex;
    height: 100vh;
  }
  .left{
    width: 200px;
    background-color: aquamarine;
  }
  .right{
    flex: 1;
    background-color:bisque;
  }
</style>
<div class="outer">
  <div class="left"></div>
  <div class="right"></div>
</div>
```

#### 4. 左元素绝对定位(类似浮动+margin-left原理)

利用绝对定位，将父级元素设置为相对定位。左边元素设置为absolute定位，并且宽度设置为200px。将右边元素的margin-left的值设置为200px。

```html
<style>
  .outer {
    position: relative;
    height: 100vh;
  }

  .left {
    position: absolute;
    width: 200px;
    height: 100vh;
    background-color: aquamarine;
  }

  .right {
    margin-left: 200px;
    height: 100vh;
    background-color: bisque;
  }
</style>
<div class="outer">
  <div class="left"></div>
  <div class="right"></div>
</div>
```

#### 5. 右元素绝对定位

利用绝对定位，将父级元素设置为相对定位。左边元素宽度设置为200px，右边元素设置为绝对定位，左边定位为200px，其余方向定位为0。

```html
<style>
  .outer {
    position: relative;
    height: 100vh;
  }

  .left {
    width: 200px;
    height: 100vh;
    background-color: aquamarine;
  }

  .right {
    position: absolute;
    left: 200px;
    top: 0;
    right: 0;
    bottom: 0;
    height: 100vh;
    background-color: bisque;
  }
</style>
<div class="outer">
  <div class="left"></div>
  <div class="right"></div>
</div>
```

### 2. 三栏布局

三栏布局一般指的是页面中一共有三栏，**左右两栏宽度固定，中间自适应的布局**，三栏布局的具体实现：

#### 1. 绝对定位

利用绝对定位，左右两栏设置为绝对定位，中间设置对应方向大小的margin的值。

```html
<style>
  .outer {
    position: relative;
    height: 100vh;
  }

  .left {
    position: absolute;
    top: 0;
    left: 0;
    width: 100px;
    height: 100vh;
    background: tomato;
  }

  .right {
    position: absolute;
    top: 0;
    right: 0;
    width: 200px;
    height: 100vh;
    background: gold;
  }

  .center {
    margin-left: 100px;
    margin-right: 200px;
    height: 100vh;
    background: lightgreen;
  }
</style>
<div class="outer">
  <div class="center">center</div>
  <div class="left">left</div>
  <div class="right">right</div>
</div>
```

#### 2. flex布局

利用flex布局，左右两栏设置固定大小，中间一栏设置为flex:1。

```html
<style>
  .outer {
    display: flex;
    height: 100vh;
  }

  .left {
    order: 1;
    width: 100px;
    background: tomato;
  }

  .right {
    order: 3;
    width: 200px;
    background: gold;
  }

  .center {
    flex: 1;
    order: 2;
    background: lightgreen;
  }
</style>
<div class="outer">
  <div class="left">left</div>
  <div class="center">center</div>
  <div class="right">right</div>
</div>
```

> `order` 是 flex 布局中的一个属性，用于控制 flex 子元素的排列顺序。默认情况下，flex 子元素按照它们在 HTML 中的顺序排列，但是通过 `order` 属性，我们可以改变它们的顺序。

> 注意如果不加`order`属性，center盒子要放中间

#### 3. 浮动

利用浮动，左右两栏设置固定大小，并设置对应方向的浮动。中间一栏设置左右两个方向的margin值，注意这种方式，**中间一栏必须放到最后：**

```html
<style>
  .outer {
    height: 100vh;
  }

  .left {
    float: left;
    width: 100px;
    height: 100vh;
    background: tomato;
  }

  .right {
    float: right;
    width: 200px;
    height: 100vh;
    background: gold;
  }

  .center {
    height: 100vh;
    margin-left: 100px;
    margin-right: 200px;
    background: lightgreen;
  }
</style>
<div class="outer">
  <div class="left">left</div>
  <div class="right">right</div>
  <div class="center">center</div>
</div>
```

> center放最后的原因：因为**浮动元素会尽可能地靠近父元素的左边或右边，并且不会覆盖非浮动元素**。当一个浮动元素与一个非浮动元素并排时，它会尽可能地靠近父元素的左边或右边，但不会覆盖前面的非浮动元素。因此，如果前一个元素没有设置浮动，它会占据一定的空间，导致后面的浮动元素无法与它并排显示，而被挤下去。

#### 4. 圣杯布局

圣杯布局，利用浮动和负边距来实现。父级元素设置左右的 padding，三列均设置向左浮动，中间一列放在最前面，宽度设置为父级元素的宽度，因此后面两列都被挤到了下一行，通过设置 margin 负值将其移动到上一行，再利用相对定位，定位到两边。

```html
<style>
  .outer {
    height: 100px;
    padding-left: 100px;
    padding-right: 200px;
  }

  .left {
    position: relative;
    left: -100px;

    float: left;
    margin-left: -100%;

    width: 100px;
    height: 100px;
    background: tomato;
  }

  .right {
    position: relative;
    left: 200px;

    float: left;
    margin-left: -200px;

    width: 200px;
    height: 100px;
    background: gold;
  }

  .center {
    float: left;

    width: 100%;
    height: 100px;
    background: lightgreen;
  }
</style>
<div class="outer">
  <div class="center">center</div>
  <div class="left">left</div>
  <div class="right">right</div>
</div>
```



#### 5. 双飞翼布局

双飞翼布局，双飞翼布局相对于圣杯布局来说，左右位置的保留是通过中间列的 margin 值来实现的，而不是通过父元素的 padding 来实现的。本质上来说，也是通过浮动和外边距负值来实现的。

```html
<style>
  .wrap {
    overflow: hidden;
    height: 100vh;
  }

  .middle {
    float: left;
    width: 100%;
    height: 100vh;
    background-color: red;
  }

  .middle-wrap {
    margin: 0 200px;
    height: 100vh;
    background-color: purple;
  }

  .left {
    float: left;
    width: 200px;
    height: 100vh;
    margin-left: -100%;
    background-color: yellow;
  }

  .right {
    float: left;
    width: 200px;
    height: 100vh;
    margin-left: -200px;
    background-color: green;
  }
</style>
<div class="wrap">
  <div class="middle">
    <div class="middle-wrap">middle</div>
  </div>
  <div class="left">left</div>
  <div class="right">right</div>
</div>
```

### 3. 水平垂直居中

#### 1. flex布局

```html
<style>
  .container {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
  }

  .center {
    width: 200px;
    height: 100px;
    background-color: #f0f0f0;
  }
</style>
<div class="container">
  <div class="center">Centered</div>
</div>
```

#### 2. 使用绝对定位和 transform 实现水平垂直居中

```html
<style>
  .container {
    position: relative;
    height: 100vh;
  }

  .center {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 200px;
    height: 100px;
    background-color: #f0f0f0;
  }
</style>
<div class="container">
  <div class="center">Centered</div>
</div>
```

#### 3. 使用绝对定位和 margin 实现水平垂直居中

```html
<style>
  .container {
    position: relative;
    height: 100vh;
  }

  .center {
    position: absolute;
    top: 50%;
    left: 50%;
    margin: -50px 0 0 -100px;
    width: 200px;
    height: 100px;
    background-color: #f0f0f0;
  }
</style>
<div class="container">
  <div class="center">Centered</div>
</div>
```

#### 4. grid布局

```html
<style>
  .container {
    display: grid;
    height: 100vh;
    place-items: center;
  }
  .center {
    width: 200px;
    height: 100px;
    background-color: #f0f0f0;
  }
</style>
<div class="container">
  <div class="center">Centered</div>
</div>
```

#### 5. 使用 line-height 属性：

将子盒子变为行内块`vertical-align: middle;`，父盒子高度等于行高

```html
<style>
  .container {
    height: 100vh;
    text-align: center;
    line-height: 100vh; /* 等于父元素高度 */
  }

  .center {
    display: inline-block;
    vertical-align: middle;
    width: 200px;
    height: 100px;
    background-color: #f0f0f0;
  }
</style>
<div class="container">
  <div class="center">Centered</div>
</div>
```

> 仅能让盒子垂直水平居中,盒子里的文字会出现问题

#### 6. 使用 calc() 函数和负 margin：

```html
<style>
  .container {
    height: 100vh;
    position: relative;
  }

  .center {
    height: 100px;
    width: 100px;
    position: absolute;
    top: calc(50% - 50px);
    left: calc(50% - 50px);
    background-color: antiquewhite;
  }

</style>
<div class="container">
  <div class="center">Centered</div>
</div>
```

### 4. 瀑布流

> 参考链接:https://zhuanlan.zhihu.com/p/94142452?utm_source=wechat_session

#### 1. flex布局

```html
<style>
  .waterfall {
    display: flex;
    flex-direction: row;
  }
  .colmun{
    display: flex;
    flex-direction: column;
    flex: 1;
    padding: 0 2px;
  }
  .item{
    width: 100%;
  }
  .item1:nth-child(1){
    height: 100px;
    background-color: aliceblue;
  }
  .item1:nth-child(2){
    height: 200px;
    background-color: antiquewhite;
  }
  .item1:nth-child(3){
    height: 150px;
    background-color: aqua;
  }
  .item1:nth-child(4){
    height: 250px;
    background-color: aquamarine;
  }
  .item2:nth-child(1){
    height: 300px;
    background-color: azure;
  }
  .item2:nth-child(2){
    height: 300px;
    background-color:beige;
  }
  .item2:nth-child(3){
    height: 300px;
    background-color:bisque;
  }
</style>
<div class="waterfall">
  <div class="colmun">
    <div class="item1 item">1</div>
    <div class="item1 item">2</div>
    <div class="item1 item">3</div>
    <div class="item1 item">4</div>
  </div>
  <div class="colmun">
    <div class="item2 item">5</div>
    <div class="item2 item">6</div>
    <div class="item2 item">7</div>
  </div>
</div>
```

