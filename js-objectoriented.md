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

​	每个构造函数都有一个原型对象，原型对象都包含一个指向构造函数的指针，实例都包含一个指向原型对象的内存指针。

​	我们让原型对象等于另一个类型的实例，此时的原型对象将包含一个指向另一个原型的指针，相应地，另一个原型中也包含这一个指向另一个构造函数的指针。如果另一个原型又是另一个类型的示例，上述关系依然成立，如此层层递进，就构成了实例与原型的链条。































