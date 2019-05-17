---
title: Google Guava 库用法整理
date: 2019-3-25 20:10:26
categories:
- teh
tags:
- java
- Guava
---

Guava 中对集合的扩展可以说是最常用的部分，在此总结一下常用的集合工具类。

<!-- more -->

### 介绍

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

### 集合

#### 不可变集合
用不变的集合进行防御性编程和性能提升。
```
ImmutableSortedSet.of("a", "b", "c", "a", "d", "b");
```
#### 新集合类型
multisets, multimaps, tables, bidirectional maps等

#### 集合工具类
提供java.util.Collections中没有的集合工具

#### 扩展工具类
让实现和扩展集合类变得更容易，比如创建Collection的装饰器，或实现迭代器


静态工厂方法


### 缓存
Guava Cache：本地缓存实现，支持多种缓存过期策略

### 并发
强大而简单的抽象，让编写正确的并发代码更简单

5.1 ListenableFuture：完成后触发回调的Future

5.2 Service框架：抽象可开启和关闭的服务，帮助你维护服务的状态逻辑

### 字符串处理
非常有用的字符串工具，包括分割、连接、填充等操作

### 反射
Guava 的 Java 反射机制工具类

### 工具类

#### 文件
```
1.从文件中按行读取内容：

File file = new File(getClass().getResource("/test.txt").getFile());
List<String> lines = null;
try {
	lines = Files.readLines(file, Charsets.UTF_8);
} catch (IOException e) {
	e.printStackTrace();
}
```
#### 排序和过滤

#### Joiner and Splitter



### 总结

### 参考
[http://ifeve.com/google-guava-collectionutilities/](http://ifeve.com/google-guava-collectionutilities/)