---
title: 适配器模式(Adapter)-设计模式
date: 2017-05-05 23:58:44
categories: [design-pattern, Structural Patterns]
tags: [javasctipt, 适配器模式]
---
> 软件模式是将模式的一般概念应用于软件开发领域，即软件开发的 总体指导思路或参照样板。
> 软件模式并非仅限于设计模式，还包括 架构模式、分析模式和过程模式等，
> 实际上，在软件生存期的每一个阶段都存在着一些被认同的模式。

### 适配器模式的定义
- 适配器模式：用于将一个接口转换成客户希望的另一个接口，适配器模式使接口不兼容的那些类可以一起工作，其别名为包装器。适配器模式既可以作为类结构型模式，也可以作为对象结构型模式。

### 适配器模式包含四个角色：
- Target：目标抽象类-定义客户要用的特定领域的接口
- Adapter：适配器类-作为一个转换器，对适配者和抽象目标类进行适配，它是适配器模式的核心
- Adaptee：适配者类-定义了一个已经存在的接口，这个接口需要适配
- Client：客户类-在客户类中针对目标抽象类（Target）进行编程，调用在目标抽象类中定义的业务方法

---

### ES6实现
``` js
class Target {
    constructor(type) {
        console.log('Target Class created!');
        let result = undefined;

        switch (type) {
            case 'adapter':
                result = new AdapterImpl();
                break
            default:
                result = undefined;
        }
        return result;
    }

    request() {
        console.log('Target.request invoked');
    }
}

class Adapter {
    constructor() {
        console.log('Adapter Class created');
    }

    specificRequest() {
        console.log('Adapter.specificRequest invoked');
    }
}

class AdapterImpl extends Adapter {
    constructor() {
        super()
        console.log('AdapterImpl Class created');
    }

    request() {
        console.log('AdapterImpl.request invoked');
        return this.specificRequest();
    }
}

var f = new Target("adapter");
f.request();
```
---

### Typescript实现
``` js
namespace AdapterPattern {
    export class Adaptee {
        public method(): void {
            console.log("`method` of Adaptee is being called");
        }
    }

    export interface Target {
        call(): void;
    }

    export class Adapter implements Target {
        public call(): void {
            console.log("Adapter's `call` method is being called");
            var adaptee: Adaptee = new Adaptee();
            adaptee.method();
        }
    }
}
```
---

## 参考以下内容:
 - [图说设计模式](https://design-patterns.readthedocs.io/zh_CN/latest/)
 - [ECMAScript2016-Design-Patterns](https://github.com/ryouaki/ECMAScript2016-Design-Patterns)
 - [Design Patterns in TypeScript](https://github.com/torokmark/design_patterns_in_typescript)
