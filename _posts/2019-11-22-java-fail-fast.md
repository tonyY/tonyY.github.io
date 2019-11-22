---
layout: post
category: java
title: Java，你告诉我 fail-fast 是什么鬼？
tagline: by 沉默王二
tag: java
---

本篇我们来聊聊 Java 的 fail-fast 机制，文字一如既往的有趣哦。

<!--more-->




### 01、前言

说起来真特么惭愧：十年 IT 老兵，Java 菜鸟一枚。今天我才了解到 Java 还有 fail-fast 一说。不得不感慨啊，学习真的是没有止境。只要肯学，就会有巨多巨多别人眼中的“旧”知识涌现出来，并且在我这全是新的。

能怎么办呢？除了羞愧，就只能赶紧全身心地投入学习，把这些知识掌握。

为了镇楼，必须搬一段英文来解释一下 fail-fast。

>In systems design, a fail-fast system is one which immediately reports at its interface any condition that is likely to indicate a failure. Fail-fast systems are usually designed to stop normal operation rather than attempt to continue a possibly flawed process. Such designs often check the system's state at several points in an operation, so any failures can be detected early. The responsibility of a fail-fast module is detecting errors, then letting the next-highest level of the system handle them.

大家不嫌弃的话，我就用蹩脚的英语能力翻译一下。某场战役当中，政委发现司令员在乱指挥的话，就立马报告给权限更高的中央军委——这样可以有效地避免更严重的后果出现。当然了，如果司令员是李云龙的话，报告也没啥用。

不过，Java 的世界里不存在李云龙。fail-fast 扮演的就是政委的角色，一旦报告给上级，后面的行动就别想执行。

怎么和代码关联起来呢？看下面这段代码。

```java
public void test(Wanger wanger) {	
	if (wanger == null) {
		throw new RuntimeException("wanger 不能为空");
	}
	
	System.out.println(wanger.toString());
}
```

一旦检测到 wanger 为 null，就立马抛出异常，让调用者来决定这种情况下该怎么处理，下一步 `wanger.toString()` 就不会执行了——避免更严重的错误出现，这段代码由于太过简单，体现不出来，后面会讲到。

瞧，fail-fast 就是这个鬼，没什么神秘的。如果大家源码看得比较多的话，这种例子多得就像旅游高峰期的人头。

然后呢，没了？三秒钟，别着急，我们继续。

### 02、for each 中集合的 remove 操作

很长一段时间里，[我](https://mp.weixin.qq.com/s/feoOINGSyivBO8Z1gaQVOA)都不明白为什么不能在 `for each` 循环里进行元素的 remove。今天我们就来借机来体验一把。

```java
List<String> list = new ArrayList<>();
list.add("沉默王二");
list.add("沉默王三");
list.add("一个文章真特么有趣的程序员");

for (String str : list) {
	if ("沉默王二".equals(str)) {
		list.remove(str);
	}
}

System.out.println(list);
```

这段代码看起来没有任何问题，但运行起来就糟糕了。

```
Exception in thread "main" java.util.ConcurrentModificationException
	at java.util.ArrayList$Itr.checkForComodification(ArrayList.java:909)
	at java.util.ArrayList$Itr.next(ArrayList.java:859)
	at com.cmower.java_demo.str.Cmower3.main(Cmower3.java:14)
```

为毛呢？

### 03、分析问题的杀手锏

这时候就只能看源码了，ArrayList.java 的 909 行代码是这样的。

```java
final void checkForComodification() {
    if (modCount != expectedModCount)
        throw new ConcurrentModificationException();
}
```

也就是说，remove 的时候执行了 `checkForComodification` 方法，该方法对 modCount 和 expectedModCount 进行了比较，发现两者不等，就抛出了 `ConcurrentModificationException` 异常。

可为什么会执行 `checkForComodification` 方法呢？这就需要反编译一下 `for each` 那段代码了。

```java
List<String> list = new ArrayList();
list.add("沉默王二");
list.add("沉默王三");
list.add("一个文章真特么有趣的程序员");
Iterator var3 = list.iterator();

while (var3.hasNext()) {
	String str = (String) var3.next();
	if ("沉默王二".equals(str)) {
		list.remove(str);
	}
}

System.out.println(list);
```

原来 `for each` 是通过迭代器 Iterator 配合 while 循环实现的。

1）`ArrayList.iterator()` 返回的 Iterator 其实是 ArrayList 的一个内部类 Itr。

```java
public Iterator<E> iterator() {
    return new Itr();
}
```

Itr 实现了 Iterator 接口。

```java
private class Itr implements Iterator<E> {
    int cursor;       // index of next element to return
    int lastRet = -1; // index of last element returned; -1 if no such
    int expectedModCount = modCount;

    Itr() {}

    public boolean hasNext() {
        return cursor != size;
    }

    @SuppressWarnings("unchecked")
    public E next() {
        checkForComodification();
        int i = cursor;
        Object[] elementData = ArrayList.this.elementData;
        if (i >= elementData.length)
            throw new ConcurrentModificationException();
        cursor = i + 1;
        return (E) elementData[lastRet = i];
    }
}
```

也就是说 `new Itr()` 的时候 expectedModCount 被赋值为 modCount，而 modCount 是 List 的一个成员变量，表示集合被修改的次数。由于 list 此前执行了 3 次 add 方法，所以 modCount 的值为 3；expectedModCount 的值也为 3。

可当执行 `list.remove(str)` 后，modCount 的值变成了 4。

```java
private void fastRemove(int index) {
    modCount++;
    int numMoved = size - index - 1;
    if (numMoved > 0)
        System.arraycopy(elementData, index+1, elementData, index,
                         numMoved);
    elementData[--size] = null; // clear to let GC do its work
}
```

注：remove 方法内部调用了 fastRemove 方法。

下一次循环执行到 `String str = (String) var3.next();` 的时候，就会调用 `checkForComodification` 方法，此时一个为 3，一个为 4，就只好抛出异常 ConcurrentModificationException 了。


不信，可以直接在 ArrayList 类的 909 行打个断点 debug 一下。

![](http://www.itwanger.com/assets/images/2019/11/java-fail-fast-1.png)

真的耶，一个是 4 一个是 3。

**总结一下**。在 `for each` 循环中，集合遍历其实是通过迭代器 Iterator 配合 while 循环实现的，但是元素的 remove 却直接使用的集合类自身的方法。这就导致 Iterator 在遍历的时候，会发现元素在自己不知情的情况下被修改了，它觉得很难接受，就抛出了异常。

读者朋友们，你们是不是觉得我跑题了，fail-fast 和 `for each` 中集合的 remove 操作有什么关系呢？

有！Iterator 使用了 fail-fast 的保护机制。


### 04、怎么避开 fail-fast 保护机制呢

通过上面的分析，相信大家都明白为什么不能在 `for each` 循环里进行元素的 remove 了。

那怎么避开 fail-fast 保护机制呢？毕竟删除元素是常规操作，咱不能因噎废食啊。

1）remove 后 break

```java
List<String> list = new ArrayList<>();
list.add("沉默王二");
list.add("沉默王三");
list.add("一个文章真特么有趣的程序员");

for (String str : list) {
	if ("沉默王二".equals(str)) {
		list.remove(str);
		break;
	}
}
```

我怎么这么聪明，忍不住骄傲一下。有读者不明白为什么吗？那我上面的源码分析可就白分析了，爬楼再看一遍吧！

略微透露一下原因：break 后循环就不再遍历了，意味着 Iterator 的 next 方法不再执行了，也就意味着 `checkForComodification` 方法不再执行了，所以异常也就不会抛出了。

但是呢，当 List 中有重复元素要删除的时候，break 就不合适了。

2）for 循环

```java
List<String> list = new ArrayList<>();
list.add("沉默王二");
list.add("沉默王三");
list.add("一个文章真特么有趣的程序员");
for (int i = 0, n = list.size(); i < n; i++) {
	String str = list.get(i);
	if ("沉默王二".equals(str)) {
		list.remove(str);
	}
}
```

for 循环虽然可以避开 fail-fast 保护机制，也就说 remove 元素后不再抛出异常；但是呢，这段程序在原则上是有问题的。为什么呢？

第一次循环的时候，i 为 0，`list.size()` 为 3，当执行完 remove 方法后，i 为 1，`list.size()` 却变成了 2，因为 list 的大小在 remove 后发生了变化，也就意味着“沉默王三”这个元素被跳过了。能明白吗？

remove 之前 `list.get(1)` 为“沉默王三”；但 remove 之后 `list.get(1)` 变成了“一个文章真特么有趣的程序员”，而 `list.get(0)` 变成了“沉默王三”。

3）Iterator

```java
List<String> list = new ArrayList<>();
list.add("沉默王二");
list.add("沉默王三");
list.add("一个文章真特么有趣的程序员");

Iterator<String> itr = list.iterator();

while (itr.hasNext()) {
	String str = itr.next();
	if ("沉默王二".equals(str)) {
		itr.remove();
	}
}
```

为什么使用 Iterator 的 remove 方法就可以避开 fail-fast 保护机制呢？看一下 remove 的源码就明白了。

```java
public void remove() {
    if (lastRet < 0)
        throw new IllegalStateException();
    checkForComodification();

    try {
        ArrayList.this.remove(lastRet);
        cursor = lastRet;
        lastRet = -1;
        expectedModCount = modCount;
    } catch (IndexOutOfBoundsException ex) {
        throw new ConcurrentModificationException();
    }
}
```

虽然删除元素依然使用的是 ArrayList 的 remove 方法，但是删除完会执行 `expectedModCount = modCount`，保证了 expectedModCount 与 modCount 的同步。

### 05、最后

在 Java 中，fail-fast 从狭义上讲是针对多线程情况下的集合迭代器而言的。这一点可以从 `ConcurrentModificationException` 定义上看得出来。

>This exception may be thrown by methods that have detected concurrent
 modification of an object when such modification is not permissible.<br>
>For example, it is not generally permissible for one thread to modify a Collectionwhile another thread is iterating over it. In general, the results of theiteration are undefined under these circumstances. Some Iteratorimplementations (including those of all the general purpose collection implementationsprovided by the JRE) may choose to throw this exception if this behavior isdetected. Iterators that do this are known as fail-fast iterators,as they fail quickly and cleanly, rather that risking arbitrary,non-deterministic behavior at an undetermined time in the future. 

再次拙劣地翻译一下。

该异常可能由于检测到对象在并发情况下被修改而抛出的，而这种修改是不允许的。

通常，这种操作是不允许的，比如说一个线程在修改集合，而另一个线程在迭代它。这种情况下，迭代的结果是不确定的。如果检测到这种行为，一些 Iterator（比如说 ArrayList 的内部类 Itr）就会选择抛出该异常。这样的迭代器被称为 fail-fast 迭代器，因为尽早的失败比未来出现不确定的风险更好。

既然是针对多线程，为什么我们之前的分析都是基于单线程的呢？因为从广义上讲，fail-fast 指的是当有异常或者错误发生时就立即中断执行的这种设计，从单线程的角度去分析，大家更容易明白。

你说对吗？

### 06、致谢

谢谢大家的阅读，原创不易，喜欢就随手点个赞👍，这将是我最强的写作动力。如果觉得文章对你有点帮助，还挺有趣，就关注一下我的公众号「**沉默王二**」；回复「**666**」更有 500G 高质量教学视频相送（[已分门别类](https://mp.weixin.qq.com/s/GjkEyPW0vgIvuDLYQkBM0A)）。