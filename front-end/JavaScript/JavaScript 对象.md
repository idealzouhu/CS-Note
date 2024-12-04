### 什么是 JavaScript 对象

JavaScript 对象是用于存储相关数据和功能的容器。

它由键值对（属性）组成，其中键是字符串（也称为属性名），值可以是任何数据类型，包括数字、字符串、函数、甚至其他对象。



### JavaScript 对象常规操作

#### 定义对象

一个 JavaScript 对象可以通过花括号 `{}` 来定义，其中包含键值对：

```javascript
const person = {
  name: 'John',
  age: 30,
  greet: function() {
    console.log('Hello, ' + this.name);
  }
};
```

在这个对象中：

- `name` 和 `age` 是对象的属性。
- `greet` 是对象的方法，它是一个函数。

#### 访问属性和方法

你可以使用点符号或方括号符号来访问对象的属性和方法：

```javascript
console.log(person.name); // 输出: John
console.log(person['age']); // 输出: 30
person.greet(); // 输出: Hello, John
```



#### 修改属性

你可以随时修改对象的属性：

```javascript
person.name = 'Jane';
console.log(person.name); // 输出: Jane
```



#### 添加属性和方法

你也可以动态地向对象添加新的属性和方法：

```javascript
person.email = 'jane@example.com';
person.sayGoodbye = function() {
  console.log('Goodbye, ' + this.name);
};
console.log(person.email); // 输出: jane@example.com
person.sayGoodbye(); // 输出: Goodbye, Jane
```