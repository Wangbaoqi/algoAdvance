---
description: 栈和队列是常用的数据结构，分别有先进后出和先进先出的特性
---

# 栈/队列结构及算法

### 栈的实现

![&#x5148;&#x8FDB;&#x540E;&#x51FA;&#x7684;&#x6570;&#x636E;&#x7ED3;&#x6784;](../.gitbook/assets/stack_0.gif)

分别用ES5和ES6实现

#### ES5 实现栈

```javascript
// push methods
function append(e) {
  this.stackList.push(e)
}

// pop methods
function pop() {
  this.stackList.pop()
}

// peek methods
function peek() {
  return this.stackList[this.size() - 1]
}

// isEmpty methods
function isEmpty() {
  return !this.stackList.length
}

// clear methods
function clear() {
  this.stackList = []
}

// size methods
function size() {
  return this.stackList.length;
}

// Stack structure
function Stack() {
  this.stackList = [];
  this.append = append;
  this.pop = pop;
  this.peek = peek;
  this.isEmpty = isEmpty;
  this.clear = clear;
  this.size = size;
}

let stack = new Stack()

stack.append('1')
stack.append('2')
stack.append('2')

```

#### ES6 实现栈

```javascript
class Stack {
  constructor() {
    this.stackList = [];
  }
  append(e) {
    this.stackList.push(e);
  }
  pop() {
    this.stackList.pop();
  }
  peek() {
    return this.stackList[this.size() - 1];
  }
  isEmpty() {
    return !this.stackList.length;
  }
  clear() {
    this.stackList = [];
  }
  size() {
    return this.stackList.length;
  }
}
let stack = new Stack()

stack.append('1')
stack.append('2')
stack.append('2')
```

### 单调栈的实现

### 

### 

### 队列的实现

#### ES5 实现队列

```javascript
// 入队操作
function enqueue(ele) {
  this.dataSource.push(ele)
}
// 出队操作
function dequeue() {
  this.dataSource.shift()
}
// 返回队列第一个元素
function front() {
  return this.dataSource[0]
}
// 队列是否为空
function isEmpty() {
  return !!this.dataSource.length
}
// 队列的大小
function size() {
  return this.dataSource.length
}
// 队列结构
function Queue() {
  // 队列数据源
  this.dataSource = []
  // 入队操作
  this.enqueue = enqueue
  // 出队操作
  this.dequeue = dequeue
  // 返回队列中的第一个元素
  this.front = front
  // 队列是否为空
  this.isEmpty = isEmpty
  // 队列的大小
  this.size = size
}
let queue = new Queue()
queue.enqueue('baoqi')
queue.enqueue('wang')
console.log(queue.isEmpty()) // false
console.log(queue.size()) // 2
console.log(queue.front()) // baoqi
console.log(queue.dataSource) // ['baoqi', 'wang']
queue.dequeue()
console.log(queue.dataSource) // ['wang']
```

#### ES6 实现队列

```javascript
class Queue {
  constructor() {
    this.dataSource = []
  }
  enqueue(ele) {
    this.dataSource.push(ele)
  }
  dequeue() {
    this.dataSource.shift()
  }
  front() {
    return this.dataSource[0]
  }
  isEmpty() {
    return !this.dataSource.length
  }
  size() {
    return this.dataSource.length
  }
}


// 测试跟上述结果一致
```

### 优先队列

顾名思义，优先队列不会遵循常规队列的规则，FIFO（先进先出），而是根据不同的优先级来出队列的。

```javascript
// ele 元素
// code 优先级
function QueueElement(ele, code) {
  this.ele = ele
  this.code = code
}
// 优先队列
class PriorityQueue {
  constructor() {
    this.dataSource = []
  }
  enqueue(val) {
    const current = this.dataSource;
    let add = false
    for(let i = 0; i < current.length; i++) {
      if(val.code < current[i].code) {
        this.dataSource.splice(i, 0, val)
        add = true
        break
      }
    }
    if(!add) {
      this.dataSource.push(val)
    }
  }
  dequeue() {
    return this.dataSource[0]
  }
  front() {
    return this.dataSource[0]
  }
  isEmpty() {
    return !this.dataSource.length
  }
  size() {
    return this.dataSource.length
  }
  print() {
    const el = this.dataSource
    for(let i = 0; i < el.length; i++) {
      console.log(`name:${el.name}, code${el.code};`)
    }
  }
}
let pqueue = new PriorityQueue()
pqueue.enqueue(new QueueElement('baoqi', 4))
pqueue.enqueue(new QueueElement('wang', 3))
pqueue.enqueue(new QueueElement('nate', 5))
pqueue.enqueue(new QueueElement('natebaoqi', 2))
pqueue.enqueue(new QueueElement('john', 2))
```

### 单调队列

单调队列是一种特殊的队列，也是一种**双端队列**。**单调**的含义是单调递增\(递减\)；就是队列中每新增一个新元素，就要把之前比新元素小的\(或大的\)都要删掉，队列中的队首就会是最大或者最小的元素。

实现单调队列算法

```javascript
class MonotonicQueue {
  constructor() {
    this.queue = []
  }

  empty() {
    return !this.queue.length
  }

  size() {
    return this.queue.length || 0
  }

  // 返回队尾的元素
  back() {
    return this.size() && this.queue[this.size() - 1]
  }
  // 删除队尾元素
  popBack() {
    this.queue.pop()
  }
  // 队尾新增元素
  pushBack(n) {
    this,queue.push(n)
  }

  // 元素入队
  push(n) {
    while(!this.empty() && this.back() < n) {
      this.popBack()
    }
    this.pushBack(n)
  }
  // 返回队首的元素
  front() {
    return this.queue[0]
  }
  // 删除队首元素
  popFront() {
    this.queue.shift()
  }
  // 队首新增元素
  pushFront(n) {
    this.queue.unshift(n)
  }

  // 返回最大元素
  max() {
    return this.front()
  }
  // 删除元素
  pop(n) {
    if(!this.size() && this.front() === n) {
      this.popFront()
    }
  }
}
```

单调队列的应用可以计算某个区域之间的最值，同时时间复杂度为O\(1\)

### 栈 队列的应用

栈和队列的应用请看**常用算法**中的[栈、队列系列](https://wangbaoqi.gitbook.io/algo/chang-yong-suan-fa-xi-lie/zhan-he-dui-lie)



