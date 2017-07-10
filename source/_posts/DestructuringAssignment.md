---
title: Destructuring_assignment
date: 2017-07-09 13:08:27
categories: [javascript, MDN]
tags: [解构赋值]
---

## [解构赋值](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)
> 解构赋值 语法是一个Javascript表达式，这使得可以将值从数组或属性从对象提取到不同的变量中。

### 作用
 - 交换变量
 ```js
 var a = 1;
var b = 3;

[a, b] = [b, a];
console.log(a); // 3
console.log(b); // 1
 ```
 - 解构数组

 ```js
 let a, b, rest;

/* array 解构赋值 */
[a, b] = [1, 2];
console.log(a); // 1
console.log(b); // 2

[a, b, ...rest] = [1, 2, 3, 4, 5];
console.log(a); // 1
console.log(b); // 2
console.log(rest); // [3, 4, 5]

/* object 解构赋值 */
({a, b} = {a:1, b:2});
console.log(a); // 1
console.log(b); // 2
 ```
 - 解构对象
 
 ```js
 var o = {p: 42, q: true};
var {p, q} = o;

console.log(p); // 42
console.log(q); // true 

// 用新变量名赋值
var {p: foo, q: bar} = o;

console.log(foo); // 42
console.log(bar); // true 
 ```
 - 解析一个从函数返回的数组(函数现在可以返回多个值了)
 ```js
 function f() {
  return [1, 2];
}

var a, b; 
[a, b] = f(); 
console.log(a); // 1
console.log(b); // 2
 ```
 - 忽略某些返回值
 ```js
function f() {
  return [1, 2, 3];
}

var [a, , b] = f();
console.log(a); // 1
console.log(b); // 3
 ```
 - 将剩余数组赋值给一个变量
 ```js
 var [a, ...b] = [1, 2, 3];
console.log(a); // 1
console.log(b); // [2, 3]
```

- 用正则表达式匹配提取值
```js
var url = "https://developer.mozilla.org/en-US/Web/JavaScript";

var parsedURL = /^(\w+)\:\/\/([^\/]+)\/(.*)$/.exec(url);
console.log(parsedURL); // ["https://developer.mozilla.org/en-US/Web/JavaScript", "https", "developer.mozilla.org", "en-US/Web/JavaScript"]

var [, protocol, fullhost, fullpath] = parsedURL;

console.log(protocol); // "https"
```
- 函数参数默认值
```js
function drawES6Chart({size = 'big', cords = { x: 0, y: 0 }, radius = 25} = {}) 
{
  console.log(size, cords, radius);
  // do some chart drawing
}

drawES6Chart({
  cords: { x: 18, y: 30 },
  radius: 30
});
```
- 加载模块
```js
const { Loader, main } = require('toolkit/loader');
```
- 解构嵌套对象和数组
```js
var metadata = {
    title: "Scratchpad",
    translations: [
       {
        locale: "de",
        localization_tags: [ ],
        last_edit: "2014-04-14T08:43:37",
        url: "/de/docs/Tools/Scratchpad",
        title: "JavaScript-Umgebung"
       }
    ],
    url: "/en-US/docs/Tools/Scratchpad"
};

var { title: englishTitle, translations: [{ title: localeTitle }] } = metadata;

console.log(englishTitle); // "Scratchpad"
console.log(localeTitle);  // "JavaScript-Umgebung"
```
- For of 迭代和解构
```js
var people = [
  {
    name: "Mike Smith",
    family: {
      mother: "Jane Smith",
      father: "Harry Smith",
      sister: "Samantha Smith"
    },
    age: 35
  },
  {
    name: "Tom Jones",
    family: {
      mother: "Norah Jones",
      father: "Richard Jones",
      brother: "Howard Jones"
    },
    age: 25
  }
];

for (var {name: n, family: { father: f } } of people) {
  console.log("Name: " + n + ", Father: " + f);
}

// "Name: Mike Smith, Father: Harry Smith"
// "Name: Tom Jones, Father: Richard Jones"
```

- 从作为函数实参的对象中提取数据、
```js
function userId({id}) {
  return id;
}

function whois({displayName: displayName, fullName: {firstName: name}}){
  console.log(displayName + " is " + name);
}

var user = { 
  id: 42, 
  displayName: "jdoe",
  fullName: { 
      firstName: "John",
      lastName: "Doe"
  }
};

console.log("userId: " + userId(user)); // "userId: 42"
whois(user); // "jdoe is John"
```
- 对象属性计算名和解构
```js
let key = "z";
let { [key]: foo } = { z: "bar" };

console.log(foo); // "bar
```

