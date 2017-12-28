---
title: spring-data-redis使用
categories:
- tech
tags:
- spring
---

<!-- more -->

大转盘抽奖功能，不能让同一用户同时发起多个抽奖，否则会出现并发问题，于是决定使用队列来做这个限制，队列要在集群上使用，又不能在服务器上安装新软件，

## 问题汇总

​	+ spring-data-redis