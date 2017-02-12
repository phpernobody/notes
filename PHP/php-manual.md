## windows系统安装php


## apache安装

1. 到网站：http://httpd.apache.org/download.cgi下载apache，下载路径`files for microsoft windows`->`ApacheHaus`;
2. 解压到自定义目录，配置`conf/httpd.conf`，配置内容：
	- `Define SRVROOT`配置为当前目录
	- `listen`配置为未占用端口
3. 配置安装apache主服务，打开cmd窗口，输入`"apache路径\Apache\bin\httpd.exe" -k install -n apache`
4. 运行`apache路径\Apache\bin\ApacheMonitor.exe`，点击start开启apache服务
5. 卸载：cmd窗口输入`sc delete apache`

