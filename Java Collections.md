# java 集合

[TOC]

https://www.zhihu.com/question/19851109

##  java - fail fast

fail-fast产生的原因：



fail fast会抛出ConcurrentModificationException异常。

用CopyOnWriterArrayList替代ArrayList。CopyOnWriterArrayList是ArrayList的一个线程安全的变体，所有可变操作add,set等都是通过对底层数组进行一次新的复制来实现的。



## equals()

​        所有对象都拥有内存地址和数据，“＝＝”比较的是两个对象的内存地址，使用Object的equals()方法比较两个对象的内存地址是否相等，object1.equals(object2)为true，表示的是object1和object2实际引用的是同一个对象。



## transient & volitile

```xml
The volatile and transient modifiers apply to fields of classes.

The transient modifier tells the Java object serialization subsystem to exclude the field when serializing an instance of the class. When the object is then deserialized, the field will be initialized to the default value; i.e. null for a reference type, and zero or false for a primitive type. Note that the JLS (see 8.3.1.3) does not say what transient means, but defers to the Java Object Serialization Specification. Other non-standard serialization mechanisms may also pay attention to a field's transient-ness.

The volatile modifier tells the JVM that writes to the field should always be synchronously flushed to memory, and that reads of the field should always read from memory. This means that fields marked as volatile can be safely accessed and updated in a multi-thread application without using native or standard library-based synchronization. Similarly, reads and writes to volatile fields are atomic. (This does not apply to >>non-volatile<< long or double fields, which may be subject to "word tearing" on some JVMs.) The relevant parts of the JLS are 8.3.1.4, 17.4 and 17.7.
```

## ArrayList

* ArrayList是基于**数组**的实现。

* ArrayList的主要性能小号来源于扩容和固定位置的增删。

  原因：

  ```java
  public void add(int index, E element) {
          rangeCheckForAdd(index);
          ensureCapacityInternal(size + 1);  // Increments modCount!!
          System.arraycopy(elementData, index, elementData, index + 1,size - index);
          elementData[index] = element;
          size++;
      }
  ```

  `elementData = Arrays.copyOf(elementData, newCapacity);` 需要知道的是， Arrays.copyOf 函数的内部实现是再创建一个数组，然后把旧的数组的值一个个复制到新数组中。**当经常增加操作的时候，容量不够的时候，就会进行上述的扩容操作，这样性能自然就下来了。** 或者说，当我们在固定位置进行增删的时候，都会进行 `System.arraycopy(elementData, index, elementData, index + 1, size - index);` 也是非常低效的。

* ArrayList创建时需要考虑是否初始化最小容量，避免扩容带来的性能消耗。

```java
ArrayList<Integer> arrayList = new ArrayList<Integer>(20);
```

* 不是线程安全的
* 实现了List接口，因而可插入空值，也可插入重复的值。

## Vector

* 是线程安全的，实现了List接口，可以插入空值，也可以插入重复的值。和HashTable一样，属于同步容器，而不是并发容器。
* **相当于 ArrayList 的线程安全版本，实现同步的方式 是通过 synchronized。**
* 性能消耗也主要来源于 扩容。
* 创建的时候 需要考虑是否要初始化最小容量，以此避免扩容带来的消耗。

## LinkedList

* 是链表的数据结构





## Others

### CAS

compare and swap.

java中可以通过锁和循环CAS的方式实现原子操作。

Java内部实现的锁机制有偏向锁，轻量级锁和互斥锁。