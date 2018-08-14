一. 初始化(init)，添加(add)到暂存区(stage)，提交(commit)到版本库(master)
初始化一个Git仓库，使用 git init 命令。

添加文件到Git仓库，分两步：

	1.使用命令 git add <file>，注意，可反复多次使用，添加多个文件；
	2.使用命令 git commit -m <message> ，完成。

二. 工作区，版本库 状态(status)，和文件不同(diff)
要随时掌握工作区的状态，使用 git status 命令。

如果git status告诉你有文件被修改过，用 git diff 可以查看修改内容。

三. 在版本之间转换(reset)， 历史提交记录，命令
HEAD指向的版本就是当前版本，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。返回上一个版本 git reset --hard HEAD^

Git允许我们在版本的历史之间穿梭，使用命令 git reset --hard commit_id。

穿梭前，用 git log 可以查看提交历史，以便确定要回退到哪个版本。

要重返未来，用 git reflog 查看命令历史，以便确定要回到未来的哪个版本。

四. 丢弃修改的三种情况
场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD <file>，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

五. 删除(rm)
一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用rm命令删了：$ rm test.txt

现在你有两个选择，一是确实要从版本库中删除该文件，那就用命令 git rm 删掉，并且git commit：
$ git rm test.txt 
$ git commit -m "remove test.txt"
现在，文件就从版本库中被删除了。

另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：
$ git checkout -- test.txt
git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

六. github关联(remote)， 推送(push)
要关联一个远程库，使用命令 git remote add origin git@server-name:path/repo-name.git；

关联后，使用命令 git push -u origin master 第一次推送master分支的所有内容；

此后，每次本地提交后，只要有必要，就可以使用命令 git push origin master 推送最新修改；

分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而SVN在没有联网的时候是拒绝干活的！当有网络的时候，再把本地提交推送一下就完成了同步，真是太方便了！