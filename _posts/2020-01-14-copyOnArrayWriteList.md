---
layout: post
category: java
title: 想去大公司，你知道Java中的CopyOnWriteArrayList吗？
tagline: by 兔子托尼啊
tag: java
---

### CopyOnWrite
-  CopyOnWrite是什么？
-  CopyOnWriteArrayList源码分享？
-  CopyOnWriteArrayList使用场景？
-  CopyOnWriteArrayList有什么优缺点？

如果你是求职者，你想想看怎么回答上面的问题？


#### 缘由
前段时间面试好多个人，问是否用过```CopyOnWriteList```,发现好多人都没有用过，感觉挺惊讶的。

```CopyOnWrite```看字面意思大概就可以明白了,copy集合之后再进行write操作，我们也称这个为**写时复制**容器。


这个从 JDK 1.5版本就已经有了，Java并发包中有两个实现这个机制的容器，分别是```CopyOnWriteArrayList```和```CopyOnWriteArraySet```。


CopyOnWrite这个容器非常有用，特别是在并发的时候能够提升效率，很多并发的的场景中都可以用到```CopyOnWrite```的容器，我们在生产环境也用到过，今天托尼就和大家顺便讲讲这个容器。

### CopyOnWrite是什么

官方解释
CopyOnWriteArrayList 是ArrayList的线程安全方式的一种方式。它的add、set方法底层实现都是重新复制一个新的容器来操作的。

CopyOnWriteArrayList 与ArrayList不同之处在于添加元素的时候会加上锁。

CopyOnWriteArrayList在修改容器元素的时候并不是直接在原来的数组上进行修改，它是先拷贝一份，然后在拷贝的数组上进行修改，在修改完成之后将引用赋给原来的数组。

###  CopyOnWriteArrayList源码分享
```java
public class CopyOnWriteArrayList<E>
    implements List<E>, RandomAccess, Cloneable, java.io.Serializable {}
```

- 实现了List接口，List的一种实现方式
- 实现RandomAccess接口，看名称就知道随机访问，和数组访问一样根据下标
- 实现Cloneable接口，代表可以克隆
- 实现了Serializable接口接口，代表可以被序列化



当容器被初始化添加元素成功之后，多个线程读取容器中的元素，如果此刻没有元素的添加，并发多个线程读取出来的数据大家都是一样的，可以理解为线程安全的 。

如果此刻有个线程往容器中添加一个新的元素，这个时候CopyOnWriteArrayList就会拷贝一个新的数组出来，将新的元素添加到新的数组中。

在添加元素的这段时间里，如果多线程访问容器中的元素，将会读取到旧的数据，等添加元素成功之后会将新的引用地址赋值给旧的list引用地址。

代码分享：
-  add 方法

```java
public boolean add(E e) {
     //加锁操作
    final ReentrantLock lock = this.lock;
    lock.lock();
    try {
        //获取原数组
        Object[] elements = getArray();
        int len = elements.length;
        // 复制一个新数组
        Object[] newElements = Arrays.copyOf(elements, len + 1);
        // 将新的元素添加到新数组里面
       newElements[len] = e;
        // 将原数组的引用指向新数组
        setArray(newElements);
        return true;
    } finally {
        lock.unlock();
    }
}
```

大家要注意上面的代码中```ReentrantLock```，在添加新元素的时候有加锁操作，多线程的情况下防止产生脏数据。

- get方法

```
public E get(int index) {
    return get(getArray(), index);
}
```

读的时候不会加锁，写的时候会加上锁，这个时候如果多线程正好写数据，读取的时候还是会读取到旧的数据。

-  set方法
 
```java
 public E set(int index, E element) {
    //加锁
    final ReentrantLock lock = this.lock;
    lock.lock();
    try {
        //获取原来数组
        Object[] elements = getArray();
        // 通过索引获取原来的地址
        E oldValue = get(elements, index);
        // 判断新旧两个值是否相等
        if (oldValue != element) {
            int len = elements.length;
            // 拷贝新的数组
            Object[] newElements = Arrays.copyOf(elements, len);
            //根据索引修改元素
            newElements[index] = element;
            // 将原数组的引用指向新数组
            setArray(newElements);
        } else {
            // Not quite a no-op; ensures volatile write semantics
            //为了确保 voliatile 的语义，所以尽管写操作没有改变数据，还是调用set方法
            setArray(elements);
        }
        return oldValue;
    } finally {
        lock.unlock();
    }
}
```

- remove方法

```java

public E remove(int index) {  
    final ReentrantLock lock = this.lock;  
    lock.lock();  
    try {  
        Object[] elements = getArray();  
        int len = elements.length;  
        E oldValue = get(elements, index);  
        int numMoved = len - index - 1;  
        if (numMoved == 0)  
            setArray(Arrays.copyOf(elements, len - 1));  
        else {  
            Object[] newElements = new Object[len - 1];  
            System.arraycopy(elements, 0, newElements, 0, index);  
            System.arraycopy(elements, index + 1, newElements, index,  
                             numMoved);  
            setArray(newElements);  
        }  
        return oldValue;  
    } finally {  
        lock.unlock();  
    }  
}  
```

同样也很简单，都是使用 System.arraycopy、Arrays.copyOf移动元素进行元素的删除操作。


- CopyOnWriteArrayList迭代
  
  
  
针对iterator使用了一个叫COWIterator的迭代器，专门针对```CopyOnWrite```的迭代器，因为不支持写操作，如上面add、set、remove都会抛出异常，都是不支持的。

```java
/**
 * Not supported. Always throws UnsupportedOperationException.
 * @throws UnsupportedOperationException always; {@code remove}
 *         is not supported by this iterator.
 */
public void remove() {
    throw new UnsupportedOperationException();
}

/**
 * Not supported. Always throws UnsupportedOperationException.
 * @throws UnsupportedOperationException always; {@code set}
 *         is not supported by this iterator.
 */
public void set(E e) {
    throw new UnsupportedOperationException();
}

/**
 * Not supported. Always throws UnsupportedOperationException.
 * @throws UnsupportedOperationException always; {@code add}
 *         is not supported by this iterator.
 */
public void add(E e) {
    throw new UnsupportedOperationException();
}

```

举个例子

```java
CopyOnWriteArrayList<String> list = new CopyOnWriteArrayList<String>();
list.add("a");
list.add("b");
list.add("c");
 
Iterator<String> iterator = list.iterator();
while(iterator.hasNext()) {
    String next = iterator.next();
       // 这句会报错的⚠️
    iterator.remove();
}
 
Exception in thread "main" java.lang.UnsupportedOperationException
    at java.util.concurrent.CopyOnWriteArrayList$COWIterator.remove(CopyOnWriteArrayList.java:1178)
```

也正好验证了迭代的时候```UnsupportedOperationException```异常。

### CopyOnWriteArrayList使用场景
从上面的代码我们可以看出来了，适用于多读少写的场景，比如电商的商品列表，添加新商品和读取商品就可以用，其他场景小伙伴们可以想想看。

### CopyOnWriteArrayList有什么优缺点

缺点：

1、***内存占用***，因为**写时复制**的原理，所以在添加新元素的时候会复制一份，此刻内存中就会有两份对象，比如这个时候有200M，你在复制一份400M，那么此刻会产生频繁的JVM的Yong GC和Full GC，
严重的会进行```STW```

>Java中Stop-The-World机制简称STW,是在执行垃圾收集算法时,Java应用程序的其他所有线程都被挂起(除了垃圾收集帮助器之外)。

2、***数据一致性问题***，因为**CopyOnWrite**容器只能保证最终的数据一致性，并不能保证数据的实时性，也就是不具备原子性的效果。

3、***数据修改***，随着数组的元素越来越多，修改的时候拷贝数组将会越来越耗时。

优点：

1、***多读少写***，很多时候我们的系统应对的都是读多写少的并发场景，读操作是无锁操作所以性能较高。

#### 最后说说

- Vector 
- ArrayList 
- CopyOnWriteArrayList

这三个集合类都继承List接口

1、ArrayList是线程不安全的

2、Vector是比较古老的线程安全的，但性能不行

3、CopyOnWriteArrayList在兼顾了线程安全的同时，又提高了并发性，性能比Vector要高

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200108163039300.jpg)

 