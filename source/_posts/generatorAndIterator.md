---
title: 生成器
date: 2017-07-09 13:08:27
categories: [javascript, MDN]
tags: [生成器]
---
## 生成器
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
## 参考：
- [mdn](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript)
- [迭代器和生成器](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Iterators_and_Generators)