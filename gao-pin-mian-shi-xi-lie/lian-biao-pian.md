# 链表篇

### 热题HOT

1. [合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)
2. 环形链表
3. 相交链表
4. 反转链表
5. 回文链表
6. 合并二叉树

### 合并两个有序链表

```text
将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 
```

#### 合并链表之后进行排序

首先将两个链表合并成一个链表，然后对[链表进行插入排序](../shu-ju-jie-gou/lian-biao.md#lian-biao-jin-hang-cha-ru-pai-xu)

```javascript
const mergeTwoLists = (node1, node2) => {
  if(node1 == null && node2 == null) return null;
  if(node1 == null) return node2;
  if(node2 == null) return node1;
  
  while(node1 != null) {
    if(node1.next == null) {
      node1.next = node2
      break
    }
    node1 = node1.next
  }
  
  // 对合并链表进行插入排序
  const dummy = new ListNode('head');
  dummy.next = node1;
  
  let lastNode = node1;
  let curNode = node1.next;
  
  while(curNode != null) {
    if(lastNode.val < curNode.val) {
      lastNode = lastNode.next
    }else {
      let preNode = dummy;
      while(preNode.next.val <= curNode.val) {
        preNode = preNode.next
      }
      lastNode.next = curNode.next;
      curNode.next = preNode.next;
      preNode.next = curNode;
    }
    curNode = curNode.next
  }
  return dummy.next;
}
```

#### 递归的方式

```javascript
const mergeTwoLists = (node1, node2) => {
  if(node1 == null) {
    return node2
  }else if(node2 == null) {
    return node1
  }else if(node1.val < node2.val) {
    node1.next = mergeTwoLists(node1.next, node2)
    return node1
  }else {
    node2.next = mergeTwoLists(node1, node2.next)
    return node2
  }
}
```

#### 迭代的方式

迭代是将上面递归的方式通过迭代实现了

```javascript
const mergeTwoLists = (node1, node2) => {
  const dummy = new ListNode('head')
  dummy.head = null;
  
  const preNode = dummy;
  
  while(node1 != null && node2 != null) {
    if(node1.val < node2.val) {
      preNode.next = node1;
      node1 = node1.next;
    }else {
      preNode.next = node2;
      node2 = node2.next
    }
    preNode = preNode.next;
  }
  
  preNode.next = node1 == null ? node2 : node1;
  return dummy.next;
}
```



