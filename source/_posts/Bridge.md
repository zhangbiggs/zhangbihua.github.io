---
title: 桥接模式(Bridge)-设计模式
date: 2017-05-05 23:58:44
categories: [design-pattern, Structural Patterns]
tags: [javasctipt, 桥接模式]
---
> 软件模式是将模式的一般概念应用于软件开发领域，即软件开发的 总体指导思路或参照样板。
> 软件模式并非仅限于设计模式，还包括 架构模式、分析模式和过程模式等，
> 实际上，在软件生存期的每一个阶段都存在着一些被认同的模式。

### 桥接模式的定义
- 桥接模式：模式将抽象部分与它的实现部分分离，使它们都可以独立地变化。它是一种对象结构型模式，又称为柄体(Handle and Body)模式或接口(Interface)模式。

### 桥接模式包含四个角色：
- Abstraction-抽象类中定义了一个实现类接口类型的对象并可以维护该对象；
- RefinedAbstraction-扩充抽象类扩充由抽象类定义的接口，它实现了在抽象类中定义的抽象业务方法，在扩充抽象类中可以调用在实现类接口中定义的业务方法；
- Implementor实现类接口定义了实现类的接口，实现类接口仅提供基本操作，而抽象类定义的接口可能会做更多更复杂的操作；
- ConcreteImplementor具体实现类实现了实现类接口并且具体实现它，在不同的具体实现类中提供基本操作的不同实现，在程序运行时，具体实现类对象将替换其父类对象，提供给客户端具体的业务操作方法。
---

### ES6实现
``` js
class Abstraction {
    constructor() {
        console.log('Abstraction Class created');
    }

    operation() {
        console.log('Abstraction.operation invoked');
        this.imp.operationImp();
    }
}

class RefinedAbstraction extends Abstraction {
    constructor() {
        super()
        console.log('RefinedAbstraction Class created');
    }

    setImp(imp) {
        console.log('RefinedAbstraction.setImp invoked');
        this.imp = imp
    }
}

class Implementor {
    constructor() {
        console.log('Implementor Class created');
    }

    operationImp() {
        console.log('Implementor.operationImp invoked');
    }
}

class ConcreteImplementorA extends Implementor {
    constructor() {
        super()
        console.log('ConcreteImplementorA Class created');
    }

    operationImp() {
        console.log('ConcreteImplementorA.operationImp invoked');
    }
}

class ConcreteImplementorB extends Implementor {
    constructor() {
        super()
        console.log('ConcreteImplementorB Class created');
    }

    operationImp() {
        console.log('ConcreteImplementorB.operationImp invoked');
    }
}

var abstraction = new RefinedAbstraction();
abstraction.setImp(new ConcreteImplementorA());
abstraction.operation();
abstraction.setImp(new ConcreteImplementorB());
abstraction.operation();
```
---

### Typescript实现
``` ts
class Abstraction {
    constructor() {
        console.log('Abstraction Class created');
    }

    operation() {
        console.log('Abstraction.operation invoked');
        this.imp.operationImp();
    }
}

class RefinedAbstraction extends Abstraction {
    constructor() {
        super()
        console.log('RefinedAbstraction Class created');
    }

    setImp(imp) {
        console.log('RefinedAbstraction.setImp invoked');
        this.imp = imp
    }
}

class Implementor {
    constructor() {
        console.log('Implementor Class created');
    }

    operationImp() {
        console.log('Implementor.operationImp invoked');
    }
}

class ConcreteImplementorA extends Implementor {
    constructor() {
        super()
        console.log('ConcreteImplementorA Class created');
    }

    operationImp() {
        console.log('ConcreteImplementorA.operationImp invoked');
    }
}

class ConcreteImplementorB extends Implementor {
    constructor() {
        super()
        console.log('ConcreteImplementorB Class created');
    }

    operationImp() {
        console.log('ConcreteImplementorB.operationImp invoked');
    }
}

var abstraction = new RefinedAbstraction();
abstraction.setImp(new ConcreteImplementorA());
abstraction.operation();
abstraction.setImp(new ConcreteImplementorB());
abstraction.operation();
```
---

## 参考以下内容:
 - [图说设计模式](https://design-patterns.readthedocs.io/zh_CN/latest/)
 - [ECMAScript2016-Design-Patterns](https://github.com/ryouaki/ECMAScript2016-Design-Patterns)
 - [Design Patterns in TypeScript](https://github.com/torokmark/design_patterns_in_typescript)
