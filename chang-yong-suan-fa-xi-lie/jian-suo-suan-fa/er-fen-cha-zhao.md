# 二分查找

### 二分查找概述

二分查找也是一种查找效率比较高的方法，查找的场景是数据结构已排序好的。这种方式查找比线性查找耗时要更低一点。

二分查找的时间复杂度也要比顺序查找的低

```bash
// n 区间的大小
n n/2 n/4 n/8 ... n/2^k 
```

因此，二分查找的时间复杂度为 $$k = logn$$ **，**随着**n区间**越来越大，查找次数就越少

二分查找的实现框架 

### 模板一 基本框架 循环方式  

{% hint style="info" %}
在一个**有序的集合**中查找**特定**的字符，有则返回其索引，无则返回-1。
{% endhint %}

```javascript
/**
 * 二分查找
 * @param {*} arr 
 * @param {*} target 
 */
function binSearch(arr, target) {
  let low = 0, high = arr.length - 1;
  // low <= high 结束条件 low > high
  while(low <= high) {
    // mid = low + ((high - low) >> 1); 位运算 防止溢出
    let mid = Math.floor((low + high) / 2);
    if(target == arr[mid]) {
      return mid;
    }else if(target < arr[mid]) {
      high = mid - 1
    }else {
      low = mid + 1
    }
  }
  return -1;
}
```

递归方式，实现思路跟循环类似

```javascript
function binSearch1(arr, target) {
  return bSearch(arr, 0, arr.length - 1, target)
}

function bSearch(arr, low, high, target) {
  if(low > high) return -1;

  let mid = low + ((high - low) >> 1);

  if(arr[mid] == target) {
    return mid
  }else if(arr[mid] < target) {
    return bSearch(arr, mid + 1, high, target)
  }else {
    return bSearch(arr, low, mid - 1, target)
  }
}
```

模板一也是最基本的二分法查找方式，这里有几个重点，查找区间**\[low, high\] 闭区间 和 循环条件 while\(low &lt;= high\)以及 mid + 1 或者 mid - 1。**

1.  查找区间为闭区间，也就是**arr.length - 1**
2.  循环条件为 low &lt;= high，如果条件为 low &lt; high，其结束循环条件为low = high，那么如果当low =   mid + 1 或者 high = mid - 1的值等于 low == high，则就找不到正确的结果
3.  mid + 1 或者 mid - 1是将整个闭区间分为两个闭区间，也是跟循环条件有关系的

#### LeetCode 有关模板一的题目

* [35. 搜索插入位置](https://leetcode-cn.com/problems/search-insert-position/)
* [69. x 的平方根](https://leetcode-cn.com/problems/sqrtx/)
* [287. 寻找重复数](https://leetcode-cn.com/problems/find-the-duplicate-number/)
* [374. 猜数字大小](https://leetcode-cn.com/problems/guess-number-higher-or-lower/)

### **模板二 高级查询方法** 

{% hint style="info" %}
**寻找左右两侧边界的查询方法**
{% endhint %}

这种查找方式在找到目标值的时候并不会直接结束查找，而是缩小对应的区间，直到循环结束。要注意的一点是循环结束要进行打补丁，否则查找结果会超出区间范围。

#### 寻找左侧边界的二分法

先来看看模板

```javascript
function leftBinSearch(nums, target) {
  // 查找区间为左开右闭 [low, high)
  let low = 0, high = nums.length; // or high = nums.length - 1
  // 循环结束条件 low == high
  while(low < high) {
    let mid = low + ((high - low) >> 1)
    // 找到目标值 继续向左缩小查找区间
    if(nums[mid] == target) {
      high = mid
    }else if(nums[mid] < target) {
      low = mid + 1
    }else if(nums[mid] > target){
      high = mid
    }
  }
  // 打补丁 未找到目标值 low会超出查找区间
  if(nums[low] != target || low >= num.length) return -1
  // 找到左侧的目标值
  return low
}
```



#### 寻找右侧边界的二分法

```javascript
function rightBinSearch(num, target) {
  // 查找区间为左开右闭 [low, high)
  let low = 0, high = num.length
  // 循环结束条件 low == high
  while(low < high) {
     let mid = low + ((high - low) >> 1)
    if(num[mid] > target) {
      high = mid
    }else if(target > num[mid]) {
      low = mid + 1
    }else if(target == num[mid]) {
      // 找到目标值 继续向右缩小查找区间
      low = mid + 1
    }
  }
  // 打补丁
  if(high - 1 >= num.length || num[high-1] !== target) return -1;
  // 因为查询区间是右开 因此向左移一位
  return high - 1
}
```



#### LeetCode 有关查找左右侧边界的题目

* [278. 第一个错误的版本](https://leetcode-cn.com/problems/first-bad-version/)
* [153. 寻找旋转排序数组中的最小值](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/)
* [162. 寻找峰值](https://leetcode-cn.com/problems/find-peak-element/)
* [34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

