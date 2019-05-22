最经典的、最常用的：冒泡排序、插入排序、选择排序、归并排序、快速排序、计数排序、基数排序、桶排序。我按照时间复杂度把它们分成了三类:

![alt text](https://static001.geekbang.org/resource/image/fb/cd/fb8394a588b12ff6695cfd664afb17cd.jpg)

排序算法的稳定性:
仅仅用执行效率和内存消耗来衡量排序算法的好坏是不够的。针对排序算法，我们还有一个重要的度量指标，稳定性。
这个概念是说，如果待排序的序列中存在值相等的元素，经过排序之后，相等元素之间原有的先后顺序不变。

比如说，我们现在要给电商交易系统中的“订单”排序。订单有两个属性，一个是下单时间，另一个是订单金额。
如果我们现在有 10 万条订单数据，我们希望按照金额从小到大对订单数据排序。对于金额相同的订单，我们希望按照下单时间从早到晚有序。对于这样一个排序需求，我们怎么来做呢？
最先想到的方法是：我们先按照金额对订单数据进行排序，然后，再遍历排序之后的订单数据，对于每个金额相同的小区间再按照下单时间排序。
这种排序思路理解起来不难，但是实现起来会很复杂。
借助稳定排序算法，这个问题可以非常简洁地解决。解决思路是这样的：我们先按照下单时间给订单排序，注意是按照下单时间，不是金额。
排序完成之后，我们用稳定排序算法，按照订单金额重新排序。两遍排序之后，我们得到的订单数据就是按照金额从小到大排序，金额相同的订单按照下单时间从早到晚排序的

冒泡排序:

![alt text](https://static001.geekbang.org/resource/image/40/e9/4038f64f47975ab9f519e4f739e464e9.jpg)

 // 冒泡排序，a 表示数组，n 表示数组大小
    public void bubbleSort(int[] a, int n) {

        if (n <= 1) return;
        for (int i = 0; i < n; ++i) {
      // 提前退出冒泡循环的标志位
      boolean flag = false;
      for (int j = 0; j < n - i - 1; ++j) {
        if (a[j] > a[j+1]) { // 交换
         int tmp = a[j];
          a[j] = a[j+1];
          a[j+1] = tmp;
          flag = true;  // 表示有数据交换      
        }
     }
    if (!flag) break;  // 没有数据交换，提前退出
  }
}


插入排序（Insertion Sort）:

![alt text](https://static001.geekbang.org/resource/image/b6/e1/b60f61ec487358ac037bf2b6974d2de1.jpg)

// 插入排序，a 表示数组，n 表示数组大小
public void insertionSort(int[] a, int n) {

    if (n <= 1) return;
 
    for (int i = 1; i < n; ++i) {
    int value = a[i];
    int j = i - 1;
    // 查找插入的位置
    for (; j >= 0; --j) {
      if (a[j] > value) {
        a[j+1] = a[j];  // 数据移动
      } else {
        break;
      }
    }
    a[j+1] = value; // 插入数据
     }
 }

冒泡排序和插入排序的时间复杂度都是 O(n2)，都是原地排序算法，为什么插入排序要比冒泡排序更受欢迎呢？

我们前面分析冒泡排序和插入排序的时候讲到，冒泡排序不管怎么优化，元素交换的次数是一个固定值，是原始数据的逆序度。插入排序是同样的，不管怎么优化，元素移动的次数也等于原始数据的逆序度。

但是，从代码实现上来看，冒泡排序的数据交换要比插入排序的数据移动要复杂，冒泡排序需要 3 个赋值操作，而插入排序只需要 1 个.

归并排序（Merge Sort）：

![alt text](https://static001.geekbang.org/resource/image/db/2b/db7f892d3355ef74da9cd64aa926dc2b.jpg)

	
// 归并排序算法, A 是数组，n 表示数组大小
merge_sort(A, n) {
  merge_sort_c(A, 0, n-1)
}
 
// 递归调用函数
merge_sort_c(A, p, r) {

   // 递归终止条件
   if p >= r  then return
 
   // 取 p 到 r 之间的中间位置 q
   q = (p+r) / 2
   // 分治递归
   merge_sort_c(A, p, q)
   merge_sort_c(A, q+1, r)
   // 将 A[p...q] 和 A[q+1...r] 合并为 A[p...r]
   merge(A[p...r], A[p...q], A[q+1...r])
}

merge(A[p...r], A[p...q], A[q+1...r]) {

   var i := p，j := q+1，k := 0 // 初始化变量 i, j, k
   var tmp := new array[0...r-p] // 申请一个大小跟 A[p...r] 一样的临时数组
   while i<=q AND j<=r do {
    if A[i] <= A[j] {
      tmp[k++] = A[i++] // i++ 等于 i:=i+1
    } else {
      tmp[k++] = A[j++]
    }
  }
  
  // 判断哪个子数组中有剩余的数据
  var start := i，end := q
  if j<=r then start := j, end:=r
  
  // 将剩余的数据拷贝到临时数组 tmp
  while start <= end do {
    tmp[k++] = A[start++]
  }
  
  // 将 tmp 中的数组拷贝回 A[p...r]
  for i:=0 to r-p do {
    A[p+i] = tmp[i]
  }
}



