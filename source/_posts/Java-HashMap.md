---
title: Java-HashMap
date: 2019-06-27 15:56:21
tags: ["java"]
---

```Java
// 默认数组容量16（必须是2幂次方）
static final int DEFAULT_INITIAL_CAPACITY = 1 << 4;

// 数组最大容量，到达后不在扩大（必须是2幂次方）
static final int MAXIMUM_CAPACITY = 1 << 30;

// 默认加载因子，当数组已经使用到 当前容量 * 加载因子时发生扩容
static final float DEFAULT_LOAD_FACTOR = 0.75f;

// 树形化阈值。当链表的节点个数大于等于这个值时，会将链表转化为树。
static final int TREEIFY_THRESHOLD = 8;

// 解除树形化阈值。当链表的节点个数小于等于这个值时，会将红黑树转换成普通的链表。
static final int UNTREEIFY_THRESHOLD = 6;

// 树形化阈值的第二条件。当数组的长度小于这个值时，就算树形化阈达标，链表也不会转化为红黑树，而是优先扩容数组resize。
static final int MIN_TREEIFY_CAPACITY = 64;
```

**条目定义`Map.Entry`**
```Java
interface Entry<K, V> {
        // 获取条目的Key 
        K getKey();

        // 获取条目的值
        V getValue();
        
        // 设置条目的值
        V setValue(V value);

        // 比较两个条目是不是相等
        boolean equals(Object o);

        // 获取条目的hash code
        int hashCode();        
```
**节点定义,实现了Entry接口`HashMap.Node`，并多了一个指向下一个节点的next成员变量**
``` Java
static class Node<K,V> implements Map.Entry<K,V> {
        final int hash;
        final K key;
        V value;
        Node<K,V> next;

        Node(int hash, K key, V value, Node<K,V> next) {
            this.hash = hash;
            this.key = key;
            this.value = value;
            this.next = next;
        }

        public final K getKey()        { return key; }
        public final V getValue()      { return value; }
        public final String toString() { return key + "=" + value; }

        public final int hashCode() {
            return Objects.hashCode(key) ^ Objects.hashCode(value);
        }

        public final V setValue(V newValue) {
            V oldValue = value;
            value = newValue;
            return oldValue;
        }

        public final boolean equals(Object o) {
            if (o == this)
                return true;
            if (o instanceof Map.Entry) {
                Map.Entry<?,?> e = (Map.Entry<?,?>)o;
                if (Objects.equals(key, e.getKey()) &&
                    Objects.equals(value, e.getValue()))
                    return true;
            }
            return false;
        }
    }
}
```

**HashMap内部数组**
```Java
transient Node<K,V> table;
```

代码分析

1. 构造方法
```Java
/**
 * 如果我们使用这个构造方法，所有的配置都是默认值
 */
public HashMap() {
    this.loadFactor = DEFAULT_LOAD_FACTOR;
}

/**
 * 可以设置Node table的初始大小（必须大于0且超过最大值默认设置最大值）
 */
public HashMap(int initialCapacity) {
        this(initialCapacity, DEFAULT_LOAD_FACTOR);
}

/**
 * 可以设置Node table的初始大小（必须大于0且超过最大值默认设置最大值），加载因子
 */
 public HashMap(int initialCapacity, float loadFactor) {
        if (initialCapacity < 0)
            throw new IllegalArgumentException("Illegal initial capacity: " +
                                               initialCapacity);
        if (initialCapacity > MAXIMUM_CAPACITY)
            initialCapacity = MAXIMUM_CAPACITY;
        if (loadFactor <= 0 || Float.isNaN(loadFactor))
            throw new IllegalArgumentException("Illegal load factor: " +
                                               loadFactor);
        this.loadFactor = loadFactor;

        // 这个方法可以将任意一个整数转换成2的次方。
        // 例如输入10，则会返回16。
        // 初始化的时候扩容阀值并非 数组容量 * loadFactor计算得到，而是直接用当前初始化大小
        // 然后第一次put操作，扩充数组时，会将这个threshold作为数组容量，然后再重新计算这个值。
        this.threshold = tableSizeFor(initialCapacity);
}
```

2. 扩容方法（变更table的长度）
```Java
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