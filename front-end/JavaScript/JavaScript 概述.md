## JavaScript概述

### 什么是 JavaScript

JavaScript 是一种运行于JavaScript解释器/引擎中的解释型脚本语言

> 解释型：运行之前是不需要编译的，运行之前不会 检查错误，直到碰到错误为止
>
> 编译型: 对源码进行编译，还能检查语法错误（C/C++）

**JS运行环境**：

- 独立安装的JS解释器(NodeJS)
- 嵌入在[浏览器]内核中JS解释器

**JS使用场合**： PC机，手机，平板，机顶盒



### JS组成

完整的JS是由三部分组成：

1. 核心(**ECMAScript**)
2. 文档对象模型(**DOM**,Document Object Model) 让JS有能力与网页进行对话
3. 浏览器对象模型(**BOM**,Browser Object Model) 让JS有能力与浏览器进行对话



### JS特点

1. 开发工具简单， 记事本即可
2. **无需编译**，直接由 JS引擎负责执行
3. **弱类型语言**， 由数据来决定数据类型
4. 面向对象



## JavaScript基础语法

### JS脚本添加方式

**1、将JS代码嵌入在元素"事件"中**（行内）

&ensp; onclick : 当单击元素时所做的操作    ` <div id="" onclick="JS代码">xxx</div>`

```html
<html>
<body>
  <!--注意单双引号的使用  -->
  <button onclick=" console.log( 'Hello World' ); ">
    打印消息
  </button>
</body>
</html>
```



**2、将JS代码嵌入在`<script>`标记中**（内嵌）

`<script></script>`允许出现网页的任意位置处

```html
<!--浏览器的内容排版引擎处理  -->
<html>
<body>
  页头
  <hr />
  <!-- 浏览器的脚本解释引擎处理 -->
  <script>
    document.write('<b>欢迎</b>');
    console.log('脚本执行结束了...');
  </script>
  <hr />
  页尾
</body>
</html>
```





**3、将JS代码写在外部脚本文件中(****.js)（外部文件）

- 创建 js 文件，并编写JS代码(***.js) 
- 在页面中引入 js文件    ,  `<script src="js文件路径"></script>`

```html
<html>
<head>
  <script src="myscript.js"></script>
</head>
<body>
</body>
</html>
```

注意：`<script src=""></script>`该对标记中，是**不允许出现任何内容**的

**错误**代码示例：

```html
<!-- 以下为错误的示例代码 -->
<script src="a.js">
  console.log();
</script>
```



### JS语法规范

**1、语句**

- 允许被JS引擎所解释的代码
- 使用 分号 来表示结束，例如`console.log();` 、  `document.write();`
- 大小写敏感。 `console.log();` 正确示范 、  ``Console.log();``错误示范
- 英文标点符号
- 由表达式、关键字、运算符 组成

**2、注释**

单行注释: // 、

多行注释: /* */

 sublime text中 Ctrl+/



## 变量

### 什么是变量

就是内存中的一段存储空间 

变量的名：内存空间的别名，可以自定义 

变量的值：保存在 内存空间中的数据



### 变量的声明

**声明**：var 变量名;

**赋值**：变量名=值;

> 声明过程中，**尽量不要省略 var 关键字，否则声明的 是"全局变量"**



**一次性声明多个变量并赋值**:var 变量名1,变量名2,...变量名n;

ex:  `var stuName=“PP.XZ",stuAge=25, stuHeight;`



### 变量命名规范

1. 不允许使用JS的关键字和保留关键字
2. 由字母、数字、下划线以及$组成
3. 不能以数字开头
4. 尽量见名知意
5. 可以采用"驼峰命名法



### 变量的使用

变量声明后，从未赋值，称之为 **未经初始化变量**

变量未被定义过，直接打印或使用,`console.log(stuHeight);` 结果为错误



**变量的存取操作**：

1、获取变量的值-GET操作

```javascript
var userPwd = '123456' ;
console.log( userPwd );
document.write( userPwd );

// 创建新变量，赋值为已有变量的值
var newPwd = userPwd;
```

2、保存(设置)变量的值 - SET操作

```javascript
var oldPwd = '123' ;
oldPwd = '456' ;

//这条语句对于变量newPwd是GET操作；对于变量oldPwd是SET操作
oldPwd = newPwd;
```



## 数据类型

数据类型主要分为：原始类型(**基本类型**)  、 **引用类型**

### 原始类型(**基本类型**)分类

**1、数字类型**

**number类型**可以表示32位的整数（4字节）以及64位的浮点数（8字节）



**2、字符串类型**

表示一系列的文本字符数据
由Unicode字符，数字，标点组成 
Unicode 下所有的 字符，数字，标点 在内存中 都占2字节

> "张".charCodeAt().toString(16) 
> 查看 字符 "张" 的 十六进制 表现方式 
> 结果为：5f20
> \u4e00 : 汉字的起始字符
> \u9fa5 : 汉字的结束字符 
> 转义字符： 1、\n 换行 2、\r 回车 3、\t 一个制表符




**3、布尔类型**

**boolean**类型         作用: 用于表示 条件的结果

除条件判断外，做运算时，true可以当 做1运算，false当做0运算



**4、空**

声明对象未赋值, **null**



**5、未定义**

**undefined**

1、声明变量未赋值 

2、访问对象不存在的属性



### 数据类型转换

**1、隐式转换**

自动转换，由JS在运算过程中，自己进行转换的操作，不需人为参与

**函数**：typeof() 或 typeof

```javascript
var num1 = 15;
var s = typeof(num1); //获取 num1 的数据类型
var s1 = typeof num1; //获取 num1 的数据类型

```

**NaN**：

Not a Number 不是一个数字

**isNaN**(数据) : 判断 数据是否为非数字

注意：所有的数据类型 与 string 做 + 运算时，最后的结果都为 string



**2、转换函数 - 显示转换(强制转换)**

- toString()   将任意类型的数据转换为 string 类型

- parseInt()

  >  整型：Integer 
  >
  >  作用：获取 指定数据的**整数**部分 
  >
  >  语法： var result = parseInt(数据); 
  >
  >  注意：
  >  parseInt ， 从左向右依次转换，**碰到第一个非整数字符，则停止转换**。如果第一个字符就是非整数字符的话，结果为 NaN

- parseFloat()

  > Float ：浮点类型->小数 
  > 作用：将 指定数据转换成**小数 **
  > 语法： var result = parseFloat(数据); 
  > ex : 
  > var result=parseFloat("35.25"); //35.25 
  > var result=parseFloat("35.2你好!");//35.2 
  > var result=parseFloat("你好35.2");//NaN

- Number()

  > 作用：将一个字符串解析为number 
  > 语法：var result=Number(数据);
  > 注意：**如果包含非法字符，则返回NaN**



## 运算符和表达式

运算符：能够完成数据计算的一组符号 比如：+,-,*,/

表达式： 由运算符和操作数所组成的式子叫作表达式， 每个表达式都有自己的值

### 运算符

#### 算数运算符

加(＋)、 减(－)、 乘(*) 、除(/) 、求余(% )

－ 可以表示减号，也可以表示负号，如：x=-y

 + 可以表示加法，也可以用于字符串的连接

% : 取余操作，俗称模     作用：取两个数字的余数     使用场合：1、判断数字的奇偶性  2、获取数字的最后几位

++ : 自增，在数值的基础上，进行+1操作

 -- : 自减，在数值的基础上，进行-1操作



#### 关系运算符

判断数据之间的大小关系，关系表达式的运算结果为 boolean类型(true 或 false)

- `>`  `<`  `>=`   `<=`

- 判断等于`==`   不等于`!=`

  > 注意：**不比较类型，只比较数值**

- 全等`===`  不全等 `1==`

  >注意：**除数值之外，连同类型也会一起比较**

- isNaN(数据)

  > isNaN()会抛开数据类型来判断数据是否为数字 
  >
  > 如果 数据 是数字类型，则返回 false 
  >
  > 如果 数据 不是数字类型，则返回 true



#### 逻辑运算符

逻辑与 : &&    逻辑或 : ||    逻辑非 : !

表达式1?表达式2:表达式3;





## 函数

函数(function)，也可以被称之为方法(method),或者 过程(procedure)    ，是一段**预定义好**，并可以被**反复使用**的**代码块**。其 中可以包含多条可执行语句

> 预定义好:事先声明好，但不被执行 
>
> 反复使用：允许被多个地方(元素，函数中)所应用 
>
> 代码块：允许包含多条可执行的代码 函数本质上是功能完整的对象



### 函数定义

- function 函数名( )  { 

  可执行语句; 

  }

- function 函数名(参数列表声明){ 

  ​    //代码块(函数体，功能体，方法体)

   }



### 函数调用

执行函数中的内容 任何 JS 的合法位置处，都允许调用函数 

语法：函数名称();



### 作用域

作用域就是变量或函数的可访问范围。它控制着变量或函 数的可见性和生命周期。

- 函数作用域，只在当前函数内可访问
- 全局作用域，一经定义，代码的任何位置都可以方式



### 声明提前

JS在正式执行之前，会将所有var声明的变量和function声明的 函数，预读到所在作用域的顶部 但是，对变量的赋值，还保留在原来的位置处

![image-20220628160359883](images/image-20220628160359883.png)





## 程序结构

### 顺序结构



### 分支结构

主要有if...else    switch-case   结构。

**switch-case在实现分支功能时和if-else的主要区别在于：**

- if...else... 可以 判定相等或不 等的情形，适用性更广；
- switch...case...  结构的结构更清晰、效率更高；
- switch...case...  一般只用于指 定变量相等于某 个范围内的某个 特定的值



注意：

if语句，**条件位置处**，必须为 boolean的值/表达式/变量

 如果条件不是boolean 类型的话，JS会自动进行转换

 以下情况，if 都会认为是 false

 **if(0/0.0/"“/null/undefined/NaN){}**

除以上情况外，一律为真



### 循环结构





## 数组

### 索引数组(下标为数字的数组)

**创建空数组**（使用时机: 在创建数组时，还不知道数组中的元素内容时），主要有两种创建方式：

-  数组直接量:` var arr=[];`
-  用`new: var arr=new Array();`



**创建数组同时初始化**

- 数组直接量: var arr=[元素1,元素2,...];
- 用new: var arr=new Array(元素1,元素2,...);



**数组的length属性:** 

记录了数组中理论上的元素个数 length属性的值永远是最大下标+1

```javascript
var arr5 = [ ]; //长度为0
arr5[0] = 87; //长度变为1
arr5[3] = 98; //长度变为4
```



**特殊：**

- 不限制数组的元素个数:**长度可变**

- 不限制**下标越界**: 

  > 获取元素值: **不报错**！返回undefined 
  >
  > 修改元素值: **不报错**! 自动在指定位置创建新元素，并且自动修改length属性为最大下标+1 
  >
  > 如果下标不连续的数组——稀疏数组

- 不限制元素的**数据类型**



### **关联数组（可自定义下标名称的数组）**

索引数组中的数字下标没有明确的意义。

但是，关联数组可自定义下标名称的数组，使每个元素都有专门的名称（即明确的意义）



创建流程（2步）：

1. 创建空数组

2. 向空数组中添加新元素，并自定义下标名称

```javascript
var bookInfo = [ ];
bookInfo['bookName'] = '西游记' ;
bookInfo['price'] = 35.5 ;
```



由于关联数组的 length 属性值无法获取其中元素的数量，所以遍历关联数组**只能使用 for..in 循环**

```javascript
for(var key in hash){
key//只是元素的下标名
hash[key]//当前元素值
}
```



### 索引数组与关联数组的对比

![image-20220628164800561](images/image-20220628164800561.png)



![image-20220628164819705](images/image-20220628164819705.png)



### 数组API

#### 数组转字符串

**1、String(arr) **

将arr中每个元素转为字符串，用逗号分隔

固定套路: 对数组拍照: 用于鉴别是否数组被修改过



**2、arr.join(“连接符”) ：**

将arr中每个元素转为字符串， 用自定义的连接符分隔 

固定套路：

- 将字符**组成单词**: `chars.join("")`->无缝拼接

​        扩展: 判断数组是空数组: arr.join("")==""

```javascript
//将字符拼接为单词
var chars=["H","e","l","l","o"];
console.log(chars.join("")); //Hello
```

- 将单词**组成句子**:` words.join(" ")`
- 将数组转化为**页面元素的内容**:

​       `"<开始标签>"+ arr.join("<开始标签>") +"</结束标签>"`



### 拼接

**concat()** 拼接两个或更多的数组，并返回结果

**语法：**`var newArr=arr1.concat(值1,值2,arr2,值3,...)`

> 将值1,值2和arr2中每个元素,以及值3都拼接到arr1的元素 之后，返回新数组 
>
> 其中: arr2的元素会被先**打散** ，再拼接

```javascript
var arr1 = [90, 91, 92];
var arr2 = [80, 81];
var arr3 = [70, 71, 72,73];
var arr4 = arr1.concat(50, 60, arr2, arr3);

console.log( arr1 ); //现有数组值不变
console.log( arr4 );
```



### 选取

**slice()** 返回现有数组的一个子数组

**语法：**`var subArr=arr.slice(starti,endi+1)`

> 选取arr中starti位置开始，到endi结束的所有元素组成新数组返回——原数组保持不变
>
> 强调: 凡是两个参数都是下标的函数， 都有一个特性:**含头不含尾**

```javascript
var arr1 = [10, 20, 30, 40, 50];
var arr2 = arr1.slice(1, 4); 	//20,30,40
var arr3 = arr1.slice(2);       //30,40,50
var arr4 = arr1.slice(-4, -2);	//20,30 (反向选取)
console.log(arr1); //现有数组元素不变
```



**选取简写：**

- 一直选取到结尾: 可省略第二个参数
- 如果选取的元素离结尾近: 可用倒数下标:`arr.slice(arr.length-n,arr.length-m+1) `;可简写为:`arr.slice(-n,-m+1);`
- 复制数组:`arr.slice(0,arr.length); `可简写为:`arr.slice();`



### 删除

**splice** 直接修改原数组

**语法：**`arr.splice(starti,n);`

> 删除arr中starti位置开始的n个元素,   不考虑含头不含尾
>
> 其实: var deletes=arr.splice(starti,n); 返回值deletes保存了被删除的元素组成的临时数组



### 插入

`arr.splice(starti,0,值1,值2,...)`

> 在arr中starti位置，插入新值1,值2,...
>
> 原starti位置的值 及其之后的值被向后顺移



### 替换

`arr.splice(starti,n,值1,值2,...)`

> 先删除arr中starti位置的n个值，再在starti位置插入新值
>
> 其实就是删除旧的，插入新的
>
> 强调: 删除的元素个数和插入的新元素个数不必一致。



### 颠倒

reverse() 颠倒数组中元素的顺序

强调: 仅负责原样颠倒数组，不负责排序



### 排序

arr.sort(): 默认将所有元素转为字符串再排列

> 问题: 只能排列字符串类型的元素 
>
> 解决: 使用自定义比较器函数



## DOM

### 什么是DOM

DOM （document object model）是 W3C（万维网联盟）的标准， 是中立于平台和语言的**接口**，它允许程序和脚本**动态地访问和更新**文档的内容、结构和样式。

实际上，DOM是对网页进行增删改查的操作。

![image-20220614215905509](images/image-20220614215905509.png)



**常用的DOM操作：**

- 查找节点
- 读取节点信息
- 修改节点信息
- 创建新节点
- 删除节点



**常用DOM方法：**

![image-20220628183645983](images/image-20220628183645983.png)



![image-20220628193816616](images/image-20220628193816616.png)







### DOM查找

#### 1、按id属性，精确查找一个元素对象

**语法：**`var elem=document.getElementById("id")`  **效率非常高**

> 强调：
>
> 何时：
>
> 问题：



#### 2、按标签名找

**语法：**`var elems=parent.getElementsByTagName("tag");` 查找指定parent节点下的所有标签为tag的子代节点

> 强调：
>
> -  可用在任意父元素上
> -  不仅查直接子节点，而且查**所有**子代节点
> -  返回一个动态集合。（即使只找到一个元素，也返回集合）必须用<font color='red'> [0] </font>,取出唯一元素
>
> 问题：不但找直接，而且找所有子代。（查找范围有点大，不太准确）



实际案例

```html
<ul id="myList">
  <li id="m1">首页</li>
  <li id="m2">企业介绍</li>
  <li id="m3">联系我们</li>
</ul>
```

```css
var ul = document.getElementById('menuList');
var list = ul.getElementsByTagName('li');
console.log(list );
```



#### 3、通过name属性查找

**语法：**`document.getElementsByName(‘name属性值’)`   可以返回DOM树中具有指定name属性值的所有子元素集合。



实际案例

```html
<form id="registerForm">
  <input type="checkbox" name="boy"/>
  <input type="checkbox" name="boy"/>
  <input type="checkbox" name="boy"/>
</form>
```

```css
var list = document.getElementsByName(‘boy');
console.log( typeof list );
```

 



#### 4、通过class查找

**语法：**`var elems=parent.getElemnetsByClassName("class");`  查找父元素下指定class属性的元素  （有兼容性问题: IE9+）

**实际案例：**

```html
<div id="news">
  <p class="mainTitle">新闻标题1</p>
  <p class="subTitle">新闻标题2</p>
  <p class=" mainTitle ">新闻标题3</p>
</div>
```

```css
var div = document.getElementById('news');
var list = div.getElementsByClassName('mainTitle');
console.log(list );
```

 

#### 5、通过CSS选择器查找

**1）只找一个元素**

**语法：**`var elem=parent.querySelector("selector")`

> selector支持一切css中选择器
>
> 如果选择器匹配的有多个，只返回第一个



**2）找多个**

**语法：**`var elem=parent.querySelector("selector")`

> selector API 返回的是非动态集合



#### 随机验证码

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
	#code{
		width: 100px;
		height: 50px;
		background-color: lightblue;
		font-size: 44px;
		letter-spacing: 5px;
	}
	</style>
</head>
<body>
	<script>
	function createRandCode(){
		var chars=['a','b','c','1','2','3'];
		var randcode="";
		for(var i=0;i<3;i++){//3位随机码
			var randpos =Math.floor(Math.random() * chars.length);	//视频修改，以这个为准		
			randcode+= chars[randpos];
		}
		document.getElementById("code").innerHTML=randcode;
	}
	</script>
	<div id="code"></div>
	<button onclick="createRandCode()">验证码</button>
</body>
</html>
```



### DOM修改

核心DOM的4个操作

#### 1、读取属性值（2种方法）

- 先获得属性节点对象，再获得节点对象的值:

  ```javascript
  var attrNode=elem.attributes[下标/属性名];
  var attrNode=elem.getAttributeNode(属性名)
  attrNode.value——属性值
  ```

- 直接获得属性值

  ```javascript
  var value=elem.getAttribute("属性名");
  ```

  

#### 2、修改属性值

**语法：**`elem.setAttribute("属性名", value);`

```javascript
var h1 = document.getElementById(“a1");
h1.setAttributeNode(“name”，zhangji);

```



#### 3、判断是否包含指定属性

**语法：**`var bool=elem.hasAttribute("属性名")`

element.hasAttribute('属性名')   //true或false

```javascript
document.getElementById('bt1').hasAttribute('onclick');

```



#### 4、移除属性

**语法：**`elem.removeAttribute("属性名")`

实例：

```html
<a id=“alink" class="slink" href=
"javascript:void(0)" onclick="jump()">百度搜索</a>


var a = document.getElementById('alink');
a.removeAttribute('class');

```



#### 修改样式

![image-20220628194700254](images/image-20220628194700254.png)



### DOM添加

#### 添加元素的步骤

##### 1）创建空元素

**语法：**`var elem=document.createElement("元素名")`

```javascript
var table = document.createElement('table');
var tr= document.createElement('tr');
var td= document.createElement('td');
var td= document.createElement('td');
console.log( table );

```



##### 2）设置关键属性

设置关键属性：

```html
a.innerHTML="go to tmooc"
a.herf="http://tmooc.cn";
<a href="http://tmooc.cn">go to tmooc</a>
```

设置关键样式：

```javascript
a.style.opacity = "1";
a.style.cssText = "width: 100px;height: 100px";
```



##### 3）将元素添加到DOM树

**语法：**`parentNode.appendChild(childNode)`   可用于将为一个父元素追加最后一个子节点

```javascript
var div = document.createElement( 'div' );
var txt = document.createTextNode('版权声明');
div.appendChild(txt);
document.body.appendChild(div);

```



**语法：**`parentNode.insertBefore(newChild, existingChild)`   用于在父元素中的指定子节点之前添加一个新的子节点

```javascript
<ul id="menu">
   <li>首页</li>
    <li>联系我们</li>
</ul>

var ul = document.getElementById('menu');
var newLi = document.createElement('li');
ul.insertBefore(newLi, ul.lastChild);

```



#### 添加元素优化

尽量少的操作DOM树

为什么：每次修改DOM树，都导致重新layout



为了避免layout，我们应该

- 如果同时创建父元素和子元素时，建议在内存中先将 子元素添加到父元素，再将父元素一次性挂到页面
- 如果只添加多个平级子元素时, 就要将所有子元素， 临时添加到文档片段中。再将**文档片段**整体添加到页面





**文档片段：**内存中，临时保存多个平级子元素的 虚拟父元素 用法和普通父元素完全一样

**文档片段使用方式：**

1. 创建片段   `var frag=document.createDocumentFragment();`
2. 将子元素临时追加到frag中  `frag.appendChild(child);`
3. 将frag追加到页面   ` parent.appendChild(frag);`   强调： append之后，frag自动释放，不会占用元素





## BOM

### 什么是BOM

BOM（Browser Object Model）是专门操作浏览器窗口的API——没 有标准, 有兼容性问题



浏览器对象模型如下

![image-20220629112145470](images/image-20220629112145470.png)



**获取当前窗口大小：**

- 完整窗口大小：`window.outerWidth/outerHeight`
- 文档显示区大小：`window.innerWidth/innerHeight`



### 定时器

定时器主要让程序按指定时间间隔自动执行任务，主要包含功能有：网页动态效果、计时功能等

定时器分类：周期性定时器  、一次性定时器



**1、周期性定时器**

让程序按指定时间间隔反复自动执行一项任务

语法：`setInterval(exp,time)`

> setInterval(exp,time)：周期性触发代码exp
>
> exp：执行语句       time：时间周期，单位为毫秒

```javascript
setInterval(function(){
console.log("Hello World");
},1000);
```



**停止定时器步骤**：

1. 给定时器取名

   ```javascript
   /*定时器取名为timer  */
   var timer = setInterval(function(){
   console.log("Hello World");
   },1000);
   ```

   

2. 停止定时器

```javascript
clearInterval(timer);
```



**2、一次性定时器**

让程序延迟一段时间执行

语法：setTimeout(exp,time)

> setTimeout(exp,time)：**一次性**触发代码exp
>
> exp：执行语句       time：时间周期，单位为毫秒

```javascript
setTimeout(function(){
alert("恭喜过关");
},3000);
```



