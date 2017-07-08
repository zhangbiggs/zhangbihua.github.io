---
title: 抽象工厂模式(Abstract Factory)-设计模式
date: 2017-05-05 23:58:44
categories: [design-pattern, Creational Patterns]
tags: [javasctipt, 抽象工厂模式]
---
> 软件模式是将模式的一般概念应用于软件开发领域，即软件开发的 总体指导思路或参照样板。
> 软件模式并非仅限于设计模式，还包括 架构模式、分析模式和过程模式等，
> 实际上，在软件生存期的每一个阶段都存在着一些被认同的模式。

### 抽象工厂模式的定义
- 抽象工厂模式是所有形式的工厂模式中最为抽象和最具一般性的一种形态。
- 抽象工厂模式与工厂方法模式最大的区别在于，工厂方法模式针对的是一个产品等级结构，而抽象工厂模式则需要面对多个产品等级结构

### 抽象工厂模式包含四个角色：
- ConcreteFactory-具体工厂实现了抽象工厂声明的生成抽象产品的方法，生成一组具体产品，这些产品构成了一个产品族，每一个产品都位于某个产品等级结构中；
- AbstractProduct-抽象产品为每种产品声明接口，在抽象产品中定义了产品的抽象业务方法；
- AbstractFactory-抽象工厂用于明生成抽象产品的方法；
- Product-具体产品定义具体工厂生产的具体产品对象，实现抽象产品接口中定义的业务方法。

### 抽象工厂模式适用情况包括：
- 一个系统不应当依赖于产品类实例如何被创建、组合和表达的细节；
- 系统中有多于一个的产品族，而每次只使用其中某一产品族；
- 属于同一个产品族的产品将在一起使用；
- 系统提供一个产品类的库，所有的产品以同样的接口出现，从而使客户端不依赖于具体实现。

---

### ES6实现
``` js
class AbstractFactory {
    constructor() {
        console.log("AbstractFactory class created");
    }

    createProductA(product) {
        console.log("AbstractFactory.createProductA created");
    }

    createProductB(product) {
        console.log("AbstractFactory.createProductB created");
    }
}

class ConcreteFactory1 extends AbstractFactory {
    constructor() {
        super();
        console.log("ConcreteFactory1 class created");
    }

    createProductA(product) {
        console.log('ConcreteFactory1 createProductA');
        return new ProductA1();
    }

    createProductB(product) {
        console.log('ConcreteFactory1 createProductB');
        return new ProductB1();
    }
}

class ConcreteFactory2 extends AbstractFactory {
    constructor() {
        super();
        console.log("ConcreteFactory2 class created");
    }

    createProductA(product) {
        console.log('ConcreteFactory2 createProductA');
        return new ProductA2();
    }

    createProductB(product) {
        console.log('ConcreteFactory2 createProductB');
        return new ProductB2();
    }
}

class AbstractProductA {
    constructor() {
        console.log('AbstractProductA class created');
    }
}

class AbstractProductB {
    constructor() {
        console.log('AbstractProductB class created');
    }
}

class ProductA1 extends AbstractProductA {
    constructor() {
        super();
        console.log('ProductA1 class created');
    }
}

class ProductA2 extends AbstractProductA {
    constructor() {
        super();
        console.log('ProductA2 class created');
    }
}

class ProductB1 extends AbstractProductB {
    constructor() {
        super();
        console.log('ProductB1 class created');
    }
}

class ProductB2 extends AbstractProductB {
    constructor() {
        super();
        console.log('ProductB2 class created');
    }
}

var factory1 = new ConcreteFactory1();
var productB1 = factory1.createProductB();
var productA1 = factory1.createProductA();

var factory2 = new ConcreteFactory2();
var productA2 = factory2.createProductA();
var productB2 = factory2.createProductB();
```
---

### Typescript实现
``` ts
namespace AbstractFactoryPattern {
    export interface AbstractProductA {
        methodA(): string;
    }
    export interface AbstractProductB {
        methodB(): number;
    }

    export interface AbstractFactory {
        createProductA(param?: any) : AbstractProductA;
        createProductB() : AbstractProductB;
    }


    export class ProductA1 implements AbstractProductA {
        methodA = () => {
            return "This is methodA of ProductA1";
        }
    }
    export class ProductB1 implements AbstractProductB {
        methodB = () => {
            return 1;
        }
    }

    export class ProductA2 implements AbstractProductA {
        methodA = () => {
            return "This is methodA of ProductA2";
        }
    }
    export class ProductB2 implements AbstractProductB {
        methodB = () => {
            return 2;
        }
    }


    export class ConcreteFactory1 implements AbstractFactory {
        createProductA(param?: any) : AbstractProductA {
            return new ProductA1();
        }

        createProductB(param?: any) : AbstractProductB {
            return new ProductB1();
        }
    }
    export class ConcreteFactory2 implements AbstractFactory {
        createProductA(param?: any) : AbstractProductA {
            return new ProductA2();
        }

        createProductB(param?: any) : AbstractProductB {
            return new ProductB2();
        }
    }


    export class Tester {
        private abstractProductA: AbstractProductA;
        private abstractProductB: AbstractProductB;

        constructor(factory: AbstractFactory) {
            this.abstractProductA = factory.createProductA();
            this.abstractProductB = factory.createProductB();
        }

        public test(): void {
            console.log(this.abstractProductA.methodA());
            console.log(this.abstractProductB.methodB());
        }
    }

 }

```
---

## 参考以下内容:
 - [图说设计模式](https://design-patterns.readthedocs.io/zh_CN/latest/)

 - [ECMAScript2016-Design-Patterns](https://github.com/ryouaki/ECMAScript2016-Design-Patterns)
 - [Design Patterns in TypeScript](https://github.com/torokmark/design_patterns_in_typescript)
