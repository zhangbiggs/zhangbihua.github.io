---
title: 组合模式(Composite)-设计模式
date: 2017-05-05 23:58:44
categories: [design-pattern, Structural Patterns]
tags: [javasctipt, 组合模式]
---
> 软件模式是将模式的一般概念应用于软件开发领域，即软件开发的 总体指导思路或参照样板。
> 软件模式并非仅限于设计模式，还包括 架构模式、分析模式和过程模式等，
> 实际上，在软件生存期的每一个阶段都存在着一些被认同的模式。

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

    add(Component) {
        console.log('Component.add invoked');
    }

    remove(Component) {
        console.log('Component.remove invoked');
    }

    getChild(key) {
        console.log('Component.getChild invoked');
    }
}

class Leaf extends Component {
    constructor(name) {
        super();
        this.name = name;
        console.log('Leaf Class created');
    }

    operation() {
        console.log('Leaf.operation invoked');
        console.log(this.name);
    }
}

class Composite extends Component {
    constructor(name) {
        super();
        this.name = name;
        this.children = [];
        console.log('Composite Class created');
    }

    operation() {
        console.log('Composite operation for: ' + this.name)
        for (var i in this.children) {
            this.children[i].operation();
        }
    }

    add(Component) {
        console.log('Composite.add invoked');
        this.children.push(Component);
    }

    remove(Component) {
        console.log('Composite.remove invoked');
        for (var i in this.children) {
            if (this.children[i] === Component) {
                this.children.splice(i, 1);
            }
        }
    }

    getChild(key) {
        console.log('Composite.getChild invoked');
        return this.children[key];
    }
}

var composite1 = new Composite('C1');
composite1.add(new Leaf('L1'));
composite1.add(new Leaf('L2'));
var composite2 = new Composite('C2');
composite2.add(composite1);
composite1.getChild(1).operation();
composite2.operation();
```
---

### Typescript实现
``` ts
namespace CompositePattern {
    export interface Component {
        operation(): void;
    }

    export class Composite implements Component {

        private list: Component[];
        private s: String;

        constructor(s: String) {
            this.list = [];
            this.s = s;
        }

        public operation(): void {
            console.log("`operation of `", this.s)
            for (var i = 0; i < this.list.length; i += 1) {
                this.list[i].operation();
            }
        }

        public add(c: Component): void {
            this.list.push(c);
        }

        public remove(i: number): void {
            if (this.list.length <= i) {
                throw new Error("index out of bound!");
            }
            this.list.splice(i, 1);
        }
    }

    export class Leaf implements Component {
        private s: String;
        constructor(s: String) {
            this.s = s;
        }
        public operation(): void {
            console.log("`operation` of Leaf", this.s, " is called.");
        }
    }
}
```
---

## 参考以下内容:
 - [图说设计模式](https://design-patterns.readthedocs.io/zh_CN/latest/)
 - [ECMAScript2016-Design-Patterns](https://github.com/ryouaki/ECMAScript2016-Design-Patterns)
 - [Design Patterns in TypeScript](https://github.com/torokmark/design_patterns_in_typescript)
