# 双指针模式

常见的双指针模式有 **左右指针** 和 **快慢指针。**

### 双指针 - 左右指针

左右指针最典型的应用就是[**二分查找**](jian-suo-suan-fa/er-fen-cha-zhao.md#er-fen-cha-zhao-gai-shu)了，初次之外，[滑动窗口](hua-dong-chuang-kou-mo-shi.md)是左右指针的进阶版

![leetCode ](../.gitbook/assets/doublepoint-left.gif)

左右指针主要是在数组（字符串）的两端进行移动的，这样做的目的旨在节省时间，更高效的进行查找或者处理元素。

上面图是比较简单的左右指针（**反转字符串**）移动的动态图。接下来看下代码

```bash
编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 char[] 的形式给出。
不要给另外的数组分配额外的空间，你必须原地修改输入数组、使用 O(1) 的额外空间解决这一问题。
你可以假设数组中的所有字符都是 ASCII 码表中的可打印字符。

输入：["h","e","l","l","o"]
输出：["o","l","l","e","h"]
```

题目要求使用`O(1)`的时间复杂度，和**原地修改**，则就说明只能循环一次和不能用额外的空间。

```javascript
const reverseString = (arr) => {
  let left = 0, right = arr.length - 1;
  while(left <= right) {
    let tmp = arr[left]
    arr[left] = arr[right]
    arr[right] = tmp
    left++;
    right--;
  }
}
```

**两数之和 也可以使用双指针来解决**

```javascript
输入：numbers = [2,7,11,15], target = 9
输出：[1,2]
解释：2 与 7 之和等于目标数 9 。因此 index1 = 1, index2 = 2 。
```

解法可以使用暴力破解或者哈希解决，但是这两种在时间和空间复杂度都有一定的损耗，最优的可以使用左右双指针模式

```javascript
const numberSum = (arr, target) => {
  let left = 0, right = arr.length - 1;
  while(left <= right) {
    const curSum = arr[left] + arr[right]
    if(curSum == target) {
      return [left+1, right+1]
    }else if(curSum < target) {
      left++
    }else {
      right--
    }
  }
  return []
}
```

### **双指针 - 快慢指针**

