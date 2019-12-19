---
layout: post
category: java
title: 灵魂拷问：如何检查Java数组中是否包含某个值 ？
tagline: by 沉默王二
tag: java
---

在逛 programcreek 的时候，我发现了一些专注细节但价值连城的主题。比如说：如何检查Java数组中是否包含某个值 ？像这类灵魂拷问的主题，非常值得深入地研究一下。


<!--more-->




另外，我想要告诉大家的是，作为程序员，我们千万不要轻视这些基础的知识点。因为基础的知识点是各种上层技术共同的基础，只有彻底地掌握了这些基础知识点，才能更好地理解程序的运行原理，做出更优化的产品。

[我](https://mp.weixin.qq.com/s/feoOINGSyivBO8Z1gaQVOA)曾在某个技术论坛上分享过一篇非常基础的文章，结果遭到了无数的嘲讽：“这么水的文章不值得分享。”我点开他的头像进入他的主页，发现他从来没有分享过一篇文章，不过倒是在别人的博客下面留下过不少的足迹，大多数都是冷嘲热讽。我就纳闷了，技术人不都应该像我这样低调谦逊吗？怎么戾气这么重！

好了，让我们来步入正题。如何检查数组（未排序）中是否包含某个值 ？这是一个非常有用并且经常使用的操作。我想大家的脑海中应该已经浮现出来了几种解决方案，这些方案的时间复杂度可能大不相同。

我先来提供四种不同的方法，大家看看是否高效。

**1）使用 List**

```java
public static boolean useList(String[] arr, String targetValue) {
    return Arrays.asList(arr).contains(targetValue);
}
```

Arrays 类中有一个内部类 ArrayList（可以通过 `Arrays.asList(arr)` 创建该实例），其 `contains()` 方法的源码如下所示。

```java
public boolean contains(Object o) {
    return indexOf(o) != -1;
}
public int indexOf(Object o) {
    E[] a = this.a;
    if (o == null) {
        for (int i = 0; i < a.length; i++)
            if (a[i] == null)
                return i;
    } else {
        for (int i = 0; i < a.length; i++)
            if (o.equals(a[i]))
                return i;
    }
    return -1;
}
```

从上面的源码可以看得出，`contains()` 方法调用了 `indexOf()` 方法，如果返回 -1 则表示 ArrayList 中不包含指定的元素，否则就包含。其中 `indexOf()` 方法用来获取元素在 ArrayList 中的下标，如果元素为 null，则使用“==”操作符进行判断，否则使用 `equals()` 方法进行判断。

PS：关于“==”操作符和 `equals()` 方法，可以参照我另外一篇文章《[如何比较 Java 的字符串？](https://mp.weixin.qq.com/s/WyrRCUlelzOxyfVBrxAGUg)》

**2）使用 Set**

```java
public static boolean useSet(String[] arr, String targetValue) {
	Set<String> set = new HashSet<String>(Arrays.asList(arr));
	return set.contains(targetValue);
}
```

HashSet 其实是通过 HashMap 实现的，当使用 `new HashSet<String>(Arrays.asList(arr))` 创建并初始化了 HashSet 对象后，其实是在 HashMap 的键中放入了数组的值，只不过 HashMap 的值为默认的一个摆设对象。大家感兴趣的话，可以查看一下 HashSet 的源码。

我们来着重看一下 HashSet 的 `contains()` 方法的源码。

```java
public boolean contains(Object o) {
    return map.containsKey(o);
}

public boolean containsKey(Object key) {
    return getNode(hash(key), key) != null;
}
```

从上面的源码可以看得出，`contains()` 方法调用了 HashMap 的 `containsKey()` 方法，如果指定的元素在 HashMap 的键中，则返回 true；否则返回 false。

**3）使用一个简单的循环**

```java
public static boolean useLoop(String[] arr, String targetValue) {
    for (String s : arr) {
        if (s.equals(targetValue))
            return true;
    }
    return false;
}
```

for-each 循环中使用了 `equals()` 方法进行判断——这段代码让我想起了几个词，分别是简约、高效、清晰。


 **4）使用 `Arrays.binarySearch()`**



```java
public static boolean useArraysBinarySearch(String[] arr, String targetValue) {
    int a = Arrays.binarySearch(arr, targetValue);
    if (a > 0)
        return true;
    else
        return false;
}
```

不过，`binarySearch()` 只适合查找已经排序过的数组。

由于我们不确定数组是否已经排序过，所以我们先来比较一下前三种方法的时间复杂度。由于调用 1 次的时间太短，没有统计意义，我们就模拟调用 100000 次，具体的测试代码如下所示。

```java
String[] arr = new String[]{"沉", "默", "王", "二", "真牛逼"};
// 使用 List
long startTime = System.nanoTime();
for (int i = 0; i < 100000; i++) {
    useList(arr, "真牛逼");
}
long endTime = System.nanoTime();
long duration = endTime - startTime;
System.out.println("useList:  " + duration / 1000000);

// 使用 Set
startTime = System.nanoTime();
for (int i = 0; i < 100000; i++) {
    useSet(arr, "真牛逼");
}
endTime = System.nanoTime();
duration = endTime - startTime;
System.out.println("useSet:  " + duration / 1000000);

// 使用一个简单的循环
startTime = System.nanoTime();
for (int i = 0; i < 100000; i++) {
    useLoop(arr, "真牛逼");
}
endTime = System.nanoTime();
duration = endTime - startTime;
System.out.println("useLoop:  " + duration / 1000000);
```

PS：`nanoTime()` 获取的是纳秒级，这样计算的时间就更精确，最后除以 1000000 就是毫秒。换算单位是这样的：1秒=1000毫秒，1毫秒=1000微秒，1微秒=1000纳秒。

统计结果如下所示：

```
useList:  6
useSet:  40
useLoop:  2
```

假如把数组的长度增加到 1000，我们再来看一下统计结果。

```java
String[] arr = new String[1000];
 
Random s = new Random();
for(int i=0; i< 1000; i++){
	arr[i] = String.valueOf(s.nextInt());
}
```

这时数组中是没有我们要找的元素的。为了做比较，我们顺便把二分查找也添加到统计项目中。

```java
// 使用二分查找
startTime = System.nanoTime();
for (int i = 0; i < 100000; i++) {
    useArraysBinarySearch(arr, "真牛逼");
}
endTime = System.nanoTime();
duration = endTime - startTime;
System.out.println("useArraysBinarySearch:  " + duration / 1000000);
```

统计结果如下所示：

```
useList:  91
useSet:  1460
useLoop:  70
useArraysBinarySearch:  4
```

我们再把数组的长度调整到 10000。

```java
String[] arr = new String[10000];

Random s = new Random();
for(int i=0; i< 10000; i++){
    arr[i] = String.valueOf(s.nextInt());
}
```

统计结果如下所示：

```
useList:  1137
useSet:  15711
useLoop:  1115
useArraysBinarySearch:  5
```

从上述的统计结果中可以很明显地得出这样一个结论：使用简单的 for 循环，效率要比使用 List 和 Set 高。这是因为把元素从数组中读出来再添加到集合中，就要花费一定的时间，而简单的 for 循环则省去了这部分时间。

在得出这个结论之前，说实话，我最喜欢的方式其实是第一种“使用 List”，因为只需要一行代码 `Arrays.asList(arr).contains(targetValue)` 就可以搞定。

虽然二分查找（`Arrays.binarySearch()`）花费的时间明显要少得多，但这个结论是不可信的。因为二分查找明确要求数组是排序过的，否则查找出的结果是没有意义的。可以看一下官方的 Javadoc。

>Searches the specified array for the specified object using the binary search algorithm. The array must be sorted into ascending order according to the natural ordering of its elements (as by the sort(Object []) method) prior to making this call. If it is not sorted, the results are undefined. 

实际上，如果要在一个数组或者集合中有效地确定某个值是否存在，一个排序过的 List 的算法复杂度为 `O(logn)`，而 HashSet 则为 `O(1)`。

我们再来发散一下思维：怎么理解 `O(logn)` 和 `O(1)` 呢？

`O(logn)` 的算法复杂度，比较典型的例子是二分查找。举个例子，假设现在一堆试卷，已经按照分数从高到底排列好了。现在要查找有没有 79 分的试卷，怎么办呢？可以先从中间找起，因为按照 100 分的卷子来看，79 分大差不差应该就在中间的位置（平均分如果低于 79 说明好学生就比较少了），如果中间这份卷子的分数是 83，那说明 79 分的卷子就在下面的一半，这时候可以把上面那半放在一边了。然后按照相同的方式，每次就从中间开始找，直到找到 79 分的卷子（当然也可能没有 79 分）。

假如有 56 份卷子，找一次，还剩 28 份，再找一次，还剩 14 份，再找一次，还剩 7 份，再找一次，还剩 2 或者 3 份。如果是 2 份，再找一次，就只剩下 1 份了；如果是 3 份，就还需要再找 2 次。

我们知道，log<sub>2</sub>(32) = 5，log<sub>2</sub>(64) = 6，而 56 就介于 32 和 64 之间。也就是说，二分查找大约需要 log<sub>2</sub>(n) 次才能“找到”或者“没找到”。而在算法复杂度里，经常忽略常数，所以不管是以 2 为底数，还是 3 为底数，统一写成 log(n) 的形式。

再来说说 `O(1)`，比较典型的例子就是哈希表（HashSet 是由 HashMap 实现的）。哈希表是通过哈希函数来映射的，所以拿到一个关键字，通过哈希函数转换一下，就可以直接从表中取出对应的值——一次直达。


------

好了各位读者朋友们，以上就是本文的全部内容了。**能看到这里的都是人才，二哥必须要为你点个赞**👍。如果觉得不过瘾，还想看到更多，我再给大家推荐几篇。

[灵魂拷问：Java 的 substring() 是如何工作的？](https://mp.weixin.qq.com/s/rLakWBPuWqYG8QT6ACetGQ)
[灵魂拷问：为什么 Java 字符串是不可变的？](https://mp.weixin.qq.com/s/CRQrm5zGpqWxYL_ztk-b2Q)
[灵魂拷问：创建 Java 字符串，用""还是构造函数](http://www.itwanger.com/java/2019/11/28/java-string-shuangyinhao-gouzaohanshu.html)



如果你有什么问题需要我的帮助，或者想喷我了，欢迎留言哟。

养成好习惯！如果觉得这篇文章有点用的话，**求点赞、求关注、求分享、求留言**，这将是我写下去的最强动力！如果大家想要第一时间看到二哥更新的文章，可以扫描下方的二维码，关注我的公众号，回复「java」再送你一份精选电子书大礼包，包含这 10 年来我读过的最优质的 Java 书籍。我们下篇文章见！


