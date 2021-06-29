# 动态规划篇

1. [爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)
2. [求数组里面最大连续项的和](https://leetcode-cn.com/problems/maximum-subarray/)



### 爬楼梯

```javascript
假设你正在爬楼梯。需要 n 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

** 给定 n 是一个正整数。
```

这个问题用范式表示为

$$
f(n) = f(n-1) + f(n-2)
$$

接下来论证一下`f(0)`和`f(1)`

$$
f(0) = 1;
f(1) = 1
$$

再多论证几个数

$$
f(2) = f(1) + f(0)
$$

$$
f(3) = f(2) + f(1) = f(1) + f(0) + f(1)
$$

以此类推，可以判断出`n`的范围值，这里很容易想到用递归的方式来解决

```javascript
const climbStairs = (n) => {
  if(n < 0) return 0;
  if(n <= 1) return 1;
  return climbStairs(n - 1) + climbStairs(n - 2)
}
```

接下来验证下递归方式所消耗的时间

```javascript
climbStairs(1); // 0.015869140625 ms
climbStairs(10); // 0.025146484375 ms
climbStairs(20); // 1.801025390625 ms
climbStairs(30); // 22.7939453125 ms
climbStairs(40); // 2278.485107421875 ms
```

可以看到，使用递归时间复杂度随着量级增加会指数级增加。因此能否用遍历的方式解决这个问题？



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

### 

