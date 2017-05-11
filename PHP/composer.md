## 参考文档
[中文文档](http://www.phpcomposer.com/)

## 安装
#### 下载安装
- windows
>下载msi或exe文件直接安装，装完`composer --version`可以检查是否成功
#### 镜像配置
>使用Packagist镜像

###### 全局
>下述命令将会在当前项目中的 composer.json 文件的末尾自动添加镜像的配置信息
`composer config repo.packagist composer https://packagist.phpcomposer.com`

###### 单个项目(修改composer.json)
- 命令行:`composer config repo.packagist composer https://packagist.phpcomposer.com`
- 手动修改:
>

	"repositories": {
	    "packagist": {
	        "type": "composer",
	        "url": "https://packagist.phpcomposer.com"
	    }
	}


## 基本用法

#### composer.json
- name：项目名
- require
	- key:包名称，由供应商名称和其项目名称组成
	- value:包版本，支持四种方式：
		- 确切版本：`1.0.2`
		- 范围：`>=1.0,<2.0 | >3.0`
		- 通配符:`1.0.*`
		- 赋值运算符:`~1.2`
#### composer.lock(锁文件)
>在安装依赖后，Composer 将把安装时确切的版本号列表写入 composer.lock 文件。这将锁定改项目的特定版本；install 命令将会检查锁文件是否存在，如果存在，它将下载指定的版本（忽略 composer.json 文件中的定义）。

如果只想安装或更新一个依赖，你可以白名单它们：`php composer.phar update monolog/monolog [...]`

#### 自动加载
>对于库的自动加载信息，Composer 生成了一个 vendor/autoload.php 文件。你可以简单的引入这个文件，你会得到一个免费的自动加载支持。


## 命令行

#### 全局参数
>进程退出代码(0:正常;1:通知/未知错误;2:依赖关系处理错误)
>下列参数可与每一个命令结合使用：

- `--verbose (-v)`: 增加反馈信息的详细度。
	- `-v` 表示正常输出。
	- `-vv` 表示更详细的输出。
	- `-vvv` 则是为了 debug。
- `--help (-h)`: 显示帮助信息。
- `--quiet (-q)`: 禁止输出任何信息。
- `--no-interaction (-n)`: 不要询问任何交互问题。
- `--working-dir (-d)`: 如果指定的话，使用给定的目录作为工作目录。
- `--profile`: 显示时间和内存使用信息。
- `--ansi`: 强制 ANSI 输出。
- `--no-ansi`: 关闭 ANSI 输出。
- `--version (-V)`: 显示当前应用程序的版本信息。-

#### init(初始化)
>初始化参数

- `--name`: 包的名称。
- `--description`: 包的描述。
- `--author`: 包的作者。
- `--homepage`: 包的主页。
- `--require`: 需要依赖的其它包，必须要有一个版本约束。并且应该遵循 foo/bar:1.0.0 这样的格式。
- `--require-dev`: 开发版的依赖包，内容格式与 --require 相同。
- `--stability (-s)`: minimum-stability 字段的值。

#### install(安装)
>安装参数

- `--prefer-source`: 下载包的方式有两种： source 和 dist。对于稳定版本 composer 将默认使用 dist 方式。而 source 表示版本控制源 。如果 --prefer-source 是被启用的，composer 将从 source 安装（如果有的话）。如果想要使用一个 bugfix 到你的项目，这是非常有用的。并且可以直接从本地的版本库直接获取依赖关系。
- `--prefer-dist`: 与 --prefer-source 相反，composer 将尽可能的从 dist 获取，这将大幅度的加快在 build servers 上的安装。这也是一个回避 git 问题的途径，如果你不清楚如何正确的设置。
- `--dry-run`: 如果你只是想演示而并非实际安装一个包，你可以运行 --dry-run 命令，它将模拟安装并显示将会发生什么。
- `--dev`: 安装 require-dev 字段中列出的包（这是一个默认值）。
- `--no-dev`: 跳过 require-dev 字段中列出的包。
- `--no-scripts`: 跳过 composer.json 文件中定义的脚本。
- `--no-plugins`: 关闭 plugins。
- `--no-progress`: 移除进度信息，这可以避免一些不处理换行的终端或脚本出现混乱的显示。
- `--optimize-autoloader (-o)`: 转换 PSR-0/4 autoloading 到 classmap 可以获得更快的加载支持。特别是在生产环境下建议这么做，但由于运行需要一些时间，因此并没有作为默认值。

#### update(更新)
>获取依赖的最新版本，并且升级 composer.lock 文件

- `--prefer-source`: 当有可用的包时，从 source 安装。
- `--prefer-dist`: 当有可用的包时，从 dist 安装。
- `--dry-run`: 模拟命令，并没有做实际的操作。
- `--dev`: 安装 require-dev 字段中列出的包（这是一个默认值）。
- `--no-dev`: 跳过 require-dev 字段中列出的包。
- `--no-scripts`: 跳过 composer.json 文件中定义的脚本。
- `--no-plugins`: 关闭 plugins。
- `--no-progress`: 移除进度信息，这可以避免一些不处理换行的终端或脚本出现混乱的显示。
- `--optimize-autoloader (-o)`: 转换 PSR-0/4 autoloading 到 classmap 可以获得更快的加载支持。特别是在生产环境下建议这么做，但由于运行需要一些时间，因此并没有作为默认值。
- `--lock`: 仅更新 lock 文件的 hash，取消有关 lock 文件过时的警告。
- `--with-dependencies`: 同时更新白名单内包的依赖关系，这将进行递归更新。

#### require(声明依赖)
>require 命令增加新的依赖包到当前目录的 composer.json 文件中

- `--prefer-source`: 当有可用的包时，从 source 安装。
- `--prefer-dist`: 当有可用的包时，从 dist 安装。
- `--dev`: 安装 require-dev 字段中列出的包。
- `--no-update`: 禁用依赖关系的自动更新。
- `--no-progress`: 移除进度信息，这可以避免一些不处理换行的终端或脚本出现混乱的显示。
- `--update-with-dependencies`: 一并更新新装包的依赖。

#### global(全局执行)
>global 命令允许你在 COMPOSER_HOME 目录下执行其它命令，像 install、require 或 update

#### search(搜索)
>为当前项目搜索依赖包，通常它只搜索 packagist.org 上的包，你可以简单的输入你的搜索条件

- `--only-name (-N)`: 仅针对指定的名称搜索（完全匹配）。

#### show(展示)
>列出所有可用的软件包

- `--installed (-i)`: 列出已安装的依赖包。
- `--platform (-p)`: 仅列出平台软件包（PHP 与它的扩展）。
- `--self (-s)`: 仅列出当前项目信息。

#### depends(依赖性检测)
>depends 命令可以查出已安装在你项目中的某个包，是否正在被其它的包所依赖，并列出他们

- `--link-type`: 检测的类型，默认为 require 也可以是 require-dev。

#### validate(有效性检测)
>在提交 composer.json 文件，和创建 tag 前，你应该始终运行 validate 命令。它将检测你的 composer.json 文件是否是有效的

- `--no-check-all`: Composer 是否进行完整的校验。

#### status(依赖包状态检测)
>如果你经常修改依赖包里的代码，并且它们是从 source（自定义源）进行安装的，那么 status 命令允许你进行检查，如果你有任何本地的更改它将会给予提示。
>你可以使用 --verbose 系列参数（-v|vv|vvv）来获取更详细的详细

#### self-update(自我更新)
> Composer 自身升级到最新版本，只需要运行 self-update 命令。它将替换你的 composer.phar 文件到最新版本

- 如果你想要升级到一个特定的版本，可以这样简单的指定它:`php composer.phar self-update 1.0.0-alpha7`
- 如果你已经为整个系统安装 Composer（参见 全局安装），你可能需要在 root 权限下运行它:`sudo composer self-update`
- 参数：
	- `--rollback (-r)`: 回滚到你已经安装的最后一个版本。
	- `--clean-backups`: 在更新过程中删除旧的备份，这使得更新过后的当前版本是唯一可用的备份。

#### config(更改配置)
>编辑 Composer 的一些基本设置，无论是本地的 composer.json 或者全局的 config.json 文件

更改方法：`config [options] [setting-key] [setting-value1] ... [setting-valueN]`

setting-key 是一个配置选项的名称，setting-value1 是一个配置的值。可以使用数组作为配置的值（像 github-protocols），多个 setting-value 是允许的。

- `--global (-g)`: 操作位于 $COMPOSER_HOME/config.json 的全局配置文件。如果不指定该参数，此命令将影响当前项目的 composer.json 文件，或 --file 参数所指向的文件。
- `--editor (-e)`: 使用文本编辑器打开 composer.json 文件。默认情况下始终是打开当前项目的文件。当存在 --global 参数时，将会打开全局 composer.json 文件。
- `--unset`: 移除由 setting-key 指定名称的配置选项。
- `--list (-l)`: 显示当前配置选项的列表。当存在 --global 参数时，将会显示全局配置选项的列表。
- `--file="..." (-f)`: 在一个指定的文件上操作，而不是 composer.json。注意：不能与 --global 参数一起使用。

#### create-project(创建项目)
>使用 Composer 从现有的包中创建一个新的项目。这相当于执行了一个 git clone 或 svn checkout 命令后将这个包的依赖安装到它自己的 vendor 目录。

- `--repository-url`: 提供一个自定义的储存库来搜索包，这将被用来代替 packagist.org。可以是一个指向 composer 资源库的 HTTP URL，或者是指向某个 packages.json 文件的本地路径。
- `--stability (-s)`: 资源包的最低稳定版本，默认为 stable。
- `--prefer-source`: 当有可用的包时，从 source 安装。
- `--prefer-dist`: 当有可用的包时，从 dist 安装。
- `--dev`: 安装 require-dev 字段中列出的包。
- `--no-install`: 禁止安装包的依赖。
- `--no-plugins`: 禁用 plugins。
- `--no-scripts`: 禁止在根资源包中定义的脚本执行。
- `--no-progress`: 移除进度信息，这可以避免一些不处理换行的终端或脚本出现混乱的显示。
- `--keep-vcs`: 创建时跳过缺失的 VCS 。如果你在非交互模式下运行创建命令，这将是非常有用的。

#### dump-autoload(打印自动加载索引)
>某些情况下你需要更新 autoloader，例如在你的包中加入了一个新的类。你可以使用 dump-autoload 来完成，而不必执行 install 或 update 命令

- `--optimize (-o)`: 转换 PSR-0/4 autoloading 到 classmap 获得更快的载入速度。这特别适用于生产环境，但可能需要一些时间来运行，因此它目前不是默认设置。
- `--no-dev`: 禁用 autoload-dev 规则。

#### run-script(执行脚本)
>运行此命令来手动执行 脚本，只需要指定脚本的名称，可选的 --no-dev 参数允许你禁用开发者模式。

#### diagnose(诊断)
>运行 diagnose 命令可以帮助检测一些常见的问题

#### archive(归档)
>此命令用来对指定包的指定版本进行 zip/tar 归档。它也可以用来归档你的整个项目，不包括 excluded/ignored（排除/忽略）的文件。

- `--format (-f)`: 指定归档格式：tar 或 zip（默认为 tar）。
- `--dir`: 指定归档存放的目录（默认为当前目录）。


#### help(获取帮助信息)

#### 环境变量
>设置一些环境变量来覆盖默认的配置。建议尽可能的在 composer.json 的 config 字段中设置这些值，而不是通过命令行设置环境变量。值得注意的是环境变量中的值，将始终优先于 composer.json 中所指定的值

- COMPOSER:环境变量 COMPOSER 可以为 composer.json 文件指定其它的文件名。
	>COMPOSER=composer-other.json php composer.phar install
- COMPOSER_ROOT_VERSION:通过设置这个环境变量，你可以指定 root 包的版本，如果程序不能从 VCS 上猜测出版本号，并且未在 composer.json 文件中申明。
- COMPOSER_VENDOR_DIR:通过设置这个环境变量，你可以指定 composer 将依赖安装在 vendor 以外的其它目录中。
- COMPOSER_BIN_DIR:通过设置这个环境变量，你可以指定 bin（Vendor Binaries）目录到 vendor/bin 以外的其它目录。
- http_proxy or HTTP_PROXY:如果你是通过 HTTP 代理来使用 Composer，你可以使用 http_proxy 或 HTTP_PROXY 环境变量。只要简单的将它设置为代理服务器的 URL。
	>建议使用 http_proxy（小写）或者两者都进行定义。因为某些工具，像 git 或 curl 将使用 http_proxy 小写的版本。另外，你还可以使用 git config --global http.proxy <proxy url> 来单独设置 git 的代理。
- no_proxy:如果你是使用代理服务器，并且想要对某些域名禁用代理，就可以使用 no_proxy 环境变量。只需要输入一个逗号相隔的域名 排除 列表。
	>此环境变量接受域名、IP 以及 CIDR地址块。你可以将它限制到一个端口（例如：:80）。你还可以把它设置为 * 来忽略所有的 HTTP 代理请求。
- HTTP_PROXY_REQUEST_FULLURI:如果你使用了 HTTP 代理，但它不支持 request_fulluri 标签，那么你应该设置这个环境变量为 false 或 0 ，来防止 composer 从 request_fulluri 读取配置。
- COMPOSER_HOME:COMPOSER_HOME 环境变量允许你改变 Composer 的主目录。这是一个隐藏的、所有项目共享的全局目录（对本机的所有用户都可用）,它在各个系统上的默认值分别为：
	- *nix `/home/<user>/.composer`
	- OSX `/Users/<user>/.composer`
	- Windows `C:\Users\<user>\AppData\Roaming\Composer`
- COMPOSER_HOME/config.json:你可以在 COMPOSER_HOME 目录中放置一个 config.json 文件。在你执行 install 和 update 命令时，Composer 会将它与你项目中的 composer.json 文件进行合并。该文件允许你为用户的项目设置 配置信息 和 资源库。若 全局 和 项目 存在相同配置项，那么项目中的 composer.json 文件拥有更高的优先级。
- COMPOSER_CACHE_DIR:COMPOSER_CACHE_DIR 环境变量允许你设置 Composer 的缓存目录，这也可以通过 cache-dir 进行配置,它在各个系统上的默认值分别为:
	- *nix and OSX `$COMPOSER_HOME/cache`
	- Windows `C:\Users\<user>\AppData\Local\Composer 或 %LOCALAPPDATA%/Composer`
- COMPOSER_PROCESS_TIMEOUT:这个环境变量控制着 Composer 执行命令的等待时间（例如：git 命令）。默认值为300秒（5分钟）
- COMPOSER_DISCARD_CHANGES:这个环境变量控制着 discard-changes config option
- COMPOSER_NO_INTERACTION:如果设置为1，这个环境变量将使 Composer 在执行每一个命令时都放弃交互，相当于对所有命令都使用了 --no-interaction。可以在搭建 虚拟机/持续集成服务器 时这样设置

## composer.json架构

#### JSON schema
>我们有一个 JSON schema 格式化文档，它也可以被用来验证你的 composer.json 文件。事实上，它已经被 validate 命令所使用。 你可以在这里找到它： res/composer-schema.json.

#### Root包
>“root 包”是指由 composer.json 定义的在你项目根目录的包。这是 composer.json 定义你项目所需的主要条件。
>
>某些字段仅适用于“root 包”上下文。 config 字段就是其中一个例子。只有“root 包”可以定义。在依赖包中定义的 config 字段将被忽略，这使得 config 字段只有“root 包”可用（root-only）。
>
>如果你克隆了其中的一个依赖包，直接在其上开始工作，那么它就变成了“root 包”。与作为他人的依赖包时使用相同的 composer.json 文件，但上下文发生了变化。

tip:一个资源包是不是“root 包”，取决于它的上下文。 例：如果你的项目依赖 monolog 库，那么你的项目就是“root 包”。 但是，如果你从 GitHub 上克隆了 monolog 为它修复 bug， 那么此时 monolog 就是“root 包”。


#### 属性

###### name(包名)
>包的名称，它包括供应商名称和项目名称，使用 / 分隔。

>对于需要发布的包（库），这是必须填写的。

###### description(描述)
>一个包的简短描述。通常这个最长只有一行。

>对于需要发布的包（库），这是必须填写的。

###### version(版本)
>version 不是必须的，并且建议忽略（见下文）。

>它应该符合 'X.Y.Z' 或者 'vX.Y.Z' 的形式， -dev、-patch、-alpha、-beta 或 -RC 这些后缀是可选的。在后缀之后也可以再跟上一个数字。

tip:Packagist 使用 VCS 仓库， 因此 version 定义的版本号必须是真实准确的。 自己手动指定的 version，最终有可能在某个时候因为人为错误造成问题。

###### type(安装类型)
>包的安装类型，默认为 library

>包的安装类型，用来定义安装逻辑。如果你有一个包需要一个特殊的逻辑，你可以设定一个自定义的类型。这可以是一个 symfony-bundle，一个 wordpress-plugin 或者一个 typo3-module。这些类型都将是具体到某一个项目，而对应的项目将要提供一种能够安装该类型包的安装程序。

composer 原生支持以下4种类型：

- library: 这是默认类型，它会简单的将文件复制到 vendor 目录。
- project: 这表示当前包是一个项目，而不是一个库。例：框架应用程序 Symfony standard - edition，内容管理系统 SilverStripe installer 或者完全成熟的分布式应用程序。使用 IDE 创建一个新的工作区时，这可以为其提供项目列表的初始化。
- metapackage: 当一个空的包，包含依赖并且需要触发依赖的安装，这将不会对系统写入额外的文件。因此这种安装类型并不需要一个 dist 或 source。
- composer-plugin: 一个安装类型为 composer-plugin 的包，它有一个自定义安装类型，可以为其它包提供一个 installler。详细请查看 自定义安装类型。

###### keywords(关键字)
>该包相关的关键词的数组。这些可用于搜索和过滤。

###### homepage(项目主页)
>该项目网站的 URL 地址。

###### time(版本发布时间)
>版本发布时间，必须符合 YYYY-MM-DD 或 YYYY-MM-DD HH:MM:SS 格式。

###### license(许可协议)
>包的许可协议，它可以是一个字符串或者字符串数组。

最常见的许可协议的推荐写法（按字母排序）：

- Apache-2.0
- BSD-2-Clause
- BSD-3-Clause
- BSD-4-Clause
- GPL-2.0
- GPL-2.0+
- GPL-3.0
- GPL-3.0+
- LGPL-2.1
- LGPL-2.1+
- LGPL-3.0
- LGPL-3.0+
- MIT

 详见[原文](http://docs.phpcomposer.com/04-schema.html)

###### authors(作者)
>包的作者。这是一个对象数组

这个对象必须包含以下属性:

- name: 作者的姓名，通常使用真名。
- email: 作者的 email 地址。
- homepage: 作者主页的 URL 地址。
- role: 该作者在此项目中担任的角色（例：开发人员 或 翻译）。

###### support(支持)
>获取项目支持的向相关信息对象。

这个对象必须包含以下属性：

- email: 项目支持 email 地址。
- issues: 跟踪问题的 URL 地址。
- forum: 论坛地址。
- wiki: Wiki 地址。
- irc: IRC 聊天频道地址，类似于 irc://server/channel。
- source: 网址浏览或下载源。

###### require
必须的软件包列表，除非这些依赖被满足，否则不会完成安装。

###### require-dev (root-only)
这个列表是为开发或测试等目的，额外列出的依赖。“root 包”的 require-dev 默认是会被安装的。然而 install 或 update 支持使用 --no-dev 参数来跳过 require-dev 字段中列出的包。

###### conflict
此列表中的包与当前包的这个版本冲突。它们将不允许同时被安装。

###### replace
这个列表中的包将被当前包取代。这使你可以 fork 一个包，以不同的名称和版本号发布，同时要求依赖于原包的其它包，在这之后依赖于你 fork 的这个包，因为它取代了原来的包。

这对于创建一个内部包含子包的主包也非常的有用。例如 symfony/symfony 这个主包，包含了所有 Symfony 的组件，而这些组件又可以作为单独的包进行发布。如果你 require 了主包，那么它就会自动完成其下各个组件的任务，因为主包取代了子包。

注意，在使用上述方法取代子包时，通常你应该只对子包使用 self.version 这一个版本约束，以确保主包仅替换掉子包的准确版本，而不是任何其他版本。

###### provide
List of other packages that are provided by this package. This is mostly useful for common interfaces. A package could depend on some virtual logger package, any library that implements this logger interface would simply list it in provide.

###### suggest
建议安装的包，它们增强或能够与当前包良好的工作。这些只是信息，并显示在依赖包安装完成之后，给你的用户一个建议，他们可以添加更多的包。

###### autoload
PHP autoloader 的自动加载映射。

###### autoload-dev (root-only)
This section allows to define autoload rules for development purposes.

Classes needed to run the test suite should not be included in the main autoload rules to avoid polluting the autoloader in production and when other people use your package as a dependency.

Therefore, it is a good idea to rely on a dedicated path for your unit tests and to add it within the autoload-dev section.

###### include-path
>不建议：这是目前唯一支持传统项目的做法，所有新的代码都建议使用自动加载。 这是一个过时的做法，但 Composer 将仍然保留这个功能。

###### minimum-stability (root-only)
这定义了通过稳定性过滤包的默认行为。默认为 stable（稳定）。因此如果你依赖于一个 dev（开发）包，你应该明确的进行定义。

对每个包的所有版本都会进行稳定性检查，而低于 minimum-stability 所设定的最低稳定性的版本，将在解决依赖关系时被忽略。对于个别包的特殊稳定性要求，可以在 require 或 require-dev 中设定（请参考 Package links）。

可用的稳定性标识（按字母排序）：dev、alpha、beta、RC、stable。


######　prefer-stable (root-only)
当此选项被激活时，Composer 将优先使用更稳定的包版本。

使用 "prefer-stable": true 来激活它。

###### repositories (root-only)
使用自定义的包资源库。

默认情况下 composer 只使用 packagist 作为包的资源库。通过指定资源库，你可以从其他地方获取资源包。

Repositories 并不是递归调用的，只能在“Root包”的 composer.json 中定义。附属包中的 composer.json 将被忽略。

支持以下类型的包资源库：

- composer: 一个 composer 类型的资源库，是一个简单的网络服务器（HTTP、FTP、SSH）上的 packages.json 文件，它包含一个 composer.json 对象的列表，有额外的 dist 和/或 source 信息。这个 packages.json 文件是用一个 PHP 流加载的。你可以使用 options 参数来设定额外的流信息。
- vcs: 从 git、svn 和 hg 取得资源。
- pear: 从 pear 获取资源。
- package: 如果你依赖于一个项目，它不提供任何对 composer 的支持，你就可以使用这种类型。你基本上就只需要内联一个 composer.json 对象。

tip: 顺序是非常重要的，当 Composer 查找资源包时，它会按照顺序进行。默认情况下 Packagist 是最后加入的，因此自定义设置将可以覆盖 Packagist 上的包。

###### config (root-only)
>下面的这一组选项，仅用于项目

- process-timeout: 默认为 300。处理进程结束时间，例如：git 克隆的时间。Composer 将放弃超时的任务。如果你的网络缓慢或者正在使用一个巨大的包，你可能要将这个值设置的更高一些。
- use-include-path: 默认为 false。如果为 true，Composer autoloader 还将在 PHP include path 中继续查找类文件。
- preferred-install: 默认为 auto。它的值可以是 source、dist 或 auto。这个选项允许你设置 Composer 的默认安装方法。
- github-protocols: 默认为 ["git", "https", "ssh"]。从 github.com 克隆时使用的协议优先级清单，因此默认情况下将优先使用 git 协议进行克隆。你可以重新排列它们的次序，例如，如果你的网络有代理服务器或 git 协议的效率很低，你就可以提升 https 协议的优先级。
- github-oauth: 一个域名和 oauth keys 的列表。 例如：使用 {"github.com": "oauthtoken"} 作为此选项的值， 将使用 oauthtoken 来访问 github 上的私人仓库，并绕过 low IP-based rate 的 API 限制。 关联知识 关于如何获取 GitHub 的 OAuth token。
- vendor-dir: 默认为 vendor。通过设置你可以安装依赖到不同的目录。
- bin-dir: 默认为 vendor/bin。如果一个项目包含二进制文件，它们将被连接到这个目录。
- cache-dir: unix 下默认为 $home/cache，Windows 下默认为 C:\Users\<user>\AppData\Local\Composer。用于存储 composer 所有的缓存文件。相关信息请查看 COMPOSER_HOME。
- cache-files-dir: 默认为 $cache-dir/files。存储包 zip 存档的目录。
- cache-repo-dir: 默认为 $cache-dir/repo。存储 composer 类型的 VCS（svn、github、bitbucket） repos 目录。
- cache-vcs-dir: 默认为 $cache-dir/vcs。此目录用于存储 VCS 克隆的 git/hg 类型的元数据，并加快安装速度。
- cache-files-ttl: 默认为 15552000（6个月）。默认情况下 Composer 缓存的所有数据都将在闲置6个月后被删除，这个选项允许你来调整这个时间，你可以将其设置为0以禁用缓存。
- cache-files-maxsize: 默认为 300MiB。Composer 缓存的最大容量，超出后将优先清除旧的缓存数据，直到缓存量低于这个数值。
- prepend-autoloader: 默认为 true。如果设置为 false，composer autoloader 将不会附加到现有的自动加载机制中。这有时候用来解决与其它自动加载机制产生的冲突。
- autoloader-suffix: 默认为 null。Composer autoloader 的后缀，当设置为空时将会产生一个随机的字符串。
optimize-autoloader Defaults to false. Always optimize when dumping the autoloader.
- github-domains: 默认为 ["github.com"]。一个 github mode 下的域名列表。这是用于GitHub的企业设置。
- notify-on-install: 默认为 true。Composer 允许资源仓库定义一个用于通知的 URL，以便有人从其上安装资源包时能够得到一个反馈通知。此选项允许你禁用该行为。
- discard-changes: 默认为 false，它的值可以是 true、false 或 stash。这个选项允许你设置在非交互模式下，当处理失败的更新时采用的处理方式。true 表示永远放弃更改。"stash" 表示继续尝试。Use this for CI servers or deploy scripts if you tend to have modified vendors.

###### scripts (root-only)
Composer 允许你在安装过程中的各个阶段挂接脚本。

###### extra
任意的供 scripts 使用的额外数据。.

这可以是几乎任何东西。若要从脚本事件访问处理程序，你可以这样做：

$extra = $event->getComposer()->getPackage()->getExtra();

###### bin
该属性用于标注一组应被视为二进制脚本的文件，他们会被软链接到（config 对象中的）bin-dir 属性所标注的目录，以供其他依赖包调用。

###### archive
>这些选项在创建包存档时使用。

- exclude: 允许设置一个需要被排除的路径的列表。使用与 .gitignore 文件相同的语法。一个前导的（!）将会使其变成白名单而无视之前相同目录的排除设定。前导斜杠只会在项目的相对路径的开头匹配。星号为通配符。

例子：
	
	{
	    "archive": {
	        "exclude": ["/foo/bar", "baz", "/*.test", "!/foo/bar/baz"]
	    }
	}

在这个例子中我们 include /dir/foo/bar/file、/foo/bar/baz、/file.php、/foo/my.test 但排除了 /foo/bar/any、/foo/baz、/my.test。



