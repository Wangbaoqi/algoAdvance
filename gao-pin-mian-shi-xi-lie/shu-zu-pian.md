# 数组篇

### 数组高频题

1. [两数之和](https://leetcode-cn.com/problems/two-sum/)
2. [有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)

### 数组扁平化

给定一个多维数组，将其扁平化为以为数组

```javascript
const arr = [1,2,[3,[4,5]6,7],8]
// es6 兼容性不好
Array.prototype.flat.call(arr)

// map 递归模式
function flat(arr) {
  let res = [];
  arr.map(item => {
    if(Array.isArray(item)) {
      Array.prototype.push.apply(res, flat(item))
      res = res.concat(flat(item))
    }else {
      res.push(item)
    }
  })
  return res;
}
```

### 有效的括号

```javascript
给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串 s ，判断字符串是否有效。
```

因为需要匹配对应括号的类型，因此需要一个键值对来保存，还需要一个额外的数据结构来保存字符，如果栈顶的元素和当前的元素不匹配的话，则整个字符串不匹配，否则栈顶的元素出栈，继续下一轮。

```javascript
const isValid = (str) => {
  // 这里的键值对没有按顺序，这样方便栈顶的元素和当前元素对比
  let map = {')': '(', ']': '[', '}': '{'};
  let stack = [];
  let index = 0;
  let len = str.length;
  
  while(index < len) {
    const key = str[index++];
    // 
    if(map[key]) {
      if(stack[stack.length - 1] != map[key]) return false;
      stack.pop()
    }else {
      stack.push(key)
    }
  }
}
```



### 数组转树形结构



