---
layout: post
category: java
title: Stack Overflow 上 250W 浏览量的一个问题：你对象丢了
tagline: by 沉默王二
tag: java
---

在逛 Stack Overflow 的时候，发现最火的问题竟然是：[什么是 NullPointerException](http://www.itwanger.com/java/2019/12/03/java-null-pointer-exception.html)（`java.lang.NullPointerException`），它是由什么原因导致的，有没有好的方法或者工具可以追踪它发生的原因？

<!--more-->

真没想到，这个问题浏览的次数多达 250 万次！所以，我想是时候把最高赞的回答整理一下分享出来了。请随我来。

声明引用变量（即对象）时，实际上是创建了一个指向对象的指针。请看以下代码：

```java
int x;
x = 10;
```

第一行代码声明了一个名为 x 的变量（int 类型），Java 会把它初始化为 0。第二行代码把 x 赋值为 10，意味着 10 将被写入到 x 所指向的内存位置上。

但是呢，当我们尝试声明一个引用类型时，情况将会有所不同。

```java
Integer num;
num = new Integer(10);
```

第一行代码声明了一个名为 num 的变量（Integer 类型），Java 把它初始化为 null，表示“什么都没有指向 ”。

第二行代码中，new 关键字创建了一个 Integer 类型的对象，并将变量 num 指向该对象。

当我们声明了一个变量，却没有将该变量指向任何创建的对象，然后就使用它的时候，NullPointerException 就发生了。大多数情况下，编译器会发现这个问题，并且提醒我们“xxxx may not have been initialized”。

假如有这样一段代码：

```java
public void doSomething(SomeObject obj) {
   //do something to obj
}
```

在这种情况下，我们没有创建对象 obj，而是假设它在 `doSomething()` 方法被调用之前就创建了。

现在假设在此之前它没有创建。我们这样调用 `doSomething()` 方法：

```java
doSomething(null);
```

这就意味着 `doSomething()` 方法的参数 obj 为 null。如果该方法还要使用 obj 继续做点什么，最好提前抛出 `NullPointerException`，因为开发者需要该信息来进行调试。

还有另外一种替代方法，判断 obj 是不是 null，如果是，就小心行事，做某些不会引起 NullPointerException 的事情；如果不是，就放心大胆地做该做的事情。

```java
/**
  * @param obj An optional foo for ____. May be null, in which case 
  *  the result will be ____.
  */
public void doSomething(SomeObject obj) {
    if(obj != null) {
       //do something
    } else {
       //do something else
    }
}
```

那假如程序真的出现了 NullPointerException，该怎么追踪堆栈信息，找到错误的根源呢？

简单来说，堆栈信息是应用程序在引发 Exception 时调用的方法列表，可以准确地定位到错误发生的根源。就像下面这样。

```
Exception in thread "main" java.lang.NullPointerException
        at com.example.myproject.Book.getTitle(Book.java:16)
        at com.example.myproject.Author.getBookTitles(Author.java:25)
        at com.example.myproject.Bootstrap.main(Bootstrap.java:14)
```

就上面这个堆栈信息来说，错误发生在“at ...”列表处，第一个“at 处”就是错误最初发生的位置。

```
at com.example.myproject.Book.getTitle(Book.java:16)
```

为了调试，我们可以打开 Book.java 类的第 16 行，它可能是：

```java
15   public String getTitle() {
16      System.out.println(title.toString());
17      return title;
18   }
```

从这段代码中可以看得出，错误的原因很可能是因为 title 为 null。

有时候，应用程序会捕获一个异常，然后把它作为另外一种类型的异常抛出。就像下面这样：

```java
34   public void getBookIds(int id) {
35      try {
36         book.getId(id);    // 这里可能会引发 NullPointerException
37      } catch (NullPointerException e) {
38         throw new IllegalStateException("A book has a null property", e)
39      }
40   }
```

此时的堆栈信息可能是下面这样的：

```
Exception in thread "main" java.lang.IllegalStateException: A book has a null property
        at com.example.myproject.Author.getBookIds(Author.java:38)
        at com.example.myproject.Bootstrap.main(Bootstrap.java:14)
Caused by: java.lang.NullPointerException
        at com.example.myproject.Book.getId(Book.java:22)
        at com.example.myproject.Author.getBookIds(Author.java:36)
        ... 1 more
```

和之前堆栈信息有所不同的是，这里多了一个“Caused by”；有时候还会有更多的“Caused by”。在这种情况下，我们通常需要追本溯源，找到最深层次的那个“cause”——它就是堆栈信息中最下面的那个。

```
Caused by: java.lang.NullPointerException <-- 根本原因
        at com.example.myproject.Book.getId(Book.java:22) 
```

同样，我们需要查看一下 Book.java 的第 22 行，找到可能引发 `NullPointerException` 的原因。

有时候，堆栈信息要比上面的例子凌乱得多。参考下面这个。

```
javax.servlet.ServletException: Something bad happened
    at com.example.myproject.OpenSessionInViewFilter.doFilter(OpenSessionInViewFilter.java:60)
    at org.mortbay.jetty.servlet.ServletHandler$CachedChain.doFilter(ServletHandler.java:1157)
    at com.example.myproject.ExceptionHandlerFilter.doFilter(ExceptionHandlerFilter.java:28)
    at org.mortbay.jetty.servlet.ServletHandler$CachedChain.doFilter(ServletHandler.java:1157)
    at com.example.myproject.OutputBufferFilter.doFilter(OutputBufferFilter.java:33)
    at org.mortbay.jetty.Server.handle(Server.java:326)
    at org.mortbay.jetty.HttpConnection.handleRequest(HttpConnection.java:542)
    at org.mortbay.jetty.HttpConnection$RequestHandler.content(HttpConnection.java:943)
    at org.mortbay.jetty.HttpParser.parseNext(HttpParser.java:756)
    at org.mortbay.jetty.HttpParser.parseAvailable(HttpParser.java:218)
    at org.mortbay.jetty.HttpConnection.handle(HttpConnection.java:404)
    at org.mortbay.jetty.bio.SocketConnector$Connection.run(SocketConnector.java:228)
    at org.mortbay.thread.QueuedThreadPool$PoolThread.run(QueuedThreadPool.java:582)
Caused by: com.example.myproject.MyProjectServletException
    at com.example.myproject.MyServlet.doPost(MyServlet.java:169)
    at javax.servlet.http.HttpServlet.service(HttpServlet.java:727)
    at javax.servlet.http.HttpServlet.service(HttpServlet.java:820)
    at org.mortbay.jetty.servlet.ServletHolder.handle(ServletHolder.java:511)
    at org.mortbay.jetty.servlet.ServletHandler$CachedChain.doFilter(ServletHandler.java:1166)
    at org.hibernate.persister.entity.AbstractEntityPersister.insert(AbstractEntityPersister.java:2822)
    at org.hibernate.action.EntityIdentityInsertAction.execute(EntityIdentityInsertAction.java:71)
    at org.hibernate.engine.ActionQueue.execute(ActionQueue.java:268)
    at org.hibernate.event.def.AbstractSaveEventListener.performSaveOrReplicate(AbstractSaveEventListener.java:321)
    at org.hibernate.event.def.AbstractSaveEventListener.performSave(AbstractSaveEventListener.java:204)
    at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:25)
    at java.lang.reflect.Method.invoke(Method.java:597)
    at org.hibernate.context.ThreadLocalSessionContext$TransactionProtectionWrapper.invoke(ThreadLocalSessionContext.java:344)
    at $Proxy19.save(Unknown Source)
    at com.example.myproject.MyEntityService.save(MyEntityService.java:59) <-- relevant call (see notes below)
    at com.example.myproject.MyServlet.doPost(MyServlet.java:164)
    ... 32 more
Caused by: java.sql.SQLException: Violation of unique constraint MY_ENTITY_UK_1: duplicate value(s) for column(s) MY_COLUMN in statement [...]
    at org.hsqldb.jdbc.Util.throwError(Unknown Source)
    at org.hsqldb.jdbc.jdbcPreparedStatement.executeUpdate(Unknown Source)
    at com.mchange.v2.c3p0.impl.NewProxyPreparedStatement.executeUpdate(NewProxyPreparedStatement.java:105)
    at org.hibernate.id.insert.AbstractSelectingDelegate.performInsert(AbstractSelectingDelegate.java:57)
    ... 54 more
```

这个例子当中的堆栈信息实在是太多了，令人眼花缭乱。如果按照之前提供的方法（堆栈信息中最下面的那个）找最深层次的那个“cause”，它就是：

```
Caused by: java.sql.SQLException: Violation of unique constraint MY_ENTITY_UK_1: duplicate value(s) for column(s) MY_COLUMN in statement [...]
    at org.hsqldb.jdbc.Util.throwError(Unknown Source)
    at org.hsqldb.jdbc.jdbcPreparedStatement.executeUpdate(Unknown Source)
    at com.mchange.v2.c3p0.impl.NewProxyPreparedStatement.executeUpdate(NewProxyPreparedStatement.java:105)
    at org.hibernate.id.insert.AbstractSelec
```

但其实它并不是的，因为抛出这个异常的方法调用者属于类库代码（c3p0 类库），所以我们需要往上找异常发生的原因，并且这个异常很可能是由我们自己编写的代码（`com.example.myproject` 包下）引发的，于是我们找到了这样一段异常信息。

```java
at com.example.myproject.MyEntityService.save(MyEntityService.java:59)
```

顺藤摸瓜，看看 MyEntityService.java 的第 59 行，它就是引发错误的根本原因。

好了各位读者朋友们，以上就是本文的全部内容了。**能看到这里的都是人才，二哥必须要为你点个赞**👍。如果觉得不过瘾，还想看到更多，可以查看我的[个人博客](http://www.itwanger.com/)。另外呢，给大家一个承诺，我每周都会更新一篇《程序人生》和一篇 Java 技术栈相关的文章，敬请期待。如果你有什么问题需要我的帮助，或者想喷我了，欢迎留言哟。

养成好习惯！如果觉得这篇文章有点用的话，**求点赞、求关注、求分享、求留言**，这将是我写下去的最强动力！如果大家想要第一时间看到二哥更新的文章，可以扫描下方的二维码，关注我的公众号。我们下篇文章见！

