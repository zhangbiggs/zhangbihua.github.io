---
title: 代理模式(Proxy)-设计模式
date: 2017-05-05 23:58:44
categories: [design-pattern, Structural Patterns]
tags: [javasctipt, 代理模式]
---
> 软件模式是将模式的一般概念应用于软件开发领域，即软件开发的 总体指导思路或参照样板。
> 软件模式并非仅限于设计模式，还包括 架构模式、分析模式和过程模式等，
> 实际上，在软件生存期的每一个阶段都存在着一些被认同的模式。

### 代理模式的定义
- 代理模式(Proxy Pattern) ：给某一个对象提供一个代理，并由代理对象控制对原对象的引用。代理模式的英 文叫做Proxy或Surrogate，它是一种对象结构型模式。

### 代理模式包含三个角色：
Subject: 抽象主题角色
Proxy: 代理主题角色
RealSubject: 真实主题角色

---

### ES6实现
``` js
class Subject {
    constructor() {
        console.log('Subject Class created');
    }

    request() {
        console.log('Subject.request invoked');
    }
}

class RealSubject extends Subject {
    constructor() {
        super()
        console.log('RealSubject Class created');
    }

    request() {
        console.log('RealSubject.request invoked');
    }
}

class Proxy extends Subject {
    constructor() {
        super()
        console.log('Proxy Class created');
    }

    request() {
        this.realSubject = new RealSubject();
        this.realSubject.request();
    }
}

var proxy = new Proxy()
proxy.request()


```
---

### Typescript实现
``` ts
namespace ProxyPattern {
    export interface Subject {
        doAction(): void;
    }

    export class Proxy implements Subject {
        private realSubject: RealSubject;
        private s: string;

        constructor(s: string) {
            this.s = s;
        }

        public doAction(): void {
            console.log("`doAction` of Proxy(", this.s, ")");
            if (this.realSubject === null || this.realSubject === undefined) {
                console.log("creating a new RealSubject.");
                this.realSubject = new RealSubject(this.s);
            }
            this.realSubject.doAction();
        }
    }

    export class RealSubject implements Subject {
        private s: string;

        constructor(s: string) {
            this.s = s;
        }
        public doAction(): void {
            console.log("`doAction` of RealSubject", this.s, "is being called!");
        }
    }
}


```
---

## 参考以下内容:
 - [图说设计模式](https://design-patterns.readthedocs.io/zh_CN/latest/)
 - [ECMAScript2016-Design-Patterns](https://github.com/ryouaki/ECMAScript2016-Design-Patterns)
 - [Design Patterns in TypeScript](https://github.com/torokmark/design_patterns_in_typescript)
