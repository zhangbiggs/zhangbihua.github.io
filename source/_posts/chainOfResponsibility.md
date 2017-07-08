---
title: 责任链模式(Chain of Responsibility)-设计模式
date: 2017-05-05 23:58:44
categories: [design-pattern, Behavioral Patterns]
tags: [javasctipt, 责任链模式]
---
> 软件模式是将模式的一般概念应用于软件开发领域，即软件开发的 总体指导思路或参照样板。
> 软件模式并非仅限于设计模式，还包括 架构模式、分析模式和过程模式等，
> 实际上，在软件生存期的每一个阶段都存在着一些被认同的模式。



### ES6实现
``` js
'use strict';
class Handler {
    constructor() {
        console.log('Handler Class created');
    }

    handleRequest() {
        console.log('Handler.handleRequest invoked');
    }
}

class ConcreteHandler1 extends Handler {
    constructor() {
        super();
        console.log('ConcreteHandler1 Class created');
    }

    setSuccessor(successor) {
        this.successor = successor;
        console.log('ConcreteHandler1.setSuccessor invoked');
    }

    handleRequest(request) {
        console.log('ConcreteHandler1.handleRequest invoked');
        if (request === 'run')
            console.log('ConcreteHandler1 has handled the request');
        else {
            console.log('ConcreteHandler1 calls his successor');
            this.successor.handleRequest(request);
        }
    }
}

class ConcreteHandler2 extends Handler {
    constructor() {
        super();
        console.log('ConcreteHandler2 Class created');
    }

    handleRequest(request) {
        console.log('ConcreteHandler2.handleRequest invoked');
    }
}

let handle1 = new ConcreteHandler1();
let handle2 = new ConcreteHandler2();
handle1.setSuccessor(handle2);
handle1.handleRequest('run');
handle1.handleRequest('stay');


```
---

### Typescript实现
``` ts
namespace ChainOfResponsibilityPattern {

    export class Handler {
        private handler: Handler;
        private req: number;

        constructor(req: number) {
            this.req = req;
        }

        public setHandler(handler: Handler): void {
            this.handler = handler;
        }

        public operation(msg: string, req: number): void {
            if (req <= this.req) {
                this.handlerRequest(msg)
            } else if (this.handler !== null && this.handler !== undefined) {
                this.handler.operation(msg, req);
            }
        }

        public handlerRequest(msg: string): void {
            throw new Error("Abstract method!");
        }
    }

    export class ConcreteHandler1 extends Handler {
        constructor(req: number) {
            super(req);
        }
        public handlerRequest(msg: string) {
            console.log("Message (ConcreteHandler1) :: ", msg);
        }
    }


    export class ConcreteHandler2 extends Handler {
        constructor(req: number) {
            super(req);
        }
        public handlerRequest(msg: string) {
            console.log("Message :: (ConcreteHandler2) ", msg);
        }
    }

    export class ConcreteHandler3 extends Handler {
        constructor(req: number) {
            super(req);
        }
        public handlerRequest(msg: string) {
            console.log("Message :: (ConcreteHandler3) ", msg);
        }
    }
}

```
---

## 参考以下内容:
 - [图说设计模式](https://design-patterns.readthedocs.io/zh_CN/latest/)
 - [ECMAScript2016-Design-Patterns](https://github.com/ryouaki/ECMAScript2016-Design-Patterns)
 - [Design Patterns in TypeScript](https://github.com/torokmark/design_patterns_in_typescript)
