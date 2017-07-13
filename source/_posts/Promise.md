---
title: Promise
date: 2017-07-13 14:11:11
categories: [javascript, MDN]
tags: [Promise]
---
![promise](https://mdn.mozillademos.org/files/8633/promises.png)
Promise 对象用于一个异步操作的最终完成（或失败）及其结果值的表示。(简单点说就是处理异步请求。。我们经常会做些承诺，如果我赢了你就嫁给我，如果输了我就嫁给你之类的诺言。这就是promise的中文含义。一个诺言，一个成功，一个失败。)

new Promise(
    /* executor */
    function(resolve, reject) {...}
);
## 参数
### executor(resolve, reject)
该函数会马上被调用，它带有两个参数的函数对象：
    executor是一个带有resolve和reject两个参数的函数 。executor 函数在Promise构造函数执行时同步执行，被传递resolve和reject函数（executor 函数在Promise构造函数返回新建对象前被调用）。resolve 和 reject 函数被调用时，分别将promise的状态改为fulfilled(完成)或rejected（失败）。executor 内部通常会执行一些异步操作，一旦完成，可以调用resolve函数来将promise状态改成fulfilled，或者在发生错误时将它的状态改为rejected
### 解决函数resolve()
用特定值满足绑定的promise，或者把状态传递到一个已存在的promise。如果绑定的promise已经被解决（可能是一个值，也可能是一个rejection，或者是另一个promise），该方法不会做任何事。

> note: 用特定值满足绑定的promise，或者把状态传递到一个已存在的promise。如果绑定的promise已经被解决（可能是一个值，也可能是一个rejection，或者是另一个promise），该方法不会做任何事。

---

### 拒绝函数reject()
用特定原因拒绝绑定的promise。如果promise已经被解决了，不管是值，拒绝，还是另一个promise，这个方法都不会做任何事。

## 描述
**Promise** 对象是一个代理对象（代理一个值），被代理的值在Promise对象创建时可能是未知的。它允许你为异步操作的成功和失败分别绑定相应的处理方法（handlers ）。 这让异步方法可以像同步方法那样返回值，但并不是立即返回最终执行结果，而是一个能代表未来出现的结果的promise对象


一个 Promise有以下几种状态:

- 未完成状态(pending), 此时完成值还不可用，他是唯一一个能够转换为其他状态的状态。
- 已完成状态(fulfilled), 此时完成值可用，完成值与promise永久绑定，可为任意值，包括undefined
- 拒绝状态(rejected), 出现错误时，拒绝理由与promise永久绑定，可为任意值，包括undefined，一般情况下为Error对象

pending 状态的 Promise 对象可能触发fulfilled 状态并传递一个值给相应的状态处理方法，也可能触发失败状态（rejected）并传递失败信息。当其中任一种情况出现时，Promise 对象的 then 方法绑定的处理方法（handlers ）就会被调用（then方法包含两个参数：onfulfilled 和 onrejected，它们都是 Function 类型。当Promise状态为fulfilled时，调用 then 的 onfulfilled 方法，当Promise状态为rejected时，调用 then 的 onrejected 方法， 所以在异步操作的完成和绑定处理方法之间不存在竞争）。

因为 Promise.prototype.then 和 Promise.prototype.catch 方法返回promise 对象， 所以它们可以被链式调用。

##构造函数
创建一个新的promise，初始化为等待状态，并提供解决函数的引用，用于改变其状态。
new Promise(executor);

## 方法
### Promise.all(iterable)

  这个方法返回一个新的promise对象，该promise对象在iterable参数对象里所有的promise对象都成功的时候才会触发成功，一旦有任何一个iterable里面的promise对象失败则立即触发该promise对象的失败。这个新的promise对象在触发成功状态以后，会把一个包含iterable里所有promise返回值的数组作为成功回调的返回值，顺序跟iterable的顺序保持一致；如果这个新的promise对象触发了失败状态，它会把iterable里第一个触发失败的promise对象的错误信息作为它的失败错误信息。Promise.all方法常被用于处理多个promise对象的状态集合。（可以参考jQuery.when方法---译者注）
  ```js
  var p1 = Promise.resolve(3);
  var p2 = 1337;
  var p3 = new Promise((resolve, reject) => {
    setTimeout(resolve, 100, "foo");
  }); 

  Promise.all([p1, p2, p3]).then(values => { 
    console.log(values); // [3, 1337, "foo"] 
  });
  ```
### Promise.race(iterable)

  当iterable参数里的任意一个子promise被成功或失败后，父promise马上也会用子promise的成功返回值或失败详情作为参数调用父promise绑定的相应句柄，并返回该promise对象。
  ```js
  ar p1 = new Promise(function(resolve, reject) { 
      setTimeout(resolve, 500, "one"); 
  });
  var p2 = new Promise(function(resolve, reject) { 
      setTimeout(resolve, 100, "two"); 
  });

  Promise.race([p1, p2]).then(function(value) {
    console.log(value); // "two"
    // 两个都完成，但 p2 更快
  });

  var p3 = new Promise(function(resolve, reject) { 
      setTimeout(resolve, 100, "three");
  });
  var p4 = new Promise(function(resolve, reject) { 
      setTimeout(reject, 500, "four"); 
  });

  Promise.race([p3, p4]).then(function(value) {
    console.log(value); // "three"
    // p3 更快，所以它完成了              
  }, function(reason) {
    // 未被调用
  });
```
### Promise.reject(reason)

  返回一个状态为失败的Promise对象，并将给定的失败信息传递给对应的处理方法
  ```js
  Promise.reject("Testing static reject").then(function(reason) {
    // 未被调用
  }, function(reason) {
    console.log(reason); // "测试静态拒绝"
  });

  Promise.reject(new Error("fail")).then(function(error) {
    // 未被调用
  }, function(error) {
    console.log(error); // 堆栈跟踪
  });
  ```
### Promise.resolve(value)
  返回一个状态由给定value决定的Promise对象。如果该值是一个Promise对象，则直接返回该对象；如果该值是thenable(即，带有then方法的对象)，返回的Promise对象的最终状态由then方法执行决定；否则的话(该value为空，基本类型或者不带then方法的对象),返回的Promise对象状态为fulfilled，并且将该value传递给对应的then方法。通常而言，如果你不知道一个值是否是Promise对象，使用Promise.resolve(value) 来返回一个Promise对象,这样就能将该value以Promise对象形式使用。
```js
// Resolve另一个promise对象

var original = Promise.resolve(true);
var cast = Promise.resolve(original);
cast.then(function(v) {
  console.log(v); // true
});

// resolve thenable的对象们并抛出错误
// Resolve一个thenable对象
var p1 = Promise.resolve({ 
  then: function(resolve, onReject) { resolve("resolve!"); }
});
console.log(p1 instanceof Promise) // true, 这是一个Promise对象

p1.then(function(v) {
    console.log(v); // 输出"resolve!"
  }, function(e) {
    // 不会被调用
});

// Thenable在callback之前抛出异常
// Promise rejects
var thenable = { then: function(resolve) {
  throw new TypeError("Throwing");
  resolve("Resolving");
}};

var p2 = Promise.resolve(thenable);
p2.then(function(v) {
  // 不会被调用
}, function(e) {
  console.log(e); // TypeError: Throwing
});

// Thenable在callback之后抛出异常
// Promise resolves
var thenable = { then: function(resolve) {
  resolve("Resolving");
  throw new TypeError("Throwing");
}};

var p3 = Promise.resolve(thenable);
p3.then(function(v) {
  console.log(v); // 输出"Resolving"
}, function(e) {
  // 不会被调用
});
```

## Promise 原型
### 属性
  - Promise.prototype.constructor

  返回创建了实例原型的函数.  默认为 Promise 函数.
### 方法
- Promise.prototype.catch(onRejected)

  添 p2 = Promise.resolve(thenable);
p2.then(function(v) {
  // 不会被调用
}, function(e) {
  console.log(e); // TypeError: Throwing
});

// Thenable在callback之后抛出异常
// Promise resolves
var thenable = { then: function(resolve) {
  resolve("Resolving");
  throw new TypeError("Throwing");
}};

var p3 = Promise.resolve(thenable);
p3.then(function(v) {
  console.log(v); // 输出"Resolving"
}, function(e) {
  // 不会被调用
});
```加一个否定(rejection) 回调到当前 promise, 返回一个新的promise。如果这个回调被调用，新 promise 将以它的返回值来resolve，否则如果当前promise 进入fulfilled状态，则以当前promise的肯定结果作为新promise的肯定结果.

- Promise.prototype.then(onFulfilled, onRejected)

添加肯定和否定回调到当前 promise, 返回一个新的 promise, 将以回调的返回值 来resolve.

## 示例
```js
var myFirstPromise = new Promise(function(resolve, reject){
    //当异步代码执行成功时，我们才会调用resolve(...), 当异步代码失败时就会调用reject(...)
    //在本例中，我们使用setTimeout(...)来模拟异步代码，实际编码时可能是XHR请求或是HTML5的一些API方法.
    setTimeout(function(){
        resolve("成功!"); //代码正常执行！
    }, 250);
});

myFirstPromise.then(function(successMessage){
    //successMessage的值是上面调用resolve(...)方法传入的值.
    //successMessage参数不一定非要是字符串类型，这里只是举个例子
    console.log("Yay! " + successMessage);
});
```

## 参考：
- [mdn](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [JavaScript 标准参考教程](http://javascript.ruanyifeng.com/)
- [JavaScript Promise](https://developers.google.com/web/fundamentals/getting-started/primers/promises?hl=zh-cn


