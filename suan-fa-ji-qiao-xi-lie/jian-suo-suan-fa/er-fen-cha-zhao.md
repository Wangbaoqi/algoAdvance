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

#### 模板一 基本框架 循环方式  

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
2.  循环条件为 low &lt;= high，如果条件为 low &lt; high，其结束循环条件为low = high，那么如果当low = mid + 1 或者 high = mid - 1的值等于 low == high，则就找不到正确的结果
3.  mid + 1 或者 mid - 1是将整个闭区间分为两个闭区间，也是跟循环条件有关系的



