# set

结构

![1531714938849](E:\mydairy\数据结构\set.png)

先说明Iterable接口

迭代器，表示实现该接口的类是可迭代的，该接口有一个定义迭代接口规范的接口Iterator，有如下方法：

```java
boolean hasNext();
E next();
default void remove() {
        throw new UnsupportedOperationException("remove");
    }
```

对于Collection接口

```java
int size();
boolean isEmpty();
boolean contains(Object o);
Iterator<E> iterator();
Object[] toArray();
<T> T[] toArray(T[] a);
boolean add(E e);
boolean remove(Object o);
boolean containsAll(Collection<?> c);
boolean addAll(Collection<? extends E> c);
boolean removeAll(Collection<?> c);
default boolean removeIf(Predicate<? super E> filter) {
        Objects.requireNonNull(filter);
        boolean removed = false;
        final Iterator<E> each = iterator();
        while (each.hasNext()) {
            if (filter.test(each.next())) {
                each.remove();
                removed = true;
            }
        }
        return removed;
    }
boolean retainAll(Collection<?> c);
void clear();
boolean equals(Object o);
int hashCode();

```

set接口

```java
int size();
boolean isEmpty();
boolean contains(Object o);
Iterator<E> iterator();
Object[] toArray();
<T> T[] toArray(T[] a);
boolean add(E e);
boolean remove(Object o);
boolean containsAll(Collection<?> c);
boolean addAll(Collection<? extends E> c);
boolean retainAll(Collection<?> c);
boolean removeAll(Collection<?> c);
void clear();
boolean equals(Object o);
int hashCode();
```

set的特性 **不能存储相同的元素。** 每一个值唯一，允许为空也只允许一个

## HashSet

以hashmap的方法存储set中的元素，元素仍然唯一，查找速度O（1）;

## TreeSet

以红黑树形结构存储set中的元素，可以实现自定义排序





