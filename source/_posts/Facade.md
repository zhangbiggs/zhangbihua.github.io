---
title: 外观模式(Facade)-设计模式
date: 2017-05-05 23:58:44
categories: [design-pattern, Structural Patterns]
tags: [javasctipt, 外观模式]
---
> 软件模式是将模式的一般概念应用于软件开发领域，即软件开发的 总体指导思路或参照样板。
> 软件模式并非仅限于设计模式，还包括 架构模式、分析模式和过程模式等，
> 实际上，在软件生存期的每一个阶段都存在着一些被认同的模式。

### 外观模式的定义
- 外观模式：在外观模式中，外部与一个子系统的通信必须通过一个统一的外观对象进行，为子系统中的一组接口提供一个一致的界面，
- 外观模式定义了一个高层接口，这个接口使得这一子系统更加容易使用。外观模式又称为门面模式，它是一种对象结构型模式。

### 外观模式包含二个角色：
- Facade-外观角色是在客户端直接调用的角色，在外观角色中可以知道相关的(一个或者多个)子系统的功能和责任，它将所有从客户端发来的请求委派到相应的子系统去，传递给相应的子系统对象处理；
- SubSystem-在软件系统中可以同时有一个或者多个子系统角色，每一个子系统可以不是一个单独的类，而是一个类的集合，它实现子系统的功能

---

### ES6实现
``` js
class Facade {
    constructor() {
        console.log("Facade class created");
    }

    gotoPage(dp) {
        switch (dp) {
            case "Facade":
                console.log("This is the Facade");
                break;
            case "AbstractFactory":
                console.log("This is the AbstractFactory");
                break;
            default:
                console.log("nothing to be matched");
        }
    }
}

let facade = new Facade();
facade.gotoPage('Facade');
facade.gotoPage('AbstractFactory');
```
---

### Typescript实现
``` ts
namespace FacadePattern {

    export class Part1 {
        public method1(): void {
            console.log("`method1` of Part1");
        }
    }

    export class Part2 {
        public method2(): void {
            console.log("`method2` of Part2");
        }
    }

    export class Part3 {
        public method3(): void {
            console.log("`method3` of Part3");
        }
    }

    export class Facade {
        private part1: Part1 = new Part1();
        private part2: Part2 = new Part2();
        private part3: Part3 = new Part3();

        public operation1(): void {
            console.log("`operation1` is called ===");
            this.part1.method1();
            this.part2.method2();
            console.log("==========================");
        }

        public operation2(): void {
            console.log("`operation2` is called ===");
            this.part1.method1();
            this.part3.method3();
            console.log("==========================");
        }
    }
}
```
---

## 参考以下内容:
 - [图说设计模式](https://design-patterns.readthedocs.io/zh_CN/latest/)
 - [ECMAScript2016-Design-Patterns](https://github.com/ryouaki/ECMAScript2016-Design-Patterns)
 - [Design Patterns in TypeScript](https://github.com/torokmark/design_patterns_in_typescript)
