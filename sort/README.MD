### 对一组数组排序

- 有没有可能包含有大量重复的元素 [3路快排](https://github.com/irying/Stark/blob/master/sort/three_sort.c)

- 是否大部分数据距离它的正确的位置很近（近乎顺序，比如银行的业务顺序）[插入排序](https://github.com/irying/Stark/blob/master/sort/insert.c)

- 是否数据的取值范围非常有限（学生的成绩）[计数、基数排序](https://github.com/irying/Stark/blob/master/sort/radix.c)

- 是否需要稳定排序[归并排序](https://github.com/irying/Stark/blob/master/sort/merge.c)

- 是否是使用链表存储[归并排序](https://github.com/irying/Stark/blob/master/sort/merge.c)

- 数据的大小是否可以装载到内存

  ​

  比如内存只有100Mb，而数据却有1Gb，所以肯定没法一次性放到内存去排序，只能用外部排序，而外排序通常是使用多路归并排序，即将原文件分解成多个能够一次性装入内存的部分（如这里的100Mb），分别把每一部分调入内存完成排序（根据情况选取适合的内排算法），**然后对已经排序的子文件进行多路归并排序（胜者树或败者树**）。

### TopN问题

来源：http://www.jianshu.com/p/d0c42c40b4e7

解法1:

在快速排序中，我们用到了一个辅助函数partition。如果基于数组的第k个数字来调整，使得比k个数字小的所有数字都位于数组的左边，比第k个数字大的所有数字都位于数组的右边。这样调整之后，位于数组左边的k个数字就是最小的k个数字（这k个数字不一定是排序的）。



解法2:

在本算法中，我们同样借助快速排序中的partition函数，但采用的是一种"分而治之"的思想。

假设N个数村粗在S中，我们从数组S中随机找出一个元素X，把数组分为两部分Sa和Sb。Sa中的元素小于等于X，Sb中的元素大于X。这时，有两种可能性：

> 1. Sa中元素的个数小于K，则Sa中所有的数和Sb中最大的k-|Sa|个元素就是数组S中最大的k个数。
> 2. Sa中元素的个数大于或等于k，则需要返回Sa中最大的k个元素。



解法3:

这三个解法有个共同之处，就是需要对数据访问多次，那么就有一个问题，如果N很大呢，100亿？这个时候数据就不能全部装入内存，所以要求尽可能少地遍历所有数据。不妨设N>K，前K个数中最小K个数是一种退化的情况，所有K个数就是最小的K个数。如果考虑第K+1个数X呢？如果X比最小的K个数中的最大的数Y大，那么最小的K个数还是保持不变。如果X比Y小，那么最小的K个数应该去掉Y，而包含X。
进一步，我们用容量为K的最大堆来存储最小的K个数。最大堆的堆顶元素就是最小K个数中最大的一个。**<u>每次考虑一个数X，如果X比堆顶元素Y大，则不需要更新堆。否则，用X替换堆顶元素Y。在X替换堆顶元素Y之后，X可能破坏最大堆的结构，需要更新堆来维持堆的性质。更新过程花费的时间复杂度O(logK)。</u>**