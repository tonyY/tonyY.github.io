---
layout: post
category: java
title: 教妹学 Java：晦涩难懂的泛型
tagline: by 沉默王二
tag: java
---

>二哥，究竟什么是面向对象呢？还有，什么是面向过程。今天去面试的时候，面试官让我用面向对象的思想谈一谈这次面试的过程。

<!--more-->

本篇通过一种趣味化的形式来讲述 Java 的[泛型](http://www.itwanger.com/java/2019/11/08/java-fanxing.html)。



### 00、故事的起源

“二哥，要不我上大学的时候也学习编程吧？”有一天，三妹突发奇想地问我。

“你确定要做一名程序媛吗？”

“我觉得女生做程序员，有着天大的优势，尤其是我这种长相甜美的。”三妹开始认真了起来。

“好像是啊，遇到女生提问，我好像一直蛮热情的。”

“二哥，你不是爱好写作嘛，还是一个 Java 程序员，不妨写个专栏，名字就叫《教妹学 Java》。我高考完就开始跟着你学习编程，还能省下一笔培训费。”三妹看起来已经替我筹划好了呀。

“真的很服气你们零零后，蛮有想法的。刚好我最近在写 Java 系列的专栏，不妨试一试！”

PS：亲爱的读者朋友们，我们今天就从晦涩难懂的“泛型”开始吧！（子标题是三妹提出来的，内容由二哥我来回答）

### 01、二哥，为什么要设计泛型啊？

三妹啊，听哥慢慢给你讲啊。

Java 在 5.0 时增加了泛型机制，据说专家们为此花费了 5 年左右的时间（听起来很不容易）。有了泛型之后，尤其是对集合类的使用，就变得更规范了。

看下面这段简单的代码。

```java
ArrayList<String> list = new ArrayList<String>();
list.add("沉默王二");
String str = list.get(0);
```

但在没有泛型之前该怎么办呢？

首先，我们需要使用 Object 数组来设计 `Arraylist` 类。

```java
class Arraylist {
    private Object[] objs;
    private int i = 0;
    public void add(Object obj) {
        objs[i++] = obj;
    }
    
    public Object get(int i) {
        return objs[i];
    }
}
```

然后，我们向 `Arraylist` 中存取数据。

```java
Arraylist list = new Arraylist();
list.add("沉默王二");
list.add(new Date());
String str = (String)list.get(0);
```

你有没有发现两个问题：

- Arraylist 可以存放任何类型的数据（既可以存字符串，也可以混入日期），因为所有类都继承自 Object 类。
- 从 Arraylist 取出数据的时候需要强制类型转换，因为编译器并不能确定你取的是字符串还是日期。

对比一下，你就能明显地感受到泛型的优秀之处：使用**类型参数**解决了元素的不确定性——参数类型为 String 的集合中是不允许存放其他类型元素的，取出数据的时候也不需要强制类型转换了。

### 02、二哥，怎么设计泛型啊？

三妹啊，你一个小白只要会用泛型就行了，还想设计泛型啊？！不过，既然你想了解，那么哥义不容辞。

首先，我们来按照泛型的标准重新设计一下 `Arraylist` 类。

```java
class Arraylist<E> {
    private Object[] elementData;
    private int size = 0;

    public Arraylist(int initialCapacity) {
        this.elementData = new Object[initialCapacity];
    }
    
    public boolean add(E e) {
        elementData[size++] = e;
        return true;
    }
    
    E elementData(int index) {
        return (E) elementData[index];
    }
}
```

一个泛型类就是具有一个或多个类型变量的类。Arraylist 类引入的类型变量为 E（Element，元素的首字母），使用尖括号 `<>` 括起来，放在类名的后面。

然后，我们可以用具体的类型（比如字符串）替换类型变量来实例化泛型类。

```java
Arraylist<String> list = new Arraylist<String>();
list.add("沉默王三");
String str = list.get(0);
```

Date 类型也可以的。

```java
Arraylist<Date> list = new Arraylist<Date>();
list.add(new Date());
Date date = list.get(0);
```

其次，我们还可以在一个非泛型的类（或者泛型类）中定义泛型方法。

```java
class Arraylist<E> {
    public <T> T[] toArray(T[] a) {
        return (T[]) Arrays.copyOf(elementData, size, a.getClass());
    }
}
```

不过，说实话，泛型方法的定义看起来略显晦涩。来一副图吧（注意：方法返回类型和方法参数类型至少需要一个）。

![](http://www.itwanger.com/assets/images/2019/11/java-fanxing-jiaomei-1.png)


现在，我们来调用一下泛型方法。

```java
Arraylist<String> list = new Arraylist<>(4);
list.add("沉");
list.add("默");
list.add("王");
list.add("二");

String [] strs = new String [4];
strs = list.toArray(strs);

for (String str : strs) {
    System.out.println(str);
}
```

最后，我们再来说说泛型变量的限定符 `extends`。在解释这个限定符之前，我们假设有三个类，它们之间的定义是这样的。

```java
class Wanglaoer {
    public String toString() {
        return "王老二";
    }
}

class Wanger extends Wanglaoer{
    public String toString() {
        return "王二";
    }
}

class Wangxiaoer extends Wanger{
    public String toString() {
        return "王小二";
    }
}
```

我们使用限定符 `extends` 来重新设计一下 `Arraylist` 类。

```java
class Arraylist<E extends Wanger> {
}
```

当我们向 `Arraylist` 中添加 `Wanglaoer` 元素的时候，编译器会提示错误：`Arraylist` 只允许添加 `Wanger` 及其子类 `Wangxiaoer` 对象，不允许添加其父类 `Wanglaoer`。

```java
Arraylist<Wanger> list = new Arraylist<>(3);
list.add(new Wanger());
list.add(new Wanglaoer());
// The method add(Wanger) in the type Arraylist<Wanger> is not applicable for the arguments 
// (Wanglaoer)
list.add(new Wangxiaoer());
```

也就是说，限定符 `extends` 可以缩小泛型的类型范围。

### 03、二哥，听说虚拟机没有泛型？

三妹，你功课做得可以啊，连虚拟机都知道了啊。哥可以肯定地回答你，虚拟机是没有泛型的。

啰嗦一句哈。我们编写的 Java 代码（也就是源码，后缀为 .java 的文件）是不能够被操作系统直接识别的，需要先编译，生成 .class 文件（也就是字节码文件）。然后 Java 虚拟机（JVM）会充当一个翻译官的角色，把字节码翻译给操作系统能听得懂的语言，告诉它该干嘛。

怎么确定虚拟机没有泛型呢？我们需要把泛型类的字节码进行反编译——强烈推荐超神反编译工具 Jad ！

现在，在命令行中敲以下代码吧（反编译 `Arraylist` 的字节码文件 `Arraylist.class`）。

```shell
jad Arraylist.class
```

命令执行完后，会生成一个 Arraylist.jad 的文件，用文本编辑工具打开后的结果如下。

```java
// Decompiled by Jad v1.5.8g. Copyright 2001 Pavel Kouznetsov.
// Jad home page: http://www.kpdus.com/jad.html
// Decompiler options: packimports(3) 
// Source File Name:   Arraylist.java

package com.cmower.java_demo.fanxing;

import java.util.Arrays;

class Arraylist
{

    public Arraylist(int initialCapacity)
    {
        size = 0;
        elementData = new Object[initialCapacity];
    }

    public boolean add(Object e)
    {
        elementData[size++] = e;
        return true;
    }

    Object elementData(int index)
    {
        return elementData[index];
    }

    private Object elementData[];
    private int size;
}
```

类型变量 `<E>` 消失了，取而代之的是 Object ！

既然如此，那如果泛型类使用了限定符 `extends`，结果会怎么样呢？我们先来看看 `Arraylist2` 的源码。

```java
class Arraylist2<E extends Wanger> {
    private Object[] elementData;
    private int size = 0;

    public Arraylist2(int initialCapacity) {
        this.elementData = new Object[initialCapacity];
    }

    public boolean add(E e) {
        elementData[size++] = e;
        return true;
    }

    E elementData(int index) {
        return (E) elementData[index];
    }
}
```

字节码文件 `Arraylist2.class` 使用 Jad 反编译后的结果如下。

```java
// Decompiled by Jad v1.5.8g. Copyright 2001 Pavel Kouznetsov.
// Jad home page: http://www.kpdus.com/jad.html
// Decompiler options: packimports(3) 
// Source File Name:   Arraylist2.java

package com.cmower.java_demo.fanxing;


// Referenced classes of package com.cmower.java_demo.fanxing:
//            Wanger

class Arraylist2
{

    public Arraylist2(int initialCapacity)
    {
        size = 0;
        elementData = new Object[initialCapacity];
    }

    public boolean add(Wanger e)
    {
        elementData[size++] = e;
        return true;
    }

    Wanger elementData(int index)
    {
        return (Wanger)elementData[index];
    }

    private Object elementData[];
    private int size;
}
```

类型变量 `<E extends Wanger>` 不见了，E 被替换成了 `Wanger`。

通过以上两个例子说明，Java 虚拟机会将泛型的类型变量擦除，并替换为限定类型（没有限定的话，就用 `Object`）。


### 04、二哥，类型擦除会有什么问题吗？

三妹啊，你还别说，类型擦除真的会有一些“问题”。

我们来看一下这段代码。

```java
public class Cmower {
    
    public static void method(Arraylist<String> list) {
        System.out.println("Arraylist<String> list");
    }

    public static void method(Arraylist<Date> list) {
        System.out.println("Arraylist<Date> list");
    }

}
```

在浅层的意识上，我们会想当然地认为 `Arraylist<String> list` 和 `Arraylist<Date> list` 是两种不同的类型，因为 String 和 Date 是不同的类。

但由于类型擦除的原因，以上代码是不会通过编译的——编译器会提示一个错误（这正是类型擦除引发的那些“问题”）：

>Erasure of method method(Arraylist<String>) is the same as another method in type 
 Cmower
>
>Erasure of method method(Arraylist<Date>) is the same as another method in type 
 Cmower

大致的意思就是，这两个方法的参数类型在擦除后是相同的。

也就是说，`method(Arraylist<String> list)` 和 `method(Arraylist<Date> list)` 是同一种参数类型的方法，不能同时存在。类型变量 `String` 和 `Date` 在擦除后会自动消失，method 方法的实际参数是 `Arraylist list`。

有句俗话叫做：“百闻不如一见”，但即使见到了也未必为真——泛型的擦除问题就可以很好地佐证这个观点。

### 05、二哥，听说泛型还有通配符？

三妹啊，哥突然觉得你很适合作一枚可爱的程序媛啊！你这预习的功课做得可真到家啊，连通配符都知道！

通配符使用英文的问号（?）来表示。在我们创建一个泛型对象时，可以使用关键字 `extends` 限定子类，也可以使用关键字 `super` 限定父类。

为了更好地解释通配符，我们需要对 `Arraylist` 进行一些改进。

```java
class Arraylist<E> {
    private Object[] elementData;
    private int size = 0;

    public Arraylist(int initialCapacity) {
        this.elementData = new Object[initialCapacity];
    }

    public boolean add(E e) {
        elementData[size++] = e;
        return true;
    }

    public E get(int index) {
        return (E) elementData[index];
    }
    
    public int indexOf(Object o) {
        if (o == null) {
            for (int i = 0; i < size; i++)
                if (elementData[i]==null)
                    return i;
        } else {
            for (int i = 0; i < size; i++)
                if (o.equals(elementData[i]))
                    return i;
        }
        return -1;
    }
    
    public boolean contains(Object o) {
        return indexOf(o) >= 0;
    }
    
    public String toString() {
        StringBuilder sb = new StringBuilder();
        
        for (Object o : elementData) {
            if (o != null) {
                E e = (E)o;
                sb.append(e.toString());
                sb.append(',').append(' ');
            }
        }
        return sb.toString();
    }

    public int size() {
        return size;
    }
    
    public E set(int index, E element) {
        E oldValue = (E) elementData[index];
        elementData[index] = element;
        return oldValue;
    }
}
```

1）新增 `indexOf(Object o)` 方法，判断元素在 `Arraylist` 中的位置。注意参数为 `Object` 而不是泛型 `E`。

2）新增 `contains(Object o)` 方法，判断元素是否在 `Arraylist` 中。注意参数为 `Object` 而不是泛型 `E`。

3）新增 `toString()` 方法，方便对 `Arraylist` 进行打印。

4）新增 `set(int index, E element)` 方法，方便对 `Arraylist` 元素的更改。

你知道，`Arraylist<Wanger> list = new Arraylist<Wangxiaoer>();` 这样的语句是无法通过编译的，尽管 Wangxiaoer 是 Wanger 的子类。但如果我们确实需要这种 “向上转型” 的关系，该怎么办呢？这时候就需要通配符来发挥作用了。

利用 `<? extends Wanger>` 形式的通配符，可以实现泛型的向上转型，来看例子。

```java
Arraylist<? extends Wanger> list2 = new Arraylist<>(4);
list2.add(null);
// list2.add(new Wanger());
// list2.add(new Wangxiaoer());

Wanger w2 = list2.get(0);
// Wangxiaoer w3 = list2.get(1);
```

list2 的类型是 `Arraylist<? extends Wanger>`，翻译一下就是，list2 是一个 `Arraylist`，其类型是 `Wanger` 及其子类。

注意，“关键”来了！list2 并不允许通过 `add(E e)` 方法向其添加 `Wanger` 或者 `Wangxiaoer` 的对象，唯一例外的是 `null`。为什么不能存呢？原因还有待探究（苦涩）。

那就奇了怪了，既然不让存放元素，那要 `Arraylist<? extends Wanger>` 这样的 list2 有什么用呢？

虽然不能通过 `add(E e)` 方法往 list2 中添加元素，但可以给它赋值。

```java
Arraylist<Wanger> list = new Arraylist<>(4);

Wanger wanger = new Wanger();
list.add(wanger);

Wangxiaoer wangxiaoer = new Wangxiaoer();
list.add(wangxiaoer);

Arraylist<? extends Wanger> list2 = list;

Wanger w2 = list2.get(1);
System.out.println(w2);

System.out.println(list2.indexOf(wanger));
System.out.println(list2.contains(new Wangxiaoer()));
```

`Arraylist<? extends Wanger> list2 = list;` 语句把 list 的值赋予了 list2，此时 `list2 == list`。由于 list2 不允许往其添加其他元素，所以此时它是安全的——我们可以从容地对 list2 进行 `get()`、`indexOf()` 和 `contains()`。想一想，如果可以向 list2 添加元素的话，这 3 个方法反而变得不太安全，它们的值可能就会变。

利用 `<? super Wanger>` 形式的通配符，可以向 Arraylist 中存入父类是 `Wanger` 的元素，来看例子。

```java
Arraylist<? super Wanger> list3 = new Arraylist<>(4);
list3.add(new Wanger());
list3.add(new Wangxiaoer());

// Wanger w3 = list3.get(0);
```

需要注意的是，无法从 `Arraylist<? super Wanger>` 这样类型的 list3 中取出数据。为什么不能取呢？原因还有待探究（再次苦涩）。

虽然原因有待探究，但结论是明确的：`<? extends T>` 可以取数据，`<? super T>` 可以存数据。那么利用这一点，我们就可以实现数组的拷贝——`<? extends T>` 作为源（保证源不会发生变化），`<? super T>` 作为目标（可以保存值）。

```java
public class Collections {
	public static <T> void copy(Arraylist<? super T> dest, Arraylist<? extends T> src) {
		for (int i = 0; i < src.size(); i++)
			dest.set(i, src.get(i));
	}
}
```

### 06、故事的未完待续

“二哥，你今天苦涩了啊！嘿嘿。竟然还有你需要探究的。”三妹开始调皮了起来。

“......”

“不要不好意思嘛，等三妹啥时候探究出来了原因，三妹给你讲，好不好？”三妹越说越来劲了。

“......”

“二哥，你还在想泛型通配符的原因啊！那三妹先去预习下个知识点了啊，你思考完了，再给我讲！”三妹看着我陷入了沉思，扔下这句话走了。

“......”








上一篇：[Java面试官：兄弟，你确定double精度比float低吗？](http://www.itwanger.com/java/2019/11/14/java-double-float.html)

下一篇：[再谈 Java 的继承和超类 Object](http://www.itwanger.com/java/2019/11/14/java-extends.html)


谢谢大家的阅读，原创不易，喜欢就随手点个赞👍，这将是我最强的写作动力。如果觉得文章对你有点帮助，还挺有趣，就关注一下我的公众号「**沉默王二**」。