### Week08

#### 知识点总结

##### 位运算基础及实战要点

```
位运算符
	左移 <<
	右移 >>
	按位或 | 只要两个中有1，运算结果就为1
	按位与 & 只要两个中有0，运算结果就为0
	按位取反 ~ 0 --> 1  1 --> 0
	按位异或 ^ 相同为0，不同为1，可以理解为不进位加法
	
指定位置的位运算
	将x最右边的n位清零：x&(~0<<n)
	获取x的第n位值（0或者1）：(x>>n)&1
	获取x的第n位的幂值：x&(1<<n)
	仅将第n位置为1：x|(1<<n)
	仅将第n位置为0：x&(~(1<<n))
	将x最高位至第n位（含）清零：x&((1<<n)-1)
	将第n位至第0位（含）清零：x&(~((1<<(n+1))-1))
	
实战位运算要点
	判断奇偶
		x%2==1 --> (x&1)==1
		x%2==0 --> (x&1)==0
	x>>1 --> x/2
	X = X & (X - 1) 清零最低位的1
	X & -X 得到最低位的1
	X & ~X 0
```

##### 布隆过滤器的实现及应用

```
一个很长的二进制向量和一系列随机映射函数。
布隆过滤器可以用于检索一个元素是否在一个集合中。

优点是空间效率和查询时间都远远超过一般的算法
缺点是有一定的误识别率和删除困难

元素去布隆过滤器中查，如果查到不在，那肯定不存在；如果查得到，则可能存在也可能不存在。

案例：
	比特币网络
	分布式系统（Map-Reduce）-- Hadoop、search engine
	Redis缓存
	垃圾邮件、评论等的过滤

参考链接：
    布隆过滤器的原理和实现
        https://www.cnblogs.com/cpselvis/p/6265825.html
    使用布隆过滤器解决缓存击穿、垃圾邮件识别、集合判重
        https://blog.csdn.net/tianyaleixiaowu/article/details/74721877
    布隆过滤器 Python 代码示例    
        https://shimo.im/docs/UITYMj1eK88JCJTH/read
    布隆过滤器 Python 实现示例
    	https://www.geeksforgeeks.org/bloom-filters-introduction-and-python-implementation/
    高性能布隆过滤器Python实现示例
    	https://github.com/jhgg/pybloof
    布隆过滤器 Java 实现示例 1
        https://github.com/lovasoa/bloomfilter/blob/master/src/main/java/BloomFilter.java
    布隆过滤器 Java 实现示例 2
        https://github.com/Baqend/Orestes-Bloomfilter
	
```

<img src="/Users/miwang/Library/Application Support/typora-user-images/image-20200815104401495.png" alt="image-20200815104401495" style="zoom: 25%;" />

```java
//Java
public class BloomFilter {
    private static final int DEFAULT_SIZE = 2 << 24;
    private static final int[] seeds = new int[] { 5, 7, 11, 13, 31, 37, 61 };
    private BitSet bits = new BitSet(DEFAULT_SIZE);
    private SimpleHash[] func = new SimpleHash[seeds.length];
    public BloomFilter() {
        for (int i = 0; i < seeds.length; i++) {
            func[i] = new SimpleHash(DEFAULT_SIZE, seeds[i]);
        }
    }
    public void add(String value) {
        for (SimpleHash f : func) {
            bits.set(f.hash(value), true);
        }
    }
    public boolean contains(String value) {
        if (value == null) {
            return false;
        }
        boolean ret = true;
        for (SimpleHash f : func) {
            ret = ret && bits.get(f.hash(value));
        }
        return ret;
    }
    // 内部类，simpleHash
    public static class SimpleHash {
        private int cap;
        private int seed;
        public SimpleHash(int cap, int seed) {
            this.cap = cap;
            this.seed = seed;
        }
        public int hash(String value) {
            int result = 0;
            int len = value.length();
            for (int i = 0; i < len; i++) {
                result = seed * result + value.charAt(i);
            }
            return (cap - 1) & result;
        }
    }
}
```

------

##### LRU Cache的实现、应用和题解

```
两个要素：大小、替换策略
HashTable + Double LinkedList

O(1)查询
O(1)修改、更新

替换策略
	LFU -- least frequently used
	LRU -- least recently used
```

------

##### 初级排序和高级排序的实现和特性

```
排序算法
	比较类排序
		通过比较来决定元素间的相对次序，由于其时间复杂度不能突破O(nlogn)，因此也称为非线性时间比较类排序。
	非比较类排序
		不通过比较来决定元素间的相对次序，它可以突破基于比较排序的时间下界，以线性时间运行，因此也称为线性时间非比较类排序
初级排序 -- O(n^2)
	选择排序（Selection Sort）
		每次找到最小值，然后放到待排序数组的起始位置
	插入排序（Insertion Sort）
		从前到后逐步构建有序序列；对于未排序数据，在已排序序列中从后向前扫描，找到相应位置并插入
	冒泡排序（Bubble Sort）
		嵌套循环，每次查看相邻的元素，如果逆序，则交换
高级排序 -- O(n*logn)
	快速排序（Quick Sort）
		数组取标杆pivot，将小元素放在pivot左边，大元素放右侧，然后依次对右边和右边的子数组继续快排，以达到整个序列有序
	归并排序（Merge Sort）-- 分治
    	把长度为n的输入序列分成两个长度为n/2的子序列
    	对这两个子序列分别采用归并排序
    	将两个排序好的子序列合并成一个最终的排序序列
    *快排和归并 具有相似性，但步骤顺序相反
    *归并：先排序左右子数组，然后合并两个左右子数组
    *快排：先调配出左右子数组，然后对于左右子数组进行排序
    堆排序（Heap Sort）-- 堆插入O(logN)，取最大/小值O(1)
    	数组元素依次建立小顶堆
    	依次取堆顶元素，并删除
特殊排序 -- O(n)
	计数排序（Counting Sort）
		计数排序要求输入的数据必须是有确定范围的整数，将输入的数据值转化为键存储在额外开辟的数组空间中，然后依次把计数大于1的填充回原数组
	桶排序（Bucket Sort）
		假设输入数据服从均匀分步，将数据分到有限数量的桶里，每个桶在分别排序（有可能再使用别的排序算法或是以递归方式继续使用桶排序进行排序）
	基数排序（Radix Sort）
		按照低位先排序，然后收集；再按照高位排序，然后再收集；依次类推，知道最高位。有时候有些属性是有优先级顺序的，先按低优先级排序，再按高优先级排序。
		
参考链接：十大排序算法（动图演示）https://www.cnblogs.com/onepixel/p/7674659.html
```

<img src="/Users/miwang/Library/Application Support/typora-user-images/image-20200816012216411.png" alt="image-20200816012216411" style="zoom:50%;" />



<img src="/Users/miwang/Library/Application Support/typora-user-images/image-20200816012322071.png" alt="image-20200816012322071" style="zoom: 25%;" />



```java
//快速排序实现 调用方式 quickSort(a, 0, a.length - 1);
public static void quickSort(int[] array, int begin, int end) {
    if (end <= begin) return;
    int pivot = partition(array, begin, end);
    quickSort(array, begin, pivot - 1);
    quickSort(array, pivot + 1, end);
}

static int partition(int[] a, int begin, int end) {
    // pivot 标杆位置 counter 小于pivot的元素的个数
    int pivot = end, counter = begin;
    for (int i = begin; i < end; i++) {
        if (a[i] < a[pivot]) {
            int temp = a[counter]; a[counter] = a[i]; a[i] = temp;
            counter++;
        }
    }
    int temp = a[pivot]; a[pivot] = a[counter]; a[counter] = temp;
    return counter;
}
```

```java
//归并排序实现
public static void mergeSort(int[] array, int left, int right) {
	if (right <= left) return;
    int mid = (left + right) >> 1; // <==> (left + right)/2
    mergeSort(array, left, mid);
    mergeSort(array, mid + 1, right);
    merge(array, left, mid, right);
}

public static void merge(int[] arr, int left, int mid, int right) {
    int[] temp = new int[right - left + 1]; //中间数组
    int i = left, j = mid + 1, k = 0; //k表示temp数组已经填入元素的个数
    while (i <= mid && j <= right) {
		temp[k++] = arr[i] <= arr[j] ? arr[i++] : arr[j++];
    }
    
    while (i <= mid) temp[k++] = arr[i++];
    while (j <= right) temp[k++] = arr[j++];
    
    for (int p = 0; p < temp.length; p++) {
        arr[left + p] = temp[p];
    }
    // or use System.arraycopy(a, start1, b, start2, length);
}
```

```java
//堆排序实现
static void heapify(int[] array, int length, int i) {
    int left = 2 * i + 1, right = 2 * i + 2；
    int largest = i;
    if (left < length && array[left] > array[largest]) {
        largest = left;
    }
    if (right < length && array[right] > array[largest]) {
        largest = right;
    }
    if (largest != i) {
        int temp = array[i]; array[i] = array[largest]; array[largest] = temp;
        heapify(array, length, largest);
    }
}
public static void heapSort(int[] array) {
    if (array.length == 0) return;
    int length = array.length;
    for (int i = length / 2-1; i >= 0; i--) 
        heapify(array, length, i);
    for (int i = length - 1; i >= 0; i--) {
        int temp = array[0]; array[0] = array[i]; array[i] = temp;
        heapify(array, i, 0);
    }
}
```

##### 特殊排序

```

```

------

##### 用java实现初级排序

```java
//选择排序
public void selectSort(int[] arr) {
    int n = arr.length;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n - 1; j++) {
			if (arr[j + 1] < arr[j]) {
				swap(arr, j, j + 1);
            }
        }
    }
}

//插入排序
public void insertSort(int[] arr) {
    int n = arr.length;
    for (int i = 1; i < n; i++) {
        for (int j = i; j > 0; j--) {
			if (arr[j] < arr[j - 1]) {
				swap(arr, j, j - 1);
            }
        }
    }
}

//冒泡排序
public void bubbleSort(int[] arr) {
    int n = arr.length;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n - i - 1; j++) {
			if (arr[j] > arr[j + 1]) {
				swap(arr, j, j + 1);
            }
        }
    }
}

public static void swap(int[] arr, int i, int j) {
    int temp = 0;
    temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
}
```

