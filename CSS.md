# DOCTYPE

参考文档：http://harttle.com/2016/01/22/doctype.html

**DTD**（document type definition，文档类型定义）是一系列的语法规则， 用来定义XML或(X)HTML的文件类型。浏览器会使用它来判断文档类型， 决定使用何种协议来解析，以及切换浏览器模式。

> 事实上DTD可以定义所有[SGML](https://en.wikipedia.org/wiki/Standard_Generalized_Markup_Language)语族的文档类型，但由于太过繁琐， XML Schema反而更加流行。

多数[HTML](http://harttle.com/tags.html#HTML)编辑器都会为我们添加一行DOCTYPE声明，但DOCTYPE却是我们最容易忽略的部分。 下面我们会看到，DOCTYPE声明**并不是**可有可无的。

```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"
  "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
```

DOCTYPE是用来声明文档类型和DTD规范的，一个主要的用途便是文件的合法性验证。 如果文件代码不合法，那么浏览器解析时便会出一些差错。 [HTML](http://harttle.com/tags.html#HTML)编辑器通常也会在语法高亮的同时提供合法性验证。

DOCTYPE声明包括标准版本和一个DTD文件的URI。常用的DOCTYPE声明有以下几种：

> 以下代码来自 http://www.w3school.com.cn/tags/tag_doctype.asp

## HTML 5

```
<!DOCTYPE html>

```

## HTML 4.01 Strict

该 DTD 包含所有 [HTML](http://harttle.com/tags.html#HTML) 元素和属性，但不包括展示性的和弃用的元素（比如 font）。不允许框架集（Framesets）。

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">

```

## HTML 4.01 Transitional

该 DTD 包含所有 [HTML](http://harttle.com/tags.html#HTML) 元素和属性，包括展示性的和弃用的元素（比如 font）。不允许框架集（Framesets）。

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" 
"http://www.w3.org/TR/html4/loose.dtd">

```

## 浏览器模式

为了能够很好地显示满足标准的页面，又能最大程度兼容不合法的HTML。 浏览器厂商一般会提供两种浏览器模式：

- *标准模式*（standards mode）：浏览器根据标准规约来渲染页面。
- *混杂模式*（quirks mode）：浏览器采用更加宽松的、向后兼容的方式来渲染页面。

混杂模式下，浏览器会模仿旧浏览器的行为，比如IE6，在此基础上兼容新的标准特性。 混杂模式又称兼容模式、怪异模式等。

## DOCTYPE切换

浏览器根据不同的DOCTYPE选择不同的渲染方法就叫做*DOCTYPE切换*。 其实DOCTYPE切换就是用来识别和兼容旧网页的。

以下情况浏览器会采用标准模式渲染：

- 给出了完整的DOCTYPE声明
- DOCTYPE声明了Strict DTD
- DOCTYPE声明了Transitional DTD和URI

以下情况浏览器会采用混杂模式渲染：

- DOCTYPE声明了Transitional DTD但未给出URI
- DOCTYPE声明不合法
- 未给出DOCTYPE声明

如果你是使用最新标准编写的页面但未给出DOCTYPE声明，这时就可能会出现一些怪异的行为。 例如盒模型不正确、窗口的`size`不正确等问题。所以，尽量为你网站的所有页面都给出合法的DOCTYPE声明。

# 盒子模型

## 1. 什么是CSS盒模型

盒模型，顾名思义，就是一个盒子。生活中的盒子，有长宽高，盒子本身也有厚度，可以用来装东西。页面上的盒模型我们可以理解为，从盒子顶部俯视所得的一个平面图，盒子里装的东西，相当于盒模型的内容（content）；东西与盒子之间的空隙，理解为盒模型的内边距（[padding](http://www.yidianzixun.com/m/channel/keyword/padding?display=padding&word_id=padding&type=token)）；盒子本身的厚度，就是盒模型的边框（[border](http://www.yidianzixun.com/m/channel/keyword/border?display=border&word_id=border&type=token)）；盒子外与其他盒子之间的间隔，就是盒子的外边距（margin）。

元素的**外边距（margin）、边框（border）、内边距（padding）、内容（content）**就构成了CSS盒模型。

![img](assets/css/boxmodel.jpeg)

**图1. 盒模型示意图**

## 2. IE盒模型和[W3C](http://www.yidianzixun.com/m/channel/keyword/w3c?display=w3c&word_id=w3c&type=token)盒模型

CSS盒模型分为IE盒模型（**图2**）和W3C盒模型（**图3**）。其实，IE盒模型是怪异模式（Quirks Mode）下的盒模型，而W3C盒模型是标准模式（[Standards Mode](http://www.yidianzixun.com/m/channel/keyword/standards%20mode?display=standards%20mode&word_id=standards^^mode&type=token)）下的盒模型。

[IE6](http://www.yidianzixun.com/m/channel/keyword/ie6?display=ie6&word_id=ie6&type=token)及其更高的版本，还有现在所有标准的浏览器都遵循的是W3C盒模型，IE6以下版本的浏览器遵循的是IE盒模型。

![img](assets/css/ieboxmodel.jpeg)

**图2. IE盒模型**

![img](assets/css/stdboxmodel.jpeg)

**图3. W3C盒模型**

从上图直观的可以看出，IE盒模型的宽度或者高度计算方式为：**width/height= content + padding + border**，W3C盒模型的宽度或者高度计算方式为：**width/height = content**。

举一个简单的例子：一个[div](http://www.yidianzixun.com/m/channel/keyword/div?display=div&word_id=div&type=token)的宽度和高度为100px，内边距为10px，边框为5px，外边距为30px。**图4**为不同模型下显示的结果，W3C盒模型下显示的div所占的总宽度和总高度（包括外边距、边框、内边距、内容）为100 + 10 + 5 + 30 = 145px，IE盒模型下显示的[div](http://www.yidianzixun.com/m/channel/keyword/div?display=div&word_id=div&type=token)所占的总宽度和总高度（包括外边距、边框、内边距、内容）为100 + 30 = 130px。很明显的区别，如果元素的宽度（width）一定的情况下，W3C盒模型的宽度（width）不包括内边距和边框，IE盒模包括。

代码如下：

页面效果如下：

![img](http://i1.go2yd.com/image.php?url=0F5LDBr59l)

**图4. 区别**

## 3. [CSS3](http://www.yidianzixun.com/m/channel/keyword/css3?display=css3&word_id=css3&type=token)属性box-sizing

如果计算一个盒子的长宽高，我们一般都是盒子本身的厚度加上盒子里的空间大小，所在在IE盒模型和W3C盒模型，我们会觉得IE盒模型更符合逻辑。

不同的人有不同的习惯，所以[CSS3](http://www.yidianzixun.com/m/channel/keyword/css3?display=css3&word_id=css3&type=token)新增了一个属性**box-sizing: content-box | border-box | inherit**，默认值为**content-box**。如果值为**content-box**，那元素遵循的是W3C盒模型；如果值为**border-box**，那元素遵循的是IE盒模型；如果值为**inherit**，该属性的值应该从父元素继承。

## 4. 关于盒模型的使用

有没有人和我一样，觉得属性**box-sizing**真是个好东西，只需设置所有元素的该属性为**content-box**或者**border-box**，满足自己的习惯。

虽说现在的浏览器都兼容该属性（如上图），还是得以防万一，在属性前最好暂时加**-webkit-**和**-moz-**前缀。

在上图，我们看到IE兼容属性**box-sizing**必须是8或者更高的版本，其他浏览器都可以自动升级，兼容性不担心，那如果是IE7、IE6或者更低的版本，怎么办？还有，如果我们不用该属性，那浏览器该选择哪种盒模型呢？

<u>其实，浏览器选择哪个盒模型，主要看浏览器处于标准模式（[Standards Mode](http://www.yidianzixun.com/m/channel/keyword/standards%20mode?display=standards%20mode&word_id=standards^^mode&type=token)）还是怪异模式（Quirks Mode）。我们都记得声明吧，这是告诉浏览器选择哪个版本的[HTML](http://www.yidianzixun.com/m/channel/keyword/html?display=html&word_id=html&type=token)，后面一般有[DTD](http://www.yidianzixun.com/m/channel/keyword/dtd?display=dtd&word_id=dtd&type=token)的声明，如果有DTD的声明，浏览器就是处于标准模式；如果没有DTD声明或者[HTML](http://www.yidianzixun.com/m/channel/keyword/html?display=html&word_id=html&type=token)4一下的DTD声明，那浏览器按照自己的方式解析代码，处于怪异模式。</u>

<u>处于标准模式的浏览器（IE浏览器版本必须是6或者6以上），会选择W3C盒模型解析代码；处于怪异模式的浏览器，则会按照自己的方式去解析代码，IE6以下则会是选择IE盒模型，其他现代的浏览器都是采用[W3C](http://www.yidianzixun.com/m/channel/keyword/w3c?display=w3c&word_id=w3c&type=token)盒模型。</u>

<u>因为[IE6](http://www.yidianzixun.com/m/channel/keyword/ie6?display=ie6&word_id=ie6&type=token)以下版本的浏览器没有遵循[Web](http://www.yidianzixun.com/m/channel/keyword/web?display=web&word_id=web&type=token)标准，不论页面开头有没有[DTD](http://www.yidianzixun.com/m/channel/keyword/dtd?display=dtd&word_id=dtd&type=token)声明，它都是按照IE盒模型解析代码的。</u>

总结以上三段话：IE6以上DTD声明是标准盒模型，未声明是IE盒模型，我测试的结果是：IE6以上不管有无声明DTD，都是用的标准盒模型

​				IE6以下都是用IE盒模型 

自己的话：盒模型是什么，IE6以上总结以上三段话，IE6以下因为没有遵循Web标准，不论XXX，其他浏览器都使用标准盒模型

# 尺寸单位

## PX特点

1. IE无法调整那些使用px作为单位的字体大小；


2. 国外的大部分网站能够调整的原因在于其使用了em或rem作为字体单位；


3. Firefox能够调整px和em，rem，但是96%以上的中国网民使用IE浏览器(或内核)。 

px像素（Pixel）。相对长度单位。像素px是相对于显示器屏幕分辨率而言的。(引自CSS2.0手册)

##  em

​	em是相对长度单位。相对于当前对象内文本的字体尺寸。如当前对行内文本的字体尺寸未被人为设置，则相对于浏览器的默认字体尺寸。(引自CSS2.0手册)

       任意浏览器的默认字体高都是16px。所有未经调整的浏览器都符合: 1em=16px。那么12px=0.75em,10px=0.625em。为了简化font-size的换算，需要在css中的body选择器中声明Font-size=62.5%，这就使em值变为 16px*62.5%=10px, 这样12px=1.2em, 10px=1em, 也就是说只需要将你的原来的px数值除以10，然后换上em作为单位就行了。

EM特点 

1. em的值并不是固定的；
2. em会继承父级元素的字体大小。 

所以我们在写CSS的时候，需要注意两点：

1. body选择器中声明Font-size=62.5%；

2. 将你的原来的px数值除以10，然后换上em作为单位；

3. 重新计算那些被放大的字体的em数值。避免字体大小的重复声明。

   （ 也就是避免1.2 * 1.2= 1.44的现象。比如说你在#content中声明了字体大小为1.2em，那么在声明p的字体大小时就只能是1em，而不是1.2em, 因为此em非彼em，它因继承#content的字体高而变为了1em=12px。）

62.5%：

《响应式Web设计实践》：桌面浏览器默认页面字体大小是16px，这种情况下设置成具体像素大小或者对应的百分比，看起来的效果是一样的，但是其他类型的设备的默认字体大小不一定是16px,特别是高分辨率的设备，16px大小的字体在它们上面看起来会非常小，所以不能在body上设置具体像素值，设置成百分比，可以按照设备的基准字体大小给编写的网页设置好最适合用户浏览的字体大小。 书中原文：最重要的不是屏幕实际的像素大小，屏幕上文字的可读性才是最重要的。

## rem

rem特点 

        rem是CSS3新增的一个相对单位（root em，根em）。，与em不同的是，[rem](http://www.yidianzixun.com/m/channel/keyword/rem?display=rem&word_id=rem&type=token)相对的是[html](http://www.yidianzixun.com/m/channel/keyword/html?display=html&word_id=html&type=token)的根元素。只改变跟元素的字体大小就可以按比例调整所有字体大小。这个单位可谓集相对大小和绝对大小的优点于一身，通过它既可以做到只修改根元素就成比例地调整所有字体大小，又可以避免字体大小逐层复合的连锁反应。目前，除了IE8及更早版本外，所有浏览器均已支持rem。对于不支持它的浏览器，应对方法也很简单，就是多写一个绝对单位的声明。这些浏览器会忽略用rem设定的字体大小。下面就是

一个例子：

p {font-size:14px; font-size:.875rem;}

## 注意 

        选择使用什么字体单位主要由你的项目来决定，如果你的用户群都使用最新版的浏览器，那推荐使用rem，如果要考虑兼容性，那就使用px,或者两者同时使用。

一个px,em,rem单位转换工具，地址：[http://pxtoem.com/](http://pxtoem.com/)

# html文档

参考文档：http://www.yidianzixun.com/n/0EwdBnE0?s=9&appid=xiaomi&ver=3.7.8&utk=4lxc4q7c

## 正常流：

​	指正常西方语言文本从左向右、从上向下显示，这也是我们熟悉的传统HTML文档的文本布局。

​	正常流中块级元素框的水平部分总和就等于父元素的width。

如果两个外边距都设置为auto，**它们会设置为相等的长度，因此将元素在其父元素中居中。**将两个外边距设置为相等的长度是将元素居中的一种正确方法，这不同于使用text-align。text-align只应用于块级元素的内联元素，所以将元素的text-align设置为center并不能将这个元素居中

## 百分数高度

如果没有显式地声明包含块的height，**百分数高度会重置为auto**。这时子元素将与包含块本身的高度完全相同。

如下：外层div没有设置高度，p虽然设置了50%，但会被重置为auto

```html
<div style="">
  <p style="height:50%">half</p>
</div>
```

# margin

参考文档：http://blog.csdn.net/bingqingsuimeng/article/details/41892433

​	margin也能用于内联元素，这是规范所允许的，但是margin-top和margin-bottom对内联元素（对行）的高度没有影响，并且由于边界效果(margin效果)是透明的，他也没有任何的视觉影响。margin-left/margin-right还是能够对内联元素产生影响的。

​	在内联元素中还有上文我们提到的非可置换inline元素（non-replaced element），这些个元素img|input|select|textarea|button|label 虽然是内联元素，但margin依旧可以影响到他的上下左右！

​	改变内联元素的行高即类似文本的行间距，只能使用这三个属性：line-height，fong-size，vertical-align。

​	总结下来margin 属性可以应用于几乎所有的元素，除了表格显示类型（不包括 table-caption, table and inline-table）的元素，而且垂直外边距对非置换内联元素（non-replaced inline element）不起作用。

**用Margin还是用Padding**
何时应当使用margin：
需要在border外侧添加空白时。
空白处不需要背景（色）时。
上下相连的两个盒子之间的空白，需要相互抵消时。如15px + 20px的margin，将得到20px的空白。

何时应当时用padding：
需要在border内测添加空白时。
空白处需要背景（色）时。
上下相连的两个盒子之间的空白，希望等于两者之和时。如15px + 20px的padding，将得到35px的空白。

个人认为：**margin是用来隔开元素与元素的间距；padding是用来隔开元素与内容的间隔。margin用于布局分开元素使元素与元素互不相干；padding用于元素与内容之间的间隔，让内容（文字）与（包裹）元素之间有一段“呼吸距离”。**



**垂直外边距合并问题**

​	垂直外边距合并问题常见于第一个子元素的margin-top会顶开父元素与父元素相邻元素的间距，而且只在标准浏览器下(FirfFox、Chrome、Opera、Sarfi)产生问题，IE下反而表现良好。例子可以查看下面代码(IE下表现“正常”，标准浏览器下查看出现“bug”)：

问题现象，middle没设置border或padding

![zhengchebing](assets/css/marginhebingwenti.png)

本来应该是这样，middle设置了border正常

![zhengchebing](assets/css/marginhebingzc.png)

```html
<style>
 
.top{width:160px; height:50px; background:#ccf;}
 
.middle{width:160px; background:red;}
 
.middle .firstChild{background:blue; margin-top:20px;}
 
</style>
 
</head>
 
<body>
 
<div class="top"></div>
 
<div class="middle">
 
  <div class="firstChild">我其实只是想和我的父元素隔开点距离。</div>
 
  <div class="secondChild"></div>
 
</div>
 
</body>
```

 	**根据规范，一个盒子如果没有上补白(padding-top)和上边框(border-top)，那么这个盒子的上边距会和其内部文档流中的第一个子元素的上边距重叠**。

​	再说了白点就是：父元素的第一个子元素的上边距margin-top如果碰不到有效的border或者padding.就会不断一层一层的找自己“领导”(父元素，祖先元素)的麻烦。只要给领导设置个有效的 border或者padding就可以有效的管制这个目无领导的margin防止它越级，假传圣旨，把自己的margin当领导的margin执行。

​	对于垂直外边距合并的解决方案上面已经解释了，为父元素例子中的middle元素增加一个border-top或者padding-top即可解决这个问题。





# 优先级

参考文档：http://www.cnblogs.com/xugang/archive/2010/09/24/1833760.html

（外部样式）External style sheet <（内部样式）Internal style sheet <（内联样式）Inline style

如果外部样式放在内部样式的后面，则外部样式将覆盖内部样式。

**CSS **优先级法则：

A  选择器都有一个权值，权值越大越优先；

B  当权值相等时，后出现的样式表设置要优于先出现的样式表设置；

C  创作者的规则高于浏览者：即网页编写者设置的CSS 样式的优先权高于浏览器所设置的样式；

D  继承的CSS 样式不如后来指定的CSS 样式；

E  在同一组属性设置中标有“!important”规则的优先级最大

# 选择器

child子节点中查找，type同类型子节点当中查找

:only-child：只有一个

:only-of-type：只有一种，在所有兄弟节点当中，只有这一个这种类型的

```
<b>XXX</b>
<p>XXX</p>
<b>xxxx</b>
only-of-type匹配p
```

first、last、only、nth、nth-last

# 媒体查询

参考文档：http://www.yidianzixun.com/n/0F1CYcOa?s=9&appid=xiaomi&ver=3.7.8&utk=4lxc4q7c

响应式headerhttp://www.yidianzixun.com/n/0F5OxzYg?s=9&appid=xiaomi&ver=3.7.8&utk=4lxc4q7c



http://www.yidianzixun.com/n/0F0L9JFB?s=9&appid=xiaomi&ver=3.7.8&utk=4lxc4q7c