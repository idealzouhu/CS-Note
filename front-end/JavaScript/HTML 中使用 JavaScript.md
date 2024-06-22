[TOC]



##  一、JavaScript 脚本添加方式

在 HTML 文件中使用 JavaScript 代码主要由以下三种方法：

- 内联
- 内嵌
- 外部引用



### 1.1 内联 JavaScript

内联 JavaScript 是将 JavaScript 代码直接写在 HTML 元素的事件属性中。例如，使用 `onclick` 属性来处理按钮点击事件。

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Inline JavaScript Example</title>
</head>
<body>
  <button onclick="alert('Button clicked!')">Click me</button>
</body>
</html>
```



### 1.2 内嵌 JavaScript

内嵌 JavaScript 是将 JavaScript 代码放在 HTML 文件中的 `<script>` 标签内，通常放置在 `<head>` 或 `<body>` 中。

`<script>`允许出现网页的任意位置处

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Embedded JavaScript Example</title>
</head>
<body>
  <button onclick="showMessage()">Click me</button>

  <script>
    function showMessage() {
      alert('Button clicked!');
    }
  </script>
</body>
</html>

```





### 1.3 外部引用 JavaScript

外部引用 JavaScript 是将 JavaScript 代码放在独立的 `.js` 文件中，然后通过 `<script>` 标签引用该文件。这种方法有助于分离内容和行为，提高代码的可维护性和重用性。

- 创建 script.js 文件

  ```javascript
  function showMessage() {
    alert('Button clicked!');
  }
  ```

- 在页面中引入  script.js 文件    ,  `<script src="js文件路径"></script>`

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>External JavaScript Example</title>
    <script src="script.js"></script>
  </head>
  <body>
    <button onclick="showMessage()">Click me</button>
  </body>
  </html>
  
  ```

  >  注意：`<script src=""></script>`该对标记中，是**不允许出现任何内容**的







## 二、外部引用 JavaScript 的注意事项

为了提高页面的加载速度和用户体验，建议优化 javascript 加载顺序（先加载页面内容，再加载 JavaScript）。**优化 javascript 加载顺序**的实现方式主要有：

- 一般来说，建议将 `<script>` 标签放在页面底部（即在 `</body>` 之前），以确保页面内容先加载，然后再加载和执行 JavaScript。这可以

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>Script at Bottom</title>
  </head>
  <body>
    <h1>Hello, World!</h1>
    <p>This is an example.</p>
    <script src="script.js"></script>
  </body>
  </html>
  
  ```

- 另一种方法是使用 `defer` 或 `async` 属性来控制脚本加载和执行的时机。

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>Script Loading Example</title>
    <script src="script.js" defer></script>
    <!-- 或者 -->
    <script src="script.js" async></script>
  </head>
  <body>
    <button onclick="showMessage()">Click me</button>
  </body>
  </html>
  ```

  