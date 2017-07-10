---
title: js对象属性判断
date: 2017-07-09 13:08:27
categories: [javascript, MDN]
tags: [js对象属性判断]
---

## [instanceof](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/instanceof)
### 作用
> instanceof 运算符用来测试一个对象在其原型链中是否存在一个构造函数的 prototype 属性。
> instanceof 运算符用来检测 constructor.prototype 是否存在于参数 object 的原型链上。

[[prototype]] 是每个 **对象** 的的属性 prototype 是每个**函数**的属性
---
### 语法
> object instanceof constructor
- instanof 是用来检测一个对象的原型链是否存在参数Oject的原型链上，js中的继承是基于原型链继承，类似的 child.prototype.constructor === parent.protorype返回true
<!--- 相对比 child.prototype.constructor === grandparent.protorype返回false-->
- child instanceof grandparernt返回true
prototype里面包含__proto__ 和 constructor

---


## Object.prototype.toString.call
> Object.prototype 属性表示 Object 的原型对象。

```js
var myObj = {}
var myArr = []
var myNumber = 1
var myStr = 'a'

Object.prototype.toString.call(myObj) // ===> '[object Object]' 
Object.prototype.toString.call(myArr) // ===> '[object Array]' 
Object.prototype.toString.call(myNumber) // ===> '[object Number]'
Object.prototype.toString.call(myStr) // ===> '[object String]'
Object.prototype.toString.call(undefined) // ===> '[object undefined]'
Object.prototype.toString.call(null) // ===> '[object null]'
var a = function () {
  this.nimei = 'nimie'
  this.say = function () {
    console.log('nimei')
  }
}
var b ;
b. 
b.say()

```
> Object.prototype.constructor 特定的函数，用于创建一个对象的原型。
---

## typeof
```js
var myArr = []
var myNumber = 1
var myStr = 'a'
console.log(typeof myObj)  // ===> 'object'
console.log(typeof myArr)  // ===> 'object'
console.log(typeof myNumber)  // ===> 'number'
console.log(typeof myStr)  // ===> 'string'
console.log(typeof undefined)  // ===> 'undefined'
console.log(typeof null)  // ===> 'object'
```
## 
**so, 相对比typeof ,Object.prototype.toString.call 来判断会对象会更加的准确，在使用typeof Array 和 typeof null 结果与 typeof Ojbect 一样是 'object'**