---
title: 单例模式(Singleton)-设计模式
date: 2017-05-05 23:58:44
categories: [design-pattern, Creational Patterns]
tags: [javasctipt, 单例模式]
---
> 软件模式是将模式的一般概念应用于软件开发领域，即软件开发的 总体指导思路或参照样板。
> 软件模式并非仅限于设计模式，还包括 架构模式、分析模式和过程模式等，
> 实际上，在软件生存期的每一个阶段都存在着一些被认同的模式。

### 单例模式的定义
- 某个类只能有一个实例；
- 它必须自行创建这个实例；
- 它必须自行向整个系统提供这个实例。

### 单例模式的实现过程中，需要注意如下三点：
- 单例类的构造函数为私有；
- 提供一个自身的静态私有成员变量；
- 提供一个公有的静态工厂方法。

---

### ES6实现
``` js
class Singleton {
    constructor(data) {
        if (Singleton.prototype.Instance === undefined) {
            this.data = data;
            Singleton.prototype.Instance = this;
        }
        return Singleton.prototype.Instance;
    }
}

let ob1 = new Singleton.getInstance();
let ob2 = new Singleton("two");

console.log(ob1 === ob2);
```
---

### Typescript实现
``` ts
class Singleton {
  private static instance: Singleton;

  constructor() {}

  static get Instance() {
      if (this.instance === null || this.instance === undefined) {
          this.instance = new Singleton();
      }
      return this.instance;
  }
}
let ob1 = new Singleton("one");
let ob2 = new Singleton("two");

console.log(ob1 === ob2);
```
---

## 参考以下内容:
 - [图说设计模式](https://design-patterns.readthedocs.io/zh_CN/latest/)

 - [ECMAScript2016-Design-Patterns](https://github.com/ryouaki/ECMAScript2016-Design-Patterns)
 - [Design Patterns in TypeScript](https://github.com/torokmark/design_patterns_in_typescript)
