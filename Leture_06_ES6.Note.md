# JavaScript ES6

## Desciption

- 本篇笔记是根据Justin老师 Lecture 06 JS ES6 的课堂内容整理的随堂笔记
- 参考资料：
  - ES6综合运用在React项目中实例: https://codepen.io/Justin-hu/pen/dyBVaWV?editors=0010
  - 匠人Web全栈前端知识点概览: https://docs.google.com/spreadsheets/d/1jB3HThHu95NGC7YUCPkgzU5yUfKuU79S_wULg_AVeX4/edit?gid=0#gid=0
  - Lecture Slice: https://www.canva.com/design/DAGKiGT6X0I/tkeuhbXXFXY_XCXWtbGsig/view?utm_content=DAGKiGT6X0I&utm_campaign=designshare&utm_medium=link&utm_source=viewer

## Table of Content

- [1. Java vs. JavaScript](#1-java-vs-javascript)
- [2. 前端开发者的疆界](#2前端开发者的疆界)
- [3. 什么是 ES6](#3什么是es6)
- [4. ES6 基本语法更新](#es6-新语法-和一些补充知识点)
  - [4.1. `let`](#41-let)
  - [4.2. `const`](#42-const)
  - [4.3. 全局作用域 vs. 块级作用域 vs. 函数级作用域](#43-全局作用域-vs-块级作用域-vs-函数级作用域)
  - [4.4. 解构赋值](#44-解构赋值destructuring-assignment)
    - [4.4.1. 数组解构赋值](#441-数组解构赋值)
    - [4.4.2. 对象结构赋值](#442-对象解构赋值)
    - [4.4.3. 默认值](#443-默认值)
  - [4.5. 默认参数](#45-默认参数)
  - [4.6. Rest/Spread Operator](#46-restspread-operator)

## 1. Java vs. JavaScript

Java 由 Sun Microsystems（现在是 Oracle Corporation 的一部分）开发，最初发布于 1995 年。它是一种面向对象的编程语言，广泛用于企业级应用和大型系统开发。**JavaScript 深受 Java 的影响**，最初由 Netscape 公司开发，旨在为网页添加交互性，使得网页能够动态地响应用户的操作的**脚本语言**。

JavaScript:

- 借鉴 C 语言的基本语法。
- 借鉴 Java 语言的数据类型和内存管理。
- 借鉴 Scheme 语言，将函数提升到“第一公民（first class）”的地位。
- 借鉴 Self 语言，使用机遇原型（prototype）的继承机制。

## 2. 前端开发者的疆界

以下包含但不限于前端开发者可涉及的领域，可作为参考和学习：

- 图表：https://echarts.apache.org/examples/en/
- 3D 领域：https://echarts.apache.org/examples/en/
- H5 游戏：https://oldj.net/static/html5-tower-defense/td.html

## 3. 什么是 ES6

- ECMAScript 6（ES6）于 2015 年发布。它引入了许多新的语法特性、标准库和改进，以提高开发人员的生产力和代码的可维护性。当前主流浏览器已全面兼容 ES6。ES6 引入的一些重要特性包括箭头函数、模板字符串、解构赋值、类、let 和 const 等变量声明方式、以及 Promise 等异步编程的改进。

- ES6 的兼容性查询工具：Can I Use https://caniuse.com/

## 4. ES6 基本语法更新

### 4.1. `let`

`let` 是 JavaScript 中用于声明变量的关键字，它引入了块级作用域的概念，并解决了 `var` 声明变量时存在的一些问题。

1. **块级作用域**：`let` 声明的变量只在当前代码块（大括号 {} 内部）内有效，超出该代码块范围外不可访问。这意味着在循环、条件语句或函数内部使用 `let` 声明的变量只在其所在的块级作用域内有效。

```js
function example() {
  let x = 10;
  if (true) {
    let y = 20;
    console.log(x); // 可以访问 x
    console.log(y); // 可以访问 y
  }
  console.log(x); // 可以访问 x
  console.log(y); // 报错：y is not defined
}
```

2. **不会发生变量提升**：与 `var` 不同，使用 `let` 声明的变量不会发生变量提升（hoisting），它们在定义之前不可用。如果在声明之前访问 `let` 声明的变量，会导致 ReferenceError 错误。

```js
console.log(x); // 报错：Cannot access 'x' before initialization
let x = 10;
```

3. **不允许重复声明**：在同一个作用域内，使用 `let` 重复声明同一个变量会导致 SyntaxError 错误

```js
// Example
let userName = "My Name";
console.log(userName);

let userName = "another name";
// 报错 error 'userName' is already defined
```

4. **适用于循环变量**：在循环中使用 `let` 声明的变量会在每次迭代中都创建一个新的独立变量，解决了使用 `var` 在循环中可能导致的闭包问题。

```js
for (let i = 0; i < 3; i++) {
  setTimeout(function () {
    console.log(i); // 输出 0, 1, 2
  }, 1000);
}
```

> 老师建议声明变量时一律使用 `let` 而不是 `var`, 但依然需要掌握 `var` 的特性。

### 4.2. const

`const` 是 JavaScript 中用于声明常量的关键字，它声明的变量是常量，即一旦赋值后就不能再被修改。

1. **不可变性**：一旦使用 `const` 声明常量并赋值后，就不能再对其重新赋值。任何试图重新赋值的操作都会导致 SyntaxError 错误。

```js
// Example
const PI = 3.1415926; // 声明后立即赋值，且声明后不可更改
PI = 3.16; // SyntaxError: "PI" is read-only
```

2. **块级作用域**：与 `let` 类似，`const` 声明的常量也具有块级作用域，只在当前代码块内有效。

```js
if (true) {
  const MAX_VALUE = 100;
  console.log(MAX_VALUE); // 可以访问 MAX_VALUE
}
console.log(MAX_VALUE); // 报错：MAX_VALUE is not defined
```

3. **不会发生变量提升**：`const` 声明的常量也不会发生变量提升，必须在声明后才能使用。

```js
console.log(PI); // 报错：Cannot access 'PI' before initialization
const PI = 3.14;
```

4. **不允许重复声明**：与 `let` 类似，同一作用域内不允许重复声明相同的常量。

```js
const PI = 3.14;
const PI = 3.14159; // 报错：Identifier 'PI' has already been declared
```

5. **对象属性可变**：使用 `const` 声明的常量指向的是一个内存地址，这意味着常量本身不能被修改，但是如果常量是一个对象（数组也是对象），那么对象的属性是可以被修改的。

```js
// Example 1:
const arr = [1, 2, 3, 4];
arr[0] = 10;
console.log(arr);
// [10, 2, 3, 4]

// Example 2:
const userInfo = {
  userName: "大漠",
  age: 18,
};
userInfo.userName = "Damo";
console.log(userInfo);
// {userName: 'Damo', age: 18}
```

### 4.3. 全局作用域 vs. 块级作用域 vs. 函数级作用域

全局作用域、块级作用域和函数级作用域是 JavaScript 中不同的作用域类型，它们决定了变量在代码中的可见性和访问范围。

1. **全局作用域**：

- 全局作用域是整个 JavaScript 程序的最外层作用域，定义在任何函数之外的变量和函数都属于全局作用域。
- 在全局作用域中声明的变量和函数可以被程序中的任何代码访问，它们存在于整个脚本的生命周期中。
- 全局作用域可以通过 var、let 或 const 关键字声明的变量和函数来定义。

2. **块级作用域**：

- 块级作用域是指由一对花括号 {} 包裹起来的代码块所形成的作用域。
- 在块级作用域中声明的变量只在该代码块内部可见，超出该代码块范围外不可访问。
- 在 ES6（ECMAScript 2015）之前，JavaScript 中没有块级作用域，但是通过 let 和 const 关键字引入了块级作用域。
- 使用 let 或 const 声明的变量具有块级作用域，而 var 声明的变量在块级作用域之外也可以访问。

3. **函数级作用域**：

- 函数级作用域是指变量的作用域由函数的声明位置所决定，通常是在函数内部。
- 在函数内部声明的变量只能在该函数内部访问，而在函数外部声明的变量则不能在函数内部访问。
- JavaScript 中的函数是拥有自己作用域的闭包，函数内部可以访问外部作用域中的变量，但是外部作用域无法访问函数内部的变量，除非通过返回值或闭包等方式。

### 4.4. 解构赋值（Destructuring assignment）

解构赋值是一种在 JavaScript 中从数组或对象中提取数据并赋值给变量的方法。它可以让你更轻松地访问和使用数组和对象中的值。解构赋值也是一种语法糖。

#### 4.4.1. **数组解构赋值**

```js
// 定义一个数组
const colors = ["red", "green", "blue"];

// 使用解构赋值获取数组中的值
const [firstColor, secondColor, thirdColor] = colors;

console.log(firstColor); // 输出 'red'
console.log(secondColor); // 输出 'green'
console.log(thirdColor); // 输出 'blue'
```

> 如果数组中的元素个数多于解构的变量个数，多余的元素会被忽略。如果解构的变量个数多于数组中的元素个数，则未匹配到的变量会被赋值为 undefined。

```js
const colors = ["red", "green", "blue"];

const [firstColor, secondColor] = colors;

console.log(firstColor); // 输出 'red'
console.log(secondColor); // 输出 'green'
```

> 数组解构赋值还可以与 rest 参数结合使用，以及嵌套数组的解构

```js
// 定义一个数组
const numbers = [1, 2, 3, 4, 5];

// 将前两个数字解构赋值给变量，将剩余的数字放入一个新数组中
const [first, second, ...rest] = numbers;

console.log(first); // 输出: 1
console.log(second); // 输出: 2
console.log(rest); // 输出: [3, 4, 5]

// 定义一个嵌套数组
const matrix = [
  [1, 2],
  [3, 4],
  [5, 6],
];

// 对嵌套数组进行解构赋值
const [[a, b], [c, d], [e, f]] = matrix;

console.log(a, b); // 输出: 1 2
console.log(c, d); // 输出: 3 4
console.log(e, f); // 输出: 5 6
```

> 变量交换

```js
const [i, j] = [1, 2];
[i, j] = [j, i];
console.log(i); // 2
console.log(j); // 1
```

> 函数返回值

```js
function getData() {
  return [111, 222];
}

const [x, y] = getData();
console.log(x); // 111
console.log(y); // 222
```

#### 4.4.2. **对象解构赋值**

当我们有一个对象，我们想要从中提取属性并将它们赋值给变量时，我们可以使用对象解构赋值。对象解构赋值使用花括号 {} 包裹，并且变量名与对象的属性名相匹配。

```js
// 定义一个对象
const person = {
  name: "Alice",
  age: 30,
  city: "New York",
};

// 使用解构赋值获取对象中的值
const { name, age, city } = person;

console.log(name); // 输出 'Alice'
console.log(age); // 输出 30
console.log(city); // 输出 'New York'
```

> 可以指定新的变量名

```js
// 定义一个对象
const person = {
  name: "Alice",
  age: 30,
  city: "New York",
};

// 使用对象解构赋值，并指定新的变量名
const { name: personName, age: personAge, city: personCity } = person;

console.log(personName); // 输出 'Alice'
console.log(personAge); // 输出 30
console.log(personCity); // 输出 'New York'
```

> 嵌套对象解构

```js
// 定义一个嵌套对象
const person = {
  name: "Alice",
  age: 30,
  address: {
    city: "New York",
    country: "USA",
  },
};

// 使用嵌套对象解构赋值
const {
  name,
  age,
  address: { city, country },
} = person;

console.log(name); // 输出 'Alice'
console.log(age); // 输出 30
console.log(city); // 输出 'New York'
console.log(country); // 输出 'USA'
```

#### 4.4.3. **默认值**

日常业务开发中很常见。

```js
// 定义一个对象
const person = {
  name: "Alice",
  age: 30,
};

// 使用解构赋值获取对象中的值，并提供默认值
const { name, age, city = "New York" } = person;

console.log(name); // 输出 'Alice'
console.log(age); // 输出 30
console.log(city); // 输出 'New York'，因为对象中没有 city 属性，所以使用了默认值
```
### 4.5. **默认参数**

默认参数（Default Parameter）是指在函数定义时为参数指定一个默认值。如果在调用该函数时没有传递相应的参数，那么函数将使用这个默认值。
在 React 中，默认参数通常用在函数式组件或事件处理函数中。它可以帮助我们简化代码，避免在没有传递参数时导致 undefined 或错误。

### 4.6 **Rest/Spread Operator**

- Rest Operator：
Rest运算符主要用于将不确定数量的参数或元素收集为一个数组。通常用在函数参数中，表示该函数可以接收任意数量的参数。

```js
function sum(...numbers) {
  return numbers.reduce((acc, curr) => acc + curr, 0);
}

console.log(sum(1, 2, 3)); // 输出: 6
console.log(sum(1, 2, 3, 4, 5)); // 输出: 15
```

- Spread Operator：
Spread 运算符用于将数组或对象中的元素“展开”成单个元素，通常用于函数调用、数组构建或对象合并。

```js
const numbers = [1, 2, 3];
console.log(...numbers); // 输出: 1 2 3

const moreNumbers = [4, 5, 6];
const combined = [...numbers, ...moreNumbers];
console.log(combined); // 输出: [1, 2, 3, 4, 5, 6]
```