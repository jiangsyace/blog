---
title: Guava Cache 使用
date: 2019-3-25 20:10:26
categories:
- tech
tags:
- java
- guava
---

Java开源工具类库

<!-- more -->


### Google Guava

Guava工程包含了若干被Google的 Java项目广泛依赖 的核心库，例如：集合 [collections] 、缓存 [caching] 、原生类型支持 [primitives support] 、并发库 [concurrency libraries] 、通用注解 [common annotations] 、字符串处理 [string processing] 、I/O 等等。

### Collections

#### 操作lists和maps
```
List<Map<String, String> list = Lists.newArrayList();
Map<String, Map<String, String>> map = Maps.newHashMap();

```
> jdk7以后自带了泛型类型推断
 

### Cache

### 参考
[https://ifeve.com/google-guava/](https://ifeve.com/google-guava/)  