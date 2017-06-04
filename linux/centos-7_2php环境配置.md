## 参考

[在CentOS上搭建PHP服务器环境](http://www.cnblogs.com/liulun/p/3535346.html)

[CentOS7.0系统上构建php开发环境--Lamp](http://www.centoscn.com/CentosServer/www/2014/0730/3384.html)

## centos7环境安装

#### 常用指令
1. 查看centos版本：`cat/etc/centos-release`
2. 查看php是否安装：`rpm -qa | grep php`
3. 检查apache安装情况：`/apache/bin/apachectl -v`
4. 查看特定文件属于哪个包(安装时出现delta错误提示)：`yum provides '*/applydeltarpm'`
5. 安装deltarpm：`yum install deltarpm`
6. 目录重命名： `mv A B` 

#### apache
###### 安装
`yum install httpd httpd-devel`
###### 启动
`systemctl start httpd.service`
`service httpd start`
###### 重启和停止
`service httpd restart`
`service httpd stop`


#### mysql
###### 安装
[Centos安装php高版本](http://www.jb51.net/article/83466.htm)
[centos7安装报错问题](http://blog.csdn.net/znb769525443/article/details/51461246)

安装repo源： `sudo rpm -Uvh http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm`
安装mysql： `yum install mysql mysql-server`
###### 启动
`service mysqld start`

#### php
###### 检查是否有安装php
`rpm -qa|grep php`

###### 检查当前安装的PHP包
`yum list installed | grep php`

###### 移除php包
`yum remove php.x86_64 php-cli.x86_64 php-common.x86_64 php-gd.x86_64 php-ldap.x86_64 php-mbstring.x86_64 php-mcrypt.x86_64 php-mysql.x86_64 php-pdo.x86_64`

###### 移除php源
`yum remove php*`

###### 安装php源
- Centos 5 安装php源：`rpm -ivh http://mirror.webtatic.com/yum/el5/latest.rpm`
- CentOs 6 安装php源：`rpm -ivh http://mirror.webtatic.com/yum/el6/latest.rpm`
- CentOs 7 安装php源和epel扩展源：`rpm -ivh https://mirror.webtatic.com/yum/el7/epel-release.rpm`    `rpm -ivh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm`
###### 安装php
`yum install php56w.x86_64 php56w-cli.x86_64 php56w-common.x86_64 php56w-gd.x86_64 php56w-ldap.x86_64 php56w-mbstring.x86_64 php56w-mcrypt.x86_64 php56w-mysql.x86_64 php56w-pdo.x86_64`

###### 安装php fpm
`yum install php56w-fpm`




###### 安装php基本包
- 安装php5.5的基本安装包：`yum install php55w php55w-gd php55w-mbstring php55w-mysql php55w-fpm`
- 安装php5.6的基本安装包：`yum install php56w php55w-gd php56w-mbstring php56w-mysql php56w-fpm`
- 安装php7.0的基本安装包：`yum install php70w php70w-gd php70w-mbstring php70w-mysql php70w-fpm`



## git工具

#### 安装
`yum install git`

#### 生成公钥
`ssh-keygen -t rsa`

#### 公钥路径
`~/.ssh/id_rsa.pub`



## 常用命令行


#### 安装phantomjs
1. 官网下载phantomjs压缩包
`http://phantomjs.org/`

2. 安装依赖
`sudo yum install fontconfig freetype libfreetype.so.6 libfontconfig.so.1 libstdc++.so.6`

3. 配置环境变量
`vi /etc/profile` 配置结束后运行`source /etc/profile`
