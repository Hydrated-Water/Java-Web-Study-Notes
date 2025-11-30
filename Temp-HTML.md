### HTML 概述

TODO 浏览器 市场占有份额 浏览器内核 渲染引擎 JS引擎 chromium Blink V8引擎 [浏览器工作方式](https://web.dev/articles/howbrowserswork?hl=zh-cn)

TODO W3C组织及标准

TODO HTML概述 标记语言 超文本

TODO 发展历史 XHTML HTML2.0 3.2 4.0 5 XHTML1.0 1.1 2.0 HTML兼容性



### HTML 入门

TODO 术语 网页 导航 标记 元素 属性 双边标记 单边标记

TODO 骨架标签

TODO 文档声明头DTD 页面语言

TODO head头标签：title base meta body link

TODO body标签 属性：bgcolor background text leftmargin topmargin rightmargin bottommargin alink vlink



HTML 不区分大小写，对换行和Tab不敏感  空白折叠（包括空格、换行、Tab）

文本级标签：如`p` `span` `a` `b` `i` `u` `em` 内部只能有文字、图片、表单

容器级标签：如`div` `h1` `li` `dt` `dd` 内部可以是任意内容



注释

```
<!-- 注释内容 -->
```



排版标签

```
<h1> 到 <h6> 标题，表现为文本加粗加大
	align属性 值left、center、right
<p> 段落，表现为文本上下有空白间隔
	<p>是文本级标签，如果其中存在如<h1>等意外内容，浏览器将主动封闭或分割<p>标签以正常地显示
	align属性
<hr/> 水平分割线
	属性
	align
	size 数值 单位像素 线条粗细 默认值2
	width 数值（单位像素）或百分比 线条长度 默认值100%
    color 线条颜色
    noshade 无阴影
<br/> 换行，常用于文本中
<div> 用于分割，单独一行，常与CSS共同使用
	align属性
<span> 用于分割，不换行，常与CSS共同使用
	align
<center> 用于居中，HTML5中不建议使用
<pre> 用于保留其中的空格、换行、Tab等空白字符，通常不使用
```



转义字符

```
&nbsp; 不间断空格
&lt; 符号<
&gt; 符号>
&amp; 符号&
&quot; 符号"
&apos; 符号'
&copy; 符号版权
&trade; 符号商标
&#32464; Unicode编码转义
```



字体标签

```
<u> 下划线
<s> <del> 删除线
<i> <em> 斜体
<b> <strong> 粗体 不建议使用
<font> 声明字体 不建议使用
	属性
	color 颜色
	size 大小
	face 字体
<sup> 上标
<sub> 下标
```



超链接

```
<a> 超链接
	属性
	href URL地址
		外部链接，如https://github.com/Hydrated-Water
		当前网站，如/page/page.html（绝对路径） home/index.html（相对路径） #title index.html#part2（锚链接，指定某个标签的id或name）
		其他链接，如邮箱mailto:xxx@xxx.com、应用steam://url/GameHub/730等
	title 悬停文本
	target 打开方式，默认值_self
		_self 在当前窗口打开
		_blank 新窗口打开（通常是新标签页）
		_parent 父窗口打开（通常用于iframe）
		_top 顶级窗口打开（通常用于iframe）
```



图片

```
<img> 图片
	属性
	src 图像URL，支持jpg、gif、png、bmp等格式，支持绝对路径、相对路径
	width height 宽与高，单位像素，HTML4支持百分比，只设置其中一个时会等比缩放
	alt 图像无法显示时显示的文本
	title 悬停文本
	align 对齐，值可以是bottom（默认值）、center、top、left、right
	border 边框颜色，已弃用
	hspace 左右边距，已弃用
	vspace 上下边距，已弃用
```



列表

```
<ul> 无序列表，使用一个或多个<li>子标签，支持列表嵌套
	属性
	type 列表项标识，dist实心原点（默认）、circle空心原点、square实心方块，也可在<li>中声明该属性
<ol> 有序列表，同上
	属性
	type 列表项序号，值可以是1、A、a、i、I
	start 列表项开始序号
<li> 列表项
```



定义列表

```
<dl> 定义列表，使用一个或多个<dt>或<dd>子标签，容器级标签
<dt> 定义列表标题,z支持多个
<dd> 定义列表项，容器级标签
```



定义表格

```
<table> 表格，一个表格<table>包含若干个行<tr>，一个行<tr>包含若干个单元格<td>
	属性
	border 边框，单位像素
	bordercolor 边框颜色
	width height 宽高，单位像素
	align 表格本身的对齐方式
	cellpadding 单元格到边的距离
	cellspacing 单元格与单元格的距离
	bgcolor 表格背景颜色
	background 背景图片
	bordercolorlight 表格3D效果颜色
	bordercolordark 表格3D效果颜色
	dir 单元格内容排列方式 值ltr从左到右 rtl从右到左
<tr> 行
	属性
	dir
	bgcolor
	height
	align
	valign
<td> 单元格
	属性
	align
	valign
	width
	height
	bgcolor
	background
	colspan 横向合并，数值
	rowspan 纵向合并，数值
<th> 加粗单元格，同<td>
<caption> 表格标题
```



框架

```
<frameset>与<frame>用于在一个网页中显示多个网页，已废弃，使用iframe代替
<frameset>需要替换<body>

<iframe> 内嵌框架，可在<body>中声明，功能受限于同源策略等安全策略
	属性
	src URL，支持绝对或相对路径
	width height 宽高
	scrolling 是否需要滚动条
```



表单

```
<form> 声明表单，用于用户填写并向服务器发起请求，一般在<form>中嵌套<table>，通常需要声明id或name
	属性 
	action 指定表单的处理程序（链接）
	method 提交方式 值为get或post
	enctype POST请求体编码方式，包括application/x-www-form-urlencoded、multipart/form-data和text/plain

<input> 声明一个输入，常用于<form>中，通常需要声明id或name
	属性
	type 输入类型
		值
		text 文本输入，此时可声明value属性以用于默认值
		password 密码输入
		hidden 隐藏，与hidden属性类似
		radio 单选按钮，要求一组单选按钮的name相同，此时可声明checked属性默认选中
		checkbox 多选按钮，要求一组单选按钮的name相同，此时可声明checked属性默认选中
		button 按钮，通常与js共同使用，此时可声明value属性用于按钮文本
		file 选择一个文件，此时可声明value属性用于按钮文本
		submit 提交按钮，点击后将触发<form>的请求，此时可声明value属性用于按钮文本
		reset 重置按钮，点击后将重置整个<form>为默认值，此时可声明value属性用于按钮文本
		image 图片按钮
	size 显示字符数
	readonly 只读
	disabled 不可用

<select> 下拉框，常用于<form>中，通常需要声明id或name，需声明一个或多个<option>子标签
	属性
	multiple 支持多选
	size 值1时为下拉视图，大于1时为滚动视图
<option> 属性
	selected 默认选中

<textarea> 多行文本框
	属性
	cols 列数
	rows 行数
	readonly 只读

<lable> 声明与某个输入相关的一段文本

<fieldset>、<legend> 使表单语义化
```



多媒体

```
以下标签的支持和特性受浏览器影响，它们有些不属于W3C标准
<bgsound> 背景音乐
<embed> 播放多媒体
<object> 播放多媒体
<marquee> 滚动字幕
```



HTML5

```
新增了如下语义标签（本身不负责样式）
<section> 区块
<article> 文章
<header> 页眉
<footer> 页脚
<nav> 导航
<aside> 侧边栏
<figure> 内容分组
<mark> 标记
<progress> 进度
<time> 时间日期
如<nav>实际上相当于<div class="nav">

新增表单<input>类型：
email 邮箱
tel 手机号码
url URL
number 数字
search 搜索框
range 滑动条
color 拾色器
time 时间
date 日期
datetime 时间日期
month 月份
week 星期

新增其他表单标签
<datalist> 定义数据列表，可为text类型的<input>新增下拉框
<meter> 度量器，可用于显示一个进度条

新增表单中标签的属性
placeholder 提示文本
autofocus 自动获得焦点
required 必填项
等等

新增多媒体
<audio> 音频
<video> 视频

支持自定义属性与通过JS的自定义属性获取，以data-开头

支持 draggable 属性定义可拖拽元素，但具体的逻辑由JS定义
```

