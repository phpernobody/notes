##npm搭建一个项目

1、安装nodejs

2、用npm安装yarn，执行指令：
	
`npm install -g yarn`

3、




##使用yarn接管项目
>由于yarn在windows环境下无法正常 init(具体原因欢迎补充），所以可以先`npm inint`后再用yarn来接管项目；

####常用yarn命令
- 初始化一个项目(仅仅创建配置文件而已)

	`yarn init`
	
	初始化后会出现一个package.json，格式如下

		{
		  "name": "my-new-project",
		  "version": "1.0.0",
		  "description": "My New Project description.",
		  "main": "index.js",
		  "repository": {
		    "url": "https://example.com/your-username/my-new-project",
		    "type": "git"
		  },
		  "author": "Your Name <you@example.com>",
		  "license": "MIT"
		}

- 添加依赖插件(如果是一个空目录，添加插件后会自动创建package.json和yarn.lock，里面包含依赖信息)

	`yarn add [package]`

- 更新依赖插件版本

	`yarn upgrade [package]` 

- 移除插件

	`yarn remove [package]`

- 安装插件(对于在package.json中已经有配置的插件)

	`yarn install`	安装所有插件

	`yarn`	安装所有插件

	`yarn install --flat`	仅安装配置中的一个版本的插件

	`yarn install --force`	重新安装插件

	`yarn install --production`	进安装产品依赖



##package.json
[官网文档](https://yarnpkg.com/en/docs/package-json)

- name	
>项目名，发布时与版本号作为唯一id

- version
>版本号

- description
>用于描述该项目内容

- keywords
>用于给其他人搜索用的关键词

- license
>许可证，主要有四种许可证：
>
>- MIT
>- (MIT or GPL-3.0)
>- SEE LICENSE IN [your license 
>
>filename]
>- UNLICENSED

- homepage
>主页，用于配置登陆页或项目的文档位置，待查明

- bugs
>用于追踪项目bug，可以是email地址等非本项目包内的地址，待查明

- repository
>项目代码的具体位置，待查明

		{
		  "repository": { "type": "git", "url": "https://github.com/user/repo.git" },
		  "repository": "github:user/repo",
		  "repository": "gitlab:user/repo",
		  "repository": "bitbucket:user/repo",
		  "repository": "gist:a1b2c3d4e5f"
		}

- author
>作者名

- contributors
>贡献者

- files
>待查明

- main
>主入口地址

- bin
>可执行文件，待查明

- man
>待查明

- directories
>目录，指定额外的目录来放置你的二进制文件、man页面、文档等等

- scripts
>脚本，在这里定义一些脚本名，可通过`yarn run [scriptName]`来快速执行脚本,格式如：
	
		{
		  "scripts": {
		    "build-project": "node build-project.js"
		  }
		}

- dependencies
>依赖包，管理开发和生产用到的依赖包

- devDependencies
>开发依赖包，如果只需要下载使用某些模块，而不下载这些模块的测试和文档框架，放在这个下面比较不错

- peerDependencies
>兼容性依赖，插件包适合该方式

- optionalDependencies
>如果你想在某些依赖即使没有找到，或则安装失败的情况下，npm都继续执行。那么这些依赖适合放在这里

- bundledDependencies
>发布包时同时打包的其他依赖

- flat
>如果你的包允许只有一个版本依赖，并且你需要执行`yarn install --flat`，则需要将flat设为true

- engines
>指定一些可以使用该包的客户端，可以通过`process.versions`来查看

- os
>指定操作系统，可通过`process.platform`来查看

- cpu
>指定CPU型号，通过`process.arch`来查看

- private
>如果不想包发在包管理器上，设置该项为`true`

- publishConfig
>配置发布信息