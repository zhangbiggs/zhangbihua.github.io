---
title: 装饰模式(Decorator)-设计模式
date: 2017-05-05 23:58:44
categories: [design-pattern, structural-patterns]
tags: [javasctipt, 装饰模式]
---
> 软件模式是将模式的一般概念应用于软件开发领域，即软件开发的 总体指导思路或参照样板。
> 软件模式并非仅限于设计模式，还包括 架构模式、分析模式和过程模式等，
> 实际上，在软件生存期的每一个阶段都存在着一些被认同的模式。

### 装饰模式的定义
- 装饰模式：装饰模式用于动态地给一个对象增加一些额外的职责，就增加对象功 能来说，装饰模式比生成子类实现更为灵活。它是一种对象结构型模 式。

### 装饰模式包含四个角色：
- Component-抽象构件定义了对象的接口，可以给这些对 象动态增加职责（方法）；
- ConcreteComponent-具体构件定义了具体的构件对象，实现了 在抽象构件中声明的方法，装饰器可以给它增加额外的职责（方法）；
- Decorator-抽象装饰类是抽象构件类的子类，用于给具体构件增加职责，但是具 体职责在其子类中实现；
- ConcreteDecorator-具体装饰类是抽象装饰类的子类，负责向构 件添加新的职责。

---

### ES6实现
``` js
class Component {
    constructor() {
        console.log('Component Class created');
    }

    operation() {
        console.log('Component.operation invoked');
    }
}

class ConcreteComponent extends Component {
    constructor() {
        super();
        console.log('ConcreteComponent Class created');
    }

    operation() {
        console.log('ConcreteComponent.operation invoked');
    }
}

class Decorator extends Component {
    constructor(component) {
        super();
        this.component = component;
        console.log('Decorator Class created');
    }

    operation() {
        console.log('Decorator.operation invoked');
        this.component.operation()
    }
}

class ConcreteDecoratorA extends Decorator {
    constructor(component, sign) {
        super(component);
        this.addedState = sign;
        console.log('ConcreteDecoratorA Class created');
    }

    operation() {
        super.operation();
        console.log('ConcreteDecoratorA.operation invoked');
        console.log(this.addedState)
    }
}

class ConcreteDecoratorB extends Decorator {
    constructor(component, sign) {
        super(component);
        this.addedState = sign;
        console.log('ConcreteDecoratorB Class created');
    }

    operation() {
        super.operation();
        console.log('ConcreteDecoratorB.operation invoked');
        console.log(this.addedState + this.addedState + this.addedState + this.addedState + this.addedState);
    }

    addedBehavior() {
        this.operation();
        console.log('ConcreteDecoratorB.operation invoked');
    }
}

var component = new ConcreteComponent();
var decoratorA = new ConcreteDecoratorA(component, 'decoratorA');
var decoratorB = new ConcreteDecoratorB(component, 'decoratorB');
console.log('component: ');
component.operation();
console.log('decoratorA: ');
decoratorA.operation();
console.log('decoratorB: ');
decoratorB.addedBehavior();
```
---

### Typescript实现
``` js
namespace DecoratorPattern {

    export interface Component {
        operation(): void;
    }

    export class ConcreteComponent implements Component {
        private s: String;

        constructor(s: String) {
            this.s = s;
        }

        public operation(): void {
            console.log("`operation` of ConcreteComponent", this.s, " is being called!");
        }
    }

    export class Decorator implements Component {
        private component: Component;
        private id: Number;

        constructor(id: Number, component: Component) {
            this.id = id;
            this.component = component;
        }

        public get Id(): Number {
            return this.id;
        }

        public operation(): void {
            console.log("`operation` of Decorator", this.id, " is being called!");
            this.component.operation();
        }
    }

    export class ConcreteDecorator extends Decorator {
        constructor(id: Number, component: Component) {
            super(id, component);
        }

        public operation(): void {
            super.operation();
            console.log("`operation` of ConcreteDecorator", this.Id, " is being called!");
        }
    }
}
```
---

## 参考以下内容:
 - [图说设计模式](https://design-patterns.readthedocs.io/zh_CN/latest/)
 - [ECMAScript2016-Design-Patterns](https://github.com/ryouaki/ECMAScript2016-Design-Patterns)
 - [Design Patterns in TypeScript](https://github.com/torokmark/design_patterns_in_typescript)
