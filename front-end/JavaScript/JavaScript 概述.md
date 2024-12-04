## 一、JavaScript概述

### 1.1 什么是  JavaScript

JavaScript 是一种运行于JavaScript解释器/引擎中的解释型脚本语言， 可以用来创建更新的内容，控制多媒体，制作图像动画等。

![标准 web 技术的三层——HTML、CSS 和 JavaScript](images/cake.png)



### 1.2   JavaScript 可以实现的常见功能

客户端 JavaScript 语言的核心包含一些普遍的编程特性，可以处理变量、操作文本。比较常见的功能有：

- 运行代码以响应网页中发生的特定事件。





### 1.3  JavaScript 运行环境

- 独立安装的JS解释器(NodeJS)
- 嵌入在[浏览器]内核中JS解释器

**JS使用场合**： PC机，手机，平板，机顶盒



### 1.4  JavaScript 语言的特点

1. 开发工具简单， 记事本即可
2. **无需编译**，直接由 JS引擎负责执行
3. **弱类型语言**， 由数据来决定数据类型
4. 面向对象



- JavaScript 通常在客户端浏览器中运行，但也可用作服务器端语言(例如 Node.js 环境)
- 



##  二、JavaScript 组成

### 2.1 JavaScript 的具体组成

完整的 JavaScript 是由三部分组成：

- 核心(**ECMAScript**)： JavaScript 的标准规范，定义了语言的基本语法和功能，例如变量、数据类型、运算符、控制结构、函数、对象、数组、错误处理、内置对象等。

- 文档对象模型(**DOM**,Document Object Model)：允许 JavaScript 操作 HTML 和 XML 文档的结构、样式和内容。

- 浏览器对象模型(**BOM**,Browser Object Model)：   提供了与浏览器进行交互的对象和方法





### 2.2 JavaScript 的第三方 API

常见的第三方 API 有：

- [地理位置 API](https://developer.mozilla.org/zh-CN/docs/Web/API/Geolocation) 获取地理信息。这就是为什么[谷歌地图](https://www.google.com/maps)可以找到你的位置，而且标示在地图上

- [画布（Canvas）](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API) 和 [WebGL](https://developer.mozilla.org/zh-CN/docs/Web/API/WebGL_API) API 可以创建生动的 2D 和 3D 图像。人们正运用这些 web 技术制作令人惊叹的作品。参见 [Chrome Experiments](https://experiments.withgoogle.com/collection/chrome) 以及 [webglsamples](https://webglsamples.org/)。

- 诸如 [`HTMLMediaElement`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLMediaElement) 和 [WebRTC](https://developer.mozilla.org/zh-CN/docs/Web/API/WebRTC_API) 等[影音类 API](https://developer.mozilla.org/en-US/docs/Web/Media/Audio_and_video_delivery) 让你可以利用多媒体做一些非常有趣的事，比如在网页中直接播放音乐和影片，或用自己的网络摄像头获取录像，然后在其他人的电脑上展示（试用简易版[截图演示](http://chrisdavidmills.github.io/snapshot/)以理解这个概念）
- [Twitter API](https://developer.twitter.com/en/docs)、[新浪微博 API](https://open.weibo.com/) 可以在网站上展示最新推文之类。
- [谷歌地图 API](https://developers.google.com/maps/)、[OpenStreetMap API](https://wiki.openstreetmap.org/wiki/API)、[高德地图 API](https://lbs.amap.com/) 可以在网站嵌入定制的地图等等。

具体细节查看 [客户端 Web API - 学习 Web 开发 | MDN](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Client-side_web_APIs)



## 参考资料

[什么是 JavaScript？ - 学习 Web 开发 | MDN](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/First_steps/What_is_JavaScript)