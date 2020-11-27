# 顺序查找

顾名思义，顺序查找就逐个遍历元素，找到匹配的解。有时也是一种暴力求解的方式，不过这种方式效率比较低，时间复杂度比较高。

比如，查找数组中的匹配项时，循环遍历数组，直到找到匹配项为止。这种查找效率要看查找的次数，最快查找一次就可以找到，也就是**O\(1\)，**最差就是**O\(n\)**

```javascript
function orderSearch(arr, target) {
  for(let i = 0; i < arr.length; i++) {
    if(arr[i] == target) return true;
  }
  return false;
}
```





LeetCode中也不乏也有类似的题目：

1. [剑指 Offer 53 - I. 在排序数组中查找数字 I](https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/)

