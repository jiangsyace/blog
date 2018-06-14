---
title: Linux下搭建SS管理端-SSRpanel
date: 2018-6-5 15:11:30
categories:
- tech
tags:
- vps
---
最近发现一个SS(R)的账号管理，感觉不错，试用一下。  
github：[https://github.com/ssrpanel/ssrpanel](https://github.com/ssrpanel/ssrpanel) 
界面预览：  
![]({{site.upload | relative_url}}/2018-06-03_201333.png)
<!-- more -->

## 安装过程

### 环境要求

PHP 7.1 （必须）
MYSQL 5.5 （推荐5.6+）
内存 1G+ 
磁盘空间 10G+
PHP必须开启curl、gd、fileinfo、openssl、mbstring组件
安装完成后记得编辑config/app.php中 'debug' => true, 改为 false

### 环境准备
#### 安装LNMP环境
[https://lnmp.org/install.html](https://lnmp.org/install.html)
内存不足1G不能安装MySQL 5.6+ or MairaDB 10+  
```
wget -c http://soft.vpser.net/lnmp/lnmp1.5.tar.gz && tar zxf lnmp1.5.tar.gz && cd lnmp1.5 && ./install.sh lnmp
```
#### 安装php的fileinfo扩展并禁用prco函数
+ 进入lnmp的安装目录
```
cd /root/lnmp1.5/src
```
+ 解压php
```
tar -xvf php-7.1.18.tar.bz2 
cd php-7.1.18/ext/fileinfo
```
+ 安装扩展
```
/usr/local/php/bin/phpize
./configure -with-php-config=/usr/local/php/bin/php-config
make && make install
```
+ 修改php.ini, 添加fileinfo扩展
```
vim /usr/local/php/etc/php.ini
按`/`查找`;extension` 
添加以下内容启用扩展
extension = fileinfo.so
```
+ 禁用prco函数
命令模式输入`/proc_`查找,按`n`查找下一个
在"disable_fuctions"这一栏把proc_开头的函数直接删掉。按esc输入 :wq保存退出。

+ 重启php
```
lnmp php-fpm restart
```
+ 检查
```
php -i|grep fileinfo
```
如果出现  
```
fileinfo
fileinfo support => enabled
```
说明安装成功  

### 拉取代码
```
cd /home/wwwroot/
git clone https://github.com/ssrpanel/ssrpanel.git
```
### 安装面板
```
cd ssrpanel/
php composer.phar install
php artisan key:generate
chown -R www:www storage/
chmod -R 777 storage/
```
### 配置数据库
#### 连接数据库

1. 登录phpmyadmin，创建一个utf8mb4的数据库`ssrpanel`
2. 编辑config/database.php，编辑mysql选项中如下配置值：host、port、database、username、password
```
vim config/database.php
```
#### 自动部署
迁移(创建表结构)

php artisan migrate

**播种(填充数据)**

php artisan db:seed --class=ConfigTableSeeder
php artisan db:seed --class=CountryTableSeeder
php artisan db:seed --class=LevelTableSeeder
php artisan db:seed --class=SsConfigTableSeeder
php artisan db:seed --class=UserTableSeeder

#### 手工部署

迁移未经完整测试，存在BUG，可以手动将sql/db.sql导入到创建好的数据库

### 添加网站(虚拟主机)
如果输入有错误需要删除时，可以按住Ctrl再按Backspace键进行删除
```
lnmp vhost add
+-------------------------------------------+
|    Manager for LNMP, Written by Licess    |
+-------------------------------------------+
|              https://lnmp.org             |
+-------------------------------------------+
Please enter domain(example: www.lnmp.org):           #输入要添加网站的域名或IP
Enter more domain name(example: lnmp.org *.lnmp.org): #是否添加更多域名，多个域名空格隔开，如不需要绑其他域名就直接回车
Please enter the directory for the domain:            #设置网站的目录,目录不存在的话会创建目录。也可以输入已经存在的目录
Allow Rewrite rule? (y/n)                             #设置伪静态
Allow access log? (y/n)                               #如果启用日志需要再输入日志的名称
Create database and MySQL user with same name (y/n)   #是否需要添加数据库
Add SSL Certificate (y/n)                             #添加SSL功能
It will be processed automatically.
Press any key to start create virtul host...

```
### 加入NGINX的URL重写规则
```
vim /usr/local/nginx/conf/vhost/你的域名.conf

location / {
    try_files $uri $uri/ /index.php$is_args$args;
}
lnmp nginx restart
```
### lnmp管理
```
+-------------------------------------------+
|    Manager for LNMP, Written by Licess    |
+-------------------------------------------+
|              https://lnmp.org             |
+-------------------------------------------+
Usage: lnmp {start|stop|reload|restart|kill|status}
Usage: lnmp {nginx|mysql|mariadb|php-fpm|pureftpd} {start|stop|reload|restart|kill|status}
Usage: lnmp vhost {add|list|del}
Usage: lnmp database {add|list|edit|del}
Usage: lnmp ftp {add|list|edit|del|show}
Usage: lnmp ssl add
Usage: lnmp {dnsssl|dns} {cx|ali|cf|dp|he|gd|aws}
```

## 定时任务

crontab加入如下命令（请自行修改php、ssrpanel路径）：
* * * * * php /home/wwwroot/SSRPanel/artisan schedule:run >> /dev/null 2>&1

注意运行权限，必须跟ssrpanel项目权限一致，否则出现无权限报错：
例如用lnmp的话默认权限用户组是 www:www，则添加定时任务是这样的，宝塔自己搞定：
crontab -e -u www

## 邮件配置
SMTP

编辑 config\mail.php

请自行配置如下内容
'driver' => 'smtp',
'host' => 'smtp.exmail.qq.com',
'port' => 465,
'from' => [
    'address' => 'xxx@qq.com',
    'name' => 'SSRPanel',
],
'encryption' => 'ssl',
'username' => 'xxx@qq.com',
'password' => 'xxxxxx',

Mailgun

编辑 config\mail.php
将 driver 值改为 mailgun

编辑 config/services.php

请自行配置如下内容
'mailgun' => [
    'domain' => 'mailgun发件域名',
    'secret' => 'mailgun上申请到的secret',
],

发邮件失败处理

如果使用了逗比的ban_iptables.sh来防止用户发垃圾邮件
可能会导致出现 Connection could not be established with host smtp.exmail.qq.com [Connection timed out #110] 这样的错误
因为smtp发邮件必须用到25,26,465,587这四个端口，逗比的一键脚本会将这些端口一并封禁
可以编辑iptables，注释掉以下这段（前面加个#号就可以），然后保存并重启iptables
#-A OUTPUT -p tcp -m multiport --dports 25,26,465,587 -m state --state NEW,ESTABLISHED -j REJECT --reject-with icmp-port-unreachable


## 日志分析（仅支持单机单节点）

找到SSR服务端所在的ssserver.log文件
进入ssrpanel所在目录，建立一个软连接，并授权
cd /home/wwwroot/ssrpanel/storage/app
ln -S ssserver.log /root/shadowsocksr/ssserver.log
chown www:www ssserver.log

## SS(R)部署
手动部署(基于SSR)

git clone https://github.com/ssrpanel/shadowsocksr.git
cd shadowsocksr
sh initcfg.sh
配置 usermysql.json 里的数据库链接，NODE_ID就是节点ID，对应面板后台里添加的节点的自增ID，所以请先把面板搭好，搭好后进后台添加节点

## 一键自动部署(基于SSR3.4)

wget -N --no-check-certificate https://raw.githubusercontent.com/ssrpanel/ssrpanel/master/server/deploy_ssr.sh;chmod +x deploy_ssr.sh;./deploy_ssr.sh

或者使用另一个脚本

wget -N --no-check-certificate https://raw.githubusercontent.com/maxzh0916/Shadowsowcks1Click/master/Shadowsowcks1Click.sh;chmod +x Shadowsowcks1Click.sh;./Shadowsowcks1Click.sh

## 更新代码

进到ssrpanel目录下执行：
git pull

如果每次更新都会出现数据库文件被覆盖，请先执行一次：
chmod a+x fix_git.sh && sh fix_git.sh

如果本地自行改了文件，想用回原版代码，请先备份好 config/database.php，然后执行以下命令：
chmod a+x update.sh && sh update.sh

如果更新完代码各种错误，请先执行一遍 php composer.phar install

网卡流量监控一键脚本（Vnstat）

wget -N --no-check-certificate https://raw.githubusercontent.com/ssrpanel/ssrpanel/master/server/deploy_vnstat.sh;chmod +x deploy_vnstat.sh;./deploy_vnstat.sh

## 单端口多用户（推荐）

编辑节点的 user-config.json 文件：
vim user-config.json

将 "additional_ports" : {}, 改为以下内容：
"additional_ports" : {
    "80": {
        "passwd": "统一认证密码", // 例如 SSRP4ne1，推荐不要出现除大小写字母数字以外的任何字符
        "method": "统一认证加密方式", // 例如 aes-128-ctr
        "protocol": "统一认证协议", // 可选值：orgin、verify_deflate、auth_sha1_v4、auth_aes128_md5、auth_aes128_sha1、auth_chain_a
        "protocol_param": "#", // #号前面带上数字，则可以限制在线每个用户的最多在线设备数，仅限 auth_chain_a 协议下有效，例如： 3# 表示限制最多3个设备在线
        "obfs": "tls1.2_ticket_auth", // 可选值：plain、http_simple、http_post、random_head、tls1.2_ticket_auth
        "obfs_param": ""
    },
    "443": {
        "passwd": "统一认证密码",
        "method": "统一认证加密方式",
        "protocol": "统一认证协议",
        "protocol_param": "#",
        "obfs": "tls1.2_ticket_auth",
        "obfs_param": ""
    }
},

保存，然后重启SSR服务。
客户端设置：

远程端口：80
密码：password
加密方式：aes-128-ctr
协议：auth_aes128_md5
混淆插件：tls1.2_ticket_auth
协议参数：1026:@123 (SSR端口:SSR密码)

或

远程端口：443
密码：password
加密方式：aes-128-ctr
协议：auth_aes128_sha1
混淆插件：tls1.2_ticket_auth
协议参数：1026:SSRP4ne1 (SSR端口:SSR密码)

经实测，节点后端使用auth_sha1_v4_compatible，可以兼容auth_chain_a
注意：如果想强制所有账号都走80、443这样自定义的端口的话，记得把 user-config.json 中的 additional_ports_only 设置为 true
警告：经实测单端口下如果用锐速没有效果，很可能是VPS供应商限制了这两个端口
提示：配置单端口最好先看下这个WIKI，防止才踩坑：https://github.com/ssrpanel/ssrpanel/wiki/%E5%8D%95%E7%AB%AF%E5%8F%A3%E5%A4%9A%E7%94%A8%E6%88%B7%E7%9A%84%E5%9D%91

## 校时

如果架构是“面板机-数据库机-多节点机”，请务必保持各个服务器之间的时间一致，否则会产生：节点的在线数不准确、最后使用时间异常、单端口多用户功能失效等。
推荐统一使用CST时间并安装校时服务：
vim /etc/sysconfig/clock 把值改为 Asia/Shanghai
cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

重启一下服务器，然后：
yum install ntp
ntpdate cn.pool.ntp.org

