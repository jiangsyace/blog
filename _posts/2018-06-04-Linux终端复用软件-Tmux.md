---
title: Linux终端复用软件-Tmux
date: 2018-6-4 19:39:55
categories:
- tech
tags:
- linux
---
Tmux是一个优秀的终端复用软件，类似GNU Screen，但来自于OpenBSD，采用BSD授权。使用它最直观的好处就是，通过一个终端登录远程主机并运行tmux后，在其中可以开启多个控制台而无需再“浪费”多余的终端来连接这台远程主机；是BSD实现的Screen替代品，相对于Screen，它更加先进：支持屏幕切分，而且具备丰富的命令行参数，使其可以灵活、动态的进行各种布局和操作。

<!-- more -->

tmux的github地址：https://github.com/tmux/tmux

## Tmux功能：
-  提供了强劲的、易于使用的命令行界面。
-  可横向和纵向分割窗口。
-  窗格可以自由移动和调整大小，或直接利用四个预设布局之一。
-  支持 UTF-8 编码及 256 色终端。
-  可在多个缓冲区进行复制和粘贴。
-  可通过交互式菜单来选择窗口、会话及客户端。
-  支持跨窗口搜索。
-  支持自动及手动锁定窗口。

## Tmux安装

+ ubuntu版本下直接apt-get安装
```
$ sudo apt-get install tmux
```
+ centos7版本下直接yum安装
```
$ yum install -y tmux
```
## Tmux的使用

tmux中有3种概念，会话(session)，窗口(window)，窗格(pane)。一个会话可以包含多个窗口，一个窗口可以被分割成多个窗格。

### 会话
tmux new -s session1 新建会话

ctrl+b d 退出会话，回到shell的终端环境

ctrl+b % 垂直分屏(组合键之后按一个百分号)，用一条垂线把当前窗口分成左右两屏。
ctrl+b  "           模向分隔窗口
ctrl+b %            纵向分隔窗口
ctrl+b q            显示分隔窗口的编号
ctrl+b o            跳到下一个分隔窗口。多屏之间的切换
ctrl+b 上下键      上一个及下一个分隔窗口
ctrl+b C-方向键    调整分隔窗口大小

### 窗口

### 窗格

## 常用组合键
```
ctrl+b ?            显示快捷键帮助
ctrl+b 空格键       采用下一个内置布局，这个很有意思，在多屏时，用这个就会将多有屏幕竖着展示
ctrl+b !            把当前窗口变为新窗口
ctrl+b  "           模向分隔窗口
ctrl+b %            纵向分隔窗口
ctrl+b q            显示分隔窗口的编号
ctrl+b o            跳到下一个分隔窗口。多屏之间的切换
ctrl+b 上下键      上一个及下一个分隔窗口
ctrl+b C-方向键    调整分隔窗口大小
ctrl+b &           确认后退出当前tmux
ctrl+b [           复制模式，即将当前屏幕移到上一个的位置上，其他所有窗口都向前移动一个。
ctrl+b c           创建新窗口
ctrl+b n           选择下一个窗口
ctrl+b l           最后使用的窗口
ctrl+b p           选择前一个窗口
ctrl+b w           以菜单方式显示及选择窗口
ctrl+b s           以菜单方式显示和选择会话。这个常用到，可以选择进入哪个tmux
ctrl+b t           显示时钟。然后按enter键后就会恢复到shell终端状态
ctrl+b d           脱离当前会话；这样可以暂时返回Shell界面，输入tmux attach能够重新进入之前的会话
```
**需要注意的几点**：

+ 进入tmux面板后，一定要先按ctrl+b，然后松开，再按其他的组合键才生效。
 
