---
title: 模板方法模式(TemplateMethod)-设计模式
date: 2017-05-05 23:58:44
categories: [design-pattern, Behavioral Patterns]
tags: [javasctipt, 模板方法模式]
---
> 软件模式是将模式的一般概念应用于软件开发领域，即软件开发的 总体指导思路或参照样板。
> 软件模式并非仅限于设计模式，还包括 架构模式、分析模式和过程模式等，
> 实际上，在软件生存期的每一个阶段都存在着一些被认同的模式。

### 模板方法模式的定义
- 表示一个作用于某对象结构中的各元素的操作。它使你可以在不改变各元素的类的前提下定义作用于这些元素的新操作。

---

### ES6实现
``` js
class AbstractClass {
    constructor() {
        console.log('AbstractClass Class created!');
    }

    templateMethod() {
        console.log('AbstractClass.templateMethod invoked');
        this.primitiveOperation1();
        this.primitiveOperation2();
    }

    primitiveOperation1() {
        console.log('AbstractClass.primitiveOperation1 invoked');
    }

    primitiveOperation2() {
        console.log('AbstractClass.primitiveOperation2 invoked');
    }
}

class ConcreteClass extends AbstractClass {
    constructor() {
        super();
        console.log('ConcreteClass Class created!');
    }

    primitiveOperation1() {
        console.log('ConcreteClass.primitiveOperation1 invoked');
    }

    primitiveOperation2() {
        console.log('ConcreteClass.primitiveOperation2 invoked');
    }
}

let obj = new ConcreteClass();
obj.templateMethod();

```
---

### Typescript实现
``` ts
namespace TemplateMethodPattern {
    export class AbstractClass {
        public method1(): void {
            throw new Error("Abstract Method");
        }

        public method2(): void {
            throw new Error("Abstract Method");
        }

        public method3(): void {
            throw new Error("Abstract Method");
        }

        public templateMethod(): void {
            console.log("templateMethod is being called");
            this.method1();
            this.method2();
            this.method3();
        }
    }

    export class ConcreteClass1 extends AbstractClass {
        public method1(): void {
            console.log("method1 of ConcreteClass1");
        }

        public method2(): void {
            console.log("method2 of ConcreteClass1");
        }

        public method3(): void {
            console.log("method3 of ConcreteClass1");
        }
    }

    export class ConcreteClass2 extends AbstractClass {
        public method1(): void {
            console.log("method1 of ConcreteClass2");
        }

        public method2(): void {
            console.log("method2 of ConcreteClass2");
        }

        public method3(): void {
            console.log("method3 of ConcreteClass2");
        }
    }
}

```
---

## 参考以下内容:
 - [图说设计模式](https://design-patterns.readthedocs.io/zh_CN/latest/)
 - [ECMAScript2016-Design-Patterns](https://github.com/ryouaki/ECMAScript2016-Design-Patterns)
 - [Design Patterns in TypeScript](https://github.com/torokmark/design_patterns_in_typescript)
