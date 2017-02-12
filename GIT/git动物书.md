##第七章	分支
1. 分支名规范
	- 可以使用/来创建一个分层的命名方案，但不能以斜杠结尾
	- 分支名不能以-开头
	- 使用斜杠分割的组件不能以.开头
	- 分支名的任何地方都不应该包括
		- 连续两个.
		- 任何空格和其他空白字符
		- 波浪线(~)、插入符(^)、冒号(:)、问号(?)、星号(*)、方括号([)
		- ASCII码制符或DEL字符

2. 创建分支
	- 指令：git branch <branchName>
	> 完整的指令是：git branch branch [starting-commit]
	- 创建并检出：git checkout -b <branchName>

3. 查看分支
	- 指令：
		- git show-branch（查看所有分支和分支的最近几次的提交情况）
		- git branch [-a] [-r]（查看所有分支和活动分支，用*和!区分，*为活动分支）
	- 更好查看分支情况
		- git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr)%Creset' --abbrev-commit --date=relative

4. 检出分支
	- 指令：git checkout <branchName>
	- checkout回去的并不是该分支主体，而是分离的HEAD分支；如果要放弃改分离分分支，只需简单地checkout到其他分支或本分支最后一次提交；如果分支要生效，可以新建一个分支，然后再与原分支合并


5. 删除分支
	- 指令：git branch -d <branchName>
	- 如果删除的分支未合并到当前分支，可以通过git branch -D <branchName>来删除

##第九章	合并
1. 合并指令
	- git merge <branchName>
	>合并是指把其他分支合并到当前分支，并不改变其他分支

2. 合并冲突处理
	- git merge <branchName>
	- 根据CONFLICT提示的文件，找出冲突文件并修改为想要的内容
	- git add [<conflict file>]
	- git commit -m "<annotation>"

3. 定位冲突文件的方法
	- 根据合并时的CONFLICT提示
	- git status
	- git ls-files -u

4. 检查冲突
	- 使用git diff <conflict file>
	- 使用git log --merge --left-right -p
		- --merge：只显示跟产生冲突的文件相关的提交
		- --left-right：提交来自“左侧”显示<，来自“右侧”的显示>
		- -p：显示提交消息和每个提交相关联的补丁

5. 回退冲突解决前的状态
	- git checkout -m

5. 终止合并
	- git reset --hard HEAD

