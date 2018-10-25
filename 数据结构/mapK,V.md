# map<K,V>

## map接口

公用的接口方法有

```java
int size();//得到map的容量大小
boolean isEmpty();//判断map 是否为空
boolean containsKey(Object key);//判断map是否包含了Key
boolean containsValue(Object value);//判断map是否包含了value
V get(Object key);//取值
V put(K key, V value);//放值
V remove(Object key);//移除某个key
void putAll(Map<? extends K, ? extends V> m);//放进来一个map
void clear();//清空map
Set<K> keySet();//得到key集合
Collection<V> values();//得到values集合
Set<Map.Entry<K, V>> entrySet();//得到key,value集合
K getKey();//得到key
V getValue();//得到value
V setValue(V value);//设置value
boolean equals(Object o);//比较函数
int hashCode();//返回hashcode
default V getOrDefault(Object key, V defaultValue) {
        V v;
        return (((v = get(key)) != null) || containsKey(key))
            ? v
            : defaultValue;
    }//去key对应的值，不存在就返回默认value
default V putIfAbsent(K key, V value) {
        V v = get(key);
        if (v == null) {
            v = put(key, value);
        }

        return v;
    }//如果不存在则添加

 default boolean remove(Object key, Object value) {
        Object curValue = get(key);
        if (!Objects.equals(curValue, value) ||
            (curValue == null && !containsKey(key))) {
            return false;
        }
        remove(key);
        return true;
    }//删除key,value
default boolean replace(K key, V oldValue, V newValue) {
        Object curValue = get(key);
        if (!Objects.equals(curValue, oldValue) ||
            (curValue == null && !containsKey(key))) {
            return false;
        }
        put(key, newValue);
        return true;
    }//如果就值相同替换key 的值
default V replace(K key, V value) {
        V curValue;
        if (((curValue = get(key)) != null) || containsKey(key)) {
            curValue = put(key, value);
        }
        return curValue;
    }//直接替换key的值
```

## HashMap

继承结构

![1531708955808](E:\mydairy\数据结构\HashMap.png)

默认参数

```java
static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // 默认初始容量
static final int MAXIMUM_CAPACITY = 1 << 30;//最大容量
static final float DEFAULT_LOAD_FACTOR = 0.75f;//装载因子
static final int TREEIFY_THRESHOLD = 8;//链表长度门槛超过该值会树化
static final int UNTREEIFY_THRESHOLD = 6;//树化结构节点少于该值转换成链表结构
static final int MIN_TREEIFY_CAPACITY = 64;//当桶中的bin被树化时最小的hash表容量。（如果没有达到这个阈值，即hash表容量小于MIN_TREEIFY_CAPACITY，当桶中bin的数量太多时会执行resize扩容操作）这个MIN_TREEIFY_CAPACITY的值至少是TREEIFY_THRESHOLD的4倍

```

HashMap扩容

```java
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
```

当table为空或是0时容量赋值为默认容量DEFAULT_INITIAL_CAPACITY，否则double原来的旧值

Hashmap 的put全过程

```JAVA
public V put(K key, V value) {
        return putVal(hash(key), key, value, false, true);
    }
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
```

首先判断容量大小是否为0，否则进行扩容，通过key的hash值与容量n-1做与运算，得到对应HashMap的具体table位置，判断该table首个元素是否为null,如果为null直接插入，否则判断该节点的key是否和插入的key 一样，一样基于直接替换value,否则就先判断这个节点结构对应的是链表还是树结构，如果是链表结构，遍历链表查看是否有相同的key值，如果有直接替换，直到末尾直接插入该值，否则就是树结构，插入树节点中。

Hashmap 的get全过程

同上，先找到对应的table,然后判断是树结构还是链表结构，然后遍历，找到就返回value,否则就返回null

## TreeMap

结构

![1531714776024](E:\mydairy\数据结构\TreeMap.png)

内部使用红黑树的结构来存储，可以对map的key进行排序，传入Comparator接口即可

