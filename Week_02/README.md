### 学习总结

#### Week02

#### 知识点总结

##### 哈希表、映射、集合的实现与特性

```
哈希表，也叫散列表，是通过把关键码值（key value）映射到表中的一个位置来访问记录，这种映射函数叫散列函数，存放记录的数组叫哈希表

查询、添加、删除时间复杂度O(1)，当出现链表的情况，时间复杂度会退化成O(n)
```

------

##### 树、二叉树、二叉搜索树的实现与特性

```
LinkedList 是特殊化的 Tree
Tree 是特殊化的 Graph

二叉树遍历
	前序（pre-order）：根-左-右
	中序（in-order）：左-根-右
	后序（post-order）：左-右-根

二叉搜索树
	是指一棵空树或者具有下列性质的二叉树：
		左子树上所有结点的值均小于它的根结点的值
		右子树上所有结点的值均大于它的根结点的值
		左、右子树也分别是二叉搜索树（重复性）
	中序遍历：升序排列
	查询、插入、删除时间复杂度 O(log(n))
```

------

##### 堆和二叉堆的实现与特性

```
Heap：可以迅速找到一堆数中的最大或者最小值的数据结构
大顶堆/大根堆 —— 根节点最大的堆
	find-max: O(1)
	delete-max: O(log(n))
	insert(create): O(log(n)) or O(1)
小顶堆/小根堆 —— 根节点最小的堆

二叉堆
	通过完全二叉树实现
		是一棵完全树
		树中任意节点的值总是>=其子节点的值
	一般通过“数组”实现
	假设第一个元素在数组中的索引为0的话，父节点和子节点位置关系如下：
		索引为i的左孩子的索引是(2*i+1)
		索引为i的右孩子的索引是(2*i+2)
		索引为i的父节点的索引是floor((i-1)/2)
	insert插入操作
		新元素一律先插入到堆的尾部
		依次向上调整整个堆的结构（一直到根即可）
	Delete Max 删除堆顶操作
		将堆尾元素替换到顶部（堆顶将被替换删除掉）
		依次从根部向下调整整个堆的结构（一直到堆尾即可）
	二叉堆是堆（优先队列priority_queue）的一种常见且简单的实现，但并不是最优的实现
```

```java
//insert 放入堆底，从下往上调整
public void insert(int x) {
	if (isFull()) {
        throw new NoSuchElementException("Heap is full, No space to insert new element");
    }
    heap[heapSize++] = x;
    heapifyUp(heapSize - 1);
}
private void heapifyUp(int i) {
    int insertValue = heap[i];
    while (i > 0 && insertValue > heap[parent(i)]) {
        heap[i] = heap[parent(i)];
        i = parent(i);
    }
    heap[i] = insertValue;
}

//删除堆顶元素
public int delete(int x) {
	if (isEmpty()) {
        throw new NoSuchElementException("Heap is empty, no element to delete");
    }
    int key = heap[x]; //堆顶元素
    heap[x] = heap[heapSize - 1];
    heapSize--;
    heapifyDown(x);
    return key;
}
private void heapifyDown(int i) {
    int child;
    int temp = heap[i];
    while (kthChild(i, 1) < heapSize) {
       	child = maxChild(i);
        if (temp >= heap[child]) {
            break;
        }
        heap[i] = heap[child];
        i = child;
    }
    heap[i] = temp;
}
//寻找两个子节点中强壮的节点返回
private int maxChild(int i) {
    int leftChild = kthChild(i, 1);
    int rightChild = kthChild(i, 2);
    return heap[leftChild] > heap[rightChild] ? leftChild : rightChild;
}
```

##### 图的实现与特性

```
图的属性
	Graph(V, E)
	V - vertex: 点
		入度和出度
		点与点之间：连通与否
	E - edge: 边
		有向和无向（单行线）
		权重（边长）
```

------

##### 关于HashMap的小总结

```
特点：
    1、存取无序
    2、键和值都可以为null
    3、 1.8前数据结构是 数组+链表
    	1.8后是 数组+链表+红黑树
    4、阈值 > 8并且数组长度 >= 64，才将链表转换成红黑树，变为红黑树的目的是为了更高效的查询

put实现的步骤：
	1、先通过hash值计算出key映射到哪个桶
	2、如果桶上没有碰撞冲突，则直接插入
	3、如果出现碰撞冲突了，则需要处理冲突
		如果该桶使用红黑树处理冲突，则调用红黑树的方法插入数据，否则采用传统的链式方法插入。如果链的长度达到临界值，则把链表转变		为红黑树
	4、如果桶中存在重复的键，则为该键替换新值value
	5、如果size大于阈值threshold，则进行扩容
	
putVal()
	1、根据哈希表中元素个数确定是扩容还是树形化
	2、如果是树形化，遍历桶中的元素，创建相同个数的树形节点，复制内容，建立起联系
	3、然后让桶中的第一个元素指向新创建的树根节点，替换桶的链表内容为树形化内容

扩容resize()
	HashMap在扩容时，因为每次扩容都是翻倍的，与原来计算的 (n - 1) & hash 的结果相比，只是多了一个bit位，所以节点要么就是在原来的位置，要么就被分配到“原位置+旧容量”这个位置。所以不需要重新计算hash，只需要看看原来的hash值新增的那个bit是1还是0，是0索引就没变，是1索引就变成“原索引+oldCap”。
	这样保证了rehash之后每个桶上的节点数一定小于等于原来桶上的节点数，保证了rehash之后不会出现更严重的hash冲突，均匀的把之前的冲突的节点分散到新的桶中。
	
get()
	1、通过hash值获取该key映射到的桶
	2、桶上的key就是要查找的key，则直接找到并返回
	3、桶上的key不是要找的key，则查看后续的节点
		如果后续节点是红黑树节点，则通过调用红黑树的方法根据key获取value
		如果后续节点是链表节点，则通过循环遍历根据key获取value
```



```java
JAVA8
  
public class HashMap<K,V> extends AbstractMap<K,V>
    implements Map<K,V>, Cloneable, Serializable {
    
    static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16
    
    static final int MAXIMUM_CAPACITY = 1 << 30;//集合最大容量
    
    static final float DEFAULT_LOAD_FACTOR = 0.75f;//负载因子
    
    static final int TREEIFY_THRESHOLD = 8;//当桶上的结点数大于这个值时会转换为红黑树
    
    static final int UNTREEIFY_THRESHOLD = 6;//当桶上的结点数小于这个值时，树转链表
    
    static final int MIN_TREEIFY_CAPACITY = 64;//桶中结构转化为红黑树对应的数组长度的最小值，不满足时扩容
    
    transient Node<K,V>[] table;//桶
    
    transient Set<Map.Entry<K,V>> entrySet;//存放具体元素的集合
    
    transient int size;//默认0，存放元素的个数，不是table的长度
    
    transient int modCount;//每次扩容和更改map结构的计数器
    
    int threshold;//阈值，当实际大小（容量 * 负载因子）超过临界值时，会进行扩容
    
    final float loadFactor;//负载因子，默认0.75，作用：衡量HashMap满的程度 = size/capacity
    
    final Node<K,V> getNode(int hash, Object key) {
        Node<K,V>[] tab; Node<K,V> first, e; int n; K k;
        if ((tab = table) != null && (n = tab.length) > 0 &&
            (first = tab[(n - 1) & hash]) != null) {
            if (first.hash == hash && // always check first node
                ((k = first.key) == key || (key != null && key.equals(k))))
                return first;
            if ((e = first.next) != null) {
                if (first instanceof TreeNode)
                    return ((TreeNode<K,V>)first).getTreeNode(hash, key);
                do {
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        return e;
                } while ((e = e.next) != null);
            }
        }
        return null;
    }
    
    public V put(K key, V value) {
        return putVal(hash(key), key, value, false, true);
    }
    
    /**
    * @param hash key的hash值
    * @param key 原始的key值
    * @param value 要存放的值
    * @param onlyIfAbsent 如果为true，代表不更改现有的值
    * @param evict 如果为false表示table为创建状态
    */
    final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        Node<K,V>[] tab; Node<K,V> p; int n, i;
        if ((tab = table) == null || (n = tab.length) == 0)
            n = (tab = resize()).length;
        if ((p = tab[i = (n - 1) & hash]) == null)
            tab[i] = newNode(hash, key, value, null);
        else {
            Node<K,V> e; K k;
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                e = p;
            else if (p instanceof TreeNode)
                e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
            else {
                for (int binCount = 0; ; ++binCount) {
                    if ((e = p.next) == null) {
                        p.next = newNode(hash, key, value, null);
                        if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                            treeifyBin(tab, hash);
                        break;
                    }
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        break;
                    p = e;
                }
            }
            if (e != null) { // existing mapping for key
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;
                afterNodeAccess(e);
                return oldValue;
            }
        }
        ++modCount;
        if (++size > threshold)
            resize();
        afterNodeInsertion(evict);
        return null;
    }
    
    final Node<K,V>[] resize() {
        Node<K,V>[] oldTab = table;
        int oldCap = (oldTab == null) ? 0 : oldTab.length;
        int oldThr = threshold;
        int newCap, newThr = 0;
        if (oldCap > 0) {
            if (oldCap >= MAXIMUM_CAPACITY) {
                threshold = Integer.MAX_VALUE;
                return oldTab;
            }
            else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                     oldCap >= DEFAULT_INITIAL_CAPACITY)
                newThr = oldThr << 1; // double threshold
        }
        else if (oldThr > 0) // initial capacity was placed in threshold
            newCap = oldThr;
        else {               // zero initial threshold signifies using defaults
            newCap = DEFAULT_INITIAL_CAPACITY;
            newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
        }
        if (newThr == 0) {
            float ft = (float)newCap * loadFactor;
            newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                      (int)ft : Integer.MAX_VALUE);
        }
        threshold = newThr;
        @SuppressWarnings({"rawtypes","unchecked"})
        Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];
        table = newTab;
        if (oldTab != null) {
            for (int j = 0; j < oldCap; ++j) {
                Node<K,V> e;
                if ((e = oldTab[j]) != null) {
                    oldTab[j] = null;
                    if (e.next == null)
                        newTab[e.hash & (newCap - 1)] = e;
                    else if (e instanceof TreeNode)
                        ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
                    else { // preserve order
                        Node<K,V> loHead = null, loTail = null;
                        Node<K,V> hiHead = null, hiTail = null;
                        Node<K,V> next;
                        do {
                            next = e.next;
                            if ((e.hash & oldCap) == 0) {
                                if (loTail == null)
                                    loHead = e;
                                else
                                    loTail.next = e;
                                loTail = e;
                            }
                            else {
                                if (hiTail == null)
                                    hiHead = e;
                                else
                                    hiTail.next = e;
                                hiTail = e;
                            }
                        } while ((e = next) != null);
                        if (loTail != null) {
                            loTail.next = null;
                            newTab[j] = loHead;
                        }
                        if (hiTail != null) {
                            hiTail.next = null;
                            newTab[j + oldCap] = hiHead;
                        }
                    }
                }
            }
        }
        return newTab;
    }
    
    //最近练习常用到的一个方法，特意做一下摘录
    @Override
    public V getOrDefault(Object key, V defaultValue) {
        Node<K,V> e;
        return (e = getNode(hash(key), key)) == null ? defaultValue : e.value;
    }
}
```

------

##### 日常总结

1、继续严格要求自己按照五毒神掌进行练习

2、经过第二周的学习，越发感觉到刻意练习的好处，而且自己现在处于工作和生活都占用很大精力的阶段，所以碎片化时间的利用在技能提升的过程中显得尤为重要。同时，重复重复再重复更能帮助自己克服艾宾浩斯遗忘规律，继续努力。

