<center><h1>常见CSS效果</h1></center>

#### 清除浮动

##### clear清除

clear属性**不允许**被清除浮动的元素的左边/右边挨着浮动元素，底层原理是在被清除浮动的元素上边或者下边添加足够的清除空间。

```css
.clearfix:before,
.clearfix:after {
    display: table;
    content: " ";
}
.clearfix:after {
    clear: both;
}
.clearfix{
    zoom: 1;
}
```

##### BFC清除

BFC 计算高度的时候浮动子元素的高度也将计算在内，利用这条规则就可以清楚浮动。

BFC 的方式很多，我们可以给父元素设置overflow: auto 来简单的实现 BFC 清除浮动，但是为了兼容 IE 最好用 overflow: hidden。

```css
.parent {
    overflow: hidden;
}
```

#### 实现三角形

```css
div {
  height: 0;
  width: 0;
  border-top-color: #fcbf07;
  border-right-color: #06daf6;
  border-bottom-color: #f40707;
  border-left-color: #d20af6;
  border-width: 50px;
  border-style: solid;
}
```

![image-20221208165049164](https://my-learning-data.oss-cn-hongkong.aliyuncs.com/imgs/image-20221208165049164.png)

用 transparent 实现三角形的原理：

- 首先盒子宽高必须是 0px，通过边框的粗细来填充内容；
- 那条边需要就要加上颜色，而不需要的边则用 transparent；
- 想要什么样的三角形，完全由上下左右 4 条边的中有颜色的边和透明的边的位置决定；
- 等腰三角形：设置一条边有颜色，然后紧挨着的 2 边是透明，且宽度是有颜色边的一半；直角三角形：设置一条边有颜色，然后紧挨着的任何一边透明即可。

##### 等腰三角

```css
div {
  height: 0;
  width: 0;
  border-left: 50px solid #06daf6;
  border-top: 25px solid transparent;
  border-bottom: 25px solid transparent;
}
```

![image-20221208165744808](https://my-learning-data.oss-cn-hongkong.aliyuncs.com/imgs/image-20221208165744808.png)

##### 直角三角

```css
div {
  height: 0;
  width: 0;
  border-left: 50px solid #06daf6;
  border-bottom: 50px solid transparent;
}
```

![image-20221208165802183](https://my-learning-data.oss-cn-hongkong.aliyuncs.com/imgs/image-20221208165802183.png)

#### 长文本处理

##### 文本过长溢出容器

![image-20221208170618321](https://my-learning-data.oss-cn-hongkong.aliyuncs.com/imgs/image-20221208170618321.png)

##### 换行显示

![image-20221208170659778](https://my-learning-data.oss-cn-hongkong.aliyuncs.com/imgs/image-20221208170659778.png)

```css
.wrap {
  overflow-wrap: break-word;
}
```

##### 连字符连接

![image-20221208170857717](https://my-learning-data.oss-cn-hongkong.aliyuncs.com/imgs/image-20221208170857717.png)

```css
.hyphens {
  hyphens: auto;
}
```

##### 单行文本超过省略

![image-20221208171213280](https://my-learning-data.oss-cn-hongkong.aliyuncs.com/imgs/image-20221208171213280.png)

```css
.ellipsis {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
```

##### 多行文本超过省略

![image-20221208171707753](https://my-learning-data.oss-cn-hongkong.aliyuncs.com/imgs/image-20221208171707753.png)

```css
.line-clamp {
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
}
```

**注：盒子不能给高度**