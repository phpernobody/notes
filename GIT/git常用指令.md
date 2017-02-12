1、设置默认编辑器
export GIT_EDITOR=[your editor]

2、配置
git config [--global] user.name [your name]
git config [--global] user.email [your email]
git config [--global] core.editor [your edit]
git config [--global] core.autocrlf [true/false/input]
git config --unset [--global] [params]	移除设置
git config --list/-l

git config --global alias.[your lias] [command]

3、查看提交信息
gitk	（图形化历史提交记录）
git log
git log --follow [fileName]	（查看单个文件的完整信息）
git log [branchName]
git log --pretty=short --abbrev-commit [branchName]~12..[branchName]~10	（查看主分支前12~10次的提交）
got log -[number]	（限制显示number数量的提交）
git show [head]
git show-branch --more=10	(--more=10表示额外10个版本，即11个版本)


git diff [head1] [head2]

4、移除
git rm [your file]	（与删除相同，也需要commit）

5、重命名
git mv [old name] [new name]	（需要commit）

6、git配置文件优先级
.git/config > ~/.gitconfig > /etc/gitconfig

7、文件操作
echo "[content]" > [fileName.suffix]
cat [fileName]
cp [to be copy file] [path]
touch [fileName] [fileName] ...


8、查看对象信息
git write-tree	（获取树对象）
git cat-file -p [散列值]		（查看对象关联信息）

9、查找散列值
git rev-parse [散列值前几位]
git hash-object [fileName]

10、标签操作
git tag -m "[tag annotated]" [tagName] [散列值]

11、git维护两个数据结构：对象库和索引
	
- 对象库：
	- 块(blob)：文件中每一个版本表示为一个块；
	- 目录树(tree)：一个目录树对象代表一层目录信息；
	- 提交(commit)：一个提交对象爆粗年版本库中每一次变化的元数据，包括作者、提交者、提交日期和日志消息；
	- 标签(tag)：一个标签对象分配一个任意的且人类可读的名字给一个特定对象，通常是一个提交对象；

- 索引
	- 索引是一个临时的、动态的二进制文件，它描述整个版本库的目录结构。

12、查看异同
git diff	查看变更（未暂存）
git diff --cached	查看变更（暂存后的变更）

13、git文件分类：
- tacked已追踪
- untracked未追踪
- ignored被忽略

14、查看文件信息
git ls-files [--stage]	（可以看到SHA1值）

15、查看暂存或提交变动的文件
git commit/add --interactive

16、提交所有变动（相当于执行了add->commit，空目录不会）
git commit -a

17、暂存变为非暂存
git rm --cached [fileName]	（删除暂存区的文件，实际目录中还存在）
git rm [fileName]	（对提交的文件起作用，连实际的文件都删除，可用git checkout HEAD -- [fileName]来恢复）


18、修改文件名
git mv [oldName] [newName]

19、二分法查询出错的提交
git bisect start
git bisect good/bad
git bisect reset

20、分支

- 创建分支：git branch [createBranchName] [commit HEAD]	(commit HEAD不填表示最近一次提交)
- 查看分支：
	- git show-branch [branchName...]/[--more=number]	（branchName用来指定查看固定分支，--more=number用来设定查看数量，也可以两个参数都不加）
	- git branch
- 切换分支：
	- git checkout [branchName] 
	- git checkout -b [branchName]	（创建并切换分支）
- 删除分支：
	- git branch -d [branchName]

