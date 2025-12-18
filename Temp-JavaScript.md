### JavaScript 概述

JavaScript 诞生于1995 年，是由网景公司（Netscape）的员工 Brendan Eich（兰登 • 艾奇，1961 年～）发明，最初命名为 LiveScript。1995 年 12 月与 SUN 公司合作，因市场宣传需要，改名为 JavaScript

1996 年，微软为了抢占市场，推出了`JScript`在 IE3.0 中使用

1996 年 11 月网景公司向 ECMA（European Computer Manufacturers Association，欧洲电脑制造商协会，属于国际标准化组织）提交了 JS的语言标准，将其成为国际标准，以此对抗微软。

1997年7月，ECMA 组织发布262号标准文件（ECMA-262）的第一版，规定了浏览器脚本语言的标准，并将这种语言规范称为 ECMAScript，这个版本就是 ECMAScript 1.0 版。简而言之，ECMA-262是一份标准文件，定义了 ECMAScript 这个语言规范。JavaScript 成为了 ECMAScript最著名的实现之一

ECMAScript 在 2015 年 6 月，发布了 ECMAScript 6 版本（ES6）



### JavaScript 入门

JavaScript的组成

- ECMAScript：JavaScript 的语法标准
- DOM：Document Object Model（文档对象模型），JS 操作页面上的元素（标签）的 API
- BOM：Browser Object Model（浏览器对象模型），JS 操作浏览器部分功能的 API

ECMAScript 定义了 JavaScript 的语法，而DOM和BOM定义了 JavaScript 在浏览器上的API



JavaScript 位置

- 行内式，如`<input type="button" value="按钮" onclick="alert('Hello World!')" />`
- 内联式，在`<script>`标签中声明
- 外链式，如`<script src="utils.js"></script>`



加载时机

一般的将内联的`<script>`标签写在`</body>`结束标签前，这是因为浏览器通常按顺序加载标签，如果过早声明JS将导致代码在标签加载前执行，导致无法正常操作DOM

可使用`window.onload`解决这一问题



语言特征

- 区分大小写
- 换行、缩进、空格不敏感
- 可以使用且建议使用分号作为语句结尾
- 弱类型
- 解释性
- 单线程



注释

```javascript
// 单行注释

/*
 多行注释
 */

/**
 * 多行注释
 */
```

支持类似Java的文档注释

```javascript
/**
 * @author 匿名
 */
```



输出

```javascript
alert('弹窗');
var result = confirm('带确认或取消的弹窗');
var result = prompt('带输入框的弹窗');

console.log('控制台打印');
console.log('FOO','BAR'); // 输出：FOO BAR

document.write('向文档尾写入'); // 如果文档未加载完也会写入当前状态的文档尾
```



字面量

```javascript
// 数字
666
3.14
// 字符串
'Hello'
"World"
// 布尔值
true
false
```



变量与常量

```javascript
const name = 'value';
var field = 1;
let flag = true;
```

`let`的行为比`var`更严格，推荐使用`let`



标识符命名规则

- 只能由字母(A-Z、a-z)、数字(0-9)、下划线(_)、美元符( $ )组成
- 不能以数字开头
- 区分大小写
- 不能使用关键字和保留字
- 长度不超过255字符
- 建议驼峰命名法



关键字

```
if、else、switch、break、case、default、for、in、do、while、

var、let、const、void、function、continue、return、

try、catch、finally、throw、debugger、

this、typeof、instanceof、delete、with、

export、new、class、extends、super、with、yield、import、static、

true、false、null、undefined、NaN
```

保留字

```
enum、await

abstract、boolean、byte、char、double、final、float、goto、int、long、native、short、synchronized、transient、volatile、

arguments eval Infinity

# 以下关键字只在严格模式中被当成保留字，在ES6中是属于关键字
implements、interface、package、private、protected、public
```



数据类型

```
基本数据类型（值类型）：String 字符串、Boolean 布尔值、Number 数值、Undefined 未定义、Null 空对象、BigInt 大型数值、Symbol

引用数据类型（引用类型）：Object 对象（包括Function、Array、Date、RegExp、Error等）
```

与Java类似，JavaScript采用严格的值传递

使用`typeof`获取变量类型

```
typeof 数字（含 typeof NaN）	  number
typeof 字符串	               string
typeof 布尔型	               boolean
typeof 对象	                object
typeof 方法	                function
typeof null	                 object
typeof undefined	         undefined
```



字符串转义

```
\' 表示 ' 单引号
\" 表示 " 双引号
\\ 表示\
\r 表示回车
\n 表示换行
\t 表示缩进
\u2600 转义4位16进制为Unicode编码字符
```

字符串的长度

```javascript
str.length
```

字符串具有不可变性



String本质是字符数组，可使用数组的形式进行操作

```javascript
let str = '123';
console.log(str[1]);
```



模版字符串（ES6支持）

使用反引号声明，且支持表达式、函数调用等等

```javascript
let s = `i = ${i}, f = ${f}, i + f = ${i+f}`;
```

模板字符串同时支持多行字符串

```javascript
let s = `this is a line and

   this is the next line`
```



数值型Number包括整数和浮点数、Infinity、-Infinity、NaN

最大正数值：Number.MAX_VALUE，这个值为： 1.7976931348623157e+308

最小正数值：Number.MIN_VALUE，这个值为： 5e-324 ，或者 2 的负1074 次方

支持使用`0x`、`0o`和`0b`声明16、8、2进制数字



Undefined

使用var声明变量但未赋值，此时其值为undefined

无返回值的函数的返回值为undefined

调用函数但未传参时参数值为undefined



显式类型转换

```
变量/值.toString()
String(变量/值)
Number(变量/值)
parseInt(string)
parseFloat(string)
Boolean(变量/值)
```

隐式类型转换

```
isNaN() 函数
自增/自减运算符：++、—-
运算符：正号+a、负号-a
运算符：加号+
运算符：-、*、/、%
比较运算符：<、>、 <=、 >=、==等
逻辑运算符：&&、||、!
```

隐式类型转换将通过显式类型转换的方法进行



运算符

```
算数运算符
+  -  *  /  %  **（幂运算，ES7特性）

自增/自减运算符
++  --

一元运算符
typeof  +（正号）  -（负号）

三元运算符（条件运算符）
条件表达式 ? 语句1 : 语句2

逻辑运算符
&&  ||  !
不支持非短路的逻辑运算符

赋值运算符
=  +=  -=  *=  /=  %=  **=

比较运算符
>  <  >=  <=  ==  ===（全等于）  !=  !==  （全不等于）
```

```
== 符号可用于布尔、数值、字符串等多种类型之间的比较，并且将进行隐式类型转换
'6' == 6 // true
true == '1' // true
NaN == NaN // false NaN不和任何值相等，包括它本身

=== 不会进行类型转换

!= 和 !== 同理
```

运算符优先级

```
.、[]、new

()

++、--

!、~、+（单目）、-（单目）、typeof、void、delete

*、/、%

+（双目）、-（双目）

<<、>>、>>>

比较运算符：<、<=、>、>=

比较运算符：==、!==、===、!==

&

^

|

逻辑运算符：&& （注意：逻辑与 && 比逻辑或 || 的优先级更高）

逻辑运算符：||

?:

=、+=、-=、*=、/=、%=、<<=、>>=、>>>=、&=、^=、|=

,
```



流程控制

```javascript
// 代码块
{
	...
}

// if 语句
if (条件表达式1) {
	...
} else if (条件表达式2) {
	...
} else {
	...
}

// switch 语句，其中case的判断逻辑是 === 全等于
switch(表达式) {
	case 值1：
		语句体1;
		break;

	case 值2：
		语句体2;
		break;

	...
	...

	default：
		语句体 n+1;
		break;
}
```

```javascript
for(初始化表达式; 条件表达式; 更新表达式){
	语句...
}

while(条件表达式){
	语句...
}

do{
	语句...
}while(条件表达式)
```

```javascript
// 使用 break 和 continue 控制循环

// break 和 continue 支持标签

outer: for (let i = 0; i < 5; i++) {
    for (let j = 0; j < 5; j++) {
        break outer;
    }
}

let i = 0;
label: while(true){
    if(i >= 5) break;
    while(true){
        i++;
        if(i == 10) continue label;
    }
}
```



对象

JavaScript中的对象有属性和方法，属于Object类型

- 内置对象

  由ES标准定义

  ```
  Arguments	函数参数集合
  Array	数组
  Boolean	布尔对象
  Math	数学对象
  Date	日期时间
  Error	异常对象
  Function	函数构造器
  Number	数值对象
  Object	基础对象
  RegExp	正则表达式对象
  String	字符串对象
  ```

- 宿主对象

  由运行环境提供，如BOM、DOM

- 自定义对象



自定义对象

以键值对的方式声明，其中值可以是任意数据类型（包括基本类型和引用类型），当值为函数类型时，通常成该函数为该对象的方法

对象的结构是动态的，可以动态地新增字段

```javascript
{
    key1: value1,
    key2: value2,
    ...
    keyf1: function() {
        ...
    }
    keyf2: funcion() {
        ...
    }
}
```

如

```javascript
let obj = {
    name: 'foobar',
    value: 0,
    func: function(){
        console.log('func()');
    }
}
console.log(obj.name); // foobar
obj.value=1;
console.log(obj.value); // 1
obj.func(); // func()
obj.id = 1000;
console.log(obj.id); // 1000
```

对象支持以数组的方式进行访问

```javascript
let obj2 = {
    name: 'name',
    value: 123
}
console.log(obj2['value']); // 123
obj2['value']++;
console.log(obj2['value']); //124
```

对象的字段可以被删除

```javascript
let obj = {
    name: 'name'
}
delete obj.name;
```



Object的拓展

使用 Object.create 可以通过指定原型来创建新对象，并指定新对象的新属性，以及这些属性是否可写、可删除、可枚举

使用 Object.defineProperties 可指定对象来新增属性，并为属性创建 setter、getter

可以通过上述方法为对象新增常量



访问器属性

可在对象字面量中为对象属性声明 setter、getter，这样的属性称为访问器属性

访问器属性通常不用于存储具体的值，仅用于当使用`obj.field`或`obj[field]`语法时触发其 setter、getter 以用于操作私有属性

```javascript
let obj = {
    _name: null,
    get name(){
        return this._name;
    },
    set name(value){
        this._name = value;
    }
}
```

不应该在 setter、getter 中调用属性自身，否则将造成无限递归

```javascript
get name(){
    return this.name; // 递归
}
```



基本数据类型如String、Number其也可以使用属性或方法，这是因为进行了隐式的包装类型的转换

但它们不支持新增字段

```javascript
let s4 = '123';
console.log(s4.charAt(0));
s4.foobar = 123;
console.log(s4.foobar); // undefined
```

JavaScript提供了包装类型，包括String Number Boolean

```javascript
let s5 = new String('123');
console.log(s5.charAt(0));
s5.foobar = 123;
console.log(s5.foobar); // 123
```



对包装类型等对象使用 == 或 === 会产生非预期的结果

在比较两个引用时会比较两个引用的值而不是引用的对象的值

```javascript
let s5 = new String('123');
let s6 = new String('123');
console.log(s5.toString()==s6.toString()); // true
console.log(s5.toString()===s6.toString()); // true
console.log(s5==s6); // false
console.log(s5===s6); // false
```



数组

数组 Array 是一种对象，其元素可以是任意类型

```javascript
let arr1 = [1,'666', [100,200,300], function(){console.log('element is a function');}];
```

创建数组的方式

```
[e1, e2, ...]
[] // 空数组
new Array(10) // 指定长度数组，默认值为undefined
new Array(e1, e2, ...)
```



数组越界获取的值为undefined

支持通过数组的length属性动态地增减数组的长度，默认值为undefined

当数组越界设置值时将增加数组的长度



数组在ES标准中被视为对象，而各JS引擎在实现上不一定将数组作为连续的内存结构进行管理，在许多情况下，数组可能以哈希的方式进行存储



伪数组

伪数组是一种包含length等属性的对象或可迭代对象，其表现与行为与数组相似

但伪数组的原型链中没有Array.prototype，不包含数组应有的方法

伪数组可以通过Array.from(...)转换为真数组



遍历

使用for in语句遍历普通的对象（无序）

```javascript
for (let key in obj) {
	console.log(key);
	console.log(obj[key]);
}
```

使用for of语句遍历数组

```javascript
for(let value of arr) {
	console.log(value);
}
```



函数

函数是一种对象，每个函数都是Function的实例



函数的声明

```javascript
function foo(){
    ...
}

let bar = function(){
    ...
}
```

其中`function(){...}`是匿名函数

支持通过`new Function()`的方式，通过传入代码字符串来创建函数



函数的调用

通过`()`符号调用或`call()`方法调用

```javascript
let foo = function(param){
    console.log(`foo(param=${param})`);
    console.log(this);
}

foo(123);

foo.call(null,123);
```

函数可以声明后立即执行

```javascript
(function bar(param){
    console.log(`bar(param=${param})`);
})(123);

(function (param){
    console.log(`(param=${param})`);
})(123);
```

构造函数式调用

```javascript
function Foobar() {
    console.log('Foobar');
}

new Foobar(); // 这在创建一个对象的同时将调用Foobar函数
```



为带参数的函数传参时，若实参少于形参，将导致部分参数值为undefined，若实参多余形参，部分值将忽略



参数求值顺序

JavaScript的参数求值顺序与Java、C#、Python语言相同，明确规定为从左到右

```javascript
fun1(fun2(), fun3(), fun4()); // 执行顺序：fun2 fun3 fun4 fun1
```



函数的返回值

函数的返回值可以是任意类型，包括Function

当没有函数的返回值时，该函数返回undefined

函数只能返回一个值

调用一个函数时会隐式地传递两个参数：this 和 arguments



arguments

arguments是一个类数组对象（伪数组），它的功能有：

- 获取所有的实参即实参个数
- 获取调用者



作用域

变量或函数具有作用域，在编译时确定

- 全局作用域
- 函数（局部）作用域



全局作用域

全局作用域在页面打开时创建，在页面关闭时销毁，其实质是window对象

当使用var声明变量或`function ...(...){...}`声明函数时，实质将在window中声明

```javascript
var a = 1;
console.log(window.a);
```

这意味这该变量或函数将成为全局变量和全局函数，它们可以在任意外链或内联中声明并在该页面的任意位置使用

注意let和const的行为有所不同，它们的作用域仅限于当前文件或当前`<script>`标签



预处理（预解析）

在解析代码之前，有一个“预处理（预解析）”阶段，这将使得当前 JS 代码中所有变量的定义和函数的定义，放到所有代码的最前面，即变量声明提升和函数声明提升

对于全局变量或全局函数，将等效于将变量的定义和函数的定义，放到当前文件或当前`<script>`标签的最前面

这个行为仅局限于当前文件或当前`<script>`标签，即当前文件或标签仍无法引用下一个尚未加载的文件或标签中的全局变量或函数

```javascript
console.log(v4);
var v4 = 456;

// 等效于

var v4;
console.log(v4);
v4 = 456;
```

```javascript
foobar();
function foobar(){ ... }

// 等效于

function foobar(){ ... }
foobar();
```

对于局部变量或局部函数，将等效于将变量的定义和函数的定义，放到当前作用域的最前面

```javascript
function foo(){
    console.log(v);
    bar();
    var v = 1;
    function bar(){
        console.log('bar()');
    }
}

// 等效于

function foo(){
    var v;
    function bar(){
        console.log('bar()');
    }
    console.log(v);
    bar();
    v = 1;
}
```

值得注意的是，局部var变量没有块级作用域，其变量声明提升总是会提升到当前函数作用域的顶部，虽然ES5中function也没有块级作用域，但ES6中function存在块级作用域，只会提升到当前块作用域的顶部

```javascript
function f7(){
    console.log(v8); // undefined
    f8(); // ERROR
    if(true){
        var v8 = 890;
        f8();
        function f8(){
            console.log('f8');
        }
    }
}
f7();
```

注意let和const的行为有所不同，它们不存在预处理的行为



this

函数执行前会创建一个执行期上下文的内部对象，一个执行期上下文定义了一个函数执行时的环境

每调用一次函数，就会创建一个新的上下文对象，当函数执行完毕，它所产生的执行期上下文会被销毁

在调用函数每次都会向函数内部传递进一个隐含的参数this，称为函数执行的上下文对象

- 以函数的形式（包括普通函数、定时器函数、立即执行函数）调用时，this 的指向永远都是 window

- 以对象方法的形式调用时，this 指向调用方法的那个对象

- 以构造函数的形式调用时，this 指向实例对象

- 以事件绑定函数的形式调用时，this 指向绑定事件的对象

- 使用 call 和 apply 、bind 调用时可以指定 this 指向的对象

  - `func.call(<this>, param1, param2, ...)`

  - `func.apply(<this>, [<params>])`

  - `newFunc = func.bind(<this>, param1, param2, ...)`

    bind 将设置目标函数的this和参数并返回一个新的无参函数，不会立即调用目标函数

- ES6中的箭头函数继承外层函数的 this



闭包

闭包是指那些能够访问和操作其外部函数作用域中变量的函数，即使外部函数已经执行完毕

闭包实际上是一个函数和这个函数能访问的局部变量的组合

```javascript
function fn_outer(p1){
    return function fn_inner(p2){
        console.log(`p1=${p1}, p2=${p2}`);
    }
}
fn_outer(1)(2); // 1 2
```

闭包的原理

编译函数时会对函数中的变量进行逃逸分析，如果一个变量因为闭包导致其在该函数的作用域外被访问，那么这个变量将在堆上而不是栈上进行分配

一个函数如果持有了其外部函数的局部变量，那么这个函数将通过作用域链访问外部函数的局部变量



注意区别

```javascript
let funs = new Array(4);
for(let a = 0; a <= 3; a++){
	funs[a] = function(){
		console.log(a);
	}
}
for(let f of funs){
	f(); // 0 1 2 3
}
```

```javascript
let funs = new Array(4);
for(var a = 0; a <= 3; a++){
	funs[a] = function(){
		console.log(a);
	}
}
for(let f of funs){
	f(); // 4 4 4 4
}
```

```javascript
let a = 0;
let funs = new Array(4);
while(a <= 3){
	funs[a] = function(){
		console.log(a);
	}
	a++;
}
for(let f of funs){
	f(); // 4 4 4 4
}
```

这是因为for循环的循环体`{}`会每次迭代建新的块级作用域，对于括号`()`，使用`let/const`时每次迭代时都会创建一个新的变量绑定新的块作用域

`for in`和`for of`的行为与之相同



面向对象

JavaScript的面向对象是基于原型实现的

在ES6中也引入了类和继承来实现面向对象



自定义对象的声明方式

- 对象字面量

  通过`{}`以键值对的方式直接声明

  ```javascript
  let obj = {
      key1: 'value1',
      key2: 'value2',
      ...
      fun1: function(){
          ...
      },
      ...
  }
  ```

- `new Object()`

  ```javascript
  let obj = new Object();
  obj.key1 = 'value1';
  obj.fun1 = function(){
      ...
  };
  ```

- 构造函数

  定义函数并在函数中使用 this，随后对该函数使用 new

  ```javascript
  function Entity(){
      this.key = 'value';
      this.fun = function(){
          ...
      };
  }
  let entity = new Entity();
  ```

  注意构造函数的声明与普通函数没有区别，仅当函数调用与 new 共同使用时它才成为构造函数，这是因为 new 会改变 this 的指向



instanceof

instanceof用于检查一个对象是否是一个类的示例

```javascript
对象 instanceof 构造函数
```

所有的对象都是Object的实例



浅拷贝

使用`目标对象 = Object.assign(目标对象, 源对象1, 源对象2...)`



hasOwnProperty

obj.hasOwnProperty(field_name) 可用于对象自身（即不包括从原型链继承来的字段）是否具有某个特定的字段



冻结对象

Object.freeze(obj)可用于冻结一个对象，使得该对象无法以任意形式修改




TODO 原型、原型链、原型继承 类 构造继承



DOM

Document object Mode 文档对象模型



节点

- 文档节点

  即整个HTML文档

- 元素节点

  HTML标签

- 属性节点

  元素的属性

- 文本节点

  标签中的文本

一个DOM树由节点构成

![img](http://img.smyhvae.com/20180126_2105.png)



获取元素节点

```javascript
document.getElementById("id") // 通过ID获取一个节点
document.getElementsByTagName("div") // 通过标签名获取一组节点
document.getElementsByClassName("class") // 通过类名获取一组节点
```

一个节点可以获取它的一个父节点、若干各兄弟节点和若干个子节点

![img](http://img.smyhvae.com/20180126_2145.png)



节点的操作

```javascript
document.createElement("标签名") // 创建节点
父节点.appendChild(新的子节点) // 插入节点
父节点.insertBefore(新的子节点,作为参考的子节点) // 插入节点
父节点.removeChild(子节点) // 删除节点
要复制的节点.cloneNode(是否复制子节点) // 复制节点
元素节点.属性名 // 设置或获取元素属性
元素节点[属性名] // 设置或获取元素属性
元素节点.getAttribute("属性名称") // 获取元素属性
元素节点.setAttribute("属性名", "新的属性值") // 设置元素属性
元素节点.removeAttribute(属性名) // 删除节点属性
```

注意原始属性可以使用`元素节点.属性名`或`元素节点[属性名]`进行操作，否则应使用`getAttribute`或`setAttribute`等方法使其完全绑定到标签上而不是声明为对象的一个字段



事件

```javascript
document.getElementById(id).onclick = function(event) {
    ...
} // 只能绑定一个
element.addEventListener('click', function (event) {
    ...
}, true表示捕获阶段触发而false表示冒泡阶段触发); // 可以绑定多个
```

元素的事件

![img](http://img.smyhvae.com/20180126_1553.png)

window也有事件，如window.onload



DOM 事件流

1. 事件捕获

   在 addEventListener 时声明 true 参数则在捕获阶段执行，但默认为false

   捕获的传递流程：window --> document --> html--> body --> ... --> 目标元素

2. 事件目标

3. 事件冒泡

   冒泡的传递流程：目标元素 -> ... -> body -> html -> document -> window

   部分事件不会冒泡：blur、focus、load、unload、onmouseenter、onmouseleave

   可以通过 event.bubbles 检查一个事件是否会冒泡

   可以通过 event.stopPropagation() 阻止冒泡

![img](http://img.smyhvae.com/20180204_1218.jpg)



BOM

- Window：浏览器的窗口，window全局对象
- Navigator
- Location：地址栏
- History：历史记录
- Screen：屏幕



定时器

- 标识符 = setInterval(函数, 毫秒)

  每隔一段时间执行一次

  clearInterval(标识符) 用于清除定时器

- 标识符 = setTimeout(函数, 毫秒)

  等待一段时间后执行一次

  clearTimeout(标识符) 用于清除定时器



严格模式

严格模式用于在ES5中限制JavaScript的语法，消除其不合理不严谨不安全之处，减少怪异行为

在文件第一行声明`use strict`使该文件使用严格模式

在函数第一行声明`use strict`使该函数使用严格模式

- 禁止使用未声明就赋值的全局变量
- 禁止自定义函数中 this 指向 window
- 禁止 with 语句
- eval 作用域
- 构造函数必须使用 new 而不能直接调用
- 禁止遍历调用栈
- 禁止删除变量
- 写对象的只读属性将报错
- 为禁止拓展的对象新增属性将报错
- 禁止为对象多次声明同一属性
- 禁止为函数声明重名参数
- 函数作用域的函数必须声明函数顶部
- 新增保留字



解构（ES6）

解构赋值支持以便捷地方式将数组、字符串或对象中的多个值赋值给多个变量



箭头函数（ES6）

箭头函数中的 this 即（其定义位置时的）外部函数的 this



参数默认值（ES6）

参数默认值支持为函数的参数定义默认值



异步

JavaScript被设计为单线程的，但浏览器是多进程/多线程的

一般的一个窗口、标签页对应一个进程，每个窗口、标签页、iframe中有一个JavaScript主线程负责所有的开发者声明的JavaScript代码执行（包括外链脚本、内联脚本、事件、定时器、异步等），所有的代码视为任务，由事件循环按所需的顺序派对执行

许多异步的底层实现仍依赖浏览器的其他线程，如定时器的计时等待、网络请求、文件IO等

关键词：Ajax、Promise、Fetch、Axios





Ajax

Ajax用于实现异步的网络请求，主要依赖XMLHttpRequest，通常的一个XMLHttpRequest示例对应一个请求-响应

XMLHttpRequest的实例属性、方法：

- `open(method, url, async)` 设置请求参数

  - method 请求方法
  - url URL，支持绝对和相对路径
  - async true表示异步，否则同步

  示例`xhr.open('GET', '/api/info', true)`

- `setRequestHeader(header,value)` 设置请求头

- `responseType`

- `timeout`

- `withCredentials`

- `onreadystatechange` 回调函数，当readyState变化时调用

- `onload`

- `onerror`

- `onabort`

- `ontimeout`

- `onprogress`

- `send(bodyString)` 发送请求

- `upload` 只读属性，一个XMLHttpRequestUpload对象表示上传进度

- `readyState` 只读属性

  - 0 （UNSENT）
  - 1 （OPENED）以调用open方法
  - 2 （HEADERS_RECEIVED）已调用send且已接收响应头
  - 3 （LOADING）正在接收响应体，此时responseText包含部分数据
  - 4 （DONE）响应完成

- `status` 只读属性，响应HTTP状态码

- `statusText` 只读属性，响应HTTP状态描述

- `getResponseHeader(header)` 获取响应头

- `getAllResponseHeaders()` 获取所有响应头

- `response` 只读属性，响应体

- `responseText` 只读属性，响应体文本

- `responseXML` 只读属性，Document类型的响应体

示例

```javascript
let xhr = new XMLHttpRequest();
xhr.open('GET','https://jsonplaceholder.typicode.com/posts/1',true);
xhr.send();
xhr.onload = () => {
    console.log(xhr.responseText);
};
```



Promise（ES6）

Promise用于提供简单明确可维护的异步语法

```javascript
new Promise((resolve, reject) => {
    let result = asyncTask();
    if(result.isSuccess) resolve(result.data);
    else reject(result.errorMsg);
}).then(data => {
    console.log(data);
}).catch(errorMsg => {
    console.log(errorMsg);
});
```

Promise的三种状态：

- pending 等待中，初始状态
- fulfilled 已解决，执行了resolve()时使一个Promise处于该状态
- rejected 已拒绝，执行了reject()时使一个Promise处于该状态

一个Promise进入fulfilled或rejected状态后它的状态不可再改变

Promise的回调函数：

- `new Promise(executor)`  executor在创建Promise时立即同步调用
- `promise.then(onFulfilled)` onFulfilled在进入fulfilled状态后进入事件循环的任务队列进行异步调用
- `promise.catch(onRejected)` onRejected在进入rejected状态后进入事件循环的任务队列进行异步调用
- `promise.then(onFulfilled, onRejected)`

一个Promise可以设置多个onFulfilled和onRejected回调函数，按声明顺序执行

Promise回调的参数：

onFulfilled和onRejected最多只有一个参数有效，同时resolve和reject最多接收一个参数

- 一般的resolve和reject的参数将传递给onFulfilled和onRejected
- 如果resolve中传入新的Promise，那么当前的Promise的状态由新的Promise决定
- 如果resolve中传入thenable对象，那么当前的Promise的状态由thenable的执行结果决定

进阶的

- `promise.finally()` 用于设置无论成功与否都执行的回调，在ES9（ES 2018）中引入
- `then`的链式调用实际上不是对同一个promise的多次then调用
- `then`的链式调用中回调可以返回普通值以传递给下一个回调，也可以返回新Promise或thenable对象
- `executor`中抛出异常会使该Promise进入rejected状态



异步函数

使用async关键字为普通函数或对象方法声明为异步函数，异步函数的返回值永远是Promise对象

在异步函数中可以返回普通值、新的Promise或thenable对象

```javascript
async function foo(){
    
}
let result = async () => {
    
}
```



await

await后接一个值为Promise的表达式，用于暂停异步函数的执行

TODO



异常处理

TODO