---
title: 享元模式(Flyweight)-设计模式
date: 2017-05-05 23:58:44
categories: [design-pattern, Structural Patterns]
tags: [javasctipt, 享元模式]
---
> 软件模式是将模式的一般概念应用于软件开发领域，即软件开发的 总体指导思路或参照样板。
> 软件模式并非仅限于设计模式，还包括 架构模式、分析模式和过程模式等，
> 实际上，在软件生存期的每一个阶段都存在着一些被认同的模式。

### 享元模式的定义
- 享元模式：享元模式运用共享技术有效地支持大量细粒度对象的复用。系统只使用少量的对象，而这些对象都很相似，状态变化很小，可以实现对象的多次复用，它是一种对象结构型模式。

### 享元模式包含四个角色：
- Flyweight-抽象享元类声明一个接口，通过它可以接受并作用于外部状态；
- ConcreteFlyweight-具体享元类实现了抽象享元接口，其实例称为享元对象；
- UnsharedConcreteFlyweight-非共享具体享元是不能被共享的抽象享元类的子类；
- FlyweightFactory-享元工厂类用于创建并管理享元对象，它针对抽象享元类编程，将各种类型的具体享元对象存储在一个享元池中。
---

### ES6实现
``` js
class FlyweightFactory {
    constructor() {
        this.flyweights = {};
        console.log('FlyweightFactory Class created');
    }

    getFlyweight(key) {
        console.log('FlyweightFactory.getFlyweight invoked');
        if (this.flyweights[key]) {
            return this.flyweights[key];
        } else {
            this.flyweights[key] = new ConcreteFlyweight(key);
            return this.flyweights[key];
        }
    }

    createGibberish(keys) {
        console.log('FlyweightFactory.createGibberish invoked');
        return new UnsharedConcreteFlyweight(keys, this);
    }
}

class Flyweight {
    constructor() {
        console.log('Flyweight Class created');
    }

    operation(extrinsicState) {
        console.log('Flyweight.operation invoked');
    }
}

class ConcreteFlyweight extends Flyweight {
    constructor(key) {
        super();
        this.intrinsicState = key;
        console.log('ConcreteFlyweight Class created');
    }

    operation(extrinsicState) {
        console.log('ConcreteFlyweight.operation invoked');
        return extrinsicState + this.intrinsicState;
    }
}

class UnsharedConcreteFlyweight extends Flyweight {
    constructor(keys, flyweights) {
        super();
        this.flyweights = flyweights;
        this.keys = keys;
        console.log('UnsharedConcreteFlyweight Class created');
    }

    operation(extrinsicState) {
        console.log('UnsharedConcreteFlyweight.operation invoked');
        var key, word = '';
        for (var i = 0; i < extrinsicState; i++) {
            key = this.keys[Math.floor(Math.random() * (this.keys.length))];
            word = this.flyweights.getFlyweight(key).operation(word);
        }
        console.log('UnsharedConcreteFlyweight Operation: ');
        console.log(word);
    }
}


var flyweights = new FlyweightFactory();
var gibberish = flyweights.createGibberish(['-', '+', '*']);
gibberish.operation(5);


```
---

### Typescript实现
``` ts
namespace FlyweightPattern {

    export interface Flyweight {
        operation(s: String): void;
    }

    export class ConcreteFlyweight implements Flyweight {
        private instrinsicState: String;

        constructor(instrinsicState: String) {
            this.instrinsicState = instrinsicState;
        }

        public operation(s: String): void {
            console.log("`operation` of ConcreteFlyweight", s, " is being called!");
        }
    }

    export class UnsharedConcreteFlyweight implements Flyweight {
        private allState: number;

        constructor(allState: number) {
            this.allState = allState;
        }

        public operation(s: String): void {
            console.log("`operation` of UnsharedConcreteFlyweight", s, " is being called!");
        }
    }

    export class FlyweightFactory {

        private fliesMap: { [s: string]: Flyweight; } = <any>{};

        constructor() { }

        public getFlyweight(key: string): Flyweight {

            if (this.fliesMap[key] === undefined || null) {
                this.fliesMap[key] = new ConcreteFlyweight(key);
            }
            return this.fliesMap[key];
        }
    }
}

```
---

## 参考以下内容:
 - [图说设计模式](https://design-patterns.readthedocs.io/zh_CN/latest/)
 - [ECMAScript2016-Design-Patterns](https://github.com/ryouaki/ECMAScript2016-Design-Patterns)
 - [Design Patterns in TypeScript](https://github.com/torokmark/design_patterns_in_typescript)
