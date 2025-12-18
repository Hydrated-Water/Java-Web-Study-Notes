### CSS 概述



### CSS 入门



CSS 中的单位

```
绝对单位
in 英寸 1 in = 2.54 cm
cm 厘米
mm 毫米
pt 点（英镑） 1 in = 72 pt
pc 皮卡  1 in = 6 pc  1 pc = 12 pt

相对单位
px 像素
em 印刷单位
% 百分比
```



CSS 中的颜色

```
CSS 2.1
red 红色
rgb(255,255,255) 白色
#00FFFF 青色

CSS 3
rgba
hsla
```



CSS的结构

```
选择器 {
	属性名: 属性值;
	属性名: 属性值;
	...
}
选择器 {
	属性名: 属性值;
	属性名: 属性值;
	...
}
```

当一个属性有多个值，用空格隔开

```
选择器 {
	属性名: 属性值1 属性值2 属性值3;
	属性名: 属性值1 属性值2 属性值3 ... ;
	...
}
```

注释

```
/* 注释内容 
   支持多行
*/
```



CSS的类型

- 行内样式
- 内嵌样式（内联样式）
- 外链样式

行内样式

使用标签的style属性声明一个或多个样式属性名-属性值对

```html
<li style="color: blueviolet; font-size: larger;">123</li>
```

内联样式

在head标签的style标签中声明样式

```html
<head>
    <style>
        b {
            font-size: 15px; 
            line-height: 30px; 
        }
        li {
            list-style-type: decimal-leading-zero;
            cursor: help;
        }
        alpha {
            opacity: 0.25;
        }
    </style>
</head>
```

外部样式

使用link标签引入或在style标签中使用import



基本选择器

- 标签选择器
- ID 选择器
- 类选择器
- 通用选择器



标签选择器

选择器即标签名，通过标签名选择所有该标签

```css
p {
	font-size: 14px;
}
```

可声明多个标签，用逗号分隔

```css
p,div {
    font-size: 14px;
}
```



ID选择器

通过 #ID 的方式声明一个ID选择器，声明了该ID的标签将运用该样式

由于标签的ID不重复，所有只有一个标签会运用该样式

```css
#style1 {
    color: coral;
}
```

```html
<div id="style1">123</div>
```



类选择器

通过 .类名 的方式声明一个样式的类名，所有在class属性中声明该类名的标签将被选择

class属性中可以声明多个类名

```css
.brownColor{
    color: brown;
}
.largeSize{
    font-size: large;
}
```

```html
<div class="brownColor">字一段</div>
<div class="largeSize">字第二段</div>
<div class="brownColor largeSize">字第三段</div>
```



通用选择器

使用符号 * 定义选择器，将对所有标签使用，可能造成性能问题

```css
* {
    color: black;
}
```



高级选择器

高级选择器由基本选择器组成

- 后代选择器
- 交集选择器
- 并集选择器
- 伪类选择器



后代选择器

使用空格选择后代，其中后代不必须为直接后代

```css
div p {
	...
}
```

可以声明类选择器等

可以声明多层级的后代

```css
.div1 .p1 .b1 {
    ...
}
```



交集选择器

使多个基本选择器紧挨在一起

```css
h3.special#title { /*选择了h3、special类且ID为title的标签*/
    ...
}
```



并集选择器

使用逗号分隔多个选择器

```css
i,em { /*同时选择了i和em标签*/
    ...
}
```



CSS3中的选择器

- 子代选择器

  使用>符号选择一个标签的直接子标签，如div>p

- 序选择器

  可以指定一个标签的某个子标签，如第一个、最后一个等

- 兄弟选择器

  使用+符号选择一个标签的下一个指定标签，如h3+p



伪类（伪类选择器）

根据一个标签的不同状态应用不同的样式，并用冒号指定状态，即伪类选择器

例如超链接存在点击前和访问后的状态，使用a:link选择点击前的状态



静态伪类

```
仅适用于超链接<a>标签
:link 点击前
:visited 访问后
```

动态伪类

```
适用于所有标签
:hover 鼠标悬停时
:active 鼠标点击时
:focus 获得焦点时
```

例如

```css
.button1:hover{ /*使类button1在鼠标悬浮时改变字体为斜体*/
    font-style: italic;
}
```

注意事项

```
<a>标签的伪类选择器必须按一定顺序声明，否则可能被覆盖导致失效
:link 、:visited 、:hover 、:active
```



CSS的继承性

```
CSS中的某些属性可以“继承”，即运用到某标签上时同时运用到该标签的所有子标签
这些属性包括文字样式，如color、text-...、line-...、font-... 等

其他的属性不能被继承，如盒子模型、定位、布局等
```



CSS的层叠性

```
CSS的层叠性即CSS处理冲突的机制，即当某标签的某样式属性值可能存在多个来源时应当使哪个生效

样式来源优先级

选择器优先级

定义顺序
```



盒模型

![img](http://img.smyhvae.com/2015-10-03-css-27.jpg)

```
width height 及内容的宽高
padding 内边距 即内容到边框的四个方向上的空白宽度
border 边框 边框在四个方向上可以有不同的宽度
margin 外边距 即边框之外的四个方向上的空白宽度
```



浮动

```
标准文档流

行内元素：
与其他行内元素共用一行
不能设置宽高，默认宽为文本宽

块级元素：
独占一行
可设置宽高，默认宽为父元素的100%

使用display属性可设置元素为块级元素或行内元素
值inline、block
```

```
使用浮动属性float可脱离标准文档流的限制，不再区分行内或块级元素
```



定位

```
通过position声明定位的类型

绝对定位：相对于父容器的左上角或左下角定义坐标（脱离标准文档流）
position: absolute;  /*绝对定位*/
left: 10px;  /*横坐标*/
top/bottom: 20px;  /*纵坐标*/

相对定位：相对于原先的位置进行位置调整
如
position: relative;
left: 50px;
top: 50px;

固定定位：相对于浏览器窗口定位
```





非布局样式

```
字体、行高、颜色、大小、背景、边框、滚动、换行、装饰性属性（粗体、斜体、下划线）等
```

字体样式

```css
p {
    font-size: 15px; /*字体大小*/
    line-height: 30px; /*行高，影响盒模型中的padding*/
    font-family: 幼圆, 黑体; /*字体类型：如果没有幼圆就显示黑体，没有黑体就显示默认*/
    font-style: italic; /*italic表示斜体，normal表示不倾斜*/
    font-weight: bold; /*粗体*/
    font-variant: small-caps; /*小写变大写*/
    vertical-align: middle; /*指定行内元素（inline）、行内块元素（inline-block）、表格的单元格（table-cell）的垂直对齐方式*/
    letter-spacing: 0.5cm ; /*字符间距*/
    word-spacing: 0.1cm; /*单词间距*/
    text-decoration: none; /*字体修饰：none 去掉下划线、underline 下划线、line-through 中划线、overline 上划线*/
    color: #33AAAA; /*字体颜色*/
    text-align: middle; /*当前容器中的对齐方式*/
    text-transform: lowercase; /*单词的字体大小写。属性值可以是：uppercase（单词大写）、lowercase（单词小写）、capitalize（每个单词的首字母大写）*/
    /*等等*/
}
```



列表

```css
ul li{
	list-style-image:url(images/2.gif) ;  /*设置列表项前标记的显示方式，此处声明为图片*/
    list-style-position: outside; /*设置列表项前标记的位置*/
    list-style-type: circle; /*设置列表项前标记的显示类型*/
}
```



背景

```css
background {
    background-color:#ff99ff; /*设置元素的背景颜色。*/
    background-image:url(images/2.gif); /*将图像设置为背景。*/
    background-repeat: no-repeat;/*设置背景图片是否重复及如何重复，默认平铺满。no-repeat不要平铺；repeat-x横向平铺；repeat-y纵向平铺。*/
    background-position:center top; /*设置背景图片在当前容器中的位置，指定值用于描述向右偏移量和向下偏移量，如background-position:100px 200px*/
    background-attachment:scroll; /*设置背景图片是否跟着滚动条一起移动。 属性值可以是：scroll、fixed*/
    background-size: 500px 500px;/*设置背景图片尺寸，可使用长度、百分比、auto、cover、contain等值*/
    background-origin: content-box;/*设置背景在盒模型中的开始显示位置（原点开始绘制点），值padding-box、border-box、content-box*/
    background-clip: content-box;/*是否裁剪超出指定部分的背景，值padding-box、border-box、content-box*/
}
```

渐变

```
可用linear-gradient(方向, 起始颜色, 终止颜色);等方式定义渐变，
如background-image: linear-gradient(to right, yellow, green);
linear-gradient、radial-gradient等支持高级的渐变声明，如声明不同的渐变方向和多个颜色
```



透明度

```
opacity: 0.3; 会将整个盒子及子盒子设置透明度。也就是说，当盒子设置半透明的时候，会影响里面的子盒子。
```



裁剪

```
clip-path 支持对元素的显示区域进行裁剪，使元素指显示特定形状的区域，支持定义图形甚至是SVG
```



```
overflow用于处理超出范围的内容，值：
visible：默认值。多余的内容不剪切也不添加滚动条，会全部显示出来。
hidden：不显示超过对象尺寸的内容。
auto：如果内容不超出，则不显示滚动条；如果内容超出，则显示滚动条。
scroll：Windows 平台下，无论内容是否超出，总是显示滚动条。Mac 平台下，和 auto 属性相同。

cursor设置鼠标的光标类型，如手、箭头、十字、问号、忙等待、自定义光标等

filter为图片设置滤镜
```



CSS3中的新增

```
属性选择器，对存在特定属性声明的标签进行选择

结构伪类选择器，类似序选择器

伪元素选择器，类似兄弟选择器
```

```
文本阴影

指定盒子宽高计算方式

私有前缀

圆角

边框阴影

边框图片

动画：过渡、2D转换（缩放、位移、旋转、倾斜）、3D转换（旋转、移动、透视、呈现）、动画

flex布局

Web字体
```



Sass
