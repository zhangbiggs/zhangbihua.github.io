---
title: prototype和__proto__(个人理解)
categories: [javascript, MDN]
tags: [prototype和__proto__]
---
## 放关系图
![prototype and __proto__](http://harttle.com/assets/img/blog/javascript/js-proto.png)

## [Object](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object)构造函数，创建一个对象包装器。
 
## [Function构造函数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function)， 创建一个新的Function对象。 在 JavaScript 中, 每个函数实际上都是一个Function对象。
### 引用ECMAScript的解释

> [Each constructor is a function that has a property named “prototype” that is used to implement prototype-based inheritance and shared properties](https://www.ecma-international.org/ecma-262/5.1/#sec-15%EF%BC%8C_%E9%B8%A1%E5%92%8C%E8%9B%8B_%E7%9A%84%E9%97%AE%E9%A2%98%E5%B0%B1%E6%98%AF%E8%BF%99%E4%B9%88%E5%87%BA%E7%8E%B0%E5%92%8C%E8%AE%BE%E8%AE%A1%E7%9A%84%EF%BC%9A**%60Function%60%E7%BB%A7%E6%89%BF%60Function%60%E6%9C%AC%E8%BA%AB%EF%BC%8C%60Function.prototype%60%E7%BB%A7%E6%89%BF%60Object.prototype%60%E3%80%82)

> 翻译： 每个构造器都是有着“prototype"属性的函数，用于实现基于原型的继承和共享属性

在javascript中， 有且只有Function构造器的prototype === \_\_proto\_\_
 ```js
 Function.ptototype === Function.__proto__ // === true
 ```
为什么Function构造器会有这样的设计，原因是因为js没有类的机制，是通过对象的\_\_proto\_\_属性指向父类的prototype属性实现

 ### 引用ECMAScript的解释
 ---
 ![how_Prototypal_Inheritance works](http://i.imgur.com/cCzkv.png)

> CF is a constructor (and also an object). Five objects have been created by using new expressions: cf1, cf2, cf3, cf4, and cf5. Each of these objects contains properties named q1 and q2. The dashed lines represent the implicit prototype relationship; so, for example, cf3’s prototype is CFp. The constructor, CF, has two properties itself, named P1 and P2, which are not visible to CFp, cf1, cf2, cf3, cf4, or cf5. The property named CFP1 in CFp is shared by cf1, cf2, cf3, cf4, and cf5 (but not by CF), as are any properties found in CFp’s implicit prototype chain that are not named q1, q2, or CFP1. Notice that there is no implicit prototype link between CF and CFp.
> Unlike class-based object languages, properties can be added to objects dynamically by assigning values to them. That is, constructors are not required to name or assign values to all or any of the constructed object’s properties. In the above diagram, one could add a new shared property for cf1, cf2, cf3, cf4, and cf5 by assigning a new value to the property in CFp.

翻译： 

> CF是一个构造（以及一个对象）。五个对象已经通过使用创建的new：表达式 CF 1，CF 2，CF 3，CF 4，和CF 5。每个对象包含一个名为性质Q1和Q2。虚线表示隐式原型关系; 因此，例如， CF 3的原型是CF p。的构造，CF，有两个属性本身，命名为P1和P2，这是不可见CF p，CF 1，CF 2，CF 3， CF 4，或CF 5。命名属性CFP1在 CF p是由共享CF 1，CF 2，CF 3， CF 4，和CF 5（而不是由CF），因为是在发现的任何属性 CF p未命名的隐式原型链Q1， Q2，或CFP1。请注意，之间不存在隐式原型链接CF和CF p。

> 不像基于类的对象的语言，属性可以动态地将它们赋值被添加到对象。也就是说，构造函数不需要命名或赋值构造的对象的属性的全部或任何。另外，在上述图中，人们可以添加对新的共享属性CF 1，CF 2，CF 3， CF 4，和CF 5 通过在属性分配一个新的值CF p。

---
### 实现一个函数的声明以及在这个函数的prototype属性加一个属性
```js
function Animate (name) {
  this.name = name 
}
Animate.prototype.sayName = function () {
  console.log(this.name)
}
var myAnimate = new Animate('nini')

```
因为我们在声明Animate函数时，broswer runtime 载入Function构造器，Animate.prototype有两个属性{constructor: XX，\_\_proto\_\_:xxx},
```js
Animate.prototype.__proto__ === Function.prototype.__proto__  && Animate.prototype.__proto__ === Object.prototype 
Animate.__proto__ === Function.__proto__
animate.__proto__ !== Animate.prototype && Animate.__proto__.__proto__ === Animate.prototype.__proto__, 
Animate.__proto__.__proto__ === Animate.prototype.__proto__
```
animate.\_\_proto\_\_ 和 animate.prototype 的原型链(\_\_proto\_\_)都指向Object.prototype
### NOTE: 前面我们说过了Function构造器是js中唯一一个prototype属性 等于 原型链（\_\_proto\_\_)属性的对象( Function.prototype === Function.\_\_proto\_\_)
> 为构造函数设置一个prototype属性。
> 这个属性包含一个对象（以下简称"prototype对象"），所有实例对象需要共享的属性和方法，都放在这个对象里面；那些不需要共享的属性和方法，就放在构造函数里面。
> 实例对象一旦创建，将自动引用prototype对象的属性和方法(Function.prototype === Function.\_\_proto\_\_ 的设计就是为了实现这个功能)
> 也就是说，实例对象的属性和方法，分成两种，一种是本地的，另一种是引用的。

---

在例子new Animate('nini')时，一个新对象被创建。它继承自Animate.prototype
## 实现一个简单的继承类

```js
function Animate (name) {
  this.name = name 
}
Animate.prototype.sayName = function () {
  console.log(this.name)
}


function Dog (name, color) {
  Animate.call(this, name)
  this.color = color
}
Dog.prototype = Object.create(Animate.prototype,{  // 设置Dog.prototype的\_\_proto\_\_为animate.prototype
  sayColor: {
    value: function  () {
      console.log(this.color)
    },
    enumerable: true,
    configurable: true,
    writable: true
  }
})

var myAnimate = new Animate('nini')
var myDog = new Dog('jiji', 'red');

myAnimate.sayName() // ===> 'nini'
myDog.sayName() // ===> 'jiji'
myDog.sayColor() // ===> 'red'
Dog.say() // Uncaught TypeError: Dog.say is not a function
Animate.say() // Uncaught TypeError: Animate.say is not a function

```

## 参考：
- [mdn](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript)