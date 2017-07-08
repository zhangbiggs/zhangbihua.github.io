---
title: 策略模式(Strategy)-设计模式
date: 2017-05-05 23:58:44
categories: [design-pattern, Behavioral Patterns]
tags: [javasctipt, 策略模式]
---
> 软件模式是将模式的一般概念应用于软件开发领域，即软件开发的 总体指导思路或参照样板。
> 软件模式并非仅限于设计模式，还包括 架构模式、分析模式和过程模式等，
> 实际上，在软件生存期的每一个阶段都存在着一些被认同的模式。

### 策略模式的定义
- 策略模式(Strategy Pattern)：定义一系列算法，将每一个算法封装起来，并让它们可以相互替换。
- 策略模式让算法独立于使用它的客户而变化，也称为政策模式(Policy)。。

### 策略模式包含三个角色：
- Context-环境类在解决某个问题时可以采用多种策略，在环境类中维护一个对抽象策略类的引用实例;
- Strategy-抽象策略类为所支持的算法声明了抽象方法，是所有策略类的父类;
- ConcreteStrategy-具体策略类实现了在抽象策略类中定义的算法。
---

### ES6实现
``` js
class Context {
    constructor(type) {
        console.log('Context Class created!');
        switch (type) {
            case "A":
                this.strategy = new ConcreteStrategyA()
                break
            case "B":
                this.strategy = new ConcreteStrategyB()
                break
            default:
                this.strategy = new ConcreteStrategyA()
        }
    }

    contextInterface() {
        console.log('Context.contextInterface invoked');
        this.strategy.algorithmInterface()
    }
}

class Strategy {
    constructor() {
        console.log('Strategy Class created!');
    }

    algorithmInterface() {
        console.log('Strategy.algorithmInterface invoked');
    }
}

class ConcreteStrategyA extends Strategy {
    constructor() {
        super();
        console.log('ConcreteStrategyA Class created!');
    }

    algorithmInterface() {
        console.log('ConcreteStrategyA.algorithmInterface invoked');
    }
}

class ConcreteStrategyB extends Strategy {
    constructor() {
        super();
        console.log('ConcreteStrategyB Class created!');
    }

    algorithmInterface() {
        console.log('ConcreteStrategyB.algorithmInterface invoked');
    }
}

let contextA = new Context("A");
contextA.contextInterface();
let contextB = new Context("B");
contextB.contextInterface();

```
---

### Typescript实现
``` ts
namespace StrategyPattern {
    export interface Strategy {
        execute(): void;
    }

    export class ConcreteStrategy1 implements Strategy {
        public execute(): void {
            console.log("`execute` method of ConcreteStrategy1 is being called");
        }
    }

    export class ConcreteStrategy2 implements Strategy {
        public execute(): void {
            console.log("`execute` method of ConcreteStrategy2 is being called");
        }
    }

    export class ConcreteStrategy3 implements Strategy {
        public execute(): void {
            console.log("`execute` method of ConcreteStrategy3 is being called");
        }
    }

    export class Context {
        private strategy: Strategy;

        constructor(strategy: Strategy) {
            this.strategy = strategy;
        }

        public executeStrategy(): void {
            this.strategy.execute();
        }
    }
}

```
---

## 参考以下内容:
 - [图说设计模式](https://design-patterns.readthedocs.io/zh_CN/latest/)
 - [ECMAScript2016-Design-Patterns](https://github.com/ryouaki/ECMAScript2016-Design-Patterns)
 - [Design Patterns in TypeScript](https://github.com/torokmark/design_patterns_in_typescript)
