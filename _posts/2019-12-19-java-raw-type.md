---
layout: post
category: java
title: 205K+程序员关注过的问题：为什么不应该使用Java的原始类型？
tagline: by 沉默王二
tag: java
---

在逛 Stack Overflow 的时候，发现了一些访问量像熊耳山一样高的问题，比如说这个：为什么不应该使用Java的原始类型？访问量足足有 205K+，这不得了啊！说明有很多很多的程序员被这个问题困扰过。实话实说吧，本文之前的[我](https://mp.weixin.qq.com/s/feoOINGSyivBO8Z1gaQVOA)就是其中之一。

<!--more-->




来回顾一下提问者的问题吧：

>Java 的原始类型是什么？为什么不要使用原始类型？如果不能使用原始类型，有什么更好的选择呢？

如果大家也被这个问题困扰过，或者正在被困扰，就请随我来，咱们肩并肩手拉手一起梳理一下这个问题，并找出最佳答案。Duang、Duang、Duang，打怪进阶喽！

### 01、Java 的原始类型是什么？

要理解 Java 的原始类型是什么，可以先看一下什么是[泛型](https://mp.weixin.qq.com/s/_uX0BTv7pVB8OzZTix1zbw)。

```java
List<String> list = null;
```

其中 list 就是一个泛型，我们通常称之为字符串（String）列表（List），也就是说 list 中只能放字符串类型的元素。

如果我们按照下面这种方式声明 list 的话，它就是一个原始类型。

```java
List list = null;
```

从 list 的声明当中我们可以对比发现，原始类型没有为容器指定明确的元素类型，所以我们可以在容器中放入一个 String，也可以放入一个 Integer，甚至任意的类型，就像下面这样。

```java
public class RawType {
    public static void main(String[] args) {
        List list = new ArrayList();
        list.add("沉默王二");
        list.add(18);
        list.add(new RawType());
    }
}
```

注意哦，编译器没有任何提醒！这预示着 Java 这门强类型的语言竟然有点弱类型的影子了。

PS：关于 Java 中的类型术语，大家可以参照下表。

术语	|含义|	举例
----|---|----
Parameterized type|	参数化类型	| `List<String>`
Actual type parameter|	实际类型参数|	`String`
Generic type|	泛型类型	|`List<E>`
Formal type parameter|	形式类型参数	|`E`
Unbounded wildcard type	|无限制通配符类型|	`List<?>`
Raw type|	原始类型	|`List`
Bounded type parameter|	限制类型参数	|`<E extends Number>`
Bounded wildcard type|	限制通配符类型	|`List<? extends Number>`

### 02、为什么不要使用原始类型？

大家可能会有一个疑惑，原始类型用起来很爽啊！因为不用关心放入 List 的元素到底是什么类型，想放什么就可以放什么，不要太爽啊！

可当我们想要从 List 中把元素取出来使用的时候，可就遇到大麻烦了。

```java
List list = new ArrayList();
list.add("沉默王二");
list.add(18);
list.add(new RawType());

for (Object o : list ) {
    String s = (String) o;
    System.out.println(s);
}
```

上面这段代码编译的时候没有任何问题，但输出的时候就会抛出 `ClassCastException`。

```
沉默王二
Exception in thread "main" java.lang.ClassCastException: java.lang.Integer cannot be cast to java.lang.String
	at com.cmower.java_demo.programcreek.RawType.main(RawType.java:14)
```

除非我们使用 `instanceof` 关键字进行类型判断，就像下面这样。

```java
List list = new ArrayList();
list.add("沉默王二");
list.add(18);
list.add(new RawType());

for (Object o : list ) {
    if (o instanceof String) {
        String s = (String) o;
        System.out.println(s);
    } else if (o instanceof Integer) {
        Integer i = (Integer) o;
        System.out.println(i);
    } else if (o instanceof RawType) {
        RawType raw = (RawType) o;
        System.out.println(raw);
    }
}
```

可假如代码写成这样，可真真算得上是糟糕的代码了。

通常来说，为了代码的安全性起见，我们希望代码的错误发生得越早越好，能在编译时就不要在运行时。可使用原始类型的时候，我们发现错误一直到运行时才可能会被检出。

还记得《扁鹊见蔡桓公》的故事吗？

>扁鹊见蔡桓公，立有间。扁鹊曰：“君有疾在腠理，不治将恐深。”桓侯曰：“寡人无疾。”扁鹊出，桓侯曰：“医之好治不病以为功。”......居十日，扁鹊望桓侯而还走。桓侯故使人问之，扁鹊曰：“疾在腠理，汤熨之所及也；在肌肤，针石之所及也；在肠胃，火齐之所及也；在骨髓，司命之所属，无奈何也。今在骨髓，臣是以无请也。”居五日，桓侯体痛，使人索扁鹊，已逃秦矣。桓侯遂死。

病情发现得越早，治疗的可能性就越大。同理，代码隐藏的问题发现的越晚，找出根源花费的精力就越大、时间就越多。

### 03、有什么更好的选择呢？

如果不能使用原始类型，有什么更好的选择呢？

为了让 List 能够容纳任意类型的元素，我们可以使用 `List<Object>`，尽管这并不是一个最优的选择。

```java
List<Object> list = new ArrayList<>();
list.add("沉默王二");
list.add(18);
list.add(new RawType());
```

鹅鹅鹅，这样的参数化类型 `List<Object>` 和原始类型 List 之间有区别吗？

当然有了！

`List<Object>` 至少明确地告诉编译器，该容器可以存放任意类型的对象，没有丢失类型的安全性。

可能我这样的解释会遭到某些抨击：“这不五十步笑百步吗？呵呵。”但我要想表达的是登月男神阿姆斯特朗的那句话：“这是我个人的一小步，却是人类的一大步。”能向前迈一步是一步啊。

那最优的选择是什么呢？

从一开始就为 List 声明具体的类型，比如说 `List<String> list`，当我们尝试放入一个 int 值的时候就会编译出错。

![](http://www.itwanger.com/assets/images/2019/12/java-raw-type-2.png)


从另一种层面上来说，这样做削弱了程序的灵活性，但保证了程序的绝对安全性，以及在表达上的明确性。

### 04、为什么 Java 允许使用原始类型？

既然原始类型是不安全的，那为什么 Java 一直允许使用原始类型呢？并且泛型擦除后仍然是个原始类型呢？

答案很简单、很无厘头、很苍白——为了版本兼容！

引入泛型的时候，Java 已经进入到第二个十年（年纪大了），市面上存在大量没有使用 Java 泛型的代码。如果因为版本升级导致它们不能使用，恐怕 Java 也活不到现在，毕竟对用户友好才是一个软件存在的硬道理。

当然了，Java 已经对开发者做出了警示：强烈建议不要在 Java 代码中使用原始类型，未来的版本中可以会禁止使用原始类型，请小心点。

### 05、鸣谢

好了各位读者朋友们，以上就是本文的全部内容了。**能看到这里的都是最优秀的程序员，二哥必须要动动手为你点个赞**👍。如果觉得不过瘾，还想看到更多，我再推荐几篇给大家。

[50W+程序员关注过的问题：为什么会发生ArrayIndexOutOfBoundsException？](https://mp.weixin.qq.com/s/TRyVTQqMGmqs4lmHtsgRuw)
[188W+程序员关注过的问题：Java 到底是值传递还是引用传递？](https://mp.weixin.qq.com/s/TRyVTQqMGmqs4lmHtsgRuw)
[250W+程序员关注过的问题：什么是 NullPointerException？](https://mp.weixin.qq.com/s/PBqR_uj6dd4xKEX8SUWIYQ)
[370W+程序员关注过的问题：如何比较 Java 的字符串？](https://mp.weixin.qq.com/s/WyrRCUlelzOxyfVBrxAGUg)


#### 有收获？就点赞、留言，让更多的人看到这篇文章。

如果想要第一时间看到我更新的文章，可以微信搜索「**沉默王二**」，关注我的公众号，回复「**java**」再送你一份**精选电子书大礼包**，包含这十年来我读过的最优质的 Java 书籍。


