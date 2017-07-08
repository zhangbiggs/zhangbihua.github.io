---
title: 状态模式(State)-设计模式
date: 2017-05-05 23:58:44
categories: [design-pattern, Behavioral Patterns]
tags: [javasctipt, 状态模式]
---
> 软件模式是将模式的一般概念应用于软件开发领域，即软件开发的 总体指导思路或参照样板。
> 软件模式并非仅限于设计模式，还包括 架构模式、分析模式和过程模式等，
> 实际上，在软件生存期的每一个阶段都存在着一些被认同的模式。

### 状态模式的定义
- 状态模式允许一个对象在其内部状态改变时改变它的行为，对象看起来似乎修改了它的类。
- 其别名为状态对象，状态模式是一种对象行为型模式。

### 状态模式包含二个角色：
- Context-环境类又称为上下文类，它是拥有状态的对象，在环境类中维护一个抽象状态类State的实例，这个实例定义当前状态，在具体实现时，它是一个State子类的对象，可以定义初始状态；
- State-抽象状态类用于定义一个接口以封装与环境类的一个特定状态相关的行为；
- ConcreteState-具体状态类是抽象状态类的子类，每一个子类实现一个与环境类的一个状态相关的行为，每一个具体状态类对应环境的一个具体状态，不同的具体状态类其行为有所不同。
---

### ES6实现
``` js
class Context {
    constructor(state) {
        console.log("Context Class created");
        switch (state) {
            case "A":
                this.state = new ConcreteStateA()
                break
            case "B":
                this.state = new ConcreteStateB()
                break
            default:
                this.state = new ConcreteStateA()
        }
    }

    request() {
        console.log('Context.request invoked');
        this.state.handle(this);
    }
}

class State {
    constructor() {
        console.log("State Class created");
    }

    handle() {
        console.log('State.handle invoked');
    }
}

class ConcreteStateA extends State {
    constructor() {
        super();
        console.log("ConcreteStateA Class created");
    }

    handle(context) {
        console.log('ConcreteStateA.handle invoked');
    }
}

class ConcreteStateB extends State {
    constructor() {
        super();
        console.log("ConcreteStateB Class created");
    }

    handle(context) {
        console.log('ConcreteStateB.handle invoked');
    }
}

let context = new Context("A")
context.request()

```
---

### Typescript实现
``` ts
namespace StatePattern {
  export interface State {
      handle(context: Context): void;
  }

  export class ConcreteStateA implements State {
      public handle(context: Context): void {
          console.log("`handle` method of ConcreteStateA is being called!");
          context.State = new ConcreteStateB();
      }
  }

  export class ConcreteStateB implements State {
      public handle(context: Context): void {
          console.log("`handle` method of ConcreteStateB is being called!");
          context.State = new ConcreteStateA();
      }
  }

  export class Context {
      private state: State;

      constructor(state: State) {
          this.state = state;
      }

      get State(): State {
          return this.state;
      }

      set State(state: State) {
          this.state = state;
      }

      public request(): void {
          console.log("request is being called!");
          this.state.handle(this);
      }
    }
}

```
---

## 参考以下内容:
 - [图说设计模式](https://design-patterns.readthedocs.io/zh_CN/latest/)
 - [ECMAScript2016-Design-Patterns](https://github.com/ryouaki/ECMAScript2016-Design-Patterns)
 - [Design Patterns in TypeScript](https://github.com/torokmark/design_patterns_in_typescript)
