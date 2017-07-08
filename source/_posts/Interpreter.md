---
title: 解析器模式(Interpreter)-设计模式
date: 2017-05-05 23:58:44
categories: [design-pattern, Behavioral Patterns]
tags: [javasctipt, 解析器模式]
---
> 软件模式是将模式的一般概念应用于软件开发领域，即软件开发的 总体指导思路或参照样板。
> 软件模式并非仅限于设计模式，还包括 架构模式、分析模式和过程模式等，
> 实际上，在软件生存期的每一个阶段都存在着一些被认同的模式。

### 解析器模式的定义
- 解析器模式：在外观模式中，外部与一个子系统的通信必须通过一个统一的外观对象进行，为子系统中的一组接口提供一个一致的界面，
- 外观模式定义了一个高层接口，这个接口使得这一子系统更加容易使用。外观模式又称为门面模式，它是一种对象结构型模式。

### 解析器模式包含二个角色：
- Interpreter-外观角色是在客户端直接调用的角色，在外观角色中可以知道相关的(一个或者多个)子系统的功能和责任，它将所有从客户端发来的请求委派到相应的子系统去，传递给相应的子系统对象处理；
- SubSystem-在软件系统中可以同时有一个或者多个子系统角色，每一个子系统可以不是一个单独的类，而是一个类的集合，它实现子系统的功能

---

### ES6实现
``` js
class Context {
    constructor(input) {
        this.sum = 0;
        this.list = [];
        console.log('Context Class created');
    }

    add(eps) {
        console.log('Context.add invoked');
        this.list.push(eps);
    }

    getList() {
        console.log('Context.getList invoked');
        return this.list;
    }

    getSum() {
        console.log('Context.getSum invoked');
        return this.sum;
    }

    setSum(_sum) {
        this.sum = _sum;
        console.log('Context.setSum invoked');
    }
}

class AbstractExpression {
    constructor() {
        console.log('AbstractExpression Class created');
    }

    interpret(context) {
        console.log('AbstractExpression.interpret invoked');
    }
}

class PlusExpression extends AbstractExpression {
    constructor(name) {
        super();
        this.name = name;
        console.log('PlusExpression Class created');
    }

    interpret(context) {
        console.log('PlusExpression.interpret invoked');
        var sum = context.getSum();
        sum++;
        context.setSum(sum);
    }
}

class MinusExpression extends AbstractExpression {
    constructor() {
        super();
        this.name = '+';
        console.log('MinusExpression Class created');
    }

    interpret(context) {
        console.log('MinusExpression.interpret invoked');
        var sum = context.getSum();
        sum--;
        context.setSum(sum)
    }
}

var context = new Context();
context.setSum(20);

context.add(new PlusExpression());
context.add(new PlusExpression());
context.add(new PlusExpression());

context.add(new MinusExpression());
context.add(new MinusExpression());

var list = context.getList();
for (var i = 0; i < list.length; i++) {
    var expression = list[i];
    expression.interpret(context);
}

console.log("Result：" + context.getSum());

```
---

### Typescript实现
``` ts
namespace InterpreterPattern {
    export class Context {
    }

    export interface AbstractExpression {
        interpret(context: Context): void;
    }

    export class TerminalExpression implements AbstractExpression {
        public interpret(context: Context): void {
            console.log("`interpret` method of TerminalExpression is being called!");
        }
    }

    export class NonterminalExpression implements AbstractExpression {

        public interpret(context: Context): void {
            console.log("`interpret` method of NonterminalExpression is being called!");
        }
    }
}


```
---

## 参考以下内容:
 - [图说设计模式](https://design-patterns.readthedocs.io/zh_CN/latest/)
 - [ECMAScript2016-Design-Patterns](https://github.com/ryouaki/ECMAScript2016-Design-Patterns)
 - [Design Patterns in TypeScript](https://github.com/torokmark/design_patterns_in_typescript)
