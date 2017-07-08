---
title: 中介模式(Mediator)-设计模式
date: 2017-05-05 23:58:44
categories: [design-pattern, Behavioral Patterns]
tags: [javasctipt, 中介模式]
---
> 软件模式是将模式的一般概念应用于软件开发领域，即软件开发的 总体指导思路或参照样板。
> 软件模式并非仅限于设计模式，还包括 架构模式、分析模式和过程模式等，
> 实际上，在软件生存期的每一个阶段都存在着一些被认同的模式。

### 中介模式的定义
- 中介者模式用一个中介对象来封装一系列的对象交互，中介者使各对象不需要显式地相互引用，从而使其耦合松散，而且可以独立地改变它们之间的交互。
- 每一个同事对象在需要和其他同事对象通信时，先与中介者通信，通过中介者来间接完成与其他同事类的通信；
- 在具体同事类中实现了在抽象同事类中定义的方法。
- 中介者模式又称为调停者模式，它是一种对象行为型模式。
### 中介模式包含二个角色：
- Mediator-抽象中介者用于定义一个接口，该接口用于与各同事对象之间的通信；
- ConcreteMediator-具体中介者是抽象中介者的子类，通过协调各个同事对象来实现协作行为，了解并维护它的各个同事对象的引用；
- Colleague-抽象同事类定义各同事的公有方法；
- Colleague-具体同事类是抽象同事类的子类，每一个同事对象都引用一个中介者对象；

---

### ES6实现
``` js
class Mediator {
    constructor() {
        console.log('Mediator Class created');
    }

    colleagueChanged(colleague) {
        console.log('Mediator.colleagueChanged invoked');
    }
}

class ConcreteMediator extends Mediator {
    constructor() {
        super();
        console.log('ConcreteMediator Class created');
        this.colleague1 = new ConcreteColleague1(this);
        this.colleague2 = new ConcreteColleague2(this);
    }

    colleagueChanged(colleague) {
        console.log('ConcreteMediator.colleagueChanged invoked');
        switch (colleague) {
            case this.colleague1:
                console.log('ConcreteColleague1 has Changed -> change ConcreteColleague2.feature: ');
                this.colleague2.setFeature('new feature 2');
                break
            case this.colleague2:
                console.log('ConcreteColleague2 has Changed, but do nothing');
                break
            default:
                console.log('Do nothing');
        }
    }
}

class Colleague {
    constructor() {
        console.log('Colleague Class created');
    }

    changed() {
        console.log('Colleague.changed invoked');
        this.mediator.colleagueChanged(this);
    }
}

class ConcreteColleague1 extends Colleague {
    constructor(mediator) {
        super();
        console.log('ConcreteColleague1 Class created');
        this.mediator = mediator;
        this.feature = "feature 1";
    }

    setFeature(feature) {
        console.log('ConcreteColleague1.setFeature invoked');
        console.log('ConcreteColleague1 Feature has changed from ' + this.feature + ' to ' + feature)
        this.feature = feature;
        this.changed();
    }
}

class ConcreteColleague2 extends Colleague {
    constructor(mediator) {
        super();
        console.log('ConcreteColleague2 Class created');
        this.mediator = mediator;
        this.feature = "feature 2";
    }

    setFeature(feature) {
        console.log('ConcreteColleague2.setFeature invoked');
        console.log('ConcreteColleague2 Feature has changed from ' + this.feature + ' to ' + feature);
        this.feature = feature;
        this.changed();
    }
}

var mediator = new ConcreteMediator();
mediator.colleague1.setFeature("new feature 1");
```
---

### Typescript实现
``` ts
namespace MediatorPattern {
    export interface Mediator {
        send(msg: string, colleague: Colleague): void;
    }

    export class Colleague {
        public mediator: Mediator;

        constructor(mediator: Mediator) {
            this.mediator = mediator;
        }

        public send(msg: string): void {
            throw new Error("Abstract Method!");
        }

        public receive(msg: string): void {
            throw new Error("Abstract Method!");
        }
    }

    export class ConcreteColleagueA extends Colleague {
        constructor(mediator: Mediator) {
            super(mediator);
        }

        public send(msg: string): void {
            this.mediator.send(msg, this);
        }

        public receive(msg: string): void {
            console.log(msg, "`receive` of ConcreteColleagueA is being called!");
        }
    }

    export class ConcreteColleagueB extends Colleague {
        constructor(mediator: Mediator) {
            super(mediator);
        }

        public send(msg: string): void {
            this.mediator.send(msg, this);
        }

        public receive(msg: string): void {
            console.log(msg, "`receive` of ConcreteColleagueB is being called!");
        }
    }

    export class ConcreteMediator implements Mediator {
        public concreteColleagueA: ConcreteColleagueA;
        public concreteColleagueB: ConcreteColleagueB;

        public send(msg: string, colleague: Colleague): void {
            if (this.concreteColleagueA === colleague) {
                this.concreteColleagueB.receive(msg);
            } else {
                this.concreteColleagueA.receive(msg);
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
