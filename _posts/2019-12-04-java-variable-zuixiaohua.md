---
layout: post
category: java
title: 为什么要将局部变量的作用域最小化？
tagline: by 沉默王二
tag: java
---

嗨，本篇文章来说说 Java 的一个小细节：为什么要将局部变量的作用域最小化？

<!--more-->

明人不说暗话啊。这篇文章的灵感来源于《Effective Java》，这本书我买了有好长好长一段时间了，书页都已经泛黄，烙下了时间的痕迹，但我仍然还没有把这本书读完。说来惭愧啊。

为什么呢？总感觉这本书的中文翻译有点拙劣，读起来烦闷枯燥。明明感觉作者说得非常有道理，但就是提不起半点兴致。

（说完这句话，总觉得有点对不住这本书的译者，毕竟吐槽容易，分享难啊。）

为什么要说这些废话呢，因为怕大家觉得这是不值一提的细节，但往往细节决定成败啊。大家不妨换一种比较轻松的心态来读一读。反正我是不怎么喜欢高谈阔论的文章，读完后往往只能感慨一句：“说得不错啊”，但也仅此而已。

好了，来步入正题。

```java
String [] strs = {"洛阳","牡丹","甲天下"};
List<String> list = Arrays.asList(strs);

Iterator<String> iterator = list.iterator();
while (iterator.hasNext()) {
    String s = (String) iterator.next();
    System.out.println(s);
}

list.add("沉默王二");
Iterator<String> iterator1 = list.iterator();
while (iterator.hasNext()) {
    String s = (String) iterator1.next();
    System.out.println(s);
}
```

大家用“肉眼”看完上面这段代码后，会觉得有问题吗？

如果不细心的话，好像真的很难发现“复制-粘贴”引发的这个问题：第二个 while 循环的条件中使用了之前的变量 iterator，而不是它应该使用的 iterator1（粘贴后遗漏了变量的修改）。这个问题将会导致代码在运行的时候抛出 `java.lang.UnsupportedOperationException` 的错误。

说句实在话，在敲代码的这十年来，没少复制粘贴，没少因为粘贴后变量没有修改彻底，而导致出现了各种意料之外的 bug。

假如把变量的作用域最小化的话，还真的能够减少这种因为“复制-粘贴”而导致出现的错误。比如说把 while 循环改造成 for 循环。

```java
for (Iterator<String> iterator = list.iterator();iterator.hasNext();) {
    String s = (String) iterator.next();
    System.out.println(s);
}

list.add("沉默王二");
for (Iterator<String> iterator = list.iterator();iterator.hasNext();) {
    String s = (String) iterator.next();
    System.out.println(s);
}
```

第二个 for 循环使用了和第一个 for 循环一模一样的代码，连 iterator 这个变量也不需要修改了。

从另一方面来看的话，for 循环比 while 循环更简短，可读性更好。for 循环还有另外一种最常用的写法，示例如下。

```java
for (int i = 0; i < list.size(); i++) {
    System.out.println(list.get(i));
}
```

但这种写法仍有改进的地方，因为从字节码的角度来看，每次循环都要调用一次 `size()` 方法。

```
2: iload_1
3: aload_0
4: getfield      #4                  // Field list:Ljava/util/List;
7: invokeinterface #5,  1            // InterfaceMethod java/util/List.size:()I
12: if_icmpge     40
15: getstatic     #6                  // Field java/lang/System.out:Ljava/io/PrintStream;
18: aload_0
19: getfield      #4                  // Field list:Ljava/util/List;
22: iload_1
23: invokeinterface #7,  2            // InterfaceMethod java/util/List.get:(I)Ljava/lang/Object;
28: checkcast     #8                  // class java/lang/String
31: invokevirtual #9                  // Method java/io/PrintStream.println:(Ljava/lang/String;)V
34: iinc          1, 1
37: goto          2
40: return
```

`size()` 方法虽然简短，但也有消耗啊。都有什么消耗呢？说几个专业名词大家感受一下，比如说：创建栈帧、调用方法时保护现场、调用方法完毕后恢复现场。

（容许我尴尬一下，在写这篇文章之前，我一直用的上面这种 for 循环格式。看来写文章还是能够督促自己进步啊。）

怎么改进呢，看下面这种写法（强烈推荐啊）。

```java
for (int i = 0, n = list.size(); i < n; i++) {
    System.out.println(list.get(i));
}
```

在 for 循环内部声明两个变量：i 和 n，n 用来保存 i 的极限值，这样就减少了 `size()` 方法的调用次数（仅有一次了）。

再来看一段代码。

```java
String pre_name = "沉默";
String last_name = "王二";

System.out.println(pre_name);
System.out.println(last_name);
```

上面这段代码看起来挺规整的，没什么问题，对吧？它没有遵守约定——将局部变量的作用域最小化。

pre_name 变量的作用域结束的有点晚；last_name 变量的作用域开始的有点早。假如第一个 `System.out.println()` 出错的话，last_name 的声明就变得毫无意义了。

（这只是一个例子，变量的处理方法可能比 `System.out.println()` 复杂得多。）

好的写法应该是下面这样子。

```java
String pre_name = "沉默";
System.out.println(pre_name);

String last_name = "王二";
System.out.println(last_name);
```

有人可能觉得这不是在吹毛求疵吗？真不是的，**变量就应该是在第一次使用它的时候声明**。否则的话，变量的作用域要么开始的太早，要么结束的太晚。

好了，这篇文章到此就结束了，非常的简短，但讲清楚了“为什么要将局部变量的作用域最小化”。


