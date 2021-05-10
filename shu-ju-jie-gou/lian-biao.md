# 链表

链表是一种线性存储结构，元素未存储的在连续的内存块中，而是通过类似指针的方式连接每一个节点

![](../.gitbook/assets/linkedlist.png)

### 单链表

单链表是链表一种表现形式，是单向的，最后的节点执行 `null` ，除非出现环形链表。

### 单链表的实现

```javascript
// 链表
// 伪代码 结构
var linkList = {
  data: 'head',
  next: {
    data: 'first',
    next: {
      data: 'second',
      next: null
    }
  }
}a

// 结点 - 内存块
function Node(element) {
  this.data = element;
  this.next = null;
}

function LinkList() {
  // 头结点
  this.head = new Node('head');

}

// 链表尾结点新增结点
LinkList.prototype.append = function (newElement) {
  var newNode = new Node(newElement);
  var curNode = this.head;
  while (curNode.next) {
    curNode = curNode.next;
  }
  curNode.next = newNode;
}


// 通过结点值查找结点
LinkList.prototype.findByValue = function (value) {
  // 遍历 从头结点

  var curNode = this.head.next;
  while (curNode != null && curNode.data != value) {
    curNode = curNode.next;
  }
  return curNode === null ? -1 : curNode;
}

// 通过index查找结点
LinkList.prototype.findByid = function (index) {
  var curNode = this.head.next;
  var pos = 0;
  while (curNode !== null && pos !== index) {
    curNode = curNode.next;
    pos++;
  }
  return curNode === null ? -1 : curNode;
}

// 查找指定结点的上一个结点
LinkList.prototype.findPrev = function (value) {
  var curNode = this.head;

  while (curNode.next !== null && curNode.next.data !== value) {
    curNode = curNode.next;
  }
  if (curNode.next === null) {
    return -1;
  }
  return curNode;
}

// 移除指定结点
LinkList.prototype.remove = function (value) {
  var prevNode = this.findPrev(value);
  if (prevNode == -1) {
    console.log('未找到结点');
    return
  }
  prevNode.next = prevNode.next.next;
}

// 遍历链表结构
LinkList.prototype.diaplay = function () {
  var curNode = this.head.next;
  while (curNode !== null) {
    console.log(curNode.data);
    curNode = curNode.next
  }
}


// 指定结点之后插入结点
LinkList.prototype.insert = function (newElement, element) {
  var curNode = this.findByValue(element);

  if (curNode == -1) {
    console.log('未找到结点');
    return;
  }
  var newNode = new Node(newElement);
  newNode.next = curNode.next;
  curNode.next = newNode;
}


var LLinkList = new LinkList();

console.log(LLinkList)

LLinkList.append('first');
LLinkList.append('second');
LLinkList.append('third');
LLinkList.append('five');


LLinkList.insert('four', 'third');
var findValue = LLinkList.findByValue('second');
console.log(findValue)

var findPrev = LLinkList.findPrev('four');
console.log(findPrev)

LLinkList.remove('first')

LLinkList.diaplay()
```

### 链表中双指针技巧

[双指针技巧](../suan-fa-ji-qiao-xi-lie/shuang-zhi-zhen-mo-shi.md)在链表的使用只能采用**快慢指针**的模式。这里收集了利用**双指针**解题的算法题。

* 环形链表
* 环形链表II
* 相交链表
* 删除链表的倒数第N个节点

### 环形链表

环形链表可以判断一个单链表是否有环



### 反转链表 



```javascript
const reverseList = function(head) {
  let dummy = new LinkNode('head')
  dummy.next = head;

  let forwardNode = dummy.next;
  let curNode = forwardNode.next;

  while(curNode != null) {
    forwardNode.next = curNode.next;
    curNode.next = dummy.next;
    dummy.next = curNode;

    curNode = forwardNode.next;
  }
  return dummy.next
}
```

### 移除链表元素



```javascript
const removeElements = function(head, val) {

  let dummy = new LinkNode('head')
  dummy.next = head;

  let prevNode = dummy;
  let curNode = head;

  while(curNode) {
    if(curNode.val == val) {
      prevNode.next = curNode.next;
    }else {
      prevNode = curNode
    }
    curNode = curNode.next;
  }

  return dummy.next
};
```







### 链表经典问题



* 反转链表
* 移除链表元素
* 奇偶链表
* 回文链表
* 复制带随机指针的链表



### 链表在线测试

链表算法测试可以到 `src/algo/linkNode`  

{% embed url="https://codesandbox.io/s/musing-torvalds-tuvbu" %}







