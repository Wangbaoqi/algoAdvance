# 数组

### 将数组扁平化去重得到升序不重复数组

测试用例

```javascript
const arr = [[1, 2, 2], [3, 4, 5, 5], [6, 7, 8, 9, [11, 12, [14]]], 10];
```

**使用 flat、set、sort** flat：数组扁平化 set：数组去重 sort： 数组排序

```javascript
// 兼容性不太好
[...new Set(arr.flat(Infinity))].sort((a, b) => a - b);
// [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 14]
```

**使用 reduce、concat 代替 flat** reduce: 采用递归以及累加器 concat: 合并数组

```javascript
function flatDeep(arr) {
  return arr.reduce(
    (acc,
    (val) => {
      return acc.concat(Array.isArray(val) ? flatDeep(val) : val);
    }),
    []
  );
}
Array.from(new Set(flatDeep(arr))).sort((a, b) => a - b);
// [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 14]
```

**使用 toString、split 代替 flat** toString: 转化为字符串 split: 转化为数组

```javascript
[...new Set(arr.toString().split(","))].sort((a, b) => a - b);
// ["1", "2", "3", "4", "5", "6", "7", "8", "9", "10", "11", "12", "14"]
```

**使用迭代方式扁平化数组**

```javascript
function falttenDeep(arrs) {
  while (arrs.some((item) => Array.isArray(item))) {
    console.log(...arrs);
    arrs = [].concat(...arrs);
  }
  return arrs;
}
```

### 数组合并

\[A1, A2, B1, B2\]和\[A, B\]合并\[A1, A2, A, B1, B2, B\]

测试用例

```javascript
let arr1 = ["A1", "A2", "B1", "B2"];
let arr2 = ["A", "B"];
```

**使用 charCodeAt**

```javascript
[...arr1, ...arr2].sort((a, b) => a.charCodeAt() - b.charCodeAt());
// ["A1", "A2", "A", "B1", "B2", "B"]
```

**使用 sort**

```javascript
const arr3 = arr2.map((i) => i + 3);
[...arr1, ...arr3].sort().map((e) => {
  return e.includes("3") ? e.split("")[0] : e;
});
// ["A1", "A2", "A", "B1", "B2", "B"]
```

### 两数之和

[LeetCode \[1 - 两数之和\]](https://leetcode-cn.com/problems/two-sum/)

```javascript
// 测试用例
const nums = [2, 4, 8, 3];
const target = 5;
nums[0] + nums[3] = target;
return [0, 5];
```

**使用双重循环**

* 时间复杂度 O\(n\) = n^2
* 空间复杂度 O\(n\) = 1
* 执行时间: **188ms**
* 内存消耗: **34.9MB**

```javascript
var twoSum = function(nums, target) {
  for (let i = 0; i < nums.length; i++) {
    for (let j = i + 1; j < nums.length; j++) {
      if (nums[i] + num[j] === target) {
        return [i, j];
      }
    }
  }
};
```

**使用 HashMap ES6**

* 时间复杂度 O\(n\) = n
* 空间复杂度 O\(n\) = n hash 表存储的元素个数
* 执行时间: **72ms**
* 内存消耗: **35.3MB**

```javascript
// 利用差值以及Hash表
var twoSum = function(nums, target) {
  let hashMap = new Map();
  for (let i = 0; i < nums.length; i++) {
    const val = target - nums[i];
    if (hashMap.has(val)) {
      return [hashMap.get(val), i];
    }
    hashMap.set(num[i], i);
  }
};
```

### 两数之和 II

[算法描述](https://leetcode-cn.com/problems/two-sum-ii-input-array-is-sorted/)以及测试用例

```javascript
const nums = [2, 7, 11, 15];
const target = 9;
return [1, 2];
```

**双指针 嵌套循环**

* 时间复杂度 O\(n\) = n\*n
* 空间复杂度 O\(n\) = 1
* 执行时间: **344ms**
* 内存消耗: **34.7MB**

```javascript
var twoSum = function(numbers, target) {
  let p = 0;
  let q = 1;
  let len = numbers.length;

  while (p < len) {
    q = p + 1;
    while (q < len) {
      if (numbers[p] + numbers[q++] === target) return [p + 1, q];
    }
    p++;
  }
};

let numbers = [5, 25, 75],
  target = 100;
twoSumN(numbers, target);
```

**双指针 二分法**

* 时间复杂度 O\(n\) = n
* 空间复杂度 O\(n\) = 1
* 执行时间: **72ms**
* 内存消耗: **34.7MB**

```javascript
var twoSumN = function(numbers, target) {
  let len = numbers.length;
  let p = 0;
  let q = len - 1;
  while (p < q) {
    let sum = numbers[p] + numbers[q];
    if (sum === target) return [p + 1, q + 1];

    if (sum > target) {
      --q;
    } else {
      ++p;
    }
  }
};
```

### 判断字符是否唯一

测试用例

```javascript
let str = "nate";
isUnique(str); // true
let str = "nate wang";
isUnique(str); // false
```

**暴力法 双重循环**

* 时间复杂度 O\(n\) = n^2
* 空间复杂度 O\(n\) = 1
* 执行时间: **68ms**
* 内存消耗: **33.7MB**

```javascript
var isUnique = function(str) {
  for (let i = 0; i < str.length; i++) {
    for (let j = i + 1; j < str.length; j++) {
      if (str[i] == str[j]) return false;
    }
  }
  return true;
};
isUnique("nate"); // true
isUnique("nate wang"); // false
```

**数组暂存**

* 时间复杂度 O\(n\) = n
* 空间复杂度 O\(n\) = 1
* 执行时间: **64ms**
* 内存消耗: **33.8MB**

```javascript
var isUnique1 = function(str) {
  let arr = [];
  for (let i = 0; i < str.length; i++) {
    if (arr.indexOf(str[i]) > -1) return false;
    arr.push(str[i]);
  }
  return true;
};
isUnique1("nate"); // true
isUnique1("nate wang"); // false
```

**HashMap**

* 时间复杂度 O\(n\) = n
* 空间复杂度 O\(n\) = 1
* 执行时间: **60ms**
* 内存消耗: **33.8MB**

```javascript
var isUnique2 = function(str) {
  let hashMap = new Map();
  for (let i = 0; i < str.length; i++) {
    if (hashMap.has(str[i])) return false;
    hashMap.set(str[i]);
  }
  return true;
};
isUnique2("nate"); // true
isUnique2("nate wang"); // false
```

### 字符串是否互为重排

测试用例

```javascript
let s1 = "abc";
let s2 = "bcd";
let s3 = "bac";
CheckPermutation(s1, s2); // false
CheckPermutation(s1, s3); // true
```

**两者互相循环判断**

```javascript
var CheckPermutation = function(s1, s2) {
  return (
    [...s1].every((e) => {
      return s2.includes(e);
    }) &&
    [...s2].every((e) => {
      return s1.includes(e);
    })
  );
};
CheckPermutation("abc", "acb"); // true
CheckPermutation("abc", "acbd"); // false
CheckPermutation("abc", "acd"); // false
```

**合并循环判断**

```javascript
var CheckPermutation = function(s1, s2) {
  const allStr = [...s1, ...s2];
  return allStr.every((e) => {
    return s1.includes(e) && s2.includes(e);
  });
};
CheckPermutation("abc", "acb"); // true
CheckPermutation("abc", "acbd"); // false
CheckPermutation("abc", "acd"); // false
```

### 删除排序数组中的重复项

测试用例

```javascript
// 给定已经排好序数组
let arr = [0, 0, 1, 2, 3, 3];
// 返回长度[0,1,2,3] 4 原地修改原数组
removeDuplicates(arr);
```

**暴力破解法**

* 时间复杂度 O\(n\) = n^2
* 空间复杂度 O\(n\) = 1

```javascript
var removeDuplicates = function(nums) {
  if (!nums || !nums.length) return 0;

  for (let i = 0; i < nums.length; i++) {
    for (let j = i + 1; j < num.length; j++) {
      if (nums[i] == nums[j]) {
        nums.splice(j, 1);
        j--;
      }
    }
  }
  return nums.length;
};
```

**双指针方法**

* 时间复杂度 O\(n\) = n
* 空间复杂度 O\(n\) = 1

```javascript
var removeDuplicates = function(nums) {
  if (!nums || !nums.length) return 0;
  let i = 0;

  for (let j = 1; j < nums.length; j++) {
    // 元素不一致 指针移动
    if (nums[i] !== nums[j]) {
      nums[i + 1] = nums[j];
      i++;
    }
  }

  return i;
};
```

{% page-ref page="shu-zu.md" %}

