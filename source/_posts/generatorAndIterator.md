---
title: 生成器(Generator)
date: 2017-07-09 13:08:27
categories: [javascript, MDN]
tags: [生成器]
---
## 生成器对象
生成器对象是由一个 generator function 返回的,并且它符合可迭代协议和迭代器协议。
### 语法
```js
function* gen() { 
  yield 1;
  yield 2;
  yield 3;
}

let g = gen(); 
```

### 方法
- Generator.prototype.next() 

  返回一个由 yield表达式生成的值。
- Generator.prototype.return()

  返回给定的值并结束生成器。
- Generator.prototype.throw()
  向生成器抛出一个错误。

## function * 
function* 这种声明方式(function关键字后跟一个星号）会定义一个生成器函数 (generator function)，它返回一个  Generator  对象。
### 语法
function* name([param[, param[, ... param]]]) { statements }

### 描述
生成器函数在执行时能中途退出，后面又能重新进入继续执行。而且在函数内定义的变量的状态都会保留，不受中途退出的影响。

调用一个生成器函数并不会马上执行它里面的语句，而是返回一个这个生成器的迭代器（iterator）对象。当这个迭代器的 next() 方法被首次（后续）调用时，其内的语句会执行到第一个（后续）出现yield表达式的位置为止，该表达式定义了迭代器要返回的值，或者被 yield*委派至另一个生成器函数。next()方法返回一个对象，这个对象包含两个属性：value 和 done，value 属性表示本次 yield 表达式的返回值，done 属性为布尔类型，表示生成器是否已经产出了它最后的值，即生成器函数是否已经返回。

调用 next() 方法时，如果传入了参数，那么这个参数会取代生成器函数中对应执行位置的 yield 表达式（整个表达式被这个值替换）

当在生成器函数中显式 return 时，会导致生成器立即变为完成状态，即调用 next() 方法返回的对象的 done 为 true。如果 return 了一个值，那么这个值会作为下次调用 next() 方法返回的 value 值。

### 示例
#### 简单示例
```js
function* idMaker(){
  var index = 0;
  while(index<3)
    yield index++;
}

var gen = idMaker();

console.log(gen.next().value); // 0
console.log(gen.next().value); // 1
console.log(gen.next().value); // 2
console.log(gen.next().value); // undefined
// ...
```
#### yield*的示例
```js
function* anotherGenerator(i) {
  yield i + 1;
  yield i + 2;
  yield i + 3;
}

function* generator(i){
  yield i;
  yield* anotherGenerator(i);
  yield i + 10;
}

var gen = generator(10);

console.log(gen.next().value); // 10
console.log(gen.next().value); // 11
console.log(gen.next().value); // 12
console.log(gen.next().value); // 13
console.log(gen.next().value); // 20
```
#### 传递参数
```js
function* logGenerator() {
  console.log(yield);//首次执行的中断处
  console.log(yield);
  console.log(yield);
}

var gen = logGenerator();

// 首次调用 next() 会执行到第一个 yield 语句处
gen.next(); 
gen.next('pretzel'); // pretzel
gen.next('california'); // california
gen.next('mayonnaise'); // mayonnaise
```
#### 显式返回
```js
function* yieldAndReturn() {
  yield "Y";
  return "R";//显式返回处
  yield "unreachable";
}

var gen = yieldAndReturn()
console.log(gen.next()); // { value: "Y", done: false }
console.log(gen.next()); // { value: "R", done: true }
console.log(gen.next()); // { value: undefined, done: true }
```
#### 生成器函数不能当构造器使用
```js
function* f() {}
var obj = new f; // throws "TypeError: f is not a constructor"
```
##  yield* expression 用于委托给另一个generator 或可迭代对象。
### 语法
yield* [[expression]];

### 描述
yield * 表达式迭代操作数，并yield 它返回的每个值。

yield * 表达式本身的值是当迭代器关闭时返回的值(即，当done时为true)。

### 示例
例子：委托给其他生成器

以下代码中，g1() yield 出去的每个值都会在 g2() 的 next() 方法中返回，就像那些 yield 语句是写在 g2() 里一样。
```js
function* g1() {
  yield 2;
  yield 3;
  yield 4;
}

function* g2() {
  yield 1;
  yield* g1();
  yield 5;
}

var iterator = g2();

console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
console.log(iterator.next()); // { value: 3, done: false }
console.log(iterator.next()); // { value: 4, done: false }
console.log(iterator.next()); // { value: 5, done: false }
console.log(iterator.next()); // { value: undefined, done: true }
```
## 参考：
- [mdn](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript)
- [迭代器和生成器](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Iterators_and_Generators)