# 中等数组算法







### 旋转数组

测试用例以及[LeetCode](https://leetcode-cn.com/problems/rotate-array/)

要求：在原数组上操作，也就是空间复杂度为O\(1\)

```javascript
let nums = [1,2,3,4,5,6,7], k = 3;
// 输入: [1,2,3,4,5,6,7] 和 k = 3
// 输出: [5,6,7,1,2,3,4]
// 解释:
// 向右旋转 1 步: [7,1,2,3,4,5,6]
// 向右旋转 2 步: [6,7,1,2,3,4,5]
// 向右旋转 3 步: [5,6,7,1,2,3,4]
rotate(nums); // [5,6,7,1,2,3,4]
```

**暴力循环法**

* 时间复杂度 O\(n_k\) = n_  k
* 空间复杂度 O\(1\) = 1

```javascript
// 旋转数组 
// 关键 元素互换
var rotate = function(nums, k) {
  let len = nums.length;
  let p = 0, q = 0;

  while(p < k) {
    let pre = nums[len - 1];
    q = 0;
    while(q  < len) {
      let tmp = nums[q];
      nums[q] = pre;
      pre = tmp;
      q++
    }
    p++
  }
};
```

**旋转法 - 循环一次**

* 时间复杂度 O\(n\) = n 
* 空间复杂度 O\(1\) = 1

```javascript
// 整体旋转: [7,6,5,4,3,2,1]
// 前 k - 1 旋转: [5,6,7,4,3,2,1]
// k 到 nums.length 旋转: [5,6,7,1,2,3,4]
var rotates = function(nums, k) {
  k %= nums.length
  reverse(nums, 0, nums.length - 1);
  reverse(nums, 0, k - 1);
  reverse(nums, k, nums.length - 1)
}
// 数组旋转
function reverse(nums, start, end) {
  while(start < end) {
    let tmp = nums[start];
    nums[start] = nums[end];
    nums[end] = tmp;
    start++;
    end--;
  }
}
```

### 数组中K-diff数对

测试用例以及[LeetCode](https://leetcode-cn.com/problems/k-diff-pairs-in-an-array/)

```javascript
// 输入: [3, 1, 4, 1, 5], k = 2
// 输出: 2
// 解释: 数组中有两个 2-diff 数对, (1, 3) 和 (3, 5)。
// 尽管数组中有两个1，但我们只应返回不同的数对的数量。
```

**哈希/散列**

* 时间复杂度 O\(n\) = n
* 空间复杂度 O\(n\) = n

```javascript
var findPairs = function(nums, k) {
  let p = 0, len = nums.length, map = new Set(), set = new Set();

  if(k < 0) {
    return 0
  }

  while(p < len) {
    if(set.has(nums[p] - k)) map.add(nums[p] - k);
    if(set.has(nums[p] + k)) map.add(nums[p]);
    set.add(nums[p++])
  }
  return map.size
};
```

### **最短无序连续子数组**

\*\*\*\*

\*\*\*\*

