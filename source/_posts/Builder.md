---
title:  建造者模式(Builder)-设计模式
date: 2017-05-05 23:58:44
categories: [design-pattern, creational-patterns]
tags: [javasctipt,  建造者模式]
---
> 软件模式是将模式的一般概念应用于软件开发领域，即软件开发的 总体指导思路或参照样板。
> 软件模式并非仅限于设计模式，还包括 架构模式、分析模式和过程模式等，
> 实际上，在软件生存期的每一个阶段都存在着一些被认同的模式。

###  建造者模式包含如下四个角色：
- Builder-抽象建造者为创建一个产品对象的各个部件指定抽象接口；
- ConcreteBuilder-具体建造者实现了抽象建造者接口，实现各个部件的构造和装配方法，定义并明确它所创建的复杂对象，也可以提供一个方法返回创建好的复杂产品对象；
- Product-产品角色是被构建的复杂对象，包含多个组成部件；
- Director-指挥者负责安排复杂对象的建造次序，指挥者与抽象建造者之间存在关联关系，可以在其construct()建造方法中调用建造者对象的部件构造与装配方法，完成复杂对象的建造

### 建造者模式的结构中引入了一个指挥者类，该类的作用主要有两个：
- 一方面它隔离了客户与生产过程；
- 另一方面它负责控制产品的生成过程。指挥者针对抽象建造者编程，客户端只需要知道具体建造者的类型，即可通过指挥者类调用建造者的相关方法，返回一个完整的产品对象。

---

### ES6实现
``` js
class Builder {
    constructor() {
        console.log('Builder Class created!');
    }

    buildPart(partName) {
        console.log('Builder.buildPart invoked!');
    }
}

class ConcreteBuilder extends Builder {
    constructor() {
        super();
        console.log('ConcreteBuilder Class created!');
    }

    buildPart(partName) {
        super.buildPart(partName);
        console.log('ConcreteBuilder.buildPart invoked!');
        this.product = new Product(partName);
    }
    getResult() {
        console.log('ConcreteBuilder.getResult invoked!');
        return this.product;
    }
}

class Product {
    constructor(material) {
        console.log("Product class created");
        this.data = material
    }
}

class Director {
    constructor() {
        this.structure = ['Prod1', 'Prod2', 'Prod3'];
        console.log("Director class created");
    }

    construct() {
        console.log("Director.Construct created");
        for (var prodName in this.structure) {
            let builder = new ConcreteBuilder();
            builder.buildPart(this.structure[prodName]);
            builder.getResult();
        }
    }
}

let director = new Director();
director.construct();

```
---

### Typescript实现
``` js
namespace BuilderPattern {
    export class UserBuilder {
        private name: string;
        private age: number;
        private phone: string;
        private address: string;

        constructor(name: string) {
            this.name = name;
        }

        get Name() {
            return this.name;
        }
        setAge(value: number): UserBuilder {
            this.age = value;
            return this;
        }
        get Age() {
            return this.age;
        }
        setPhone(value: string): UserBuilder {
            this.phone = value;
            return this;
        }
        get Phone() {
            return this.phone;
        }
        setAddress(value: string): UserBuilder {
            this.address = value;
            return this;
        }
        get Address() {
            return this.address;
        }

        build(): User {
            return new User(this);
        }
    }

    export class User {
        private name: string;
        private age: number;
        private phone: string;
        private address: string;

        constructor(builder: UserBuilder) {
            this.name = builder.Name;
            this.age = builder.Age;
            this.phone = builder.Phone;
            this.address = builder.Address
        }

        get Name() {
            return this.name;
        }
        get Age() {
            return this.age;
        }
        get Phone() {
            return this.phone;
        }
        get Address() {
            return this.address;
        }
    }

}

```
---

## 参考以下内容:
 - [图说设计模式](https://design-patterns.readthedocs.io/zh_CN/latest/)

 - [ECMAScript2016-Design-Patterns](https://github.com/ryouaki/ECMAScript2016-Design-Patterns)
 - [Design Patterns in TypeScript](https://github.com/torokmark/design_patterns_in_typescript)
