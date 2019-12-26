---
layout: post
category: java
title: 五分钟学Java：打印Java数组最优雅的方式是什么？
tagline: by 沉默王二
tag: java
---

在逛 Stack Overflow 的时候，发现了一些访问量像‎安第斯山一样高的问题，比如说这个：打印 Java 数组最优雅的方式是什么？访问量足足有 220W+，想不到啊，这么简单的问题竟然有这么多程序员被困扰过。

<!--more-->



来回顾一下提问者的问题吧：

>在 Java 中，数组虽然是一个对象，但并未明确的定义这样一个类，因此也就没有覆盖 `toString()` 方法的机会。如果尝试直接打印数组的话，输出的结果并不是我们预期的结果。那有没有一些简单可行的方式呢？

如果大家也被这个问题困扰过，或者正在被困扰，就请随[我](https://mp.weixin.qq.com/s/feoOINGSyivBO8Z1gaQVOA)来，咱们肩并肩手拉手一起梳理一下这个问题，并找出最佳答案。Duang、Duang、Duang，打怪进阶喽！

### 01、为什么不能直接打印

很好奇，是不是，为什么不能直接使用 `System.out.println()` 等系列方法来打印数组？来看这样一个例子。

```java
String [] cmowers = {"沉默","王二","一枚有趣的程序员"};
System.out.println(cmowers);
```

程序打印的结果是：

```java
[Ljava.lang.String;@3d075dc0
```

`[Ljava.lang.String;` 表示字符串数组的 Class 名，@ 后面的是十六进制的 hashCode——这样的打印结果太“人性化”了，一般人表示看不懂！为什么会这样显示呢？查看一下 `java.lang.Object` 类的 `toString()` 方法就明白了。

```java
public String toString() {
    return getClass().getName() + "@" + Integer.toHexString(hashCode());
}
```

PS：数组虽然没有显式定义成一个类，但它的确是一个对象，继承了祖先类 Object 的所有方法。

那为什么数组不单独定义一个类来表示呢？就像字符串 String 类那样呢？

一个合理的解释是 Java 将其隐藏了。假如真的存在一个 Array.java，我们也可以假想它真实的样子，它必须要定义一个容器来存放数组的元素，就像 String 类那样。

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    /** The value is used for character storage. */
    private final char value[];
}
```

但这样做真的有必要吗？为数组单独定义一个类，是不是有点画蛇添足的意味。



### 02、使用 Stream

如果使用的是 JDK8 以上的版本，我们可以使用 Stream 这种时髦、fashion 的方式来遍历数组，顺带将其打印出来。

第一种：

```java
Arrays.asList(cmowers).stream().forEach(s -> System.out.println(s));
```

第二种：

```java
Stream.of(cmowers).forEach(System.out::println);
```

第三种：

```java
Arrays.stream(cmowers).forEach(System.out::println);
```

打印的结果如下所示。

```java
沉默
王二
一枚有趣的程序员
```

没错，这三种方式都可以轻松胜任本职工作，并且显得有点高大上，毕竟用到了 Stream，以及 lambda 表达式。但在我心目中，它们并不是最优雅的方式。

### 03、使用 for 循环

当然了，如果不喜欢 Stream 的方式，也可以使用 for 循环对数组进行变量顺便打印的方式，甚至 for-each 也行。

```java
for(int i = 0; i < cmowers.length; i++){
    System.out.println(cmowers[i]);
}

for (String s : cmowers) {
    System.out.println(s);
}
```

但如果你是一名有追求的程序员的话，不免觉得这样的方式有点 low。那到底最优雅的方式是什么呢？

### 04、使用 Arrays.toString()

`Arrays.toString()` 可以将任意类型的数组转成字符串，包括基本类型数组和引用类型数组，截个图大家感受一下。

![](http://www.itwanger.com/assets/images/2019/12/java-print-array-1.png)

Arrays 类就不用我多做介绍了吧？虽然我的意思大家懂，但我还是忍不住要废话两句：该类包含了各种操作数组的便捷方法，与其命名为 Arrays，不如命名为 ArrayUtil。

使用 `Arrays.toString()` 方法来打印数组再优雅不过了，就像，就像，就像蒙娜丽莎的微笑。

![](http://www.itwanger.com/assets/images/2019/12/java-print-array-2.png)

被逗笑了吧？来，怀揣着愉快的心情看一下代码示例。

```java
String [] cmowers = {"沉默","王二","一枚有趣的程序员"};
System.out.println(Arrays.toString(cmowers));
```

程序打印结果：

```java
[沉默, 王二, 一枚有趣的程序员]
```

哇，打印格式不要太完美，不多不少！完全是我们预期的结果：`[]` 表明是一个数组，`,` 点和空格用来分割元素。

顺便再来看一下 `toString()` 方法的源码。

```java
public static String toString(Object[] a) {
    if (a == null)
        return "null";

    int iMax = a.length - 1;
    if (iMax == -1)
        return "[]";

    StringBuilder b = new StringBuilder();
    b.append('[');
    for (int i = 0; ; i++) {
        b.append(String.valueOf(a[i]));
        if (i == iMax)
            return b.append(']').toString();
        b.append(", ");
    }
}
```

1）如果数组为 null，那就返回“null”字符串，考虑很周全，省去了 `NullPointerException` 的麻烦。

2）如果数组长度为 0，那就返回“[]”字符串。注意，此处没有使用 `a.length == 0` 进行判空，而是用了 `a.length - 1 == -1`，又为之后的 for 循环中的 `i == iMax` 埋下了伏笔，资源一点也没有浪费。

3）for 循环中字符串的拼接更是巧妙，for 循环的条件中没有判断 `i < a.length`，而在循环体内使用了 `i == iMax`，这样有什么好处呢？

通常来说，一般的程序员拼接字符串的时候是这样做的。

```java
StringBuilder b = new StringBuilder();
b.append('[');
for (int i = 0; i < cmowers.length; i++) {
    b.append(cmowers[i]);
    b.append(", ");
}
b.delete(b.length()-2, b.length());
b.append(']');
```

没错吧，非常的循规蹈矩，但比起 `toString()` 方法源码中的写法，就要相形见绌了。情不自禁地感慨一下啊：**要想成为一名卓越的程序员，而不只是一名普通的程序员，最快的捷径就是学习 Java 的源码**。

### 05、使用 `Arrays.deepToString()`

如果需要打印多维码数组的话，`Arrays.toString()` 就无能为力了。

```java
String[][] deepArray = new String[][] {{"沉默", "王二"}, {"一枚有趣的程序员"}};
System.out.println(Arrays.toString(deepArray));
```

打印结果如下所示。

```java
[[Ljava.lang.String;@7ba4f24f, [Ljava.lang.String;@3b9a45b3]
```

不不不，这不是我们期望的结果，怎么办呢？使用  `Arrays.deepToString()`，专为多维数组而生。

```java
String[][] deepArray = new String[][] {{"沉默", "王二"}, {"一枚有趣的程序员"}};
System.out.println(Arrays.deepToString(deepArray));
```

打印结果如下所示。

```java
[[沉默, 王二], [一枚有趣的程序员]]
```

优秀吧！至于 `deepToString()` 的源码，本文就不再分析了，大家感兴趣的话自己看一看。（如果你想卓越的话，必须要看啊）

### 06、鸣谢

好了各位读者朋友们，以上就是本文的全部内容了。能看到这里的都是最优秀的程序员，升职加薪就是你了👍。如果觉得不过瘾，还想看到更多，我再给大家推荐几篇。

[五分钟学Java：为什么不应该使用Java的原始类型？](https://mp.weixin.qq.com/s/ztYEmcdQ00c3L5nqgb2meg)

[五分钟学Java：为什么会发生ArrayIndexOutOfBoundsException？](https://mp.weixin.qq.com/s/TRyVTQqMGmqs4lmHtsgRuw)

[五分钟学Java：为什么会发生ArrayIndexOutOfBoundsException？](https://mp.weixin.qq.com/s/TRyVTQqMGmqs4lmHtsgRuw)

[五分钟学Java：Java 到底是值传递还是引用传递？](https://mp.weixin.qq.com/s/TRyVTQqMGmqs4lmHtsgRuw)

[五分钟学Java：什么是 NullPointerException？](https://mp.weixin.qq.com/s/PBqR_uj6dd4xKEX8SUWIYQ)

[五分钟学Java：如何比较 Java 的字符串？](https://mp.weixin.qq.com/s/WyrRCUlelzOxyfVBrxAGUg)

日常操作来了！如果是二哥铁杆读者的话，请不要吝啬你手中**在看**和**转发**的权力，明人不说暗话，我喜欢这种被大家伙宠爱的感觉。

PS：最近绞尽脑汁想到了一句魔性的宣传词，大家一起高声朗读一下：

**这里是「沉默王二」公众号，谁关注谁发大财，谁关注谁皮肤白**。

自己忍不住笑场了（哈哈哈）。