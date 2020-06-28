### 学习总结

#### Week01

#### 知识点总结

##### 掌握五毒神掌

##### 掌握LeetCode刷题攻略

```
1、5-10分钟：读题+思考
2、有思路：自己开始做和写代码；不然，马上看题解
3、默写背诵、熟练
4、然后开始自己写（闭卷）
```

##### 时间复杂度和空间复杂度分析

```
时间复杂度
	O(1) 常数复杂度
	O(log n) 对数复杂度
	O(n) 线性时间复杂度
	O(n^2) 平方
	O(n^3) 立方
	O(2^n) 指数
	O(n!) 阶乘
	注意：只看最高复杂度的运算，且不考虑系数
```

主定理

<img src="/Users/miwang/Library/Application Support/typora-user-images/image-20200627174849635.png" alt="image-20200627174849635" style="zoom:50%;" />

```
空间复杂度
	数组的长度
	递归的深度
```

------

##### 数组、链表、跳表实现

```
数组时间复杂度
	prepend  O(1) //申请稍大一些的内存空间，prepend操作是把头下标前移一个位置
	append   O(1)
	lookup   O(1)
	insert   O(n)
	delete   O(n)
链表时间复杂度
	prepend  O(1)
	append   O(1)
	lookup   O(n)
	insert   O(1)
	delete   O(1)
跳表
	只能用于元素有序的情况，跳表对标的是平衡树和二分查找
	假设索引有h级，最高级的索引有两个结点，n/(2^h)=2，h=log2(n)-1
	跳表时间复杂度
        prepend  O(log n)
        append   O(log n)
        lookup   O(log n)
        insert   O(log n)
        delete   O(log n)
    跳表空间复杂度
    	O(n)
```

------

##### 栈、队列、双端队列、优先队列

```
栈：先进后出 FILO，添加、删除皆为O(1)，查O(n)
队列：先进先出 FIFO，添加、删除皆为O(1)，查O(n)
双端队列：
	两端可以进出的队列 Deque - double ended queue
	插入、删除都是O(1)，查O(n)
优先队列：
	插入：O(1)
	查找：O(log n) 按照元素的优先级取出
	底层具体实现的数据结构较为多样和复杂：heap、bst、treap
```

------

##### 日常总结

1、通过部分题目题解的学习，发现大部分优秀的代码都是从暴力解法中逐步优化，通过升维或者空间换时间等方式对代码进行一步步优化，虽然自己平常工作中写代码也是这样子，但是真正运用到算法练习中还是不熟练，需要多加练习

2、在个别题目的解答过程中，经常是在自己有了一点点思路后，就不能冷静的分析题目的要求。导致费时完成的代码并不是题目的正解，虽然有了思考和动手的过程，但是在有限的时间，总犯这个错误也是需要反思的

3、通过总结也希望自己能继续坚持，认真完成每一周的学习和修炼

------

##### 用 add first 或 add last 这套新的 API 改写 Deque 的代码

```java
Deque<String> deque = new LinkedList<>();

deque.addLast("a");
deque.addLast("b");
deque.addLast("c");
System.out.println(deque);

String str = deque.getFirst();
System.out.println(str);
System.out.println(deque);

while (deque.size() > 0) {
    System.out.println(deque.removeFirst());
}
System.out.println(deque);
```

##### 分析 Queue 和 Priority Queue 的源码

```java
JAVA8
public interface Queue<E> extends Collection<E> {
    //尾部插入，越界抛出异常
	boolean add(E e);
    //尾部插入，越界返回false
    boolean offer(E e);
    //移除第一个元素；如果队列为空，抛出NoSuchElementException
    E remove();
    //移除第一个元素；如果队列为空，返回null
    E poll();
    //检索第一个元素，但不移除；如果队列为空，抛出NoSuchElementException
    E element();
	//检索第一个元素，但不移除；如果队列为空，返回null
    E peek();
}
    
    
public class PriorityQueue<E> extends AbstractQueue<E>
    implements java.io.Serializable {
    
    //默认初始容量11
    private static final int DEFAULT_INITIAL_CAPACITY = 11;
    //自然排序
    private final Comparator<? super E> comparator;
    //构造函数，初始容量+按照指定的比较器排列
    public PriorityQueue(Comparator<? super E> comparator) {
        this(DEFAULT_INITIAL_CAPACITY, comparator);
    }
    //构造函数，指定容量+按照指定的比较器排列
    public PriorityQueue(int initialCapacity,
                         Comparator<? super E> comparator) {
        // Note: This restriction of at least one is not actually needed,
        // but continues for 1.5 compatibility
        if (initialCapacity < 1)
            throw new IllegalArgumentException();
        this.queue = new Object[initialCapacity];
        this.comparator = comparator;
    }
    
    private void initFromCollection(Collection<? extends E> c) {
        initElementsFromCollection(c);//提取Collection中的元素
        heapify();//调整PriorityQueue
    }
    
    private void heapify() {
        //从底往上，对每个有自节点对元素进行下沉操作。
        for (int i = (size >>> 1) - 1; i >= 0; i--)
            siftDown(i, (E) queue[i]);
    }
    
    private void siftDown(int k, E x) {
        if (comparator != null)
            siftDownUsingComparator(k, x);
        else
            siftDownComparable(k, x);
    }

    @SuppressWarnings("unchecked")
    private void siftDownComparable(int k, E x) {
        Comparable<? super E> key = (Comparable<? super E>)x;
        int half = size >>> 1;        // loop while a non-leaf
        while (k < half) {
            int child = (k << 1) + 1; // assume left child is least
            Object c = queue[child];
            int right = child + 1;
            if (right < size &&
                ((Comparable<? super E>) c).compareTo((E) queue[right]) > 0)
                c = queue[child = right];
            if (key.compareTo((E) c) <= 0)
                break;
            queue[k] = c;
            k = child;
        }
        queue[k] = key;
    }

    @SuppressWarnings("unchecked")
    private void siftDownUsingComparator(int k, E x) {
        int half = size >>> 1;
        while (k < half) {
            int child = (k << 1) + 1;
            Object c = queue[child];
            int right = child + 1;
            if (right < size &&
                comparator.compare((E) c, (E) queue[right]) > 0)
                c = queue[child = right];
            if (comparator.compare(x, (E) c) <= 0)
                break;
            queue[k] = c;
            k = child;
        }
        queue[k] = x;
    }
    
    public boolean add(E e) {
        return offer(e);
    }
    
    public boolean offer(E e) {
        if (e == null)
            throw new NullPointerException();
        modCount++;
        int i = size;
        if (i >= queue.length)
            //调用grow进行扩容
            grow(i + 1);
        size = i + 1;
        if (i == 0)
            //如果此前size为0，则说明原本没有element，所以新插入的element会放在顶堆，不需要调整。
            queue[0] = e;
        else
            //否则调用sifitUp()方法，将新element插入queue[i]，然后调整PriorityQueue，上浮插入元素。
            siftUp(i, e);
        return true;
    }
    
    private void siftUp(int k, E x) {
        if (comparator != null)
            //如果comparator不为空，则使用comparator进行调整
            siftUpUsingComparator(k, x);
        else
            //否则用element实现的comparable接口进行调整
            siftUpComparable(k, x);
    }

    @SuppressWarnings("unchecked")
    private void siftUpComparable(int k, E x) {//k为PriorityQueue的尾端
        Comparable<? super E> key = (Comparable<? super E>) x;
        while (k > 0) {
            //计算当前插入位置父节点的下标
            int parent = (k - 1) >>> 1;
            Object e = queue[parent];
            //如果父节点小于插入节点，则停止上浮
            if (key.compareTo((E) e) >= 0)
                break;
            //否则将父节点放入queue[k]，并将k赋值为父节点的下标，继续上浮
            queue[k] = e;
            k = parent;
        }
        //退出循环后，将新的element插入到得到的queue[k]的位置。
        queue[k] = key;
    }

    @SuppressWarnings("unchecked")
    private void siftUpUsingComparator(int k, E x) {
        while (k > 0) {
            int parent = (k - 1) >>> 1;
            Object e = queue[parent];
            if (comparator.compare(x, (E) e) >= 0)
                break;
            queue[k] = e;
            k = parent;
        }
        queue[k] = x;
    }
    
}
```

------

