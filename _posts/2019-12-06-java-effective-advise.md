---
layout: post
category: java
title: 推荐给Java程序员的优质书单：《Effective Java》
tagline: by 沉默王二
tag: java
---

《Effective Java》除了翻译让众多 Java 程序员诟病之外，再没有任何缺点了（有读者戏称：“这本书为翻译们作出了杰出的表率”）。其目标是帮助 Java 程序员更加有效地使用 Java 编程语言及其基本类库，主要涉及到 `java.lang`、`java.util`、 `java.io` 包下面的类。

<!--more-->

尽管《Head First Java》也非常的厚，至少比我的脸皮后，但趣味性就要甩前面两本好几条街了。这年头，大家都没时间读枯燥的技术书，尤其是厚的。上一张图大家感受一下《Head First Java》的调皮吧。

![](http://www.itwanger.com/assets/images/2019/12/effective-java-1.png)

《Effective Java》第三版一共包含了 90 条极具实用价值的经验规则，每条规则都值得 Java 程序员在实战中去参照。这本书不需要按部就班地从头到尾读，可以随意挑选任意小节进行阅读，因为每条规则相对都是独立的，尽管它们之间会交叉引用，但并不妨碍我们随心所欲地阅读。

作者 Josh Bloch 非常的牛逼，曾是 Google 的首席 Java 架构师，《Java开发者杂志》将他列为世界上最顶尖的四十名软件人物之一。Java 之父詹姆斯·高斯林对《Effective Java》的评价也非常的高。

我这里整理了一份第三版的中文在线翻译文档，大家可以参照一下。

*   [01\. 考虑使用静态工厂方法替代构造方法.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/01.%20%E8%80%83%E8%99%91%E4%BD%BF%E7%94%A8%E9%9D%99%E6%80%81%E5%B7%A5%E5%8E%82%E6%96%B9%E6%B3%95%E6%9B%BF%E4%BB%A3%E6%9E%84%E9%80%A0%E6%96%B9%E6%B3%95.md)
*   [02\. 当构造方法参数过多时使用builder模式.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/02.%20%E5%BD%93%E6%9E%84%E9%80%A0%E6%96%B9%E6%B3%95%E5%8F%82%E6%95%B0%E8%BF%87%E5%A4%9A%E6%97%B6%E4%BD%BF%E7%94%A8builder%E6%A8%A1%E5%BC%8F.md)
*   [03\. 使用私有构造方法或枚类实现Singleton属性.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/03.%20%E4%BD%BF%E7%94%A8%E7%A7%81%E6%9C%89%E6%9E%84%E9%80%A0%E6%96%B9%E6%B3%95%E6%88%96%E6%9E%9A%E7%B1%BB%E5%AE%9E%E7%8E%B0Singleton%E5%B1%9E%E6%80%A7.md)
*   [04\. 使用私有构造方法执行非实例化.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/04.%20%E4%BD%BF%E7%94%A8%E7%A7%81%E6%9C%89%E6%9E%84%E9%80%A0%E6%96%B9%E6%B3%95%E6%89%A7%E8%A1%8C%E9%9D%9E%E5%AE%9E%E4%BE%8B%E5%8C%96.md)
*   [05\. 使用依赖注入取代硬连接资源(hardwiring resources).md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/05.%20%E4%BD%BF%E7%94%A8%E4%BE%9D%E8%B5%96%E6%B3%A8%E5%85%A5%E5%8F%96%E4%BB%A3%E7%A1%AC%E8%BF%9E%E6%8E%A5%E8%B5%84%E6%BA%90(hardwiring%20resources).md)
*   [06\. 避免创建不必要的对象.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/06.%20%E9%81%BF%E5%85%8D%E5%88%9B%E5%BB%BA%E4%B8%8D%E5%BF%85%E8%A6%81%E7%9A%84%E5%AF%B9%E8%B1%A1.md)
*   [07\. 消除过期的对象引用.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/07.%20%E6%B6%88%E9%99%A4%E8%BF%87%E6%9C%9F%E7%9A%84%E5%AF%B9%E8%B1%A1%E5%BC%95%E7%94%A8.md)
*   [08\. 避免使用Finalizer和Cleaner机制.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/08.%20%E9%81%BF%E5%85%8D%E4%BD%BF%E7%94%A8Finalizer%E5%92%8CCleaner%E6%9C%BA%E5%88%B6.md)
*   [09\. 使用try-with-resources语句替代try-finally语句.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/09.%20%E4%BD%BF%E7%94%A8try-with-resources%E8%AF%AD%E5%8F%A5%E6%9B%BF%E4%BB%A3try-finally%E8%AF%AD%E5%8F%A5.md)
*   [10\. 重写equals方法时遵守通用约定.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/10.%20%E9%87%8D%E5%86%99equals%E6%96%B9%E6%B3%95%E6%97%B6%E9%81%B5%E5%AE%88%E9%80%9A%E7%94%A8%E7%BA%A6%E5%AE%9A.md)
*   [11\. 重写equals方法时同时也要重写hashcode方法.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/11.%20%E9%87%8D%E5%86%99equals%E6%96%B9%E6%B3%95%E6%97%B6%E5%90%8C%E6%97%B6%E4%B9%9F%E8%A6%81%E9%87%8D%E5%86%99hashcode%E6%96%B9%E6%B3%95.md)
*   [12\. 始终重写 toString 方法.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/12.%20%E5%A7%8B%E7%BB%88%E9%87%8D%E5%86%99%20toString%20%E6%96%B9%E6%B3%95.md)
*   [13\. 谨慎地重写 clone 方法.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/13.%20%E8%B0%A8%E6%85%8E%E5%9C%B0%E9%87%8D%E5%86%99%20clone%20%E6%96%B9%E6%B3%95.md)
*   [14\. 考虑实现Comparable接口.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/14.%20%E8%80%83%E8%99%91%E5%AE%9E%E7%8E%B0Comparable%E6%8E%A5%E5%8F%A3.md)
*   [15\. 使类和成员的可访问性最小化.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/15.%20%E4%BD%BF%E7%B1%BB%E5%92%8C%E6%88%90%E5%91%98%E7%9A%84%E5%8F%AF%E8%AE%BF%E9%97%AE%E6%80%A7%E6%9C%80%E5%B0%8F%E5%8C%96.md)
*   [16\. 在公共类中使用访问方法而不是公共属性.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/16.%20%E5%9C%A8%E5%85%AC%E5%85%B1%E7%B1%BB%E4%B8%AD%E4%BD%BF%E7%94%A8%E8%AE%BF%E9%97%AE%E6%96%B9%E6%B3%95%E8%80%8C%E4%B8%8D%E6%98%AF%E5%85%AC%E5%85%B1%E5%B1%9E%E6%80%A7.md)
*   [17\. 最小化可变性.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/17.%20%E6%9C%80%E5%B0%8F%E5%8C%96%E5%8F%AF%E5%8F%98%E6%80%A7.md)
*   [18\. 组合优于继承.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/18.%20%E7%BB%84%E5%90%88%E4%BC%98%E4%BA%8E%E7%BB%A7%E6%89%BF.md)
*   [19\. 如使用继承则设计，应当文档说明，否则不该使用.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/19.%20%E5%A6%82%E4%BD%BF%E7%94%A8%E7%BB%A7%E6%89%BF%E5%88%99%E8%AE%BE%E8%AE%A1%EF%BC%8C%E5%BA%94%E5%BD%93%E6%96%87%E6%A1%A3%E8%AF%B4%E6%98%8E%EF%BC%8C%E5%90%A6%E5%88%99%E4%B8%8D%E8%AF%A5%E4%BD%BF%E7%94%A8.md)
*   [20\. 接口优于抽象类.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/20.%20%E6%8E%A5%E5%8F%A3%E4%BC%98%E4%BA%8E%E6%8A%BD%E8%B1%A1%E7%B1%BB.md)
*   [21\. 为后代设计接口.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/21.%20%E4%B8%BA%E5%90%8E%E4%BB%A3%E8%AE%BE%E8%AE%A1%E6%8E%A5%E5%8F%A3.md)
*   [22\. 接口仅用来定义类型.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/22.%20%E6%8E%A5%E5%8F%A3%E4%BB%85%E7%94%A8%E6%9D%A5%E5%AE%9A%E4%B9%89%E7%B1%BB%E5%9E%8B.md)
*   [23\. 优先使用类层次而不是标签类.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/23.%20%E4%BC%98%E5%85%88%E4%BD%BF%E7%94%A8%E7%B1%BB%E5%B1%82%E6%AC%A1%E8%80%8C%E4%B8%8D%E6%98%AF%E6%A0%87%E7%AD%BE%E7%B1%BB.md)
*   [24\. 优先考虑静态成员类.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/24.%20%E4%BC%98%E5%85%88%E8%80%83%E8%99%91%E9%9D%99%E6%80%81%E6%88%90%E5%91%98%E7%B1%BB.md)
*   [25\. 将源文件限制为单个顶级类.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/25.%20%E5%B0%86%E6%BA%90%E6%96%87%E4%BB%B6%E9%99%90%E5%88%B6%E4%B8%BA%E5%8D%95%E4%B8%AA%E9%A1%B6%E7%BA%A7%E7%B1%BB.md)
*   [26\. 不要使用原始类型.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/26.%20%E4%B8%8D%E8%A6%81%E4%BD%BF%E7%94%A8%E5%8E%9F%E5%A7%8B%E7%B1%BB%E5%9E%8B.md)
*   [27\. 消除非检查警告.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/27.%20%E6%B6%88%E9%99%A4%E9%9D%9E%E6%A3%80%E6%9F%A5%E8%AD%A6%E5%91%8A.md)
* [28\. 列表优于数组.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/28.%20%E5%88%97%E8%A1%A8%E4%BC%98%E4%BA%8E%E6%95%B0%E7%BB%84.md)
*   [29\. 优先考虑泛型.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/29.%20%E4%BC%98%E5%85%88%E8%80%83%E8%99%91%E6%B3%9B%E5%9E%8B.md)
*   [30\. 优先使用泛型方法.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/30.%20%E4%BC%98%E5%85%88%E4%BD%BF%E7%94%A8%E6%B3%9B%E5%9E%8B%E6%96%B9%E6%B3%95.md)
*   [31\. 使用限定通配符来增加API的灵活性.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/31.%20%E4%BD%BF%E7%94%A8%E9%99%90%E5%AE%9A%E9%80%9A%E9%85%8D%E7%AC%A6%E6%9D%A5%E5%A2%9E%E5%8A%A0API%E7%9A%84%E7%81%B5%E6%B4%BB%E6%80%A7.md)
*   [32\. 合理地结合泛型和可变参数.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/32.%20%E5%90%88%E7%90%86%E5%9C%B0%E7%BB%93%E5%90%88%E6%B3%9B%E5%9E%8B%E5%92%8C%E5%8F%AF%E5%8F%98%E5%8F%82%E6%95%B0.md)
*   [33\. 优先考虑类型安全的异构容器.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/33.%20%E4%BC%98%E5%85%88%E8%80%83%E8%99%91%E7%B1%BB%E5%9E%8B%E5%AE%89%E5%85%A8%E7%9A%84%E5%BC%82%E6%9E%84%E5%AE%B9%E5%99%A8.md)
*   [34\. 使用枚举类型替代整型常量.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/34.%20%E4%BD%BF%E7%94%A8%E6%9E%9A%E4%B8%BE%E7%B1%BB%E5%9E%8B%E6%9B%BF%E4%BB%A3%E6%95%B4%E5%9E%8B%E5%B8%B8%E9%87%8F.md)
*   [35\. 使用实例属性替代序数.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/35.%20%E4%BD%BF%E7%94%A8%E5%AE%9E%E4%BE%8B%E5%B1%9E%E6%80%A7%E6%9B%BF%E4%BB%A3%E5%BA%8F%E6%95%B0.md)
*   [36\. 使用EnumSet替代位属性.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/36.%20%E4%BD%BF%E7%94%A8EnumSet%E6%9B%BF%E4%BB%A3%E4%BD%8D%E5%B1%9E%E6%80%A7.md)
*   [37\. 使用EnumMap替代序数索引.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/37.%20%E4%BD%BF%E7%94%A8EnumMap%E6%9B%BF%E4%BB%A3%E5%BA%8F%E6%95%B0%E7%B4%A2%E5%BC%95.md)
* [38\. 使用接口模拟可扩展的枚举.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/38.%20%E4%BD%BF%E7%94%A8%E6%8E%A5%E5%8F%A3%E6%A8%A1%E6%8B%9F%E5%8F%AF%E6%89%A9%E5%B1%95%E7%9A%84%E6%9E%9A%E4%B8%BE.md)
*   [39\. 注解优于命名模式.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/39.%20%E6%B3%A8%E8%A7%A3%E4%BC%98%E4%BA%8E%E5%91%BD%E5%90%8D%E6%A8%A1%E5%BC%8F.md)
*   [40\. 始终使用Override注解.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/40.%20%E5%A7%8B%E7%BB%88%E4%BD%BF%E7%94%A8Override%E6%B3%A8%E8%A7%A3.md)
*   [41\. 使用标记接口定义类型.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/41.%20%E4%BD%BF%E7%94%A8%E6%A0%87%E8%AE%B0%E6%8E%A5%E5%8F%A3%E5%AE%9A%E4%B9%89%E7%B1%BB%E5%9E%8B.md)
*   [42\. lambda表达式优于匿名类.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/42.%20lambda%E8%A1%A8%E8%BE%BE%E5%BC%8F%E4%BC%98%E4%BA%8E%E5%8C%BF%E5%90%8D%E7%B1%BB.md)
*   [43\. 方法引用优于lambda表达式.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/43.%20%E6%96%B9%E6%B3%95%E5%BC%95%E7%94%A8%E4%BC%98%E4%BA%8Elambda%E8%A1%A8%E8%BE%BE%E5%BC%8F.md)
*   [44\. 优先使用标准的函数式接口.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/44.%20%E4%BC%98%E5%85%88%E4%BD%BF%E7%94%A8%E6%A0%87%E5%87%86%E7%9A%84%E5%87%BD%E6%95%B0%E5%BC%8F%E6%8E%A5%E5%8F%A3.md)
*   [45\. 明智审慎地使用Stream.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/45.%20%E6%98%8E%E6%99%BA%E5%AE%A1%E6%85%8E%E5%9C%B0%E4%BD%BF%E7%94%A8Stream.md)
*   [46\. 优先考虑流中无副作用的函数.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/46.%20%E4%BC%98%E5%85%88%E8%80%83%E8%99%91%E6%B5%81%E4%B8%AD%E6%97%A0%E5%89%AF%E4%BD%9C%E7%94%A8%E7%9A%84%E5%87%BD%E6%95%B0.md)
*   [47\. 优先使用Collection而不是Stream来作为方法的返回类型.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/47.%20%E4%BC%98%E5%85%88%E4%BD%BF%E7%94%A8Collection%E8%80%8C%E4%B8%8D%E6%98%AFStream%E6%9D%A5%E4%BD%9C%E4%B8%BA%E6%96%B9%E6%B3%95%E7%9A%84%E8%BF%94%E5%9B%9E%E7%B1%BB%E5%9E%8B.md)
*   [48\. 谨慎使用流并行.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/48.%20%E8%B0%A8%E6%85%8E%E4%BD%BF%E7%94%A8%E6%B5%81%E5%B9%B6%E8%A1%8C.md)
*   [49\. 检查参数有效性.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/49.%20%E6%A3%80%E6%9F%A5%E5%8F%82%E6%95%B0%E6%9C%89%E6%95%88%E6%80%A7.md)
*   [50\. 必要时进行防御性拷贝.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/50.%20%E5%BF%85%E8%A6%81%E6%97%B6%E8%BF%9B%E8%A1%8C%E9%98%B2%E5%BE%A1%E6%80%A7%E6%8B%B7%E8%B4%9D.md)
*   [51\. 仔细设计方法签名.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/51.%20%E4%BB%94%E7%BB%86%E8%AE%BE%E8%AE%A1%E6%96%B9%E6%B3%95%E7%AD%BE%E5%90%8D.md)
*   [52\. 明智审慎地使用重载.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/52.%20%E6%98%8E%E6%99%BA%E5%AE%A1%E6%85%8E%E5%9C%B0%E4%BD%BF%E7%94%A8%E9%87%8D%E8%BD%BD.md)
*   [53\. 明智审慎地使用可变参数.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/53.%20%E6%98%8E%E6%99%BA%E5%AE%A1%E6%85%8E%E5%9C%B0%E4%BD%BF%E7%94%A8%E5%8F%AF%E5%8F%98%E5%8F%82%E6%95%B0.md)
*   [54\. 返回空的数组或集合，不要返回 null.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/54.%20%E8%BF%94%E5%9B%9E%E7%A9%BA%E7%9A%84%E6%95%B0%E7%BB%84%E6%88%96%E9%9B%86%E5%90%88%EF%BC%8C%E4%B8%8D%E8%A6%81%E8%BF%94%E5%9B%9E%20null.md)
*   [55\. 明智审慎地返回 Optional.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/55.%20%E6%98%8E%E6%99%BA%E5%AE%A1%E6%85%8E%E5%9C%B0%E8%BF%94%E5%9B%9E%20Optional.md)
*   [56\. 为所有已公开的 API 元素编写文档注释.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/56.%20%E4%B8%BA%E6%89%80%E6%9C%89%E5%B7%B2%E5%85%AC%E5%BC%80%E7%9A%84%20API%20%E5%85%83%E7%B4%A0%E7%BC%96%E5%86%99%E6%96%87%E6%A1%A3%E6%B3%A8%E9%87%8A.md)
*   [57\. 最小化局部变量的作用域.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/57.%20%E6%9C%80%E5%B0%8F%E5%8C%96%E5%B1%80%E9%83%A8%E5%8F%98%E9%87%8F%E7%9A%84%E4%BD%9C%E7%94%A8%E5%9F%9F.md)
*   [58\. for-each 循环优于传统 for 循环.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/58.%20for-each%20%E5%BE%AA%E7%8E%AF%E4%BC%98%E4%BA%8E%E4%BC%A0%E7%BB%9F%20for%20%E5%BE%AA%E7%8E%AF.md)
*   [59\. 了解并使用库.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/59.%20%E4%BA%86%E8%A7%A3%E5%B9%B6%E4%BD%BF%E7%94%A8%E5%BA%93.md)
*   [60\. 若需要精确答案就应避免使用 float 和 double 类型.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/60.%20%E8%8B%A5%E9%9C%80%E8%A6%81%E7%B2%BE%E7%A1%AE%E7%AD%94%E6%A1%88%E5%B0%B1%E5%BA%94%E9%81%BF%E5%85%8D%E4%BD%BF%E7%94%A8%20float%20%E5%92%8C%20double%20%E7%B1%BB%E5%9E%8B.md)
*   [61\. 基本数据类型优于包装类.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/61.%20%E5%9F%BA%E6%9C%AC%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B%E4%BC%98%E4%BA%8E%E5%8C%85%E8%A3%85%E7%B1%BB.md)
*   [62\. 当使用其他类型更合适时应避免使用字符串.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/62.%20%E5%BD%93%E4%BD%BF%E7%94%A8%E5%85%B6%E4%BB%96%E7%B1%BB%E5%9E%8B%E6%9B%B4%E5%90%88%E9%80%82%E6%97%B6%E5%BA%94%E9%81%BF%E5%85%8D%E4%BD%BF%E7%94%A8%E5%AD%97%E7%AC%A6%E4%B8%B2.md)
*   [63\. 当心字符串连接引起的性能问题.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/63.%20%E5%BD%93%E5%BF%83%E5%AD%97%E7%AC%A6%E4%B8%B2%E8%BF%9E%E6%8E%A5%E5%BC%95%E8%B5%B7%E7%9A%84%E6%80%A7%E8%83%BD%E9%97%AE%E9%A2%98.md)
*   [64\. 通过接口引用对象.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/64.%20%E9%80%9A%E8%BF%87%E6%8E%A5%E5%8F%A3%E5%BC%95%E7%94%A8%E5%AF%B9%E8%B1%A1.md)
*   [65\. 接口优于反射.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/65.%20%E6%8E%A5%E5%8F%A3%E4%BC%98%E4%BA%8E%E5%8F%8D%E5%B0%84.md)
*   [66\. 明智审慎地本地方法.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/66.%20%E6%98%8E%E6%99%BA%E5%AE%A1%E6%85%8E%E5%9C%B0%E6%9C%AC%E5%9C%B0%E6%96%B9%E6%B3%95.md)
*   [67\. 明智审慎地进行优化.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/67.%20%E6%98%8E%E6%99%BA%E5%AE%A1%E6%85%8E%E5%9C%B0%E8%BF%9B%E8%A1%8C%E4%BC%98%E5%8C%96.md)
*   [68\. 遵守被广泛认可的命名约定.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/68.%20%E9%81%B5%E5%AE%88%E8%A2%AB%E5%B9%BF%E6%B3%9B%E8%AE%A4%E5%8F%AF%E7%9A%84%E5%91%BD%E5%90%8D%E7%BA%A6%E5%AE%9A.md)
*   [69\. 只针对异常的情况下才使用异常.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/69.%20%E5%8F%AA%E9%92%88%E5%AF%B9%E5%BC%82%E5%B8%B8%E7%9A%84%E6%83%85%E5%86%B5%E4%B8%8B%E6%89%8D%E4%BD%BF%E7%94%A8%E5%BC%82%E5%B8%B8.md)
*   [70\. 对可恢复的情况使用受检异常，对编程错误使用运行时异常.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/70.%20%E5%AF%B9%E5%8F%AF%E6%81%A2%E5%A4%8D%E7%9A%84%E6%83%85%E5%86%B5%E4%BD%BF%E7%94%A8%E5%8F%97%E6%A3%80%E5%BC%82%E5%B8%B8%EF%BC%8C%E5%AF%B9%E7%BC%96%E7%A8%8B%E9%94%99%E8%AF%AF%E4%BD%BF%E7%94%A8%E8%BF%90%E8%A1%8C%E6%97%B6%E5%BC%82%E5%B8%B8.md)
*   [71\. 避免不必要的使用受检异常.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/71.%20%E9%81%BF%E5%85%8D%E4%B8%8D%E5%BF%85%E8%A6%81%E7%9A%84%E4%BD%BF%E7%94%A8%E5%8F%97%E6%A3%80%E5%BC%82%E5%B8%B8.md)
*   [72\. 优先使用标准的异常.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/72.%20%E4%BC%98%E5%85%88%E4%BD%BF%E7%94%A8%E6%A0%87%E5%87%86%E7%9A%84%E5%BC%82%E5%B8%B8.md)
*   [73\. 抛出与抽象对应的异常.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/73.%20%E6%8A%9B%E5%87%BA%E4%B8%8E%E6%8A%BD%E8%B1%A1%E5%AF%B9%E5%BA%94%E7%9A%84%E5%BC%82%E5%B8%B8.md)
*   [74\. 每个方法抛出的异常都需要创建文档.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/74.%20%E6%AF%8F%E4%B8%AA%E6%96%B9%E6%B3%95%E6%8A%9B%E5%87%BA%E7%9A%84%E5%BC%82%E5%B8%B8%E9%83%BD%E9%9C%80%E8%A6%81%E5%88%9B%E5%BB%BA%E6%96%87%E6%A1%A3.md)
*   [75\. 在细节消息中包含失败一捕获信息.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/75.%20%E5%9C%A8%E7%BB%86%E8%8A%82%E6%B6%88%E6%81%AF%E4%B8%AD%E5%8C%85%E5%90%AB%E5%A4%B1%E8%B4%A5%E4%B8%80%E6%8D%95%E8%8E%B7%E4%BF%A1%E6%81%AF.md)
*   [76\. 保持失败原子性.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/76.%20%E4%BF%9D%E6%8C%81%E5%A4%B1%E8%B4%A5%E5%8E%9F%E5%AD%90%E6%80%A7.md)
*   [77\. 不要忽略异常.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/77.%20%E4%B8%8D%E8%A6%81%E5%BF%BD%E7%95%A5%E5%BC%82%E5%B8%B8.md)
*   [78\. 同步访问共享的可变数据.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/78.%20%E5%90%8C%E6%AD%A5%E8%AE%BF%E9%97%AE%E5%85%B1%E4%BA%AB%E7%9A%84%E5%8F%AF%E5%8F%98%E6%95%B0%E6%8D%AE.md)
*   [79\. 避免过度同步.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/79.%20%E9%81%BF%E5%85%8D%E8%BF%87%E5%BA%A6%E5%90%8C%E6%AD%A5.md)
*   [80\. executor 、task 和 stream 优先于线程.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/80.%20executor%20%E3%80%81task%20%E5%92%8C%20stream%20%E4%BC%98%E5%85%88%E4%BA%8E%E7%BA%BF%E7%A8%8B.md)
*   [81\. 相比 wait 和 notify 优先使用并发工具.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/81.%20%E7%9B%B8%E6%AF%94%20wait%20%E5%92%8C%20notify%20%E4%BC%98%E5%85%88%E4%BD%BF%E7%94%A8%E5%B9%B6%E5%8F%91%E5%B7%A5%E5%85%B7.md)
*   [82\. 文档应包含线程安全属性.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/82.%20%E6%96%87%E6%A1%A3%E5%BA%94%E5%8C%85%E5%90%AB%E7%BA%BF%E7%A8%8B%E5%AE%89%E5%85%A8%E5%B1%9E%E6%80%A7.md)
*   [83\. 明智审慎的使用延迟初始化.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/83.%20%E6%98%8E%E6%99%BA%E5%AE%A1%E6%85%8E%E7%9A%84%E4%BD%BF%E7%94%A8%E5%BB%B6%E8%BF%9F%E5%88%9D%E5%A7%8B%E5%8C%96.md)
*   [84\. 不要依赖线程调度器.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/84.%20%E4%B8%8D%E8%A6%81%E4%BE%9D%E8%B5%96%E7%BA%BF%E7%A8%8B%E8%B0%83%E5%BA%A6%E5%99%A8.md)
*   [85\. 优先选择 Java 序列化的替代方案.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/85.%20%E4%BC%98%E5%85%88%E9%80%89%E6%8B%A9%20Java%20%E5%BA%8F%E5%88%97%E5%8C%96%E7%9A%84%E6%9B%BF%E4%BB%A3%E6%96%B9%E6%A1%88.md)
*   [86\. 非常谨慎地实现 Serializable.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/86.%20%E9%9D%9E%E5%B8%B8%E8%B0%A8%E6%85%8E%E5%9C%B0%E5%AE%9E%E7%8E%B0%20Serializable.md)
*   [87\. 考虑使用自定义的序列化形式.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/87.%20%E8%80%83%E8%99%91%E4%BD%BF%E7%94%A8%E8%87%AA%E5%AE%9A%E4%B9%89%E7%9A%84%E5%BA%8F%E5%88%97%E5%8C%96%E5%BD%A2%E5%BC%8F.md)
*   [88\. 保护性的编写 readObject 方法.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/88.%20%E4%BF%9D%E6%8A%A4%E6%80%A7%E7%9A%84%E7%BC%96%E5%86%99%20readObject%20%E6%96%B9%E6%B3%95.md)
*   [89\. 对于实例控制，枚举类型优于 readResolve.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/89.%20%E5%AF%B9%E4%BA%8E%E5%AE%9E%E4%BE%8B%E6%8E%A7%E5%88%B6%EF%BC%8C%E6%9E%9A%E4%B8%BE%E7%B1%BB%E5%9E%8B%E4%BC%98%E4%BA%8E%20readResolve.md)
*   [90\. 考虑用序列化代理代替序列化实例.md](https://github.com/sjsdfg/effective-java-3rd-chinese/blob/master/docs/notes/90.%20%E8%80%83%E8%99%91%E7%94%A8%E5%BA%8F%E5%88%97%E5%8C%96%E4%BB%A3%E7%90%86%E4%BB%A3%E6%9B%BF%E5%BA%8F%E5%88%97%E5%8C%96%E5%AE%9E%E4%BE%8B.md)

如果需要 PDF 版，可以微信搜索「沉默王二」，关注公众号后回复「effectivejava」即可获取。强烈大家购买纸质版，读起来不费眼。

我在读这本书的时候，曾写过两篇文章，大家也可以阅读一下。

[为什么要将局部变量的作用域最小化？](http://www.itwanger.com/java/2019/12/04/java-variable-zuixiaohua.html)
[面试官：兄弟，说说基本类型和包装类型的区别吧](http://www.itwanger.com/java/2019/11/01/java-int-integer.html)

---------
往期推荐书单：

[《Java编程思想》](http://www.itwanger.com/java/2019/10/30/think-java-book-read-jianyi.html)

[《Java核心技术卷一》](http://www.itwanger.com/java/2019/11/14/java-core-advise.html)

[《Head First Java》](http://www.itwanger.com/java/2019/12/04/java-head-first-advise.html)

[Effective Java](http://www.itwanger.com/java/2019/12/06/java-effective-advise.html)