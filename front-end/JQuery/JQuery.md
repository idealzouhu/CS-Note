# JQuery

## JQuery概述

### 什么是JQuery

JQuery是快速、简洁的第三方js库

JQuery的**核心理念**是write less，do more（写更少的代 码，做更多的事情）



### 为什么要使用JQuery

- DOM操作的终极简化

 > 核心DOM： 万能，但繁琐 
 >
 > HTML DOM： 简单，但不是万能
 >
 > Jquery： 对DOM操作的终极简化

- 屏蔽了浏览器的兼容性问题



### 如何使用JQuery（2种方式）

主要思想：先引入JQuery.js , 再编写自定义脚本

2种使用方式

- 将JQuery.js 下载到服务器本地，在script中使用服务器路径
- 使用CDN网络上共享的JQuery.js     **生产环境中用的最多**



### 工厂函数

在JQuery中，无论使用哪种类型的选择符，都要从一个美元符号`$`和一对圆括号开始：**$（）**

所有能在样式表中使用的选择符，都能放到这个圆括号中 的引号内。



```javascript
<ul id="myList">
<li id="m1">首页</li>
<li id="m2">企业介绍</li>
<li id="m3">联系我们</li>
</ul>

DOM:
	document.getElementById('myList');
Jquery:
	$("#myList");

```



## JQuery查找

### **1、基本选择器**

\#id .class         同CSS



### **2、层级选择器**

后代选择器 子代选择器 			同CSS



### 3、兄弟关系

`$("...").next/prev()  `      紧邻的前一个或者后一个元素

`$("...").nextAll/prevAll() `    之前或者之后的所有元素

`$("...").siblings() `   除自己之外的所有兄弟



## JQuery修改

### 1、属性

访问元素属性：

- 获取： `$("...").attr("属性名")`
- 修改：  `$("...").attr("属性名",值)`

```js
// 获取北京节点的name属性值
$bj.attr("name")
// 设置北京节点的name属性值
$bj.attr("name","beijing");

```



### 2、内容

**1)html操作**

html( )：读取或修改节点的HTML内容

```javascript
//获取<p>元素的HTML代码
$("p").html()

//设置<p>元素的HTML代码
$("p").html("<strong>你最喜欢的水果是?</strong>");

```



**2)文本操作**

text( )：读取或修改节点的文本内容

```javascript
//获取<p>元素的文本
$("p").text()

//设置<p>元素的文本
$("p").text("你最喜欢的水果是?");

```



**3)值操作**

val( )：读取或修改节点的value属性值

```javascript
//获取按钮的value值
$("input:eq(5)").val();

//设置按钮的value值
$("input").val("我被点击了!");

```



### 3、样式

**1)直接修改css属性**

获取css样式(计算后的样式)      `$("...").css("CSS属性名")`

修改css样式 					  		`$("...").css("css属性名"，值)`

**2)通过修改class批量修改样式**

①判断是否包含指定class          `$("...").hasClass("类名")`

②添加class								 `$("...").addClass("类名")`

③移除class								 `$("...").removeClass("类名")`



## JQuery添加

操作步骤为：

1、创建新元素      						 `var $new = $("html代码片段")`

2、将新元素结尾添加到DOM树   `$(parent).append($newelem)`

```javascript
var $li = $("<li title='香蕉'>香蕉</li>");
var $parent = $("ul");
$parent.append($li);
```



## JQuery删除

`$("...").remove()`

```javascript
// 获取第二个<li>元素节点后，将它从网页中删除
$("ul li:eq(1)").remove();

//把<li>元素中属性title不等于"菠萝"的<li>元素删除
$("ul li").remove("li[title!=菠萝]");

```



## JQuery事件

### 事件绑定

语法：`$("...").bind("事件类型" ，function(e){....})`

如：`$("...").bind("click" ，function(e){....})` 简写形式为 `$("...").click(function(e){....})`

```javascript
$("#btn").click(function(e){
console.log("hello");
})

```





### 事件对象

```javascript
/* e为事件对象 */
$("#btn").click(function(e){
console.log("hello");
})

```

在这段代码中，e为事件对象。

这个对象中包含与事件相关的信息，也提供了可以影 响事件在DOM中传递进程的一些方法。



事件对象记录事件发生时的鼠标位置、键盘按键状态和触 发对象等信息

```
clientX/offsetX/pageX/screenX/x：事件发生的X坐标
clientY/offsetY/pageY/screenY/y：事件发生的Y坐标
keyCode : 键盘事件中按下的按键的值
```

