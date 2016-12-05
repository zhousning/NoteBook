# 闭包

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

在一个函数内部定义的全局变量，如果函数没执行，内存当中就不存在相应的全局变量，所以在外边直接调用函数内部当中的全局变量是不管用的。如：直接调用nAdd()是不行的，因为内存当中没有，f1();nAdd();是可以的，先执行f1，在内存中添加aAdd，在window对象上创建window.nAdd = fun;，实际就是在window对象上添加一个属性，使得可以在任何地方访问

# 垃圾回收机制

http://blog.chinaunix.net/uid-26672038-id-3522560.html