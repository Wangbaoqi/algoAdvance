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
    }else {
      res.push(item)
    }
  })
  return res;
}
```

### 有效的括号



### 数组转树形结构



