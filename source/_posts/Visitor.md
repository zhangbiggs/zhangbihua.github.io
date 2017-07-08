---
title: 访问者模式(Visitor)-设计模式
date: 2017-05-05 23:58:44
categories: [design-pattern, Behavioral Patterns]
tags: [javasctipt, 访问者模式]
---
> 软件模式是将模式的一般概念应用于软件开发领域，即软件开发的 总体指导思路或参照样板。
> 软件模式并非仅限于设计模式，还包括 架构模式、分析模式和过程模式等，
> 实际上，在软件生存期的每一个阶段都存在着一些被认同的模式。

### 访问者模式的定义
- 表示一个作用于某对象结构中的各元素的操作。它使你可以在不改变各元素的类的前提下定义作用于这些元素的新操作。

---

### ES6实现
``` js
class Visitor {
    constructor() {
        console.log('Visitor Class created!');
    }

    visitConcreteElementA(ConcreteElementA) {
        console.log('Visitor.visitConcreteElementA invoked');
    }

    visitConcreteElementB(ConcreteElementB) {
        console.log('Visitor.visitConcreteElementB invoked');
    }
}

class ConcreteVisitor1 extends Visitor {
    constructor() {
        super();
        console.log('ConcreteVisitor1 Class created!');
    }

    visitConcreteElementA(ConcreteElementA) {
        console.log('ConcreteVisitor1.visitConcreteElementA invoked');
    }

    visitConcreteElementB(ConcreteElementB) {
        console.log('ConcreteVisitor1.visitConcreteElementB invoked');
    }
}

class ConcreteVisitor2 extends Visitor {
    constructor() {
        super();
        console.log('ConcreteVisitor2 Class created!');
    }

    visitConcreteElementA(ConcreteElementA) {
        console.log('ConcreteVisitor2.visitConcreteElementA invoked');
    }

    visitConcreteElementB(ConcreteElementB) {
        console.log('ConcreteVisitor2.visitConcreteElementB invoked');
    }
}

class ObjectStructure {
    constructor() {
        console.log('ObjectStructure Class created!');
    }
}

class Element {
    constructor() {
        console.log('Element Class created!');
    }

    Accept(visitor) {
        console.log('Element.visitConcreteElementB invoked');
    }
}

class ConcreteElementA extends Element {
    constructor() {
        super();
        console.log('ConcreteElementA Class created!');
    }

    accept(visitor) {
        console.log('ConcreteElementA.accept invoked');
        visitor.visitConcreteElementA(this);
    }

    operationA() {
        console.log('ConcreteElementA.operationA invoked');
    }
}

class ConcreteElementB extends Element {
    constructor() {
        super();
        console.log('ConcreteElementB Class created!');
    }

    accept(visitor) {
        console.log('ConcreteElementB.accept invoked');
        visitor.visitConcreteElementB(this);
    }

    operationB() {
        console.log('ConcreteElementB.operationB invoked');
    }
}

let visitor1 = new ConcreteVisitor1();
let visitor2 = new ConcreteVisitor2();
let elementA = new ConcreteElementA();
let elementB = new ConcreteElementB();
elementA.accept(visitor1);
elementB.accept(visitor2);

```
---

### Typescript实现
``` ts
namespace VisitorPattern {
    export interface Visitor {
        visitConcreteElement1(concreteElement1: ConcreteElement1): void;
        visitConcreteElement2(concreteElement2: ConcreteElement2): void;
    }

    export class ConcreteVisitor1 implements Visitor {
        public visitConcreteElement1(concreteElement1: ConcreteElement1): void {
            console.log("`visitConcreteElement1` of ConcreteVisitor1 is being called!");
        }

        public visitConcreteElement2(concreteElement2: ConcreteElement2): void {
            console.log("`visitConcreteElement2` of ConcreteVisitor1 is being called!");
        }
    }

    export class ConcreteVisitor2 implements Visitor {
        public visitConcreteElement1(concreteElement1: ConcreteElement1): void {
            console.log("`visitConcreteElement1` of ConcreteVisitor2 is being called!");
        }

        public visitConcreteElement2(concreteElement2: ConcreteElement2): void {
            console.log("`visitConcreteElement2` of ConcreteVisitor2 is being called!");
        }
    }


    export interface Element {
        operate(visitor: Visitor): void;
    }

    export class ConcreteElement1 implements Element {
        public operate(visitor: Visitor): void {
            console.log("`operate` of ConcreteElement1 is being called!");
            visitor.visitConcreteElement1(this);
        }
    }

    export class ConcreteElement2 implements Element {
        public operate(visitor: Visitor): void {
            console.log("`operate` of ConcreteElement2 is being called!");
            visitor.visitConcreteElement2(this);
        }
    }

    export class Objs {
        private elements: Element[] = [];

        public attach(e: Element): void {
            this.elements.push(e);
        }

        public detach(e: Element): void {
            var index = this.elements.indexOf(e);
            this.elements.splice(index, 1);
        }

        public operate(visitor: Visitor): void {
            var i = 0,
                max = this.elements.length;

            for(; i < max; i += 1) {
                this.elements[i].operate(visitor);
            }
        }
    }

}


```
---

## 参考以下内容:
 - [图说设计模式](https://design-patterns.readthedocs.io/zh_CN/latest/)
 - [ECMAScript2016-Design-Patterns](https://github.com/ryouaki/ECMAScript2016-Design-Patterns)
 - [Design Patterns in TypeScript](https://github.com/torokmark/design_patterns_in_typescript)
