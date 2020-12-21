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

#### 基本框架 循环方式  

{% hint style="info" %}
在一个**有序的集合**中查找**特定**的字符，有则返回，无则返回-1。
{% endhint %}

```javascript
/**
 * 二分查找
 * @param {*} arr 
 * @param {*} target 
 */
function binSearch(arr, target) {
  let low = 0, high = arr.length;
  // low <= high 防止死循环
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

### 

### 二分查找拓展



