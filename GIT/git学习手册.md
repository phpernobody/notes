1、初始化仓库

- git init [-y/--yes]
	>后面加-y或--yes表示使用默认配置，只要填写作者就行

2、配置仓库

- git config --local user.name <your name>
- git config --local user.email <your email>

3、添加文件到暂存区

- git add <file name>

4、提交文件

- git commit [-a] -m "<commentary>"（-a相当于执行git add）
- git commit --amend（修改push前的最后一次提交）

5、克隆远程仓库[自己指定克隆仓库在本地的名字]

- git clone <remote repository> [<your repository name>]

6、不加入版本库

创建.ignore文件（与.git同级），并在里面添加不加入版本库的文件名（可以通过正则匹配）
例如：

- *.php（.php后缀的不加入版本控制）
- !me.php（.php中除了me.php其他都不加入版本库）

7、查看仓库信息

- git status

8、将文件从暂存区里移除

- git rm --cached <fileName> (加上--cached表示只从git仓库里移除，实际硬盘中还存在该文件)

9、git栈

- git stash <save "stashName">（从最近的一次提交中读取内容，并备份当前的工作内容到git栈中）
- git stash pop（从git栈中提取最近一次保存的内容）
- git stash list（读取git栈内容）
- git stash clear（清空git栈）
- git apply（恢复以前的工作状态）

10、修改文件名（必须是已追踪状态）

- git mv <oldfileName> <newfileName>

11、移动文件

- git mv <fileName> <dir> (fileName可以用正则)

12、查看记录

- git log

13、标签

- git tag [-l]（查看当前仓库所有标签，）
- git tag [-a] [tagName] [-m "commentary"]（打新标签，-a是添加标签，-m是标签注释）
- git tag [-d] [tagName] (-d表示删除标签) 
- git push [--tags] origin master（把本地打的标签提交到远程仓库,--tags表示所有标签）
- git push origin :[tagName] （删除远程仓库的标签）



question：

git mv *.html src使用正则失败，为什么？？第15关
