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

### 求数组里面最大连续项的和

```javascript
输入一个整型数组，数组中的一个或连续多个整数组成一个子数组。求所有子数组的和的最大值。
```

这是个典型的动态规划问题，首先用暴力破解法解决

时间复杂度 $$O(n^2)$$ 

空间复杂度 $$O(1)$$ 

```javascript
const maxSubArray = function(arr) {
  let max = -Infinity;
  let len = arr.length;

  for (let i = 0; i < len; i++) {
    let sum = 0
    for (let j = i; j < len; j++) {
      sum += arr[j];
      max = Math.max(max, sum)
    }
  }
  return max
};
```

动态规划解法

时间复杂度 $$O(n)$$ 

空间复杂度 $$O(1)$$ 

```javascript
const maxSubArray = (arr) => {
  let res = arr[0];
  let sum = arr[0];
  let index = 1;
  
  while(index < arr.length) {
    const item = arr[index++]
    sum = sum > 0 ? sum + item : item; // sum = Math.max(sum+item, item)
    res = Math.max(res, sum)
  }
  return res
}
```

### 数组转树形结构



