## CSS添加方法

将 CSS 应用于文档的三种方法：

- 外部样式表
- 内部样式表
- 内联样式



### 行内样式

```html
<html>
	<head>
		<title>内联定义</title>
	</head>
	<body>
		<p style="color:red">内联定义</p>
		<p style="font-size:12px">内联定义</p>
	</body>
</html>


```



### 内嵌样式

在html文件里添加`<style type="text/css"></style>`

注意：需要将样式放入`<head></head>`中

```html
<html>
	<head>
		<style type="text/css">
			p{
				color:red;
			}
        </style>
	</head>
	<body>
		<p>链入内部css</p>
	</body>
</html>


```



```html
在css中,id前面是要加一个# ,    class前面要加一个.

<html>
	<head>
		<title>链入内部css</title>
		<style type="text/css">
			#myid
			{
				width:200px;
				height:300px;
				color:red;
			}
			.myclass
			{
				width:200px;
				height:300px;
				color:red;
			}
		</style>
	</head>
	<body>
		<p id="myid">链入内部css</p>
		<p class="myclass">链入内部css</p>
		
	</body>
</html>


```

**注意：**

- 即使有公共CSS代码，也是每个页面都要定义的
- 适合文件很少，CSS代码也不多的情况
- 如果一个网站有很多页面，每个文件都会变大，后期维护难度也大



### 外部样式表

创建css样式表如style.css，再在HTML头部中的<head>标签里链接此style.css样式表。

主要通过`<link type="text/css" rel="stylesheet" href="style.css"/> `来实现的

```html
<html>
	<head>
		<title>链入外部css</title>
        <!-- style.css和html文件在同一个文件夹，这一行代码可以利用”link:CSS“扩展快速生成 -->
		<link type="text/css" rel="stylesheet" href="style.css"/>
	</head>
	<body>
		<p id="p1">链入外部css</p>
		<p id="p2">链入外部css</p>
		<p class="p3">链入外部css</p>
	</body>
</html>


```



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>外联式css样式03</title>
    <!-- 引入外部的css样式表
        引入css样式表有两种方式:(面试)
        1.link标签引入  推荐使用
        2.@import引入
    -->
    <!-- 
        link标签引入,该标签写在head标签里(与文档配置有关,不需显示)
    -->
      <link rel="stylesheet" href="style.css">
    <!-- 
        @import引入,需要写在style标签里
    -->
    <style typle="text/css">
        @import url(style.css)
    </style>
    <!-- link与import的区别:
        1.link是html语法,import是css语法.
        2.link是在html文档加载时同时开始加载对应的css文件:@import是在html文档加载完成后才开始加载对应的css文件.
        3.link可以引入任何类型的文件,而import只能引入css文件.
        4.使用link方式引入的css样式我们在后期可以使用js的方式进行修改,但是import不可以.

        我们以后使用link

        当一个网站有多个文档时,建议使用外联式.

     -->
</head>
<body>
    <div>lalala</div>
</body>
</html>


```



**主要优点：**

- 页面结构HTML代码与样式CSS代码的完全分离
- 维护方便
- 如果需要改变网站风格，只需要修改公共CSS文件
- 可以在同一个 HTML 文档内部引用多个外部样式表



## 添加方式优先级

- 多重样式可以**层叠**，可以**覆盖**

- 样式的优先级按照“**就近原则**”：**行内样式> 内嵌样式> 链接样式**>浏览器默认样式

