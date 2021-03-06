---
description: 采用双指针滑动窗口高级技巧可以解决一些很复杂的算法问题
---

# 滑动窗口模式

采用双指针滑动窗口高级技巧可以解决一些很复杂的算法问题，可以用解决数组/字符串的子元素问题，将嵌套的循环问题转换为单循环的问题，类似于[最小覆盖字符串](https://leetcode-cn.com/problems/minimum-window-substring/)等。

这里通过一个简单的例子，熟悉一下什么是滑动窗口模式。

**示例**：给定一个整数数组，计算长度为**k**的连续子数组的最大总和?

题意很好理解，找出连续**K**个子数组，其值是最大值。

**先来看下暴力解法**

暴力解法就是采用双重循环，将每个连续 K 个数组的和比较，记录其中最大值。

```javascript
var arr = [100, 200, 150, 300, 250, 350],
  k = 3;

function maxArr(arr) {
  let len = arr.length - k + 1;
  let maxNum = Number.MAX_SAFE_INTEGER;
  let maxCount = 0;

  for (let i = 0; i < len; i++) {
    let curNum = 0;
    for (let j = 0; j < k; j++) {
      curNum += arr[i + j];
    }
    maxCount = Math.max(maxCount, curNum);
  }

  return maxCount;
}
```

暴力求解的时间复杂度`O(len*k)`

**滑动窗口解法**

滑动窗口将多重循环可以简化为单次循环，这里可以将连续的 K 个子数组当做一个窗口，首先获取第一个窗口的和值，下一个窗口的和值为上一个窗口的和值加当前值减掉`arr[n-k]`，对比前后窗口的值就可以了

```javascript
function maxSliderArr(arr, k) {
  let len = arr.length;
  if (len < k) {
    return -1;
  }

  let maxNum = 0;
  // 获取第一个窗口的和值
  for (let i = 0; i < k; i++) {
    maxNum += arr[i];
  }

  let curMax = maxNum;

  for (let j = n; j < len; j++) {
    curMax += arr[j] - arr[j - k];
    maxNum = Math.max(maxNum, curMax);
  }

  return maxNum;
}
```

接下来看下滑动窗口的算法思想:

### 算法思想

1. 在字符串或者数组中使用双指针（左右指针），_left=0_, _right=0_，初始化为 0，将左右指针的闭合区间称之为**窗口**
2. 开始遍历的时候，先不断增加**right**，左指针**left**此时仍为 0，知道当前窗口满足要寻找的所有字符
3. 此时停止增加**right**，转而增加**left**，不断的缩小窗口，知道窗口不满足要寻找的字符，每增加一次**left**，都要更新一轮结果
4. 重复 2，3 步骤

下图借用[leetCode](https://leetcode-cn.com/problems/minimum-window-substring/)的动图

![](../.gitbook/assets/slidewindow.gif)

根据这个动图，可以简单的概括滑动窗口的伪代码；

```javascript
function slideWindowCode(str, tar) {
  let left = 0,
    right = 0;
  let len = str.length;
  let map_window,
    res = str;

  while (right < len) {
    // 进入窗口
    map_window.add(str[right]);
    right++;
    // 整个tar都在窗口中时
    while (tar in map_window) {
      // 获取最小窗口
      res = minLen(res, map_window);
      map_window.remove(str[left]);
      left++;
    }
  }
  return res;
}
```

下面就**LeetCode**中有关所有的滑动窗口算法进行分析以及归纳

### 最小覆盖子串

[最小覆盖子串 - LeetCode](https://leetcode-cn.com/problems/minimum-window-substring/) 是属于**hard level**，不过使用滑动窗口技巧就不会那么困难了。

算法描述:

```text
# 给你一个字符串 S、一个字符串 T 。请你设计一种算法，可以在 O(n) 的时间复杂度内，从字符串 S 里面找出：包含 T 所有字符的最小子串。

# 示例

输入：S = "ADOBECODEBANC", T = "ABC"
输出："BANC"

# 解析

套用滑动窗口的框架 code，不难发现几个条件点

1. 字符何时进入窗口
   在 right 指针移动时，若当前字符在目标字符中，则该字符进入到窗口，记录窗口中的字符和目标字符中每个字符出现的次数。
2. 如何判断目标字符全部在窗口中
   如果窗口中的每个字符次数和目标窗口的对应字符的次数一致时，则 right 指针停止，计算当前窗口大小以及开始移动 left 指针
3. 如何获取最小窗口
   每移动一次 left 指针，就要更新一次窗口大小，如果当前 left 指针指向的字符在目标字符中，则当前窗口需要移除该字符
4. 何时 right 指针会移动  
   在 left 指针移动的过程中，窗口中字符出现的次数和目标字符次数不一致的时候。
```

下面看下完整的 code:

```javascript
function slideWindow(str, tar) {
  let left = 0,
    right = 0,
    len = str.length,
    matchs = 0;

  let minLen = Number.MAX_SAFE_INTEGER,
    start = 0;

  // 窗口字符 和 目标字符
  let map_need = {},
    map_window = {};

  // 将目标字符转化为字典模式 { 'a': 1, 'b': 1, 'c': 1 }
  for (let k of tar) {
    if (map_need[k]) {
      map_need[k]++;
    } else {
      map_need[k] = 1;
    }
  }

  while (right < len) {
    let charR = str[right];
    if (map_need[charR]) {
      map_window[charR] = (map_window[charR] || 0) + 1;
      if (map_window[charR] === map_need[charR]) {
        matchs++;
      }
    }
    right++;

    // 当前窗口的字符满足目标字符，则开始移动左指针
    while (matchs === Object.keys(map_need).length) {
      const charL = str[left];
      // 更新窗口大小
      if (right - left < minLen) {
        start = left;
        minLen = right - left;
      }

      // 移除窗口中的字符
      if (map_need[charL]) {
        map_window[charL]--;

        if (map_window[charL] < map_need[charL]) {
          matchs--;
        }
      }
      left++;
    }
  }
  return minLen === Number.MAX_SAFE_INTEGER ? '' : str.substr(start, minLen) 
}
```

### 找到字符串中所有字母异位词

[找到字符串中所有字母异位词 - LeetCode](https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/) 是属于**hard level** 算法描述:

```text
# 给定一个字符串  s  和一个非空字符串  p，找到  s  中所有是  p  的字母异位词的子串，返回这些子串的起始索引。

# 字符串只包含小写英文字母，并且字符串  s  和 p  的长度都不超过 20100。

# 示例

输入：s: "cbaebabacd" p: "abc"
输出："[0, 6]"

# 解析

这个跟最小覆盖子串比较类似，其是找到符合最短的满足目标字符的字符长度。
而这个找到字符串中所有字母异位词是寻找完全符合目标字符长度
```

下面看下完整的 code:

```javascript
var findAnagrams = function(s, p) {
  let left = 0,
    right = 0;
  (len = s.length), (matchs = 0);
  let mp_need = {};
  let mp_window = {};
  let res = [];

  for (const k of p) {
    if (mp_need[k]) {
      mp_need[k]++;
    } else {
      mp_need[k] = 1;
    }
  }

  while (right < len) {
    let charR = s[right];

    if (mp_need[charR]) {
      mp_window[charR] = (mp_window[charR] || 0) + 1;
      if (mp_need[charR] === mp_window[charR]) {
        matchs++;
      }
    }
    right++;

    while (matchs === Object.keys(mp_need).length) {
      let charL = s[left];
      if (right - left === Object.keys(mp_need).length) {
        res.push(left);
      }
      if (mp_need[charL]) {
        mp_window[charL]--;

        if (mp_window[charL] < mp_need[charL]) {
          matchs--;
        }
      }
      left++;
    }
  }
  return res;
};
```

### 无重复字符的最长子串

[无重复字符的最长子串 - LeetCode](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/) 是属于**middle level** 算法描述:

```text
# 给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

# 字符串只包含小写英文字母，并且字符串  s  和 p  的长度都不超过 20100。

# 示例

输入：s: "abcabcbb"
输出：3
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

# 解析
```

下面看下完整的 code:

```javascript
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLongestSubstring = function(s) {
  let left = 0,
    right = 0,
    res = 0,
    len = s.length;
  let map_window = {};

  while (right < len) {
    let charR = s[right];
    map_window[charR] ? map_window[charR]++ : (map_window[charR] = 1);
    right++;

    while (map_window[charR] > 1) {
      let charL = s[left];
      map_window[charL]--;
      left++;
    }

    res = Math.max(res, right - left);
  }
  return res;
};
```

### 最大连续1的个数 III

[最大连续1的个数 III - LeetCode](https://leetcode-cn.com/problems/max-consecutive-ones-iii/) 是属于**middle level** 算法描述:

```text
# 给定一个由若干 0 和 1 组成的数组 A，我们最多可以将 K 个值从 0 变成 1 。

# 返回仅包含 1 的最长（连续）子数组的长度。

# 示例

输入：A = [1,1,1,0,0,0,1,1,1,1,0], K = 2
输出：6
解释: [1,1,1,0,0,1,1,1,1,1,1]，粗体数字从 0 翻转到 1，最长的子数组长度为 6。

# 解析

这个算法解法跟**无重复字符的最长子串**类似，窗口每滑动一次，都要更新一遍窗口的大小（窗口没有收缩），在窗口每缩小之后，也要更新一遍窗口的大小
```

完整解题算法

```javascript
var longestOnes = function(A, K) {
  let left = 0, right = 0, sum = 0, len = A.length;
  let res = 0;

  while(right < len) {
    let charR = A[right];
    (charR === 0 ) && sum++;
    right++;

    while(sum > K) {
      let chatL = A[left];
      (charL === 0) && sum--;
      left++
    }
    res = Math.max(res, right - left)
  }
  return res;
}
```

### 滑动窗口的最大值

[滑动窗口的最大值 - LeetCode](https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/) 是属于**easy level** 算法描述:

```text
# 给定一个数组 nums 和滑动窗口的大小 k，请找出所有滑动窗口里的最大值。

# 示例

输入: nums = [1,3,-1,-3,5,3,6,7], 和 k = 3
输出: [3,3,5,5,6,7]
解释:

滑动窗口的位置 最大值

---

[1 3 -1] -3 5 3 6 7 3
1 [3 -1 -3] 5 3 6 7 3
1 3 [-1 -3 5] 3 6 7 5
1 3 -1 [-3 5 3] 6 7 5
1 3 -1 -3 [5 3 6] 7 6
1 3 -1 -3 5 [3 6 7] 7

# 解析

该算法主要解决的是某一个窗口中最大值，用到了单调队列特殊的数据结构
```

掌握了[单调队列](/algorithm/structure/posts/queue.html#单调队列)之后，就可以解决这道算法了，下面解题的大概框架

```javascript
var maxSlidingWindow = function(nums, k) {
  let res = [];
  // 初始化单调队列滑动窗口
  let queue_window = new MonotonicQueue();

  for (let i = 0; i < nums.length; i++) {
    // 先将窗口中 k - 1 个元素入队
    if (i < k - 1) {
      queue_window.push(nums[i]);
    } else {
      // 窗口滑动
      queue_window.push(nums[i]);
      // 获取当前窗口中的最大值
      res.push(queue_window.max());
      // 窗口首位元素出队
      queue_window.pop(nums[i - k + 1]);
    }
  }
};
```

接下来看下单调队列的实现

```javascript
class MonotonicQueue {
  constructor() {
    this.queue = [];
  }

  empty() {
    return !this.queue.length;
  }

  size() {
    return this.queue.length || 0;
  }

  // 返回队尾的元素
  back() {
    return this.size() && this.queue[this.size() - 1];
  }
  // 删除队尾元素
  popBack() {
    this.queue.pop();
  }
  // 队尾新增元素
  pushBack(n) {
    this.queue.push(n);
  }

  // 元素入队
  push(n) {
    while (!this.empty() && this.back() < n) {
      this.popBack();
    }
    this.pushBack(n);
  }
  // 返回队首的元素
  front() {
    return this.queue[0];
  }
  // 删除队首元素
  popFront() {
    this.queue.shift();
  }
  // 队首新增元素
  pushFront(n) {
    this.queue.unshift(n);
  }

  // 返回最大元素
  max() {
    return this.front();
  }
  // 删除元素
  pop(n) {
    if (!this.empty() && this.front() === n) {
      this.popFront();
    }
  }
}
```

