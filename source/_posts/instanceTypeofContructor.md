---
title: js对象属性判断
date: 2017-07-09 13:08:27
categories: [javascript, MDN]
tags: [js对象属性判断]
---
## [constructor](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/constructor)
所有对象都会从它的原型上继承一个 constructor 属性 ,返回一个指向创建了该对象原型的函数引用
```js### 语法

object instanceof constructor
function Tree(name) {
   this.name = name;
}

var myTree = new Tree("Redwood");
console.log(myTree.constructor ) // ===> function Tree(name) {this.name = name};
```
---
## [isPrototypeOf](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/isPrototypeOf) 
### 语法
prototypeObj.isPrototypeOf(object)
### 作用
用于测试一个对象是否**存在**于另一个对象的原型链上。

当需要判断对象的后代是否在特定原型链上，例如，以保证一定的方法或属性将存在该对象上，这时候就需要用到 instanceof。

## [instanceof](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/instanceof)
### 语法
object instanceof constructor
> Object.prototype.constructor 特定的函数，用于创建一个对象的原型。
- instanof 是用来检测一个对象的原型链是否存在参数Oject的原型链上，js中的继承是基于原型链继承，
- 类似的 Object.getPrototypeOf(child) === parent.protorype返回true
- 相对比 Object.getPrototypeOf(child) === grandparent.protorype返回false
- child instanceof grandparernt返回true

### 作用
instanceof 运算符用来测试 对象(object) 在其原型链(__proto__) 中是否存在构造函数(constructor) 的 prototype 属性。

- \_\_proto\_\_ 是每个 **对象** 的的属性 
- prototype 是每个 **函数** 的属性, Object构造器生成的对象没有prototype属性，Object构造器本身就是一个Function
- [Object.prototype 属性表示 Object 的原型对象。](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/prototype)
---

```js
function A(a){
  this.varA = a;
}

A.prototype = {
  varA : null,
  doSomething : function(){
    // ...
  }
}

function B(a, b){
  A.call(this, a);
  this.varB = b;
}
B.prototype = Object.create(A.prototype, {
  varB : {
    value: null, 
    enumerable: true, 
    configurable: true, 
    writable: true 
  },
  doSomething : { 
    value: function(){ // override
      A.prototype.doSomething.apply(this, arguments); // call super
      // ...
    },
    enumerable: true,
    configurable: true, 
    writable: true
  }
});

var b = new B();
b.doSomething()

b instanceOf B // ===>true
b instanceOf A // ===>true
Object.getPrototypeOf(b) === B.prototype // ===> true
Object.getPrototypeOf(b) === A.prototype // ===> false
```
另外对于A B的两个函数对象，B继承A
参考： [继承与原型链](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)

```js
B instanceof A // ===> false 是继承关系不是实例构造
Object.getPrototypeOf(B.prototype) === A.prototype // ===> true B函数的prototype的原型是A函数的prototype,以此来实现js的原型继承
```
---


## Object.prototype.toString.call

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

**so, 相对比typeof ,Object.prototype.toString.call 来判断会对象会更加的准确，在使用typeof Array 和 typeof null 结果与 typeof Ojbect 一样是 'object'**

---

## Array.isArray() 
用于确定传递的值是否是一个 Array

## Number.isNan() 
用于确定传递的值是否是Nan

## 参考：
- [mdn](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript)
- [JavaScript判断对象的类型](http://www.cnblogs.com/zhenchaoni/p/JavaScript_Determine_Types.html)