---
title: Guava Cache 使用方法及原理探析
date: 2019-3-25 20:10:26
categories:
- teh
tags:
- java
- Guava
---

Guava 是Google推出的Java核心增强的库，日常使用比较频繁，尤其是Cache缓存。
总结一下Guava Cache的使用方法，使用场景和实现原理。

<!-- more -->

### 介绍


### 使用方式
Guava和它的cache

Guava是google开源的一个公共java库，类似于Apache Commons，它提供了集合，反射，缓存，科学计算，xml，io等一些工具类库。

cache只是其中的一个模块。使用Guva cache能够方便快速的构建本地缓存。
使用Guava构建第一个缓存

首先需要在maven项目中加入guava依赖
```
<dependency>
	<groupId>com.google.guava</groupId>
	<artifactId>guava</artifactId>
	<version>27.1-jre</version>
</dependency>
```
然后便可以通过Guava创建一个缓存，例如：
```
// 通过CacheBuilder构建一个缓存实例

Cache<String, String> cache = CacheBuilder.newBuilder()
                .maximumSize(100) // 设置缓存的最大容量
                .expireAfterWrite(1, TimeUnit.MINUTES) // 设置缓存在写入一分钟后失效
                .concurrencyLevel(10) // 设置并发级别为10
                .recordStats() // 开启缓存统计
                .build();
// 放入缓存
cache.put("key", "value");
// 获取缓存
String value = cache.getIfPresent("key");
```
### 实现原理


### 总结


### 参考
[https://ifeve.com/google-guava/](https://ifeve.com/google-guava/)
[https://crossoverjie.top/2018/06/13/guava/guava-cache/](https://crossoverjie.top/2018/06/13/guava/guava-cache/)