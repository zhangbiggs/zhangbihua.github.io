---
title:  原型模式(Prototype)-设计模式
date: 2017-05-05 23:58:44
categories: [design-pattern, creational-patterns]
tags: [javasctipt,  原型模式]
---
> 软件模式是将模式的一般概念应用于软件开发领域，即软件开发的 总体指导思路或参照样板。
> 软件模式并非仅限于设计模式，还包括 架构模式、分析模式和过程模式等，
> 实际上，在软件生存期的每一个阶段都存在着一些被认同的模式。


---

### ES6实现
``` js
class Prototype {
    constructor(prototype) {
        console.log("Prototype Class created");
    }

    setFeature(key, val) {
        this[key] = val
    }

    clone() {
        console.log("Prototype.clone invoked");
    }
}

class ConcretePrototype1 extends Prototype {
    constructor() {
        super();
        console.log("ConcretePrototype1 created");
        this.feature = "feature 1"
    }

    clone() {
        console.log('ConcretePrototype1.clone invoked');
        let clone = new ConcretePrototype1();
        let keys = Object.keys(this);

        keys.forEach(k => clone.setFeature(k, this[k]));

        console.log("ConcretePrototype1 cloned");
        return clone;
    }
}

class ConcretePrototype2 extends Prototype {
    constructor() {
        super();
        console.log("ConcretePrototype2 created");
        this.feature = "feature 2"
    }

    clone() {
        console.log('ConcretePrototype2.Clone function');
        let clone = new ConcretePrototype2();
        let keys = Object.keys(this);

        keys.forEach(k => clone.setFeature(k, this[k]));
        console.log("ConcretePrototype2 cloned");
        return clone;
    }
}

var proto1 = new ConcretePrototype1();
proto1.setFeature('feature', "feature 11");
var clone1 = proto1.clone();
console.log(clone1.feature);
console.log(typeof clone1);
console.log(clone1 === proto1);

var proto2 = new ConcretePrototype2();
proto2.setFeature('feature', "feature 22");
var clone2 = proto2.clone();
console.log(clone2.feature);
console.log(typeof clone2);
console.log(clone2 === proto2);
```
---

### Typescript实现
``` js
namespace PrototypePattern {
    export interface Prototype {
        clone(): Prototype;
        toString(): string;
    }

    export class Concrete1 implements Prototype {

        clone() : Prototype {
            return new Concrete1();
        }

        toString(): string {
            return "This is Concrete1";
        }
    }

    export class Concrete2 implements Prototype {

        clone() : Prototype {
            return new Concrete2();
        }

        toString(): string {
            return "This is Concrete2";
        }
    }

    export class Concrete3 implements Prototype {

        clone() : Prototype {
            return new Concrete3();
        }

        toString(): string {
            return "This is Concrete3";
        }
    }


    export class Builder {
        private prototypeMap: { [s: string]: Prototype; } = {};

        constructor() {
            this.prototypeMap['c1'] = new Concrete1();
            this.prototypeMap['c2'] = new Concrete2();
            this.prototypeMap['c3'] = new Concrete3();
        }

        createOne(s: string): Prototype {
            console.log(s);
            return this.prototypeMap[s].clone();
        }
    }
}
```
---

## 参考以下内容:
 - [图说设计模式](https://design-patterns.readthedocs.io/zh_CN/latest/)

 - [ECMAScript2016-Design-Patterns](https://github.com/ryouaki/ECMAScript2016-Design-Patterns)
 - [Design Patterns in TypeScript](https://github.com/torokmark/design_patterns_in_typescript)
