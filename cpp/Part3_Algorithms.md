[TOC]

# 一、基础

## 1. 时间复杂度分析

程序复杂度测试的外部因素

1. 测试结果非常依赖测试环境
2. 测试结果受数据规模的影响很大



时间复杂度的分析

1. 复杂度的分析不一定要精确，有时候为了快速分析可以很粗略
   一般，只关注循环执行次数最多的一段代码
2. 时间复杂度反应的是数据规模 n 很大的时候的一个增长趋势
   一般**系数、常数、低阶**会被忽略，但在实际开发中由于使用的数据规模一般都不大
   反而需要考虑**系数、常数、低阶**
3. 加法法则：总复杂度等于量级最大的那段代码的复杂度
4. 乘法法则：嵌套代码的复杂度等于嵌套内外代码复杂度的乘积



多个计算的总时间复杂度分析

1. 平均复杂度分析：平均计算每个复杂度值
2. 均摊复杂度分析：根据每个复杂的计算出现概率 * 复杂度，累加后得到的最终复杂度



常用时间复杂度大小的比较

- 由于对数可以互相转换 $log_{10}n = log_{10}2 \cdot log_2n = C_{常数}log_2n$

  不管是以 2 为底、以 3 为底，还是以 10 为底复杂度都计做 logn
  
- 时间复杂度 logn，不管问题 n 有多大，最后 logn 总会维持在一个较小的范围下，如下图

<img src="./images/asymptotic_time_complexity.png" style="zoom:67%;" />



## 2. 使用哨兵

例：
在数组 a 中，查找 key，返回 key 所在的位置
其中，n 表示数组 a 的长度

- 一般做法

  ```c
  int find(char* a, int n, char key) {  
    // 边界条件处理，如果 a 为空，或者 n<=0，说明数组中没有数据，就不用 while 循环比较了  
    if(a == null || n <= 0) {    
      return -1;  
    }
    
    int i = 0;  
    // 这里有两个比较操作：i<n 和 a[i]==key
    while (i < n) {    
      if (a[i] == key) {      
        return i;
      }
      ++i;
    }
    return -1;
  }
  ```



- 使用哨兵
  可以省去循环体中的判断操作，从而达到简化问题的目的

  ```c
  // 通过减小循环体中的比较次数(耗时最多的地方)，来提升效率
  // 例：char *a = {'4','2','3','5','9','6'} n=6 key='7'
  int find(char* a, int n, char key) {  
    if(a == null || n <= 0) {    
      return -1;
    }
    
    // 这里因为要将 a[n-1] 的值替换成 key，所以要特殊处理这个值  
    if (a[n-1] == key) {
      return n-1;
    }
    
    // 把 a[n-1] 的值临时保存在变量 tmp 中，以便之后恢复 tmp='6'   
    // 之所以这样做的目的是：希望find()代码不要改变a数组中的内容
    char tmp = a[n-1];
    
    // 把 key 的值放到 a[n-1] 中，此时 a = {'4','2','3','5','9','7'}
    a[n-1] = key;
    
    int i = 0;
    // while 循环比起代码一，少了 i<n 这个比较操作
    while (a[i] != key) {
      ++i;
    }
    
    // 恢复 a[n-1] 原来的值,此时 a= {'4','2','3','5','9','6'}
    a[n-1] = tmp;
    
    if (i == n-1) {
      // 如果 i == n-1 说明，在 0...n-2 之间都没有 key，所以返回 -1
      return -1;
    } else {
      // 否则，返回 i，就是等于 key 值的元素的下标
      return i;  
    }
  }
  ```



## 3. 递归

结构

1. **递推公式**，例：$F(n) = F(n-1) * F(n-2)$
   一个问题的解可以分解为几个子问题的解
   这个问题与分解之后的子问题，除了数据规模不同，求解思路完全一样
2. **递归终止条件**，例：$F(1) = 1,\space F(2) = 2$



缺点

1. **空间复杂度高**

   递归函数内的局部变量过多，在栈内递归调用的成本会过高

2. **堆栈溢出**
   通过堆栈剩余内存大小，来限制递归深度

   ```c
   // 全局变量，表示递归的深度
   int depth = 0;
   int F(int n) { 
     ++depth； 
     if (depth > 1000) throw exception;
     
     if (n == 1) return 1;		// 终止条件
     if (n == 2) return 2;
     
     return F(n-1) * F(n-2);	// 递推公式
   }
   ```

3. **重复计算**

   可以通过一个数据结构（比如散列表）来保存已经求解过的值

   ```c
   // 以下 F(2) 被重复计算了
   // F(3) = F(2) + F(1)
   // F(4) = F(3) + F(2)
   int F(int n) {
     if (n == 1) return 1;
     if (n == 2) return 2;
     
     // hasSolvedList 可以理解成一个 Map，key 是 n，value 是 F(n)
     if (hasSolvedList.containsKey(n)) {
       return hasSolvedList.get(n);
     }
     
     int ret = f(n-1) + f(n-2);
     hasSolvedList.put(n, ret);
     
     return ret;
   }
   ```

   

<u>根据开发所面临的具体问题</u>，可以将递归转换为**迭代循环**的方式

1. 用迭代循环来替代递归并不能避免递归原有的缺陷

2. 迭代循环本质上与递归的方法没有任何区别
   但具体细节为手动实现，使**迭代循环比递归更具有可控性**

   ```c
   int F(int n) {
     // 终止条件
     if (n == 1) return 1;
     if (n == 2) return 2;
     
     // 原递归栈所需要存储的数据
     int ret = 0;					
     int pre = 2;
     int prepre = 1;
     
     // 递归体
     for (int i = 3; i <= n; ++i) {
       ret = pre + prepre;
       prepre = pre;
       pre = ret;
     }
     
     return ret;
   }
   ```





# 二、排序

**稳定排序**：相同的数字排序后顺序不变

**有序度**：有序关系的元素对的个数
				例 序列 3，1，5，6 中，有序度为 5，有序关系元素对为 (3, 5) (3,6) (1,5) (1,6) (5,6)
**逆序度**： 满有序度 - 有序度
**满有序度**：序列全部顺序排列时的有序度，计算公式 ${n(n-1) \over 2}$



## 1. 比较排序
### 1.1 冒泡排序

![](./images/sort_bubble.png)

每次确保固定后段区间（冒泡上去的）顺序是正确的，不断缩小排序范围

- 稳定排序
- 空间复杂度 $O(1)$
- 时间复杂度，**取决于逆序度的数量**
  最好 $O(n)$
  最坏 $O(n^2)$

```c
// 冒泡排序，a表示数组，n表示数组大小
void bubble_sort(int* arr, int n)
{
	if (n <= 1) return;
  
  for (int i = 0; i < n; ++i) {
  	bool flag = false;	// 提前退出冒泡循环的标志位
    for (int j = 0; j < n - 1 - i; ++j) {
      if (arr[j] > arr[j+1]) {
        int tmp = arr[j];	// 交换
        arr[j] = arr[j+1];
        arr[j+1] = tmp;
        flag = true;    // 表示有数据交换
      }
    } // for j
    
    if (!flag) break;		// 没有数据交换，提前退出
    
  } // for i
  
}
```



### 1.2 插入排序

![](./images/sort_insert.jpg)

区分已排序区间和未排序区间，每次将未排序区间的第一元素插入到已排序区间

- 稳定排序
- 空间复杂度 $O(1)$
- 时间复杂度，**取决于逆序度的数量**
  最好 $O(n)$
  最坏 $O(n^2)$

```c
// 插入排序，a表示数组，n表示数组大小
void insertion_sort(int* arr, int n)
{
  if (n <= 1) return;
  
  for (int i = 1; i < n; ++i) {
    int tmp = arr[i];
    int j = i - 1; 		// 查找插入的位置
    for (; j >= 0; --j) {
      if (arr[j] > tmp) {
        arr[j+1] = arr[j];// 这里相比于冒泡排序只进行了一次赋值，所以插入排序比冒泡排序更高效
      } else {
        break;
      }
    }
    arr[j+1] = tmp; 		// 插入数据
  }
  
}
```



### 1.3 选择排序

![](./images/sort_selection.jpg)

区分已排序区间和未排序区间，每次将未排序区间中找到最小的元素，将其放到已排序区间的末尾

- 不是稳定排序
- 空间复杂度 $O(1)$
- 时间复杂度较稳定，最好最坏都是 $O(n^2)$

```c
// 选择排序，a表示数组，n表示数组大小
void selection_sort(int* arr, int n)
{
  if (n <= 1) return;
  
  for (int i = 0; i < n - 1; ++i) {
    int min = i;
    for (int j = i + 1; j < n; ++j) {
      if (arr[min] > arr[j]) {
        min = j;
      }
    } // for j
    
    if (min != i) {
    	std::swap(arr[i], arr[min]);    
		}
  } // for i
  
}
```



### 1.4 归并排序

![](./images/sort_merge.png)

采用分而治之的思想，先将序列二分，直到不能二分为止，后通过合并序列的方法来排序

- 稳定排序

- 空间复杂度 $O(n)$

- 时间复杂度较稳定，最好最坏都是 $O(nlogn)$
  由下面的推论可知，归并排序经过 ${n \over 2^k}$ 次分解才变为 1，因此 $k = log_2n$ 则 $T(n) = nlog_2n$
  $$
  \begin{align}
  T(1) &= C \\
  T(n) &= 2*T({n \over 2}) \\
  		 &= 2*(2*T({n \over 4}))  &= 4*T({n \over 4}) \\
       &= 4*(2*T({n \over 8}))  &= 8*T({n \over 8}) \\
       &= 2^k * T({n \over 2^k}) \\
  \end{align}
  $$

```c
// 归并排序算法, A是数组，n表示数组大小
void merge_sort(int* arr, int n)
{
  __merge_sort(arr, 0, n-1);
}

// 递归调用函数
void __merge_sort(int* arr, int p, int r)
{
  // 递归终止条件
  if (p >= r) return;  
  
  // 取 p 到 r 之间的中间位置 q
  int q = (p + r) / 2;
    
  // 分治递归
  __merge_sort(arr, p, q);
  __merge_sort(arr, q + 1, r);
    
  // 将 arr[p...q] 和 arr[q+1...r] 合并为 arr[p...r]
  __merge(arr, p, q, r)
}

void __merge(int* arr, int p, int q, int r)
{
  int *tmp;
	int i, j, k;

	tmp = (int*)malloc((r - p + 1) * sizeof(int));

	if (!tmp) abort();

	for (i = p, j = q + 1, k = 0; i <= q && j <= r;) {
		if (arr[i] <= arr[j])
			tmp[k++] = arr[i++];
		else
			tmp[k++] = arr[j++];
	}

	if (i == q + 1) {
		for (; j <= r;)
			tmp[k++] = arr[j++];
	} else {
		for (; i <= q;)
			tmp[k++] = arr[i++];
	}

	memcpy(arr + p, tmp, (r - p + 1) * sizeof(int));
	free(tmp);
}
```



### 1.5 快速排序

![](./images/quick_sort.png)

采用分而治之的思想，在序列中选择任意一个数为分区点（Pivot），遍历当前序列，将小于分区点的数据放在其前面，大于分区点的数据放在其后面，之后前后分区重新选择新的分区点，不断细分，最后到不能划分后，排序完成

- 不是稳定排序

- 空间复杂度 $O(1)$

- 时间复杂度
  最好 $O(nlogn)$
  最坏 $O(n^2)$ （区分点选择不当）



优化，主要是优化区分点的选择

1. 三数取中
   从区间的首、尾、中间，分别取出一个数，然后对比大小，取这 3 个数的中间值作为分区点
2. 随机法
   每次从要排序的区间中，随机选择一个元素作为分区点

```c
void quick_sort(int *arr, int size)
{
	__quick_sort(arr, 0, size - 1);
}

void __quick_sort(int *arr, int p, int r)
{
	if (p >= r) return;

	int q = partition(arr, p, r);
	__quick_sort(arr, p, q-1);
	__quick_sort(arr, q+1, r);
}

int partition(int *arr, int p, int r)
{
	int i; // 将数组分为 < i 的已处理区，和 >= i 的未处理区
  int j; // 指向未处理区第一个元素
	for (i = j = p; j < r; ++j) {
		if (arr[j] < arr[r]) { 									// 设 pivot = arr[r]
			if(i != j) std::swap(arr[i], arr[j]); // 为避免插入数据后的移动，这里使用交换
      
			++i;																	// 遇到 > pivot 的数就停住，等待交换
		}
	}
	
	std::swap(arr[i], arr[r]);
  
	return i;
}
```



### 1.6 堆排序

利用堆这么一个逻辑上的完全二叉树结构，而存储上用数组这个数据结构来进行排序

- 不是稳定排序
- 空间复杂度 $O(1)$
- 时间复杂度，最好，最坏都是 $O(nlogn)$
- **堆擅长取数据的前几个值，而不是将整个数据排序**
- 堆擅长取**动态变化数据**的前几个值



对比于快速排序，堆排序性能较差

1. 堆排序访问数据是跳着访问的，快速排序是顺序访问的，跳着访问如果幅度过大可能会导致 CPU 缓存的不断更换，从而降低性能
2. 同样的数据堆排序进行的交换次数比快速排序多



步骤

1. **建堆**：时间复杂度 $O(n)$
   将数组原地建成一个堆（不借助其他数组）
   <u>方法一：从前到后</u>
   尽管堆数组中包含 n 个数据，但可以假设，起初堆中只包含一个数据，就是下标为 1 的数据。然后将剩下的数据逐个插入堆中
   <u>方法二：从后到前</u>
   从后到前将堆节点全部遍历堆化，减少方法一中**堆化不断从前到后**的**重复遍历**

2. **堆化**
   从上到下（或从下到上）将最大值（或最小值）放到堆的根目录，构建有序的堆

3. **排序**：从后向前

   1. 交换堆顶和堆最后一个数

   2. 堆<u>逻辑上删除</u>堆最后一个数（堆的范围减小）

   3. 堆化

   4. 重复 1 ～ 3 步骤，直到堆的范围缩小为 1，堆排序结束

      ![](./images/sort_heap.jpg)

```c++
// 建堆：从后到前
// 这里的 a 默认结构是从 1 开始存储的数组，不同于数组 a 的长度，n 表示数据的个数
void buildHeap(int* a, int n) {
  // 对于堆这样的完全二叉树结构第 n/2+1 ～ n 都是叶子节点
  // 堆化时会访问下一层的节点，因此这里不需要对最底层的节点做堆化
  for (int i = n/2; i >= 1; --i) {
    heapify(a, n, i);
  }
}

// 堆化：自上往下
void heapify(int* a, int n, int i) { 
  while (true) {
    int maxPos = i;
    if (i*2 <= n && a[i] < a[i*2]) 					maxPos = i*2;
    if (i*2+1 <= n && a[maxPos] < a[i*2+1]) maxPos = i*2+1;
    if (maxPos == i) 												break;
    std::swap(a[i], a[maxPos]);
    i = maxPos;
  }
}

// 这里的 a 默认结构是从 1 开始存储的数组，不同于数组 a 的长度，n 表示数据的个数
void heap_sort(int* a, int n) {
  buildHeap(a, n);
  int k = n;
  while (k > 1) {
    std::swap(a[1], a[k]);
    --k;
    heapify(a, k, 1);
  }
}
```





## 2. 线性排序

排序算法的时间复杂度是线性的，对排序的数据要求苛刻
<u>线性排序比较强调数据的范围，范围不等同于大小，一组很大的数，他们的范围可以很小</u>

### 2.1 桶排序

把含有的 n 个数据的数组，均匀地划分到 m 个数组（桶）内，每个数组内部使用快速排序，最后重新合并为一个数组（如果划分后，某个数组的数据仍过多，可单独将这个数组继续划分）

- 不是稳定排序
- 空间复杂度 $O(n)$
- 时间复杂度
  平均 $O(nlog{n \over m}) = O(m * {n \over m} * log{n \over m})$
  最好 $O(n)$，当 m 和 n 接近时



**使用桶排序的前置条件**

1. 数组的数据可以被均匀的划分为几个数组（极端情况，都被划分为一个桶里，桶排序会退化为一个快速排序）

2. 数组划分为几个数组后，数组间的顺序要求已经排好了，不需要数组间在重新排序

   

**适用场景**

桶排序比较适合用在外部排序中
外部排序就是数据存储在外部磁盘中，数据量比较大，内存有限，无法将数据全部加载到内存中

```c++
#include <algorithm>
#include <iterator>

template <size_t BucketSize,
          typename IterT,
          typename T = typename std::iterator_traits<IterT>::value_type,
          typename Compare = std::less<T>>
void bucket_sort(IterT first, IterT last, Compare comp = Compare()) {
    const T min = *std::min_element(first, last);
    const T max = *std::max_element(first, last);
    const T range = max + 1 - min;
    const size_t bucket_num = (range - 1) / BucketSize + 1;

    std::vector<std::vector<T>> buckets(bucket_num);
    for (auto b : buckets) {
        b.reserve(2 * BucketSize);
    }

    for (IterT i = first; i != last; ++i) {
        size_t idx = (*i - min) / BucketSize;
        buckets[idx].emplace_back(*i);
    }

    IterT dest = first;
    for (auto b : buckets) {
        std::sort(b.begin(), b.end(), comp);
        std::copy(b.begin(), b.end(), dest);
        dest += b.size();
    }

    return;
}
```



### 2.2 计数排序

![](./images/sort_counting.png)

计数排序是桶排序的特殊情况，把含有的 n 个数据的数组，均匀地划分到 m 个数组（桶）内，**每个数组内部的数据都相同**

- 稳定排序（必须从后向前遍历）
- 空间复杂度 $O(n)$
- 时间复杂度 $O(n)$



**使用计数排序的前置条件**

1. 只能用在数据范围不大的场景中，如果数据范围 k 比要排序的数据个数 n 大很多，不适合用计数排序
2. 只能给非负整数排序，如果要排序的数据是其他类型的，要将其在不改变相对大小的情况下，转化为非负整数



**适用场景**

排序的数据范围小

```c++
#include <algorithm>
#include <iterator>

template <typename IterT,
          typename T = typename std::iterator_traits<IterT>::value_type>
void counting_sort(IterT first, IterT last) {
    const auto len = std::distance(first, last);
    if (len < 2) return;

    const T max = *std::max_element(first, last);
    if (max == 0) return;

    std::vector<size_t> counter(max + 1);
    // 记录每个数对应的个数
    for (IterT i = first; i != last; ++i) {
        ++counter[*i];
    }
    // 记录小于当前数的个数
    for (size_t i = 1; i < counter.size(); ++i) {
        counter[i] += counter[i - 1];
    }

    std::vector<T> temp(len);
    // 根据计数表，从后向前遍历原数组（确保稳定排序），生成一个新的排序后的数组
    for (IterT i = last - 1; i >= first; --i) {
        temp[counter[*i] - 1] = *i;
        --counter[*i];
    }
    std::copy(temp.begin(), temp.end(), first);
}

// 测试函数
template <typename Container,
          typename T = typename Container::value_type>
void test_counting_sort(Container cont) {
    counting_sort(cont.begin(), cont.end());
    std::transform(cont.begin(), 
                   cont.end(), 
                   std::ostream_iterator<T>(std::cout, " "),
                   [](T i){ return i; });
    std::cout << std::endl;
}
```



### 2.3 基数排序

![](./images/radix_sort.jpg)

在一些较大的数组成的数组里，通过**逐个比较**不同数同一位数字（基数）的大小来进行排序

- 稳定排序（必须从后向前遍历）
- 空间复杂度 $O(1)$
- 时间复杂度 $O(n) = O(C_{位数} * n)$



**使用基数排序的前置条件**

1. 数组里的数据可以按位划分
2. 位之间有递进的关系
   如果 a 数据的高位比 b 数据大，那剩下的低位就不用比较了
3. 每一位的数据**范围**不能太大，要可以用线性排序算法来排序



**适用场景**

- 单词在字典中的排序
  所有的单词补齐到相同长度，位数不够的可以在后面补 0
  ASCII 值所有字母大于 0，补 0 不会影响原有的顺序
- 手机号排序
  手机号要是看作一个数的话，即便位数要相同，数据的范围会很大
  使用基数排序，会细分手机号的分布范围，从而提高排序效率

```c++
#include <math.h>

void radix_sort(int a[], int len, int digit_max)
{
	int *tmp = (int *)std::malloc(sizeof(int)*len);
	assert(nullptr != tmp);

  int digit = 1;
	int counter[10];
	for (int i = 0, j = 0, k = 0; i < digit_max; ++i) {
    memset(counter, 0, sizeof(int)*10);
    
    // 从后向前遍历原数的位数（确保稳定排序）
    digit = pow(10, i);
    
    for (j = 0; j < len; ++j) {
			k = (a[j] / digit) % 10;
			++counter[k];
		}
		for(j = 1; j < 10; ++j) {
			counter[j] += counter[j-1];
		}

    // 从后向前遍历原数组（确保稳定排序）
		for(j = len - 1; j >= 0; --j) {
			k = (a[j] / digit) % 10;
      tmp[counter[k] - 1] = a[j];
			--counter[k];
		}
    
    memcpy(a, tmp, sizeof(int)*len);
	}

}
```



# 三、查找数据

不同的数据结构，适合不同的数据查找方式



## 1. 二分查找（依赖数组）

空间复杂度 $O(1)$

时间复杂度 $O(logn)$
其中 ${n \over 2^k}=1$ 时，k 的值就是总共缩小的次数，即时间复杂度



### 1.1 使用二分查找的前置条件

- 查找数据已经**有序**
- 二分查找**依赖于数组**这种连续内存的数据结构
- 由于连续内存的存储结构，二分查找的数量不宜太大或者太小

```c++
// 1. 简单的二分查找实现（数据已经排好序，且数据不重复）
// 1.1 循环实现
int binary_search(int* a, int n, int value) {
  int low = 0; 
  int high = n - 1;
  while (low <= high) {
    // 为防止相加带来的过大数据移除，可优化为：low + (high - low) / 2
    int mid = (low + high) / 2;
    if (a[mid] == value) {
      return mid;
    } else if (a[mid] < value) {
      low = mid + 1;
    } else {
      high = mid - 1;
    }
  }
  
  return -1;
}

// 1.2 递归实现
int __binary_search(int* a, int low, int high, int value) {
  if (low > high) return -1;
  
  int mid = low + ((high - low) >> 1);
  if (a[mid] == value) {
    return mid;
  } else if (a[mid] < value) {
    return __binary_search(a, mid+1, high, value);
  } else {
    return __binary_search(a, low,  mid-1, value);
  }
}

int bsearch(int* a, int n, int val) {
  return __binary_search(a, 0, n - 1, val);
}

// 2. 考虑重复情况的二分查找（数据已经排好序，且数据含有多个重复的数）
// 2.1 查找第一个值等于给定值的元素
int bsearch(int* a, int n, int val) {
  int low = 0;  
  int high = n - 1;  
  while (low <= high) {
    int mid = low + ((high - low) >> 1);
    if (a[mid] > val) {
      high = mid - 1;
    } else if (a[mid] < val) {
      low = mid + 1;
    } else {
      if ((mid == 0) || (a[mid - 1] != val)) return mid;
      else high = mid - 1;   // 由于是找可能重复数的第一个，这里不断取前半段数据
    }
  }
  
  return -1;
}

// 2.2 查找最后一个值等于给定值的元素
int bsearch(int* a, int n, int val) {
  int low = 0;  
  int high = n - 1;  
  while (low <= high) {
    int mid = low + ((high - low) >> 1);
    if (a[mid] > val) {
      high = mid - 1;
    } else if (a[mid] < val) {
      low = mid + 1;
    } else {
      if ((mid == 0) || (a[mid - 1] != val)) return mid;
      else low = mid + 1;   // 由于是找可能重复数的最后一个，这里不断取后半段数据
    }
  }
  
  return -1;
}

// 2.3 查找第一个大于等于给定值的元素
int bsearch(int* a, int n, int val) {
  int low = 0;  
  int high = n - 1;  
  while (low <= high) {
    int mid = low + ((high - low) >> 1);
    if (a[mid] >= val) {
      if ((mid == 0) || (a[mid - 1] < val)) return mid;
      else high = mid - 1;   // 由于是找可能重复数的第一个，这里不断取前半段数据
    } else {
      low = mid + 1;
    }
  }
  
  return -1;
}

// 2.4 查找最后一个小于等于给定值的元素
int bsearch(int* a, int n, int val) {
  int low = 0;  
  int high = n - 1;  
  while (low <= high) {
    int mid = low + ((high - low) >> 1);
    if (a[mid] > val) {
      high = mid - 1;
    } else {
      if ((mid == n - 1) || (a[mid + 1] > val)) return mid;
      else low = mid + 1;   // 由于是找可能重复数的最后一个，这里不断取后半段数据
    }
  }
  
  return -1;
}
```



### 1.2 二分查找适用场景

- 二分查找更适合用在查找近似值的问题
- 对于给定值的查找，用散列表或者二叉树比二分查找更好

```c++
// 应用举例：
// 1. 查找 IP 在某一个 IP 范围内，确定归属地（二分查找最后一个小于等于给定值的元素）
// 2. 求数的平方根（停止二分查找的依据是一个精度范围）
double sqrt(double x, double precision) {
	if (x < 0) return NAN;
	
	double low = 0;
	double high = x;
	if (x < 1 && x > 0) {
		low = x;
		high = 1;
	}
  
  double mid = low + ((up - high) >> 1);
	while((high - low) > precision) {
    // mid * mid 这里可能会溢出
		if (mid * mid > x ) {
			high = mid;
		} else if (mid * mid < x) {
			low = mid;
		} else {
			return mid;
		}
    mid = low + ((up - high) >> 1);
	}
  
	return mid;
}
```






# 引用

- [十大经典排序算法总结](https://www.cnblogs.com/guoyaohua/p/8600214.html)
- [谈谈 STL 中的 std::sort](https://liam.page/2018/09/18/std-sort-in-STL/)
- [谈谈内省式排序算法](https://liam.page/2018/08/29/introspective-sort/)
- [谈谈基于比较的排序算法的复杂度下界](https://liam.page/2018/08/28/lower-bound-of-comparation-based-sort-algorithm/)