# 面向对象

## 创建对象

参考文档：http://javascript.ruanyifeng.com/oop/basic.html

### 1.有参构造函数

```javascript
function Sup(name) {
  this.name = name;
  this.run = function() {
    console.log("sup function run");
  }
}

var object = new Sup("object");
```

new命令的作用，就是执行构造函数，返回一个实例对象。原理：

1. 创建一个空对象，作为将要返回的对象实例

2. 将这个空对象的原型，指向构造函数的`prototype`属性

3. 将这个空对象赋值给函数内部的`this`关键字

4. 开始执行构造函数内部的代码

   缺点

   主要问题：每个方法都要在构造函数的实例上重新创建一遍，因为方法也是对象

### 2.无参构造函数

```javascript
function Sup() {
}

var object = new Sup();
object.name = "object";
object.run = function() {
  console.log("sup function run");
}
```

### 3.工厂方式

```javascript
var object = new Object();
object.name = "object";
object.run = function() {
  console.log("sup function run");
}
```

### 4.原型对象方式

```javascript
function Sup() {
}
Sup.prototype.name = "object";
Sup.prototype.run = function() {
  console.log("sup function run");
}
var object = new Sup();
```

### 5.有参构造函数+原型对象混合

```javascript
function Sup(name) {
  this.name = name;
}
Sup.prototype.run = function() {
  console.log("sup function run");
}
var object = new Sup("object");
```

### 6.setPrototypeOf方式

```javascript
var object = Object.setPrototypeOf({}, Sup.prototype);
F.call(f);
```

### 7.对象字面量

```javascript
var object = {
	name："object",
	run：function(){
       console.log("sup function run");
	}
}
```

### 7.Object.create方式

​	参考：http://javascript.ruanyifeng.com/oop/prototype.html#toc8

​	从原型对象生成新的实例对象，可以替代`new`命令。它接受一个对象作为参数，返回一个新对象，后者完全继承前者的属性，即原有对象成为新对象的原型。

​	第二个参数为属性描述对象，它所描述的对象属性，会添加到新对象。

```javascript
var object = {
	name："object",
	run：function(){
       console.log("sup function run");
	}
}

var obj = Object.create(object, {
  p1: { value: 123, enumerable: true },
  p2: { value: 'abc', enumerable: true }
});
```

## 构造函数继承

参考文档：http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_inheritance.html

### 1.构造函数绑定

apply、call。参考文档：http://javascript.ruanyifeng.com/oop/this.html

```javascript
function Sup(name) {
  this.name = name;
  this.run = function() {
    console.log("sup function run");
  }
}
Sup.prototype.msg = function() {
  console.log("sup function msg");
}

function Sub(name, age) {
  Sup.apply(this, arguments);
  this.name = name;
  this.age = age;
}
```

问题：    1.方法都在构造函数中定义，函数无法复用
                2.在超类型的原型中定义的方法，对子类是不可见的，也就是说子类无法访问父类原型对象的msg方法

### 2.prototype模式

```javascript
Sub.prototype = new Sup();
Sub.prototype.constructor = Sub;
```

对象是引用类型，一个引用对象的变量对其作出更改，其他的引用也会相应改变，比如

```javascript
var a = {name: "zhou"};
var b = a;
var c = a;
b.name = "ning";
console.log(c.name);//"ning"
```

​	改变了Sub.prototype的指向，原先construcor指向Sub，现在指向了Sup，所以需要更改constructor的指向，使其重新指向Sub

​	这个会继承父类的所有内容，包括父类的超类。

### 3.**直接继承prototype**

```javascript
function Sup(){};
Sup.prototype.name = "zhou";

Sub.prototype = Sup.prototype;
Sub.prototype.constructor= Sub;
```

​	与前一种方法相比，这样做的优点是效率比较高（不用执行和建立Sup的实例了），比较省内存。缺点是 Sub.prototype和Sup.prototype现在指向了同一个对象，那么任何对Sub.prototype的修改，都会反映到Sup.prototype。

### 4.**利用空对象作为中介**

```javascript
function extend(Sub, Sup) {
	var F = function() {}
	F.prototype = Sup.prototype;

	Sub.prototype = new F();
	Sub.prototype.constructor = Sub;
}
```

F是空对象，所以几乎不占内存。这时，修改Sub的prototype对象，就不会影响到Sup的prototype对象。

### 5.拷贝继承

```javascript
function extend(Sub, Sup) {
  var sup = Sup.prototype;
  var sub = Sub.prototype;
  for(var i in sup) {
    sub[i] = sup[i];
  }
}
```

## 非构造函数继承

参考文档：http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_inheritance_continued.html

```javascript
var Chinese = {
　　　　nation:'中国'
　　};
var Doctor ={
　　　　career:'医生'
　　}
```

### 1.**object()方法**

```javascript
function object(o) {
  var F = function() {};
  F.prototype = o;
  return new F();
}
使用：
var Doctor = object(Chinese);
Doctor.career = '医生';
```

### 2.浅拷贝

```javascript
function extendCopy(o) {
  var c ={};
  for(var i in o) {
    c[i] = o[i];
  }
  return c;
}

var Doctor = extendCopy(Chinese);
Doctor.career = '医生';
```

​	这样的拷贝有一个问题。那就是，如果父对象的属性等于数组或另一个对象，那么实际上，子对象获得的只是一个内存地址，而不是真正拷贝，因此存在父对象被篡改的可能。

### 3.深拷贝

​	所谓"深拷贝"，就是能够实现真正意义上的数组和对象的拷贝。它的实现并不难，只要递归调用"浅拷贝"就行了。

```javascript
function extendCopy(p, c) {
  var c = c || {};
  for(var i in p) {
    if (typeOf p[i] === 'object') {
      c[i] = (p[i].constructor == Array) ? [] : {};
      extendCopy(p[i], c[i]);
    } else {
      c[i] = p[i];
    }
  }
  return c;
}
```

## 原型链

​	每创建一个函数就创建一个prototype对象，这个对象自动获得constructor属性

​	每个构造函数都有一个原型对象，原型对象都包含一个指向构造函数的指针，实例都包含一个指向原型对象的内存指针。

​	我们让原型对象等于另一个类型的实例，此时的原型对象将包含一个指向另一个原型的指针，相应地，另一个原型中也包含这一个指向另一个构造函数的指针。如果另一个原型又是另一个类型的示例，上述关系依然成立，如此层层递进，就构成了实例与原型的链条。



方法

constructor和prototype是不可枚举的，遍历只能遍历可枚举的

for-in：遍历对象以及原型对象中的属性

Object.key取得对象上所有可枚举的属性，不包含原型对象

原型对象问题：

1.所有实例默认取得相同的属性值

2原型中所有属性被实例共享，引用类型的改变会反应到所有实例当中

## 作用域链

参考课本179页

```javascript
function comapre(v1, v2) {
  if (v1<v2) {
    return -1;
  } else if (v1>v2) {
    return 1;
  } else {
    return 0;
  }
}

var result = compare(5, 10);
```

​	后台的每个执行环境都有一个表示变量的对象——变量对象。全局环境的变量对象始终存在，而像compare()函数这样的局部环境变量对象，只在函数执行的过程中存在。

​	第一步：创建compare()函数时，会创建一个预先包含全局变量对象的作用域链，这个作用域链保存在内部的Scope属性中。   ×××××创建函数创建Scope，这时Scope只保存当前函数中定义的变量

​	第二步：当调用compare()函数时，会为函数创建一个执行环境，然后通过复制函数的Scope属性中的对象构建起执行环境的作用域链。    ×××××调用函数，创建执行环境，复制Scope中的对象构建执行环境的作用域链

​	第三步：又有一个活动对象（在此作为变量对象使用）被创建并被推入执行环境作用域连的前段。 ×××××将涉及到的外部活动对象添加到作用域连中

​	对于compare函数的执行环境，包含两个变量对象：本地活动对象和全局变量对象。作用域链本质上是一个指向变量对象的指针列表，它只引用但不实际包含变量对象。

​	无论什么时候在函数中访问一个变量时，就会从作用域链中搜索具有相应名字的变量。一般当函数执行完毕后，局部活动对象就会被销毁，内存中仅保存全局作用域（全局执行环境的变量对象），但闭包不同。

​	在另一个函数内部定义的函数会将包含函数（外部函数）的活动对象添加到它的作用域链中。

```javascript
function createComparisionFunction(){
  return compareFunction(v1, v2)
}
var comare = createComparisionFunction();
var result = compare(5,10);
//解除对匿名函数的引用，以便释放内存
compare = null
```

匿名函数compareFunction从createComparisionFunction中被返回后，它的作用域连被初始化为包含createComparisionFunction函数的活动对象和全局变量对象。这样匿名函数就可以访问createComparisionFunction中定义的所有变量。当createComparisionFunction执行完后，其活动对象也不会被销毁，因为匿名函数的作用域连仍然在引用这个活动对象。换句话说，当createComparisionFunction返回后，其执行环境的作用域连会被销毁，但它的活动对象仍然会留在内存中，直到匿名函数被销毁后，createComparisionFunction的活动对象才会被销毁。



















