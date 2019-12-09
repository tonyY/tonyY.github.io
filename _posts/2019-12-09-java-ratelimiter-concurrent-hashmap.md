---
layout: post
category: java
title: RateLimiter配合ConcurrentHashMap对用户进行简单限流
tagline: by 沉默王二
tag: java
---

对于小不点的项目来说，RateLimiter配合ConcurrentHashMap可以对用户进行简单的限流，防止用户频繁刷量或者高频请求。

<!--more-->


RateLimiter 是 Guava 下的一个包，采用的是令牌桶算法：以一个恒定的速率向固定容量大小的桶中放入令牌，当有流量来的时候从桶中取出一个令牌。如果桶中没有可用的令牌时就丢弃请求或者阻塞。

![](http://www.itwanger.com/assets/images/2019/12/java-ratelimiter-concurrent-hashmap-1.png)

ConcurrentHashMap 是一个可以在并发环境下使用的 HashMap，通俗点说就是线程安全的。

大致的思路也很简单：当一个请求过来时，根据用户的 ID 从 ConcurrentHashMap 取出 RateLimiter，然后尝试取出令牌，如果获取失败，则提示用户请求过于频繁；否则进行业务处理。

首先在 Controller 类中创建一个 ConcurrentHashMap 对象。

```java
private final static Map<Integer, RateLimiter> outMoneyLimitMap = new ConcurrentHashMap<>();
```

然后是请求的处理代码。

```java
public ModelAndView submitOut() {
	RateLimiter rateLimiter = null;
    if (outMoneyLimitMap.containsKey(uid)) {
        logger.debug("包含有该uid");
    
        rateLimiter = outMoneyLimitMap.get(uid);
    } else {
        logger.debug("没有该uid");
        // 需要新增，一秒内 0.1 个令牌（10秒内一个令牌）
        rateLimiter = RateLimiter.create(0.1);
    
        outMoneyLimitMap.put(uid, rateLimiter);
    }
    
     if(!rateLimiter.tryAcquire()) {
        throw new OrderException("请求过于频繁");
    }
	// 正常业务处理
}
```

`RateLimiter.create()` 可以创建一个限流器，参数可以是 double，例子中为 0.1，即 10 秒内一个令牌（便于模拟）。

然后通过 `rateLimiter.tryAcquire()` 尝试获取令牌，如果获取失败，就提示用户。否则正常业务处理。

整体思路非常非常简单，实现起来也非常容易，是一种非常好用的限流示例。




