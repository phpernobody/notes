## 常用命令行
#### 监视日志
`tail -f [filename] -n [number]`

#### 查看linux版本
`lsb_release -a`

#### 查看某文件权限
`ls -al /usr/local/scws/lib/libscws.la`

#### 配置环境变量
`vi /etc/profile`
在最下面添加`export PATH=$PATH:具体路径`,多个路径用`:`分隔开



## 压缩包相关
[Linux下的tar压缩解压缩命令详解](http://www.cnblogs.com/qq78292959/archive/2011/07/06/2099427.html)

#### 参数说明
- -c: 建立压缩档案
- -x：解压
- -t：查看内容
- -r：向压缩归档文件末尾追加文件
- -u：更新原压缩包中的文件

这五个是独立的命令，压缩解压都要用到其中一个，可以和别的命令连用但只能用其中一个。下面的参数是根据需要在压缩或解压档案时可选的。

- -z：有gzip属性的
- -j：有bz2属性的
- -Z：有compress属性的
- -v：显示所有过程
- -O：将文件解开到标准输出
- -f: 使用档案名字，切记，这个参数是最后一个参数，后面只能接档案名。

#### 例子
`tar -cf all.tar *.jpg`
这条命令是将所有.jpg的文件打成一个名为all.tar的包。-c是表示产生新的包，-f指定包的文件名

`tar -rf all.tar *.gif`
这条命令是将所有.gif的文件增加到all.tar的包里面去。-r是表示增加文件的意思。

`tar -uf all.tar logo.gif`
这条命令是更新原来tar包all.tar中logo.gif文件，-u是表示更新文件的意思。

`tar -tf all.tar`
这条命令是列出all.tar包中所有文件，-t是列出文件的意思。

`tar -xf all.tar`
这条命令是解出all.tar包中所有文件，-x是解开的意思	

#### 常用压缩命令行
`tar -cvf jpg.tar *.jpg` //将目录里所有jpg文件打包成tar.jpg 

`tar -czf jpg.tar.gz *.jpg`   //将目录里所有jpg文件打包成jpg.tar后，并且将其用gzip压缩，生成一个gzip压缩过的包，命名为jpg.tar.gz

`tar -cjf jpg.tar.bz2 *.jpg` //将目录里所有jpg文件打包成jpg.tar后，并且将其用bzip2压缩，生成一个bzip2压缩过的包，命名为jpg.tar.bz2

`tar -cZf jpg.tar.Z *.jpg`   //将目录里所有jpg文件打包成jpg.tar后，并且将其用compress压缩，生成一个umcompress压缩过的包，命名为jpg.tar.Z

`rar a jpg.rar *.jpg` //rar格式的压缩，需要先下载rar for linux

`zip jpg.zip *.jpg` //zip格式的压缩，需要先下载zip for linux

#### 常用解压命令行
`tar -xvf file.tar` //解压 tar包

`tar -xzvf file.tar.gz` //解压tar.gz

`tar -xjvf file.tar.bz2`   //解压 tar.bz2

`tar -xZvf file.tar.Z`   //解压tar.Z

`unrar e file.rar` //解压rar

`unzip file.zip` //解压zip

#### 总结
1. *.tar 用 tar -xvf 解压
2. *.gz 用 gzip -d或者gunzip 解压
3. *.tar.gz和*.tgz 用 tar -xzf 解压
4. *.bz2 用 bzip2 -d或者用bunzip2 解压
5. *.tar.bz2用tar -xjf 解压
6. *.Z 用 uncompress 解压
7. *.tar.Z 用tar -xZf 解压
8. *.rar 用 unrar e解压
9. *.zip 用 unzip 解压

#### 常见问题
1. .tar.bz2解压失败：未安装bzip2，运行`yum install bzip2`即可

## 定时器crond

#### 查看是否运行
`service crond status`

#### 启动/重启/关闭
`service crond start/restart/stop`

#### 配置文件
`/etc目录下面存在cron.hourly,cron.daily,cron.weekly,cron.monthly,cron.d五个目录和crontab,cron.deny二个文件`

`linux的cron服务是每隔一分钟去读取一次/var/spool/cron,/etc/crontab,/etc/cron.d下面所有的内容.`

#### 用法
crontab -u [用户名] [对应的操作，如下]

- –e : 修改 crontab 文件，如果文件不存在会自动创建。 
- –l : 显示 crontab 文件。 
- -r : 删除 crontab 文件。
- -ir : 删除 crontab 文件前提醒用户。

`minute hour day-of-month month-of-year day-of-week commands` 

除了数字还有几个特殊的符号："*"、"/"和"-"、","

*代表所有的取值范围内的数字
"/"代表每的意思,"/5"表示每5个单位
"-"代表从某个数字到某个数字
","分开几个离散的数字