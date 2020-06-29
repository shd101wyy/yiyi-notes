---
tags:
  - algorithm
  - interview
created: 2020-05-19T08:14:04.596Z
modified: 2020-05-19T09:01:45.655Z
---

# 排序算法

1. 时间复杂度为 $O(n^2)$ 的排序算法
   - 冒泡排序
   - 选择排序
   - 插入排序
   - 希尔排序（希尔排序比较特殊，它的性能略优于 $O(n^2)$，但又比不上 $O(nlogn)$，姑且把它归入本类
2. 时间复杂度为 $O(nlogn)$ 的排序算法
   - 快速排序
   - 归并排序
   - 堆排序
3. 时间复杂度为线性的排序算法
   - 计数排序
   - 桶排序
   - 基数排序

| 排序算法 | 平均时间复杂度 | 最坏时间复杂度 | 空间复杂度 | 是否稳定排序 |
| -------- | -------------- | -------------- | ---------- | ------------ |
| 冒泡排序 | $O(n^2)$       | $O(n^2)$       | $O(1)$     | 稳定         |
| 快速排序 | $O(nlogn)$     | $O(n^2)$       | $O(logn)$  | 不稳定       |
| 归并排序 | $O(nlogn)$     | $O(nlogn)$     | $O(n)$     | 稳定         |
| 堆排序   | $O(nlogn)$     | $O(nlogn)$     | $O(1)$     | 不稳定       |
| 计数排序 | $O(n + k)$     | $O(n + k)$     | $O(k)$     | 稳定         |
| 桶排序   | $O(n)$         | $O(nlogn)$     | $O(n)$     | 稳定         |

所谓**稳定排序**，表示对于具有相同值的多个元素，其间的先后顺序保持不变。对于基本数据类型而言，一个排序算法是否稳定，影响很小，但是对于结构体数组，稳定排序就十分重要。例如对于 student 结构体按照关键字 score 进行非降序排序：

```
// A structure data definition
typedef struct __Student
{
    char name[16];
    int score;
}Student;
// Array of students
name :  A      B     C     D
score:  80     70    75    70

Stable sort in ascending order:
name :  B      D     C     A
score:  70     70    75    80

Unstable sort in ascending order:
name :  D      B     C     A
score:  70     70    75    80

其中稳定排序可以保证 B 始终在 D 之前；而非稳定排序，则无法保证。
```

## 冒泡排序 Bubble sort

其实就是两两比较互换位置

![](https://www.bournetocode.com/projects/GCSE_Computing_Fundamentals/pages/img/bubble_sort_ani.gif)

## 快速排序 Quick sort

需要选择 Pivot 基准元素，根据基准元素，把比基准元素大的以及比基准元素小的隔开

![](https://i.loli.net/2020/05/19/KyuiU895kSVwqgv.gif)

## 堆排序 Heap sort

其实就是使用 Binary heap，不停的删除顶部元素，删除的结果顺序就是排序的结果

![](https://www.codingeek.com/wp-content/uploads/2016/07/Heapsort-example.gif)

## 计数排序 Count sort

只能排序有确定范围的整数

## 桶排序 Bucket sort

...
