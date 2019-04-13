## 思想：

1．先从数列中取出一个数作为基准数。

2．分区过程，将比这个数大的数全放到它的右边，小于或等于它的数全放到它的左边。

3．再对左右区间重复第二步，直到各区间只有一个数。


## 稳定性
    　快速排序是非稳定的，例如[2，2，1]。

## 实现：

### 方法一：
```
def quickSort1(arr):
    if len(arr) <= 1:
        return arr

    bigList = []
    smallList = []
    middle = arr[0]
    for i in arr[1:]:
        if i <= middle:
            smallList.append(i)
        else:
            bigList.append(i)
    return quickSort1(smallList) + [middle] + quickSort1(bigList)
```

### 方法二：
```
def quickSort2(arr):
    if len(arr) <= 1:
        return arr

    low = 0
    high = len(arr) - 1
    privot = arr[0]
    while low < high:
        while low < high and arr[high] >= privot:
            high -= 1
        arr[low] = arr[high]

        while low < high and arr[low] <= privot:
            low += 1
        arr[high] = arr[low]

    arr[low] = privot
    arr[:low] = quickSort2(arr[:low])
    arr[low + 1:] = quickSort2(arr[low + 1:])
    return arr
  ```
### 方法三：
```
def quickSort3(arr):
    if len(arr) <= 1:
        return arr

    low = 0
    privot = arr[0]
    privot_pos = 0

    for index, val in enumerate(arr):
        if arr[index] < privot:
            privot_pos += 1

            if index != privot_pos:
                arr[index], arr[privot_pos] = arr[privot_pos], arr[index]

    arr[privot_pos], arr[low] = arr[low], arr[privot_pos]
    arr[:privot_pos] = quickSort3(arr[:privot_pos])
    arr[privot_pos + 1:] = quickSort3(arr[privot_pos + 1:])

    return arr
```

### 测试
```
import random

arr = list(range(10))

import copy

for item in range(10):
    old_arr = copy.copy(arr)
    print('#' * 10)
    random.shuffle(arr)
    print(arr)
    assert quickSort1(arr) == old_arr
    assert quickSort2(arr) == old_arr
    assert quickSort3(arr) == old_arr
```
