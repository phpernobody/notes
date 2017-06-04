[搜索引擎技术揭密：中文分词技术](http://www.williamlong.info/archives/333.html)
#### 笔记
1. 现有的分词算法可分为三大类：基于字符串匹配的分词方法、基于理解的分词方法和基于统计的分词方法





#### SWCG安装（linux）
1. 获取安装包：`wget http://www.xunsearch.com/scws/down/scws-1.2.3.tar.bz2`
2. 解压：`tar xjvf scws-1.2.3.tar.bz2`
3. 安装（GNU安装方法），依次执行以下命令:
	- `cd scws-1.2.3`
	- `./configure --prefix=/usr/local/scws`
	- `make`
	- `make install`
4. 下载词典，依次执行：
	- `cd /usr/local/scws/etc`
	- `wget http://www.xunsearch.com/scws/down/scws-dict-chs-gbk.tar.bz2`
	- `wget http://www.xunsearch.com/scws/down/scws-dict-chs-utf8.tar.bz2`
	- `wget http://www.xunsearch.com/scws/down/scws-dict-cht-utf8.tar.bz2`
	- `tar xvjf scws-dict-chs-gbk.tar.bz2`
	- `tar xvjf scws-dict-chs-utf8.tar.bz2`
	- `tar xvjf scws-dict-cht-utf8.tar.bz2`

5. 安装php扩展
	- `cd /scws-1.2.3/phpext`
	- `phpize`
	- `./configure --with-scws=/usr/local/scws`,若 php 安装在特殊目录`$php_prefix`, 则请在 configure 后加上 `--with-php-config=$php_prefix/bin/php-config`
	- `make;makeinstall`
	- 在php.ini中加入以下几行：

			[scws] 
			; 
			; 注意请检查 php.ini 中的 extension_dir 的设定值是否正确, 否则请将 extension_dir 设为空， 
			; 再把 extension = scws.so 指定绝对路径。 
			; 
			extension = scws.so 
			scws.default.charset = utf8 
			scws.default.fpath = /usr/local/scws/etc 
	- 重启php使新的 php.ini 生效。命令行下执行 php -m 就能看到 scws 了或者在 phpinfo() 中看看关于 scws 的部分
	- 如果重启php-fpm过程中出现错误信息`NOTICE: PHP message: PHP Warning: PHP Startup: Unable to load dynamic library '/opt/rh/php55/root/usr/lib64/php/modules/scws.so' - libscws.so.1: cannot open shared object file: No such file or directory in Unknown on line 0`，需执行:
		- `ln -s /usr/local/scws/lib/libscws.so.1.1.0 /usr/local/lib/libscws.so`
		- `ln -s /usr/local/scws/lib/libscws.so.1.1.0 /usr/local/lib/libscws.so.1`

#### autoconfig、automake安装
1. 安装`autoconfig`，按照以下步骤：
	- `wget http://ftp.gnu.org/gnu/autoconf/autoconf-2.69.tar.gz`
	- `tar -zxvf autoconf-2.69.tar.gz`
	- `cd autoconf-2.69`
	- `./configure`
	- `make;make install`
	- `autoconfig --version`

2. 安装`automake`，按照以下步骤：
	- `wget http://ftp.gnu.org/gnu/automake/automake-1.14.tar.gz`
	- `tar -zxvf automake-1.14.tar.gz`
	- `cd automake-1.14`
