---
title: 命令模式(Command)-设计模式
date: 2017-05-05 23:58:44
categories: [design-pattern, Behavioral Patterns]
tags: [javasctipt, 命令模式]
---
> 软件模式是将模式的一般概念应用于软件开发领域，即软件开发的 总体指导思路或参照样板。
> 软件模式并非仅限于设计模式，还包括 架构模式、分析模式和过程模式等，
> 实际上，在软件生存期的每一个阶段都存在着一些被认同的模式。

### 命令模式的定义
- 命令模式：将一个请求封装为一个对象，从而使我们可用不同的请求对客户进行参数化；
- 对请求排队或者记录请求日志，以及支持可撤销的操作。
- 命令模式是一种对象行为型模式，其别名为动作模式或事务模式。

### 命令模式包含四个角色：
- Command-抽象命令类中声明了用于执行请求等方法，通过这些方法可以调用请求接收者的相关操作；
- ConcreteCommand-具体命令类是抽象命令类的子类，实现了在抽象命令类中声明的方法，它对应具体的接收者对象，将接收者对象的动作绑定其中；
- Invoker-调用者即请求的发送者，又称为请求者，它通过命令对象来执行请求；
- Receiver-接收者执行与请求相关的操作，它具体实现对请求的业务处理。

### ES6实现
``` js
class Invoker {
    constructor() {
        console.log('Invoker Class created');
    }

    storeCommand(command) {
        this.command = command;
        console.log('Invoker.storeCommand invoked');
    }
}

class Command {
    constructor() {
        console.log('Command Class created');
    }

    execute() {
        console.log('Command.execute invoked');
    }
}

class ConcreteCommand extends Command {
    constructor(receiver, state) {
        super();
        this.receiver = receiver;
        console.log('ConcreteCommand Class created');
    }

    execute() {
        console.log('ConcreteCommand.execute invoked');
        this.receiver.action();
    }
}

class Receiver {
    constructor() {
        console.log('Receiver Class created');
    }

    action() {
        console.log('Receiver.action invoked');
    }
}

var invoker = new Invoker();
var receiver = new Receiver();
var command = new ConcreteCommand(receiver);
invoker.storeCommand(command);
invoker.command.execute();

```
---

### Typescript实现
``` ts
namespace CommandPattern {
    export class Command {
        public execute(): void {
            throw new Error("Abstract method!");
        }
    }

    export class ConcreteCommand1 extends Command {
        private receiver: Receiver;

        constructor(receiver: Receiver) {
            super();
            this.receiver = receiver;
        }

        public execute(): void {
            console.log("`execute` method of ConcreteCommand1 is being called!");
            this.receiver.action();
        }
    }

    export class ConcreteCommand2 extends Command {
        private receiver: Receiver;

        constructor(receiver: Receiver) {
            super();
            this.receiver = receiver;
        }

        public execute(): void {
            console.log("`execute` method of ConcreteCommand2 is being called!");
            this.receiver.action();
        }
    }

    export class Invoker {
        private commands: Command[];

        constructor() {
            this.commands = [];
        }

        public storeAndExecute(cmd: Command) {
            this.commands.push(cmd);
            cmd.execute();
        }
    }

    export class Receiver {
        public action(): void {
            console.log("action is being called!");
        }
    }
}

(function main() {
    var receiver: CommandPattern.Receiver = new CommandPattern.Receiver(),
        command1: CommandPattern.Command  = new CommandPattern.ConcreteCommand1(receiver),
        command2: CommandPattern.Command  = new CommandPattern.ConcreteCommand2(receiver),
        invoker : CommandPattern.Invoker  = new CommandPattern.Invoker();

    invoker.storeAndExecute(command1);
    invoker.storeAndExecute(command2);

}());

```
---

## 参考以下内容:
 - [图说设计模式](https://design-patterns.readthedocs.io/zh_CN/latest/)
 - [ECMAScript2016-Design-Patterns](https://github.com/ryouaki/ECMAScript2016-Design-Patterns)
 - [Design Patterns in TypeScript](https://github.com/torokmark/design_patterns_in_typescript)
