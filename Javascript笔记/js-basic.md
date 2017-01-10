# script

```javascript
<script type="text/javascript"  src=""  async=""  defer="defer" charset=""></script>
	defer 只适用于外部脚本，告诉浏览器立即下载，但延迟运行。现实当中，延迟脚本不一定会按照顺序执行（理论是这样），也不一定会在DOMContentLoaded事件前执行
    async 只适用于外部脚本，告诉浏览器立即下载，但不一定按照指定顺序执行。异步脚本一定会在load之前执行，但可能会在DOMContentLoaded之前或之后执行。
	XHTML 可以 <script />
	HTML 必须 <script></script>
//<! [DATA[ 
    function a(){}
//]]>   XHTML将<解析成代码的开始，我们需要将<转为html代码&lt；来使用，比较麻烦，使用左侧办法可以解决XHMTL/HTML都支持js

嵌入外部文件优点：可维护/可缓存(下载一次，处处可用)/适应未来（XHTML HTML）
```

# 文档模式

IE5引入，使用doctype切换,文档开头没标明，默认开启混杂模式

混杂模式：让IE的行为（包含非标准特性）与IE5相同

标准模式：让IE的行为更接近标准行为  严格型strict.dtd/html5

​	IE之后又提出了准标准模式，这种模式下浏览器特性很多都是符合标准的，不标准的地方主要提现在处理图片间隙的时候，表格中体现最为明显。过渡性loose.dtd/transition.dtd,框架集型：frameset.dtd

# noscript

浏览器不支持脚本/支持但脚本被禁用，使用这个标签用以在不支持js的浏览器中显示替代内容,其中可以是任何允许在body内显示的html标签

```javascript
<noscript>
  <p>浏览器不支持js</p>
</noscript>
```

# ECMPscript

## 区分大小写

​	变量/函数/操作符区分大小写

## 严格模式

“use strict”

为js定义一种不同的解析与执行模型。ECMAScript3中的一些不确定行为将得到处理，对不安全操作抛出错误。

```

```

IE10+/FF4+/SF5.1+/Opera12+/chrome

# 类型

## 数值转换

1.Number

2.parseInt

​	转换字符串时，忽略前面的空格，直到找到第一个非空格字符，如果不是数值或负号，则返回NaN。只解析数值部分，非数值部分被丢弃，如“123abc”转换为123

3.parseFloat

## 字符串转换

1.toString(基数)  指定基数，数值将以指定基数转换，不能转换null和undefined

2.String()  null转为“null”，undefined转为“undefined”

## label

```javascript
start：
for （var i=0； i<10; i++) {
	for （var j=0； j<10; j++) {
		if  (i==5 && j==5) {
			break start;//结束内外循环
		}
	}
}
```

## with

将代码的作用域放在一个特定的作用域中，简化多次编写同一个对象的工作

```javascript
var qs = location.search.substring(1);
var hostname = location.hostname;

with (location) {
	qs = search.substring(1);
	hostname = hostname;
}
```

## switch

case可以是任何类型，也可以是变量/表达式

```javascript
switch (i) {
  case "1":
  case "2":
      console.log("1 or 2");
      break;
  case "3":
      console.log("3");
      break;
    default:
      console.log("default")
}
```

## 没有函数重载

重载：函数名相同，定义的签名(参数数量/类型)不同

# 第4章

## 传递参数

按值传递，把函数外部的值复制给函数内部的变量，就和把值从一个变量复制到另一个变量一样。

基本类型传递传递变量的值

引用类型传递对象在内存中的地址

## 检测类型

typeOf

null —— object，regexp（chrome function，ie ff object）

## 执行环境及作用域

​	执行环境定义了变量或函数有权访问的其他数据，决定了它们各自的行为。每个执行环境都有一个与之关联的变量对象，环境中定义的所有变量和函数都保存在这个对象中。我们编写的代码无法访问，解析器在处理数据时会在后台使用它。

​	全局执行环境是最外围的一个执行环境。在web浏览器当中被认为是window对象，所有的全局变量和函数都作为window对象的属性和方法创建，某个执行环境中的所有代码执行完毕后，该环境被销毁，保存在其中的所有变量和函数定义也随之销毁。全局：直到应用程序退出时才会被销毁

参考课本

## 垃圾收集

找到不被继续使用的变量，然后释放其内存

局部变量只在函数执行过程中存在，在这个过程中，会为局部变量在栈或堆内存上分配相应的空间，以便存储它们的值。然后在函数中使用这些变量，直至函数执行结束。此时局部变量就没有存在的必要，因此可以释放它们的内存以供将来使用。垃圾收集器必须跟踪哪个变量有用哪个没用，对于不再有用的变量打上标记，以备将来收回其占用的内存。

#### 标记清除

垃圾收集器将内存中的所有变量打上标记，然后去除在环境中的以及被环境引用的变量的标记，之后在被打上标记的变量就是要清除的变量。最后完成清除工作，销毁那些带标记的值，收回被清除的变量占用的内存。标记可以使用翻转位等策略。

![执行环境](assets/执行环境.png)

# 第5章

## Array

### 检测数组

value instanceof Array  只能检测当前全局环境，如果数组来自另一个全局环境（另一个框架）则不行

Array.isArray(value)只是检测是不是数组，不管来源

### 操作方法

push/unshift返回数组长度   pop/shift返回被移除项

比较 sort

```javascript
function compare（V1，V2) {
  if (V1<V2) {
    return 1;
  } else if (V1>V2) {
    return -1;
  } else {
    return 0；
  }
}
function compare（V1，V2) {
  return V2-V1;
}
```

splice:返回删除的项数组，如果没用就空数组

迭代方法

every/some/filter/forEach/map(function(item, index, array){})

缩小方法

reduce（），reduceRight（function(prev, cur, index, Array)）

## RegExp

正则表达式字面量始终共享一个实例

```javascript
var reg = null;
for (var i=0; i<5; i++) {
  reg = /cat/g;
  reg.test("catddjkf");//循环第二次从索引3开始
}
for (var i=0; i<5; i++) {
  reg = new RegExp("cat", "g");
  reg.test("catddjkf");//每次循环都从0开始
}
```

正则表达式实例toString（），toLocalString()返回正则表达式字面量

## Function

函数声明提升

解析器率先读取函数声明，并将函数声明添加到执行环境中，使其在执行任何代码之前可用；

函数表达式必须等到解析器执行到它所在的代码行，才会真正被执行。

函数声明

```javascript
//正常执行
alert(sum(10, 10));
function sum(v1, v2) {
  return v1 + v2;
}
```

函数表达式

```javascript
//出错
alert(sum(10, 10));
var sum = function(v1, v2) {
  return v1 + v2;
};
```

arguments属性callee：一个指针指向包含arguments对象的函数

```javascript
function factoria(num) {
  if (num < 1) {
    return 1;
  } else {
    return num*arguments.callee(num-1);
  }
}
```

函数对象属性caller：调用该函数的函数的引用，如果是全局环境，则返回null

```javascript
function outer() {
  inner();//outer
}
function inner() {
  console.log(arguments.callee.caller);
}
```

函数属性和方法

属性：length：定义了函数接受的命名参数的个数；

​	prototype不可枚举，不能使用for-in遍历

## 基本包装类型

与引用类型的区别在于对象的生存期。使用new创建的引用类型的实例在执行流离开当前作用于之前一直保存在内存中。自动创建的基本包装类型对象，则只存在与一行代码的执行瞬间，然后立即被销毁。这意味着我们不能在运行时为基本类型值和添加属性和方法。

```javascript
var s = “text"
var s2 = s.substr(1);
相当于
var s = new String("text");
var s2 = s.substr(1);
s = null;//只在执行的一瞬间存在执行完就被销毁了
```

Number

```javascript
toFixed(num)//按照指定小数位数返回，可执行四舍五入
toExponential(num)//按指定小数为显示指数表示数值1.2e+1
toPrecision(num)//返回固定大小格式或指数格式；
```

String

截取：不改变原字符串

​	substr：第一个负值与字符串长度加一，第二个为0

​	slice：所有负值与字符串长度加一

​	substring：所有负值都为0，将小的作为开始位置

search(/REG/):按模式查找，找到返回索引，没找到返回-1

## 单体内置对象

由ECMASscript提供的/不不依赖于宿主环境的对象，这些对象在ES程序执行之前就已经存在。

Object/Array/String/Global/Math

Global

1.编码

​	encodeURI/encodURIComponent

​	decodeURI/decodURIComponent

2.eval()

​	通过eval执行的代码被认为是包含该次调用的执行环境的一部分，因此被执行的代码具有与该执行环境相同的作用域链。其中创建的变量或函数不会被提升，它们只在eval执行时创建，严格模式下，外部访问不到eval中创建的任何变量或函数。

Math

```javascript
Math.max(2,5,2,3);
Math.min(2,5,2,3);
var arr = [2,5,2,3];
Math.max.apply(Max, arr);
Math.min.apply(Max, arr);

Math.ceil()//向上舍入
Math.floor()//向下舍入
Math.rount()//标准舍入

从某个整数范围内随机选择数
Math.floor(Math.random()*可能值总数 + 第一个可能值)
1到10  Math.floor(Math.random()*10 + 1）
2到10  Math.floor(Math.random()*9 + 2）
```

# 第7章 函数表达式

1.函数声明：重要特征是函数声明提升，执行代码之前先读取函数声明

## 递归

```javascript
阶乘
function factoria(n) {
  if (n<=1) {
    return 1;
  } else {
    return n*factoria(n-1);
  }
}

var b = factoria(n)会出问题

arguments.callee严格模式下不管用
function factoria(n) {
  if (n<=1) {
    return 1;
  } else {
    return n*arguments.callee(n-1);
  }
}

var factoria = (function f(n) {
  if (n<=1) {
    return 1;
  } else {
    return n*f(n-1);
  }
});
```

## 闭包

指有权访问另一个函数作用域中变量的函数

参考文档：http://www.ruanyifeng.com/blog/2009/08/learning_javascript_closures.html

它的最大用处有两个，一个是前面提到的可以读取函数内部的变量，另一个就是让这些变量的值始终保持在内存中。

```javascript
function f1(){
　　var n=999;
　　nAdd=function(){
      n+=1;
      console.log(n);
    }
　　function f2(){
　　　　alert(n);
　　}
　　return f2;
}
var result=f1();
result(); // 999
nAdd();
result(); // 1000
```

​	在一个函数内部定义的全局变量，如果函数没执行，内存当中就不存在相应的全局变量，所以在外边直接调用函数内部当中的全局变量是不管用的。如：直接调用nAdd()是不行的，因为内存当中没有，f1();nAdd();是可以的，先执行f1，在内存中添加aAdd，在window对象上创建window.nAdd = fun;，实际就是在window对象上添加一个属性，使得可以在任何地方访问

​	闭包会引用包含函数的整个活动对象。

## 模块级作用域

```javascript
function outNumber(count){
  for (var i=0; i<count; i++) {
    alert(i);
  }
  var i;
  alert(i);//因为没有块级作用域，即使重新声明变量，js也会忽略这个声明
}
匿名函数
第一个括号内是函数表达式，函数表达式后面可以跟圆括号
在匿名函数当中定义的任何变量，都会在执行结束时被销毁。这种技术经常在全局作用域中被用在函数外部，从而限制向全局作用域中添加过多的变量和函数。
匿名函数也是一个闭包
（function(){
 
})()

这是错误的
function(){
  
}() js将function关键字当一个函数声明开始，而函数声明不能跟圆括号
```

## 静态私有变量

在私有作用域中定义私有变量或函数

```javascript
（function(){
  //私有作用域
  //私有变量
  var name = "";
  //全局函数表达式
  Person = function(val) {
    name = value;
  };
  Person.prototype.getName = function() {
    return name;
  }
})()
```

闭包和私有变量缺点：多查找作用域连中的一个层次，就会在一定程度上影响查找速度。

## 模块模式

参考课本190页

单例：只有一个实例的对象。js以对象字面量的方式来创建单例对象

模块模式通过为单例添加私有变量和特权方法能够使其得到增强,

​	这个对象字面量定义的是单例的公共接口。这种模式在需要对单例进行某些初始化是，同时又要维护其私有变量时是非常有用的。

​	在web应用中，经常需要一个单例来管理应用程序级的信息。

```javascript
var singleton = function(){
  var privateVar = 10;
  function privateFun(){
    return false;
  }
  return {
    publicProperty: true,
    publicMethod: function(){
      privateVar++;
      return privateFun();
    }
  }
}(); 
```

## 增强的模块模式

```javascript
var application = function() {
  var components = new Array();
  components.push(new BaseComponent());
  
  var app = new BaseComponent();
  //公共接口
  app.getComponentCount = function(){
    return components.length;
  };
  
  app.registerComponent = function(component){
    if (typeOf component == "object"){
      components.push(component);
    }
  }
  
  return app;
}
```

# 垃圾回收机制

http://blog.chinaunix.net/uid-26672038-id-3522560.html