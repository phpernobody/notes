1. 初始化
	- npm init [-y/--yes]
	>加-y或--yes表示使用默认初始化，只需填写author
	- 创建`~/.npm-init.js`，在里面定制初始化内容，格式如下：
		
			module.exports = {
					customField: 'Custom Field',
					otherCustomField: 'This field is really cool'
			}
	
		或通过`prompt`函数：
	
			module.exports = prompt("what's your favorite flavor of ice cream buddy?", "I LIKE THEM ALL");




2. 安装插件
	- npm install [--save] [--save-dev] [-g]

3. 升级插件
	- npm update [-g] [<packageName>]

4. 查看哪些插件可以升级
	- npm outdated [-g] [--depth=0]

5. 卸载插件
	- npm uninstall [--save] [--save-dev] [-g] <packName>

6. 查看安装的插件
	- npm ls [-g] [--depth=0]

7. 查看配置列表
	- npm config ls [-g]


##代理
1. 设置为非https
	- npm config set strict-ssl false

2. 设置registry地址
	- npm config set registry "http://registry.npmjs.org/"

3. 设置代理服务器
	- npm config set proxy=http://代理服务器ip:代理服务器端口

4. 删除代理服务器
	- npm config delete http-proxy