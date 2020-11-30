# 二分查找

二分查找也是一种查找效率比较高的方法，查找的场景是数据结构已排序好的。这种方式查找比线性查找耗时要更低一点。

二分查找的实现框架

```javascript
/**
 * 二分查找
 * @param {*} arr 
 * @param {*} target 
 */
function binSearch(arr, target) {
  let low = 0, high = arr.length;
  while(low <= high) {
    let mid = Math.floor((low + high) / 2);
    if(target == arr[mid]) return mid;

    if(target < arr[mid]) {
      high = mid - 1
    }else {
      low = mid + 1
    }
  }
  return -1;
}
```



