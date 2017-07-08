---
title: 观察者模式(Observer)-设计模式
date: 2017-05-05 23:58:44
categories: [design-pattern, Behavioral Patterns]
tags: [javasctipt, 观察者模式]
---
> 软件模式是将模式的一般概念应用于软件开发领域，即软件开发的 总体指导思路或参照样板。
> 软件模式并非仅限于设计模式，还包括 架构模式、分析模式和过程模式等，
> 实际上，在软件生存期的每一个阶段都存在着一些被认同的模式。

### 观察者模式的定义
- 观察者模式定义对象间的一种一对多依赖关系，使得每当一个对象状态发生改变时，其相关依赖对象皆得到通知并被自动更新。
- 观察者模式又叫做发布-订阅模式、模型-视图模式、源-监听器模式或从属者模式。
- 观察者模式是一种对象行为型模式。


### 观察者模式包含四个角色：
- Subject-目标又称为主题，它是指被观察的对象；
- ConcreteSubject-具体目标是目标类的子类，通常它包含有经常发生改变的数据，当它的状态发生改变时，向它的各个观察者发出通知；
- Observer-观察者将对观察目标的改变做出反应；
- ConcreteObserver-在具体观察者中维护一个指向具体目标对象的引用，它存储具体观察者的有关状态，这些状态需要和具体目标的状态保持一致。

---

### ES6实现
``` js
class Subject {
    constructor() {
        console.log('Subject Class created');
    }

    attach(observer) {
        this.observers.push(observer);
        console.log('Subject.attach invoked');
    }

    dettach(observer) {
        console.log('Subject.dettach invoked');
        for (var i in this.observers) {
            if (this.observers[i] === observer) {
                this.observers.splice(i, 1);
            }
        }
    }

    notify() {
        console.log('Subject.notify invoked');
        for (var i in this.observers) {
            this.observers[i].update(this);
        }
    }
}

class ConcreteSubject extends Subject {
    constructor() {
        super();
        this.subjectState = null;
        this.observers = [];
        console.log('ConcreteSubject Class created');
    }

    getState() {
        console.log('ConcreteSubject.getState invoked');
        return this.subjectState;
    }

    setState(state) {
        console.log('ConcreteSubject.setState invoked');
        this.subjectState = state;
        this.notify();
    }
}

class Observer {
    constructor() {
        console.log('Observer Class created');
    }

    update() {
        console.log('Observer.update invoked');
    }
}

class ConcreteObserver extends Observer {
    constructor() {
        super();
        this.observerState = '';
        console.log('ConcreteObserver Class created');
    }

    update(Subject) {
        console.log('ConcreteObserver.update invoked');
        this.observerState = Subject.getState();
        console.log('Observer new state: ' + this.observerState);
    }
}

var observer1 = new ConcreteObserver();
var observer2 = new ConcreteObserver();
var subject = new ConcreteSubject();
subject.attach(observer1);
subject.attach(observer2);
subject.setState('state 1');

```
---

### Typescript实现
``` ts
namespace ObserverPattern {
    export class Subject {
        private observers: Observer[] = [];

        public register(observer: Observer): void {
            console.log(observer, "is pushed!");
            this.observers.push(observer);
        }

        public unregister(observer: Observer): void {
            var n: number = this.observers.indexOf(observer);
            console.log(observer, "is removed");
            this.observers.splice(n, 1);
        }

        public notify(): void {
            console.log("notify all the observers", this.observers);
            var i: number
              , max: number;

            for (i = 0, max = this.observers.length; i < max; i += 1) {
                this.observers[i].notify();
            }
        }
    }

    export class ConcreteSubject extends Subject {
        private subjectState: number;

        get SubjectState(): number {
            return this.subjectState;
        }

        set SubjectState(subjectState: number) {
            this.subjectState = subjectState;
        }
    }

    export class Observer {
        public notify(): void {
            throw new Error("Abstract Method!");
        }
    }

    export class ConcreteObserver extends Observer {
        private name: string;
        private state: number;
        private subject: ConcreteSubject;

        constructor (subject: ConcreteSubject, name: string) {
            super();
            console.log("ConcreteObserver", name, "is created!");
            this.subject = subject;
            this.name = name;
        }

        public notify(): void {
            console.log("ConcreteObserver's notify method");
            console.log(this.name, this.state);
            this.state = this.subject.SubjectState;
        }

        get Subject(): ConcreteSubject {
            return this.subject;
        }

        set Subject(subject: ConcreteSubject) {
            this.subject = subject;
        }
    }
}

```
---

## 参考以下内容:
 - [图说设计模式](https://design-patterns.readthedocs.io/zh_CN/latest/)
 - [ECMAScript2016-Design-Patterns](https://github.com/ryouaki/ECMAScript2016-Design-Patterns)
 - [Design Patterns in TypeScript](https://github.com/torokmark/design_patterns_in_typescript)
