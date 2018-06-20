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

## 简介

tmux 应用程序的名称来源于终端（terminal）复用器（muxer）或多路复用器（multiplexer）。tmux中有3种概念，会话(session)，窗口(window)，窗格(pane)。一个会话可以包含多个窗口，一个窗口可以被分割成多个窗格。

下面是三个元素在tmux中的具体展现：
![]({{site.upload | relative_url}}/6941baebjw1et4uosbtuhj21kw0qvqf1.jpg)

tmux的github地址：[https://github.com/tmux/tmux](https://github.com/tmux/tmux)  
tmux官网：[http://tmux.github.io/](http://tmux.github.io/)

**Tmux功能** ：
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

**运行tmux程序**
```
$ tmux                          
```

### Session相关操作
```
ctrl+b s       查看/切换session
ctrl+b d       离开session，回到shell的终端环境
ctrl+b $       重命名当前session 	
```
### Window相关操作
```
ctrl+b c       新建窗口
ctrl+b &       关闭一个窗口
ctrl+b 窗口号   使用窗口号切换
```
### Pane相关操作
```
ctrl+b o       切换到下一个窗格
ctrl+b q       查看所有窗格的编号
ctrl+b "       垂直拆分出一个新窗格
ctrl+b %       水平拆分出一个新窗格
ctrl+b z       暂时把一个窗体放到最大
ctrl+b x       删除窗格
```
### 脱离和附加
tmux 最强大的功能之一是能够脱离和重新附加到会话。 当你脱离的时候，你可以离开你的窗口和窗格独立运行。 此外，您甚至可以完全注销系统。 然后，您可以登录到同一个系统，重新附加到 tmux 会话，查看您离开时的所有窗口和窗格。 脱离的时候你运行的命令一直保持运行状态。

为了脱离一个会话，请敲 Ctrl+b, d。然后会话消失，你重新返回到一个标准的单一 shell。如果要重新附加到会话中，使用以下命令：
```
$ tmux a
$ tmux a -t session
$ tmux attach
$ tmux attach-session
$ tmux attach -t session
```
当你连接到主机的网络不稳定时，这个功能就像救生员一样有用。如果连接失败，会话中的所有的进程都会继续运行。只要连接恢复了，你就可以恢复正常，就好像什么事情也没有发生一样。

如果这些功能还不够，在每个会话的顶层窗口和窗格中，你可以运行多个会话。你可以列举出这些窗口和窗格，然后通过编号或者名称把他们附加到正确的会话中：
```
$ tmux ls
$ tmux list-sessions
```

## Tmux配置
tmux配置文件在`~/.tmux.conf `，不存在可以新建该文件。  
修改配置文件后，重启tmux或者在命令模式（按ctrl+b : )，输入以下命令生效：
``` 
source-file ~/.tmux.conf 
```
### 修改默认前缀
tmux命令都具有一个前缀命令(PREFIX)，默认的是ctrl+b，可以自己修改，改为ctrl+a。 
在`~/.tmux.conf`中加入如下行：
```
set -g prefix C-a 
unbind C-b 
```

### 启用鼠标控制
编辑`~/.tmux.conf`，添加：  
```
# tmux2.1及以后版本：
set-option -g mouse on

# tmux2.1之前版本：
setw -g mouse-resize-pane on
setw -g mouse-select-pane on
setw -g mouse-select-window on
setw -g mode-mouse on
```

## 命令汇总

```
$ tmux                            运行tmux程序
$ tmux -V                         查看tmux当前版本
$ tmux new -s session             新建名为session的会话
$ tmux new -s session -d          在后台建立会话 
$ tmux kill-session -t session    删除session
$ tmux ls                         列出所有会话
$ tmux list-sessions              列出所有会话
```
### 常用组合键

**注意**：进入tmux面板后，一定要先按ctrl+b，然后松开，再按其他的组合键才生效。
```
ctrl+b 空格键         采用下一个内置布局，这个很有意思，在多屏时，用这个就会将多有屏幕竖着展示
ctrl+b 方向键         切换窗格
ctrl+b ?               显示快捷键帮助
ctrl+b s               以菜单方式显示和选择会话。这个常用到，可以选择进入哪个tmux
ctrl+b d               脱离当前会话；这样可以暂时返回Shell界面，输入tmux attach能够重新进入之前的会话
ctrl+b $               重命名当前session 	
ctrl+b c               创建新窗口
ctrl+b &               确认后关闭当前窗口
ctrl+b !               把当前窗格变为新窗口
ctrl+b l               最后使用的窗口
ctrl+b "               模向分隔窗口，拆分出一个新窗格
ctrl+b %               纵向分隔窗口，拆分出一个新窗格
ctrl+b q               显示窗格的编号
ctrl+b [               复制模式，即将当前屏幕移到上一个的位置上，其他所有窗口都向前移动一个。
ctrl+b w               以菜单方式显示及选择窗口
ctrl+b o               跳到下一个窗格。多屏之间的切换
ctrl+b n               选择下一个窗格
ctrl+b p               选择上一个窗格
ctrl+b t               显示时钟。然后按enter键后就会恢复到shell终端状态
ctrl+b z               最大化/最小化一个窗格
ctrl+b x               删除窗格
ctrl+b :               命令模式
ctrl+b :kill-session   删除当前session
ctrl+b :kill-server    删除所有session
```

## 文档链接
[http://man.openbsd.org/OpenBSD-current/man1/tmux.1](http://man.openbsd.org/OpenBSD-current/man1/tmux.1)
[https://linux.cn/article-8421-1.html](https://linux.cn/article-8421-1.html) 
