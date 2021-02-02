# 排序算法

排序算法是日常开发中常用的一种算法。排序算法有很多种，冒泡、选择、插入、快速、归并以及希尔排序等。每种排序算法都有一定的优缺点。首先从最简单的冒泡排序开始。

### 冒泡排序

下图为动图演示

![](../.gitbook/assets/bubble-sort.gif)

排序冒泡会有两层循环，分为外循环和内循环。外循环负责整个数组的循环，内循环的区间随着外循环的区间变化而变化，内循环判断相邻元素大小，从而进行元素互换。

```javascript
// 元素交换
function swap(arr, a, b) { 
  let tmp = arr[a];
  arr[a] = arr[b];
  arr[b] = tmp;
}
```

内循环判断相邻元素之间的大小 ，从而进行位置互换

```javascript
// 冒泡算法 解法一
function bubbleSort(arr) {
  for (let i = 0; i < arr.length; i++) {
    for (let j = 0; j < arr.length - 1; j++) {   
      if(arr[j] > arr[j + 1])  {   
        swap(arr, j, j+1)
      }
    }
  }
}
```

解法一的时间复杂度为

$$
O（n）= n * (n - 1)
$$

从动态图中不难发现，外循环每循环一次，数组后面的元素的顺序已经排好了，因此这是一部分没有必要的循环，可以在解法一的基础上优化，降低时间复杂度

```javascript
// 冒泡算法 解法二 优化时间复杂度
function bubbleSort(arr) {
  console.time('bubble start')
  for (let i = 0; i < arr.length; i++) {
    for (let j = 0; j < arr.length - 1 - ; j++) {   
      if(arr[j] > arr[j + 1])  {   
        swap(arr, j, j+1)
      }
    }
  }
  console.timeEnd('bubble start')
}
```

可以看到，解法二在内循环的区间随着外循环的区间变化，这样内循环的判断就是在未排序好的区间进行循环，这样最大限度的节省了时间复杂度

### 选择排序

下图为动图展示

![](../.gitbook/assets/select-sort.gif)

可以看到，选择排序也是两重循环，外循环的第一个元素和内循环剩下的元素进行比较，将最小的元素放到第一个位置上，以此类推。

```javascript
// 选择排序
function selectSort(arr) {
  console.time('select start')
  let min
  for (let i = 0; i < arr.length - 1; i++) {
    min = i;
    for (let j = i + 1; j < arr.length; j++) {
      if(arr[min] > arr[j]) {
        min = j
      }
    }
    swap(arr, i, min)
  }
  console.timeEnd('select start')
}
```

### 插入排序

下图为动图展示

![](../.gitbook/assets/insert-sort.gif)

插入排序也有两层循环，外层循环遍历每一个元素，内层循环对外层循环选中的选中的元素跟前一个元素 进行比较，如果选中的元素小于前一个元素，则将前一个元素后移，以此类推。

```javascript
// 插入排序
function insertSort(arr) {
  console.time('insert start')
  let tmp, inner;
  for (let outer = 1; outer < arr.length; outer++) {
    const tmp = arr[outer];
    let inner = outer
    while (inner > 0 && arr[inner - 1] > tmp) {
      arr[inner] = arr[inner - 1]
      inner--
    }
    arr[inner] = tmp
  }
  console.timeEnd('insert start')
}
```

### 三种基本排序算法耗时

这三种基本排序算法基本已经写完了，但是哪个是最优的呢，下面通过耗时来进行比较

```javascript
// 生成随机数组
function randomArr(num) {
  let arr = []
  for (let i = 0; i < num; i++) {
    arr[i] = Math.floor(Math.random() * (num+1))
  }
  return arr
}
// 生成100个随机数
let testArr = randomArr(100);
bubbleSort(testArr); // bubble start: 0.76513671875 ms
selectSort(testArr); // select start: 0.239990234375 ms
insertSort(testArr); // insert start: 0.02197265625 ms

// 生成1000个随机数
let testArr = randomArr(1000);
bubbleSort(testArr); // bubble start: 4.07421875 ms
selectSort(testArr); // select start: 3.81494140625 ms
insertSort(testArr); // insert start: 4.828125 ms

// 生成10000个随机数
let testArr = randomArr(1000);
bubbleSort(testArr); // bubble start: 162.80908203125 ms
selectSort(testArr); // select start: 83.802001953125 ms
insertSort(testArr); // insert start: 0.68994140625 ms
```

可以看到，随着测试数量的增加，插入排序的耗时越来越少，在测试数量较少时，三种算法的耗时差不多。不过判断哪种算法最优，还要进行大量的数据进行测试。

### 快速排序

快速排序是处理大量数据最快的一种算法，其核心思想是分而治之，采用递归的方式将数组分为多个单元，每个单元进行排序，最终依次将每个单元合并。

![&#x5FEB;&#x901F;&#x6392;&#x5E8F;&#x7684;&#x56FE;&#x89E3;](../.gitbook/assets/quicksort.png)

算法过程：

1. 找到一个基准值（pivot），将待排序数组分成两个字数组
2. 将小于基准值的值放到左子数组中，大于基准值的值放到右子数组中。依次类推，最终将各个子数组合并

```javascript
// 快速排序
const quickSort = (arr) => {
  if(!arr.length) return [];

  let leftArr = [], rightArr = [];
  let pivot = arr[0]

  for (let i = 1; i < arr.length; i++) {
    if(arr[i] <= pivot) {
      leftArr.push(arr[i])
    }else {
      rightArr.push(arr[i])
    }
  }
  return quickSort(leftArr).concat(pivot, quickSort(rightArr))
}
```

