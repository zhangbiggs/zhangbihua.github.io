---
title: 简单工厂模式(Factory)-设计模式
date: 2017-05-05 23:58:44
categories: [design-pattern, creational-patterns]
tags: [javasctipt, 简单工厂模式]
---
> 软件模式是将模式的一般概念应用于软件开发领域，即软件开发的 总体指导思路或参照样板。
> 软件模式并非仅限于设计模式，还包括 架构模式、分析模式和过程模式等，
> 实际上，在软件生存期的每一个阶段都存在着一些被认同的模式。

### 简单工厂模式的定义
- 简单工厂模式：当你需要什么，只需要传入一个正确的参数，就可以获取你所需要的对象，而无须知道其创建细节。

### 简单工厂模式包含三个角色：
- Factory-工厂角色负责实现创建所有实例的内部逻辑；
- Product-抽象产品角色是所创建的所有对象的父类，负责描述所有实例所共有的公共接口；
- ConcreteProduct-具体产品角色是创建目标，所有创建的对象都充当这个角色的某个具体类的实例。

---

### ES6实现
``` js
class Product {
    constructor() {
        console.log('Product Class created');
    }
}

class ConcreteProduct extends Product {
    constructor() {
        super();
        console.log('ConcreteProduct Class created');
    }
}

class Creator {
    constructor() {
        console.log('Creator Class created');
    }

    factoryMethod() {
        console.log('Creator.factoryMethod created');
    }

    anOperation() {
        console.log('Creator.anOperation created');
        this.product = this.factoryMethod();
        console.log(this.product instanceof ConcreteProduct);
    }
}

class ConcreteCreator extends Creator {

    constructor() {
        super();
        console.log('ConcreteCreator Class created');
    }

    factoryMethod() {
        return new ConcreteProduct();
    }
}

var factory = new ConcreteCreator();
factory.anOperation();
```
---

### Typescript实现
``` js
interface AbstractProduct {
    method(param?: any) : void;
}

class ConcreteProductA implements AbstractProduct {
    method = (param?: any) => {
        return "Method of ConcreteProductA";
    }
}

class ConcreteProductB implements AbstractProduct {
    method = (param?: any) => {
        return "Method of ConcreteProductB";
    }
}

class ProductFactory {
    public static createProduct(type: string) : AbstractProduct {
        if (type === "A") {
            return new ConcreteProductA();
        } else if (type === "B") {
            return new ConcreteProductB();
        }

        return null;
    }
}
```
---

## 参考以下内容:
 - [图说设计模式](https://design-patterns.readthedocs.io/zh_CN/latest/)

 - [ECMAScript2016-Design-Patterns](https://github.com/ryouaki/ECMAScript2016-Design-Patterns)
 - [Design Patterns in TypeScript](https://github.com/torokmark/design_patterns_in_typescript)
