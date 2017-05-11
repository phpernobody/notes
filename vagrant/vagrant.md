- 设置virtualBox镜像文件存放目录：VirtualBox->全局设置->常规->默认虚拟电脑位置(M)
- 更改Vagrant的镜像存储位置：
	- 将 c:\User\<username>\.vagrant.d 目录移动到新的位置,`cp -r ~/.vagrant.d/ newpath/`
	- 设置 VAGRANT_HOME 环境变量指向新的位置即可,`grep 'VAGR' ~/.bashrc` `export VAGRANT_HOME='d:/vagrant-disk'`
- 从远程加载镜像：`E:\vagrant_starter>vagrant box add ubuntu/trusty64`，系统会自动从网上加载镜像
- 从本地加载镜像：`E:\vagrant_starter>vagrant box add ubuntu/trusty64 file:///e:\download\trusty-server-cloudimg-amd64-vagrant-disk1.box`
- 初始化虚拟机：`E:\vagrant_starter>vagrant init ubuntu/trusty64`
- 启用虚拟机：`E:\vagrant_starter>vagrant up`，如果没有add镜像，仅仅init的话会在这个时候自动下载镜像
- 登录虚拟机：`E:\vagrant_starter>vagrant ssh`
- 虚拟机关机：`E:\vagrant_starter>vagrant halt`
- 虚拟机挂起：`E:\vagrant_starter>vagrant suspend`
- 删除虚拟机：`E:\vagrant_starter>vagrant destory`

- 镜像地址:`https://atlas.hashicorp.com/boxes/search?_ga=1.197515559.929485040.1493129091`

- 查看本地box列表：`vagrant box list`
- 