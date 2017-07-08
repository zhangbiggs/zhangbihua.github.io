---
title: 迭代器(Iterator)-设计模式
date: 2017-05-05 23:58:44
categories: [design-pattern, Behavioral Patterns]
tags: [javasctipt, 迭代器]
---
> 软件模式是将模式的一般概念应用于软件开发领域，即软件开发的 总体指导思路或参照样板。
> 软件模式并非仅限于设计模式，还包括 架构模式、分析模式和过程模式等，
> 实际上，在软件生存期的每一个阶段都存在着一些被认同的模式。

---

### ES6实现
``` js
class Iterator {
    constructor() {
        console.log('Iterator Class created');
    }

    first() {
        console.log('Iterator.first invoked');
    }

    next() {
        console.log('Iterator.next invoked');
    }

    isDone() {
        console.log('Iterator.isDone invoked');
    }

    currentItem() {
        console.log('Iterator.currentItem invoked');
    }
}

class ConcreteIterator extends Iterator {
    constructor(aggregate) {
        super();
        this.index = 0;
        this.aggregate = aggregate;
        console.log('ConcreteIterator Class created');
    }

    first() {
        console.log('ConcreteIterator.first invoked');
        return this.aggregate.list[0];
    }

    next() {
        console.log('ConcreteIterator.next invoked');
        this.index += 1;
        return this.aggregate.list[this.index];
    }

    currentItem() {
        console.log('ConcreteIterator.currentItem invoked');
        return this.aggregate.list[this.index];
    }
}

class Aggregate {
    constructor() {
        console.log('Aggregate Class created');
    }

    createIterator() {
        console.log('Aggregate.CreateIterator invoked');
    }
}

class ConcreteAggregate extends Aggregate {
    constructor(list) {
        super();
        this.list = list;
        console.log('ConcreteAggregate Class created');
    }

    createIterator() {
        console.log('ConcreteAggregate.CreateIterator invoked');
        this.iterator = new ConcreteIterator(this);
    }
}

var aggregate = new ConcreteAggregate([0, 1, 2, 3, 4, 5, 6, 7]);
aggregate.createIterator();
console.log(aggregate.iterator.first());
console.log(aggregate.iterator.next());
console.log(aggregate.iterator.currentItem());


```
---

### Typescript实现
``` ts
namespace IteratorPattern {
	export namespace Demo {

		export function show() : void {
		    var nArray = [1, 7, 21, 657, 3, 2, 765, 13, 65],
				numbers: IteratorPattern.Numbers = new IteratorPattern.Numbers(nArray),
				it: IteratorPattern.ConcreteIterator = <IteratorPattern.ConcreteIterator>numbers.createIterator();

			while (it.hasNext()) {
				console.log(it.next());
			}

		}
	}
}
```
---

## 参考以下内容:
 - [图说设计模式](https://design-patterns.readthedocs.io/zh_CN/latest/)
 - [ECMAScript2016-Design-Patterns](https://github.com/ryouaki/ECMAScript2016-Design-Patterns)
 - [Design Patterns in TypeScript](https://github.com/torokmark/design_patterns_in_typescript)
