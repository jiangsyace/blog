---
title: Google Guava 类库用法整理
date: 2019-3-25 20:10:26
categories:
- teh
tags:
- java
- Guava
---

Guava经常用，但好像总是用到很少的一部分，所以把 Guava 中的常用工具方法都捋一遍，避免以后重复造轮子。

<!-- more -->

### 准备工作

Guava 是一个工具包，直接在maven项目中加入依赖就可以使用了
```
<dependency>
	<groupId>com.google.guava</groupId>
	<artifactId>guava</artifactId>
	<version>27.1-jre</version>
</dependency>
```

### 基本工具

#### 排序和过滤

#### Joiner and Splitter

#### Queues


### 集合

#### 不可变集合
用不变的集合进行防御性编程和性能提升。
```
//集合前面加上Immutable
ImmutableSortedSet.of("a", "b", "c", "a", "d", "b");
ImmutableMap.of("k1", "v1", "k2", "v2");

```
#### 新集合类型
+ Multiset：
+ Multimap
+ BiMap
+ Table
+ MutableClassToInstanceMap
+ RangeSet
+ RangeMap

#### 集合工具类
提供java.util.Collections中没有的集合工具
+ Maps
+ Lists
+ Sets
```
Set<Integer> sets = Sets.newHashSet(1, 2, 3, 4, 5);
Set<Integer> sets2 = Sets.newHashSet(3, 4, 5, 6, 7, 8);
// 交集
SetView<Integer> intersection = Sets.intersection(sets, sets2);
// 差集
SetView<Integer> diff = Sets.difference(sets, sets2);
// 并集
SetView<Integer> union = Sets.union(sets, sets2);
```
+ Queues


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

### I/O

```
String path = "C:/Users/Administrator/Desktop/";
File from = new File(path + "from.txt");
File to = new File(path + "to.txt");

//读文件（按行读取）
List<String> lines = Files.readLines(from, Charsets.UTF_8);
//读文件（全部内容）
String content = Files.asCharSource(from, Charsets.UTF_8).read();
//读文件（只读第一行）
String firstLine = Files.asCharSource(from, Charsets.UTF_8).readFirstLine();
//按行读取文件，传入回调函数，控制结束位置和返回内容
String readLines = Files.asCharSource(from, Charset.defaultCharset()).readLines(new LineProcessor<String>() {
	public boolean processLine(String line) throws IOException {
		System.out.println("processing : " + line);
		//返回false则中断读取操作
		return true;
	}
	public String getResult() {
		//所有行处理完后返回的结果
		return "done";
	}
});

//写文件（使用ByteSink，覆盖原有内容）
Files.write("文件内容".getBytes(), from);
//写文件（使用CharSink重写）
Files.asCharSink(from, Charsets.UTF_8).write("使用CharSink重写文件");
//写文件（使用CharSink追加）
Files.asCharSink(from, Charsets.UTF_8, FileWriteMode.APPEND).write("\n使用CharSink追加内容");

//比较文件内容是否一致
Files.equal(from, to);
//复制文件
Files.copy(from, to);
//java NIO复制文件
java.nio.file.Files.copy(Paths.get(fromPath), Paths.get(toPath), StandardCopyOption.REPLACE_EXISTING);
//移动文件（重命名）
Files.move(from, to);
//从url拷贝内容到文件
Resources.asByteSource(new URL("https://www.baidu.com")).copyTo(Files.asByteSink(to));
//计算文件hashcode (可对比两个文件是否一样)
HashCode hashFrom = Files.asByteSource(from).hash(Hashing.sha256());
HashCode hashTo = Files.asByteSource(to).hash(Hashing.sha256());
//获取path下子目录
Files.fileTraverser().breadthFirst(new File(path));
```
