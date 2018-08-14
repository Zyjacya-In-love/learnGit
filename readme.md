#分布式版本控制系统 Git ----https://git-scm.com/
##一. 初始化(init)，添加(add)到暂存区(stage)，提交(commit)到版本库(master)
- 初始化一个Git仓库，使用 git init 命令。

- 添加文件到Git仓库，分两步：

	1. 使用命令 git add <file>，注意，可反复多次使用，添加多个文件；
	2. 使用命令 git commit -m <message> ，完成。

##二. 工作区，版本库 状态(status)，和文件不同(diff)
- 要随时掌握工作区的状态，使用 git status 命令。

- 如果git status告诉你有文件被修改过，用 git diff 可以查看修改内容。

##三. 在版本之间转换(reset)， 历史提交记录，命令
- HEAD指向的版本就是当前版本，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。返回上一个版本 git reset --hard HEAD^

- Git允许我们在版本的历史之间穿梭，使用命令 git reset --hard commit_id。

- 穿梭前，用 git log 可以查看提交历史，以便确定要回退到哪个版本。
$ git log --pretty=oneline --abbrev-commit （更清晰）

- 要重返未来，用 git reflog 查看命令历史，以便确定要回到未来的哪个版本。

##四. 丢弃修改的三种情况
1. 场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

2. 场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD <file>，就回到了场景1，第二步按场景1操作。

3. 场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

##五. 删除(rm)
- 一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用rm命令删了：$ rm test.txt

- 现在你有两个选择，
 1. 一是确实要从版本库中删除该文件，那就用命令 git rm 删掉，并且git commit：
$ git rm test.txt 
$ git commit -m "remove test.txt"
现在，文件就从版本库中被删除了。

 2. 另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：
$ git checkout -- test.txt
git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

##六. github关联(remote)， 推送(push)
1. 要关联一个远程库，使用命令 git remote add origin git@server-name:path/repo-name.git；

- 关联后，使用命令 git push -u origin master 第一次推送master分支的所有内容；

3. 此后，每次本地提交后，只要有必要，就可以使用命令 git push origin master 推送最新修改；

-- 分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而SVN在没有联网的时候是拒绝干活的！当有网络的时候，再把本地提交推送一下就完成了同步，真是太方便了！

##七. 克隆(clone)
- 要克隆一个仓库，首先必须知道仓库的地址，然后使用 git clone 命令克隆。

- Git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快。

	1. ssh $ git clone git@github.com:Zyjacya-In-love/Gitskill.git 
	2. https $ git remote add origin https://github.com/Zyjacya-In-love/learnGit.git 

##八. 分支(branch)

- 查看分支：git branch

- 创建分支：git branch <name>

- 切换分支：git checkout <name>

- 创建+切换分支：git checkout -b <name>

- 合并某分支到当前分支：git merge <name> (当前分支移动)

- 删除分支：git branch -d <name> (如果要丢弃一个没有被合并过的分支，可以通过 git branch -D <name> 强行删除。)

- **注意 :** 开发一个新feature，最好新建一个分支；- Git鼓励大量使用分支：


##九. 分支冲突
- 当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

- 解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。

- 用 git log --graph 命令可以看到分支合并图。
	- $ git log --graph --pretty=oneline --abbrev-commit（更清晰）

##十. 舍弃快进，让历史记住 "分支" ，一眼看出曾做过合并
 - Git分支十分强大，在团队开发中应该充分应用。

 - 合并分支时，加上 --no-ff 参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而  fast forward 合并就看不出来曾经做过合并。

	- $ git merge --no-ff -m "merge with no-ff" dev（因为本次合并要创建一个新的commit，所以加上-m参数）

##十一. bug 修复，工作现场保存(stash)
- 修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

- 当手头工作没有完成时，先把工作现场 git stash 一下，然后去修复bug，修复后，再 git stash pop ，回到工作现场。
 - 储藏 :  $ git stash

 - 查看 ： $ git stash list

 - 恢复 ： $ git stash apply stash@{0}

 - 删除 ： $ git stash drop stash@{0}

 - 恢复并删除 ： $ git stash pop

##十二. 多人协作
1. 另一个电脑远程 clone 参与协作 
	
	(1) 查看远程库信息，使用 git remote -v；会显示可以 抓取 和 推送 的 origin 的地址。如果没有推送权限，就看不到push的地址。要把 SSH Key 添加到 GitHub
	
	(2) 从远程库 clone 时，默认情况下，只能看到本地的 master 分支。要在 dev 分支上开发，就必须创建远程 origin 的 dev 分支到本地 $ git checkout -b dev origin/dev  ，本地和远程分支的名称最好一致；

2. 多人协作下的推送(push)
	
	(1) 首先，可以试图用 git push origin <branch-name> 推送自己的修改；
	
	(2) 如果推送失败，则因为远程分支比你的本地更新，需要先用 git pull 抓取远程的新提交 并 试图合并；
	
	(3) 如果 git pull 提示 no tracking information ，则说明本地分支和远程分支的链接关系没有创建，用命令 git branch --set-upstream-to <branch-name> origin/<branch-name>
	
	(4) 如果合并有冲突，则解决冲突，并在本地提交；
	
	(5) 没有冲突或者解决掉冲突后，再用 git push origin <branch-name> 推送就能成功！

##十三. 变基(rebase)
- rebase操作可以把本地未push的分叉提交历史整理成直线；$ git rebase

- rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。

##十四. 标签(tag)
- 命令 git tag 可以查看所有标签。

- 命令 git tag <tagname> 用于新建一个标签，默认为HEAD(最新提交的commit)，也可以指定一个commit id $ git tag <tagname> commit_id

- 还可以创建带有说明的标签，用-a指定标签名，-m指定说明文字：$ git tag -a <tagname> -m "blablabla..." commit_id eg: $ git tag -a v0.1 -m "version 0.1 released" 1094adb

- 用 git show <tagname> 查看标签信息(可以看到说明文字)

- **注意：**标签总是和某个commit挂钩。如果这个commit既出现在master分支，又出现在dev分支，那么在这两个分支上都可以看到这个标签。

- 命令 git push origin <tagname> 可以推送一个本地标签；

- 命令 git push origin --tags 可以推送全部未推送过的本地标签；

- 命令 git tag -d <tagname> 可以删除一个本地标签；

- 删除一个远程标签 ： 先从本地删除，再远程删除， 命令 git push origin :refs/tags/<tagname> 

##十五. 忽略文件
- 忽略某些文件时，需要编写 .gitignore 文件；
.gitignore 文件本身要放到版本库里，并且可以对.gitignore做版本管理！

- 强制添加被忽略文件 $ git add -f App.class
- 检查 .igtignore 文件规则 $ git check-ignore -v App.class

- **PS：**Windows如果在资源管理器里新建一个.gitignore文件，它会非常弱智地提示你必须输入文件名，但是在文本编辑器里“保存”或者“另存为”就可以把文件保存为 .gitignore 了。

##十六. 配置别名
- **eg :** $ git config --global alias.st status
- **目前的别名 ：** 
		
	> st = status
	> 
	> co = checkout 
	>
	>	ci = commit (提交)
	>
	>	br = branch (分支)
	>
	>	unstage = 'reset HEAD' (暂存区的修改撤销)
	>
	>	last = 'log -1'(显示最后一次提交信息)
	>
	>	lg = "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit" (分支合并图)

- **PS :** 当前用户的Git配置文件放在用户主目录下的一个隐藏文件.gitconfig中， 修改即可