## 连接mysql

`mysql -u root -p`

#### 出现error 2002
ERROR 2002 (HY000) Can’t connect to local MySQL server through socket ‘/var/run/mysqld/mysqld.sock 

1. mysql.cnf 中的client ，mysqld，mysqld_safe中 socket = /var/run/mysqld/mysqld.sock，在实际目录中没有生成，可能是启动的时候没有相应权限所以无法生成sock文件，把三个地方的socket都改为 /tmp/mysql.sock 即可。 
2. 另外设置一个链接，#ln -s /tmp/mysql.sock /var/run/mysqld/mysqld.sock应该也可以。
3. 指定IP地址，使用tcp方式连接mysql，而不使用本地sock方式

#### 解决无法远程登录的问题

1. vim /etc/MySQL/my.cnf找到bind-address = 127.0.0.1 ，注释掉这行，如：
	- #bind-address = 127.0.0.1 
	- bind-address = 0.0.0.0 ，允许任意IP访问 
	- 自己指定一个IP地址

	重启 MySQL：sudo /etc/init.d/mysql restart 
 
2. 授权用户能进行远程连接 
`grant all privileges on . to root@”%” identified by “password” with grant option; `
`flush privileges; `

第一行命令解释如下，.：第一个代表数据库名；第二个代表表名。这里的意思是所有数据库里的所有表都授权给用户。root：授予root账号。“%”：表示授权的用户IP可以指定，这里代表任意的IP地址都能访问MySQL数据库。“password”：分配账号对应的密码，这里密码自己替换成你的mysql root帐号密码。 
第二行命令是刷新权限信息，也即是让我们所作的设置马上生效。






## 一些基本的命令：

#### 登录：
`MySQL -u username -p`
#### 显示所有的数据库：
`show databases;`
#### 使用某一个数据库：
`use databasename;`
#### 显示一个数据库的所有表：
`show tables;`
#### 退出：
`quit;`
#### 删除数据库和数据表
`mysql>drop database 数据库名;`

`mysql>drop table 数据表名;`

#### 查看所有的用户：

`SELECT DISTINCT CONCAT('User: ''',user,'''@''',host,''';') AS query FROM mysql.user;`

#### 新建用户：

`CREATE USER 'dog'@'localhost' IDENTIFIED BY '123456'; `


#### 为用户授权：

###### 格式：
`grant 权限 on 数据库.* to 用户名@登录主机 identified by "密码";`

示例：
`grant all privileges on testDB.* to test@localhost identified by '1234';`

###### 然后需要执行刷新权限的命令：

`flush privileges;`

#### 为用户授予部分权限：

`grant select,update on testDB.* to test@localhost identified by '1234';`

#### 授予一个用户所有数据库的某些权限：

`grant select,delete,update,create,drop on *.* to test@"%" identified by "1234";`

#### 删除用户：

`Delete FROM user Where User='test' and Host='localhost';`
`flush privileges;`

#### 删除账户及权限：
>drop user 用户名@'%';
>drop user 用户名@ localhost; 

#### 修改指定用户密码

###### 使用root登录：
`mysql -u root -p`
###### 执行命令：
`update mysql.user set password=password('新密码') where User="test" and Host="localhost";`
###### 刷新权限：
`flush privileges;`



## linux下关于Apache设置二级域名绑定二级目录的方法

以下配置的路径以阿里云提供的标准环境路径为准，如果您另行安装，请根据实际安装路径配置。
 
1.cd /alidata/server/httpd/conf/vhosts/ 进入绑定域名所在目录，
 
2.vim test.conf  建立一个配置文件，test可以自己命名；
 
3.点击字母“i”开始编辑文件，输入内容：
<VirtualHost *:80>
        DocumentRoot /alidata/www/phpwind
        ServerName localhost
        ServerAlias localhost
        <Directory "/alidata/www/phpwind">
            Options -Indexes FollowSymLinks
            AllowOverride all
            Order allow,deny
            Allow from all
        </Directory>
        <IfModule mod_rewrite.c>
                RewriteEngine On
                RewriteRule ^(.*)-htm-(.*)$ $1.php?$2
                RewriteRule ^(.*)/simple/([a-z0-9\_]+\.html)$ $1/simple/index.php?$2
        </IfModule>
        ErrorLog "/alidata/log/httpd/phpwind-error.log"
        CustomLog "/alidata/log/httpd/access/phpwind.log" common
</VirtualHost>
 
其中：
ServerName www.test.com 绑定的网站域名
ServerAlias test.com 绑定的网站别名（您如果有多个域名添加在这里）
DirectoryIndex index.html index.php index.htm 设置默认首页
DocumentRoot /alidata/www/test 和 Directory "/alidata/www/test" 都是指定网站的目录，需要一致。
 
按“esc”退出编辑模式，输入“:wq”保存退出。
 
4.输入命令：/alidata/server/httpd/bin/apachectl restart 重启apache测试。
 
5.测试网站。请在浏览器中输入域名，测试设置。


[虚拟主机配置](https://my.oschina.net/u/2366984/blog/509819)