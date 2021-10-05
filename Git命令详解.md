# Git 命令详解
### 1. Git 下载和安装
#### 1.1 Git 下载地址
- 官网下载地址：https://git-scm.com/downloads
根据自己电脑型号下载对应版本即可。本机是Windows 10 64 位，下载对应 64 位版本。
#### 1.2 Git 安装
- 默认安装到 C 盘，一路 next 点击就行。
- 指定安装，本机指定安装到 D:\Git，修改路径一路 next 点击就行。
Git 有名为 git-bash 的终端，使用该终端执行 git 命令。

### 2. 创建版本库
#### 2.1 用户配置
使用如下命令为所有版本库配置用户名和Email地址，因为Git是分布式版本控制系统，每位用户需要自报家门。

```shell
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
```

`--global` 参数表示为本机器上所有 Git 仓库都使用这个配置，当然也可以为每个 Git 仓库配置不同的用户名和 Email 地址。

#### 2.2 创建仓库
使用git init命令可以将任何目录夹创建成Git仓库，本机Git仓库存放目录为 D:\Git\Repository ，打开 git-bash ，进入上述所述目录，然后使用 `git init` 命令将 learngit 目录创建为仓库：
```shell
cd D:/Git/Repository/learngit/
git init
```
Tips：Windows 使用 git-bash 终端输入文件路径是使用斜杠而不是反斜杠。

### 3 添加和提交文件到仓库
在 learngit 目录下新建一个 readme.txt 文件，并用 Notepad++ 编辑如下内容并保存：
```shell
Git is a version control system.
Git is free sofware.
```
Tips：不要用记事本进行编辑，Windows的记事本进行编辑，以为它会在每个文件开头添加了0xefbbbf（十六进制）的字符，你会遇到很多不可思议的问题，比如，网页第一行可能会显示一个 “ ? ” 。

编辑保存完 readme.txt 文件后，按如下步骤将其放入版本库：

1. 使用 `git add` 命令将 readme.txt 添加到仓库。
```shell
git add readme.txt
```
2. 使用 `git commit` 命令将readme.txt提交到仓库。
```shell
git commit -m "wrote a readme file"
```
`-m` 参数表示本次提交的说明，最好填写有意义的内容。

Tips：`git add` 可以一次添加多个文件，文件之间用空格分开。`git commit` 一次可以提交多个文件。

### 4 查询仓库当前状态
修改 readme.txt 文件内容如下：
```shell
Git is a distributed version control system.
Git is free software.
```
使用 `git status` 命令查看仓库当前状态：
```shell
git status
On branch master
Changes not staged for commit:
　　(use "git add <file>..." to update what will be committed)
　　(use "git checkout -- <file>..." to discard changes in working directory)

　　　　　　modified: readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```
结果显示 readme.txt 被修改过，但是还没准备提交。

使用 `git diff` 命令查看 readme.txt 哪些地方被修改过：
```shell
git diff readme.txt
diff --git a/readme.txt b/readme.txt
index d8036c1..013b5bc 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,2 +1,2 @@
-Git is a version control system.
+Git is a distributed version control system.
Git is free software.
\ No newline at end of file
```
Tips:在提交之前查看被修改的地方是有必要的。

接下来将文件添加到仓库：
```shell
git add readme.txt
```
再用 `git status` 查看仓库当前状态：
```shell
git status
On branch master
Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

            modified: readme.txt        
```
结果显示将要被提交的修改包括 readme.txt 。

这个时候如果使用 `git diff` 命令是查看不到 `readme.txt` 哪些地方被修改了：
```shell
git diff readme.txt
```
没有任何输出结果，但是可以使用 `git diff` 命令来查看工作区中的 readme.txt 和版本库中最新 readme.txt 的不同：
```shell
git diff HEAD -- readme.txt
diff --git a/readme.txt b/readme.txt
index d8036c1..013b5bc 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,2 +1,2 @@
-Git is a version control system.
+Git is a distributed version control system.
Git is free software.
\ No newline at end of file
```
接下来可以放心提交：
```shell
git commit -m "add distributed"
[master 1245bca] add distributed
1 file changed, 1 insertion(+), 1 deletion(-)
```
提交完成后，用 `git status` 命令查看当前仓库状态：
```shell
git status
On branch master
nothing to commit, working tree clean
```
结果显示没有需要提交的修改，而且工作目录是干净的。

### 5 版本回退
修改 readme.txt 文件如下：
```shell
Git is a distributed version control system.
Git is free software distributed under the GPL.
```
然后尝试提交：
```shell
git commit -m "add GPL"
[master 49c07cc] add GPL
1 file changed, 1 insertion(+), 1 deletion(-)
```
每一次的 commit 就相当于保存一个快照，一旦你将文件弄乱或者误删了文件，就可以从最近的一个 commit 恢复，继续工作。

可以使用 `git log` 命令来查看历史版本：
```shell
git log
commit 49c07cc0c5b1bd1bf5df2199129a7ed063709f4e (HEAD -> master)
Author: StrivePy <1013974267@qq.com>
Date: Sun Mar 18 19:49:59 2018 +0800

    add GPL

commit 73a55c6fadf4380e784380b28e8e3ca49b85ba40
Author: StrivePy <1013974267@qq.com>
Date: Sun Mar 18 19:42:16 2018 +0800

    add distributed
```
使用 `--pretty=oneline` 参数可以简化输出：
```shell
git log --pretty=oneline
49c07cc0c5b1bd1bf5df2199129a7ed063709f4e (HEAD -> master) add GPL
73a55c6fadf4380e784380b28e8e3ca49b85ba40 add distributed
```
`49c07cc0c5b1bd1bf5df2199129a7ed063709f4e` 一串表示 commit id，只要知道任何 commit id 就可以使用 `git reset` 命令切换到该提交版本：
```shell
git reset --hard 73a55
HEAD is now at 73a55c6 add distributed
```
结果显示已经回退到 add distributed 版本。

若想回到 add GPL 版本，同样适用 commit id：
```shell
git reset --hard 49c07
HEAD is now at 49c07cc add GPL
```
结果显示回到 add GPL 版本。

也可以使用 HEAD 指针来进行版本回退，Git 中，用 HEAD 表示当前版本， HEAD^ 表示上一版本， HEAD^^ 表示上上版本，当然往上 100 个版本写 100 个 ^ 数不过来，用 HEAD~100 表示。用 HEAD 指针进行回退：
```shell
git reset --hard HEAD^
HEAD is now at 73a55c6 add distributed
```
结果显示已经回退到 add distributed 版本。

现在关闭 git-bash ，再重新打开并进入 learngit 目录，使用 `git log` 命令：
```shell
git log
commit 73a55c6fadf4380e784380b28e8e3ca49b85ba40 (HEAD -> master)
Author: StrivePy <1013974267@qq.com>
Date: Sun Mar 18 19:42:16 2018 +0800

　　add distributed
```
结果已经看不见 add GPL 版本了，想使用 commit id 前进到 add GPL 版本但是没有 commit id ,此时可以使用 `git reflog` 来查看每一次你输入的命令：
```shell
git reflog
73a55c6 (HEAD -> master) HEAD@{0}: reset: moving to HEAD^
49c07cc HEAD@{1}: reset: moving to 49c07
73a55c6 (HEAD -> master) HEAD@{2}: reset: moving to 73a55
49c07cc HEAD@{3}: commit: add GPL
73a55c6 (HEAD -> master) HEAD@{4}: commit: add distributed
```
结果显示 add GPL 的 commit id 为 `49c07cc` ，使用该 id 进行回退：
```shell
git reset --hard 49c07cc
HEAD is now at 49c07cc add GPL
```
结果显示已经回退到add GPL版本。

小结：

HEAD指向的是当前版本，可以使用如下命令在版本之间穿梭：
```shell
git reset --hard commit_id
```
穿梭前，可以使用如下命令查看历史版本的 commit id：
```shell
git log
```
要重返未来，可以使用如下命令查看 commit id：
```shell
git reflog
```

### 6 管理修改
Git 管理的是修改而不是文件，文件内容的增减，文件的创建和删除都属于修改。

对 readme.txt 新增一行后保存：
```shell
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git tracks changes.
```
然后添加到仓库：
```shell
git add readme.txt
```
查看仓库当前状态：
```shell
git status
On branch master
Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

            modified: readme.txt    
```
结果显示 readme.txt 的修改将被提交，然后再修改 readme.txt 如下后保存：
```shell
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git tracks changes of files.
```
然后提交修改：
```shell
git commit -m "git tracks changes"
[master 6ea23a0] git tracks changes
 1 file changed, 2 insertions(+), 1 deletion(-)
```
再查看仓库当前状态：
```shell
git status
On branch master
Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

            modified: readme.txt
```
结果显示， readme.txt 还有修改没有提交。原因是 commit 是负责提交缓存区的修改，第二次的修改没有添加到缓存区，所以没有没提交到版本库。可以用 `git diff HEAD -- readme.txt` 命令开查看 readme.txt 第二次修改和版本库中的不同。

Tips：Git 跟踪的是修改，若修改没有用 `git add` 命令添加到缓存区，就不会被 `git commit` 命令提交到版本库。

### 7 撤销修改
先查看 learngit 仓库的当前状态：
```shell
git status
On branch master
nothing to commit, working tree clean
```
结果显示工作区是干净的，现对 readme.txt 做如下修改：
```shell
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git tracks changes of files.
Git revokes modification.
```
但此时不想要最后添加的一行了，可以直接在编辑器中删除，也可以使用 `git checkout` 命令来撤销工作区中的修改：
```shell
git checkout -- readme.txt
```
然后查看 readme.txt ，果然修改被撤销了：
```shell
cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git tracks changes of files.
```
Tips：撤销修改的 `git checkout` 命令后面必须带上 `--`，不然变成了切换分支命令。

现在继续加上最后一行的修改，并将 readme.txt 添加到缓存区继续工作：
```shell
git add readme.txt
```
查看仓库当前状态：
```shell
git status
On branch master
Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

            modified: readme.txt
```
结果显示 readme.txt 的修改将被提交，但是现在不想提交，因为工作就剩一步就完成了，然后一起提交，但是当工作快完成的时候，最后一步的思路被完全打乱，文件非常混乱怎么都整理不好了， readme.txt 文件被修改如下：
```shell
cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git tracks changes of files.
Git revokes modification.
My stupied boss still prefers SVN.
```
此时可以使用 `git checkout` 命令回到上一次 `git add` 状态：
```shell
git checkout -- readme.txt
```
查看readme.txt：
```shell
cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git tracks changes of files.
Git revokes modification.
```
结果显示 `git add` 后的修改被撤销了，回到了上一次 `git add` 时的状态，查看仓库当前状态：
```shell
git status
On branch master
Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

            modified: readme.txt
```
结果显示上上次的修改添加到缓存区了将要被提交。此时接到新的需求，已经提交到缓存区的修改也不需要了，可以使用 `git reset` 命令撤销已经添加到缓存区中的修改，回到最初的工作区状态：
```shell
git reset HEAD readme.txt
Unstaged changes after reset:
M        readme.txt    
```
查看仓库当前状态：
```shell
git status
On branch master
Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

            modified: readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```
结果显示 readme.txt 的修改还没有被添加到缓存区，查看 readme.txt ：
```shell
cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git tracks changes of files.
Git revokes modification.
```
结果显示修改还保存在工作区，继续使用 `git checkout` 命令撤销工作区的需改：
```shell
git checkout -- readme.txt
```
查看仓库当前状态：
```shell
git status
On branch master
nothing to commit, working tree clean
```
结果显示工作区是干净的，工作区的修改已经被撤销。查看 readme.txt ：
```shell
cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git tracks changes of files.
```
确认修改已经被撤销。

小结：

- 当弄乱了工作区，想要撤销修改，可以使用命令：
```shell
git checkout -- filename
```
- 当弄乱了工作区，并已经添加到缓存区，可以使用如下命令后，再使用上述命令：
```shell
git reset HEAD filename
```
- 当已经提交不合适的内容到版本库，可以使用版本回退命令回到你想要的版本，参考版本回退。如果已经推送到远程仓库，那就。。呵呵。
- 
### 8 删除文件
Git 中删除和新增文件也是修改操作，在 learngit 仓库中新增 test.txt 文件，产看仓库仓前状态：
```shell
git status
On branch master
Untracked files:
    (use "git add <file>..." to include in what will be committed)

        test.txt

nothing added to commit but untracked files present (use "git add" to track)
```
结果显示 test.txt 还没有被跟踪，现在将 test.txt 添加到仓库并提交：
```shell
git commit -m "add test.txt"
[master 07332c3] add test.txt
1 file changed, 1 insertion(+)
create mode 100644 test.txt
```
一般要删除 test.txt 可以直接在文件资源管理器中直接删除，或者使用 `rm` 命令删除：
```shell
rm test.txt
```
此时， Git 知道你删除了哪些文件，可以用 `git status` 命令查看哪些文件被删除了：
```shell
git status
On branch master
Changes not staged for commit:
    (use "git add/rm <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

            deleted: test.txt

no changes added to commit (use "git add" and/or "git commit -a")
```
结果显示， test.txt 文件被删除了，但是这个修改还没有被提交到版本库，也就是说版本库中 test.txt 文件还存在。现在有两种情况：

- 确定要删除该文件，并且需要删除版本库中的该文件。用 `git rm` 命令删除版本库中的 test.txt 文件，并用 `git commit` 提交修改：
```shell
git rm test.txt
rm 'test.txt'
```
查看仓库当前状态：
```shell
git status
On branch master
Changes to be committed:
(use "git reset HEAD <file>..." to unstage)

        deleted: test.txt
```
提交修改：
```shell
git commit -m "delete test.txt"
[master bd9ecd8] delete test.txt
1 file changed, 1 deletion(-)
delete mode 100644 test.txt
```
至此，版本库中的 test.txt 文件就被删除了。

- 只是在工作区误删了该文件，可以使用 `git checkout -- filename` 命令可以撤销该文件的删除操作。
查看仓库当前状态：
```shell
git status
On branch master
Changes not staged for commit:
    (use "git add/rm <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

            deleted: test.txt

no changes added to commit (use "git add" and/or "git commit -a")
```
小结：

- 要删除版本库中的文件，使用命令：
```shell
git rm filename
```
- 如果一个文件已经提交到版本库，你永远不用担心误删，可以使用如下命令恢复到你最近一次提交到版本库时的版本，但你会丢失提交后的修改：
```shell
git checkout -- filename
```

### 9 远程仓库
将 GitHub 作为远程仓库，该网站提供 Git 仓库托管服务。

#### 9.1 创建 SSH Key
在用户主目录下看有没有 .ssh 目录，若有看该目录有没有 id_rsa 和 id_rsa.pub 文件，这两个文件就是 SSH Key 的密钥对，id_rsa 是私钥，不能泄露出去，id_rsa.pub 是公钥，可以告诉任何人。如果没有这两个文件，可以打开 git-bash 输入以下命令：
```shell
ssh-keygen -t rsa -C "youremail@example.com"
```
将上述 Email 地址换成自己的，一路回车即可，然后可以看见.ssh 目录下生成了这两个文件。可以直接在 git-bash 下使用 `cat` 命令来查看 id_rsa.pub 文件的内容。

#### 9.2 给 GitHub 账户添加 SSH Key
登陆 GitHub ，打开 Account settings ，SSH Keys 页面。然后，点 Add SSH Key，填上任意 Title，在 Key 文本框里粘贴 id_rsa.pub 文件的内容

#### 9.3 创建 GitHub 仓库
在 GitHub 上创建一个名为 learngit 的空仓库，该仓库有两个地址：
```shell
SSH地址：git@github.com:StrivePy/learngit.git
HTTPS地址：https://github.com/StrivePy/learngit.git
```
一般使用是 SSH 地址，传输速度较快，HTTPS 地址除传输速度慢外，还需要额外输入端口地址。

#### 9.4 关联 GitHub 仓库
现在本机上有一个名为 learngit 的本地仓库，Github 上有一个名为 learngit 的远程仓库，使用 `git remote` 命令可以将本地仓库和远程仓库关联起来：
```shell
git remote add origin git@github.com:StrivePy/learngit.git
```
StrivePy 是本机用户的 GitHub 用户名，该账户已经添加了本机的 SSH Key ，可以进行关联和推送，但是其它机器也可以关联，但是 SSH Key 不在 StrivePy 账户的 SSH Key 列表中，所以不能推送消息到该用户的仓库。origin 表示远程仓库的名字，当然也可以取你自己喜欢的名字。

#### 9.5 向远程仓库推送文件
远程仓库关联完成后，就可以使用 `git push` 命令将本地仓库当前分支文件推送到远程仓库：

```shell
git push -u origin master
Counting objects: 18, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (18/18), done.
Writing objects: 100% (18/18), 1.80 KiB | 614.00 KiB/s, done.
Total 18 (delta 5), reused 0 (delta 0)
remote: Resolving deltas: 100% (5/5), done.
To github.com:StrivePy/learngit.git
    be0db50..07332c3 master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```
进入 GitHub 的 learngit 仓库，可以看见仓库里面有文件，而且文件和本地 learngit 仓库文件一样，说明推送成功。本次是将本地 master 分支内容到远程仓库的 master 分支， `-u` 参数表示在将本地 master 分支推送到远程 master 分支的同时，并将两个 master 分支关联起来，以后就可以使用简化的 `git push` 命令进行推送。 现在只要在本地仓库做了提交，就可以使用 `git push` 命令将提交推送到远程仓库：
```shell
git push origin master
```
以上是理想的状况，实际操作中，GitHub 新建的仓库默认会有一个 README.md 文件，在使用 `git push -u origin master` 命令时，会提示远程仓库还有文件没有同步到本地，推送失败，而且会提示本地分支和远程仓库没有关联，可以使用如下命令进行关联：
```shell
git branch --set-upstream-to=origin/remote_branch  your_branch
```
然后再用 `git push` 命令进行推送，但是还会提示：
```shell
fatal: refusing to merge unrelated histories
```
原因是在合并了两个不同的提交仓库时（本地仓库和远程仓库）， git 会发现这两个仓库可能不是同一个，为了防止推送错误，所以给出上述提示。

如果的却清楚需要合并，可是在 `git pull` 命令后加上参数 `--allow-unrelated-histories` 来进行版本的合并：
```shell
git pull origin master --allow-unrelated-histories
```
如果还不能合并，看是否存在冲突，解决冲突后再进行合并。

小结：

- 关联远程仓库使用命令：
```shell
git remote add origin git@github.com:StrivePy/learngit.git
```
- 第一次推送分支使用命令：
```shell
git push -u origin master
```
此后，本地提交后若有需要推送到远程仓库，使用命令：
```shell
git push origin master
```
#### 9.6 从远程仓库克隆
在本地仓库 learngit 的同级目录下新建一个名为 clone 的目录，使用 git-bash 进入 clone 目录，使用如下命令将远程仓库 learngit 克隆到该目录：
```shell
git clone git@github.com:StrivePy/learngit.git
```
进入 clone 目录可以看见远程仓库 learngit 已经被克隆下来了。

小结：

- 要克隆一个远程仓库，必须先知道该仓库的地址，然后使用如下命令进行克隆：
```shell
git clone repository_address
```
Git 支持多种协议，包括 https ，但通过 ssh 支持的原生 git 协议速度最快。

### 10 分支管理
#### 10.1 创建与合并分支
master 是默认的主分支，我们一般不在上面进行工作，只是将其它分支合并到该分支，如有需要再将 master 分支推送到远程仓库。

首先创建名为 dev 的分支，并切换到该分支，使用如下命令：
```shell
git checkout -b dev
Switched to a new branch 'dev'
```
`-b` 参数表示创建并切换，该命令等效于：
```shell
git branch dev
git checkout dev
Switched to branch 'dev'
```
然后用 `git branch` 命令查看当前分支：
```shell
git branch
* dev
master
```
现在我们就可以在 dev 分支上正常工作了，因为是从 master 分支创建的 dev 分支，所有现在 dev 分支现在的工作区和 master 分支的工作区是一样的，现在在 readme.txt 加上一行：
```shell
Creating a new branch is quick.
```
然后添加并提交：
```shell
git add readme.txt
git commit -m "branch test"
[dev a9f9dbb] branch test
1 file changed, 4 insertions(+), 9 deletions(-)
```
现在切回 master 分支:
```shell
git checkout master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.
```
查看 readme.txt 内容发现刚刚添加的一样没有添加上，原因是在 dev 分支上添加的，现在将 dev 分支的工作合并到 master 分支上：
```shell
git merge dev
Updating d17efd8..fec145a
Fast-forward
readme.txt | 1 +
1 file changed, 1 insertion(+)
```
`git merge` 用于合并指定分支到当前分支，合并后发现 dev 的添加的一行合并过来了， Fast-forward 表示快进合并，直接把 master 指向 dev ,最后删除 dev 分支：
```shell
git branch -d dev
Deleted branch dev (was fec145a).
```
再查看分支：
```shell
git branch
* master
```
小结：

- 查看分支:
```shell
git branch
```
- 创建分支：
```shell
git branch name
```
- 切换分支：
```shell
git checkout branch_name
```
- 创建并切换分支：
```shell
git checkout -b name
```
- 合并某分支到当前分支：
```shell
git merge name
```
- 删除分支：
```shell
git branch -d name
```
#### 10.2 解决冲突
有时候合并并不是一帆风顺的，例如新建并切换到 feature1 分支:
```shell
git checkout -b feature1
Switched to a new branch 'feature1'
```
在 readme.txt 最后添加一行：
```shell
Creating a new branch is quick AND simple.
```
添加并提交：
```shell
git add readme.txt
git commit -m "AND simple"
[feature1 ddd008d] AND simple
1 file changed, 2 insertions(+), 1 deletion(-)
```
然后切换到 master 分支：
```shell
git checkout master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 1 commit.
(use "git push" to publish your local commits)
```
结果显示本地仓库比远程仓库要新，这个不用管。在 master 分支对 readme.txt 最后添加一行：
```shell
Creating a new branch is quick & simple.
```
添加并提交：
```shell
git add readme.txt
git commit -m "& simple"
[master 8368fab] & simple
1 file changed, 2 insertions(+), 1 deletion(-)
```
接下来合并，但是这种情况下， Git 无法执行快进合并，只能尝试把各自的修改合并起来，有可能产生冲突，接下来尝试一下：
```shell
git merge feature1
Auto-merging readme.txt
CONFLICT (content): Merge conflict in readme.txt
Automatic merge failed; fix conflicts and then commit the result.
```
结果提示产生冲突，需要手动解决冲突后再提交，使用 `git status` 可以查看冲突的文件：
```shell
git status
On branch master
Your branch is ahead of 'origin/master' by 2 commits.
    (use "git push" to publish your local commits)

    You have unmerged paths.
    (fix conflicts and run "git commit")
    (use "git merge --abort" to abort the merge)

Unmerged paths:
    (use "git add <file>..." to mark resolution)

            both modified: readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```
可以查看一下 readme.txt ：
```shell
cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
<<<<<<< HEAD
Creating a new branch is quick & simple.
=======
Creating a new branch is quick AND simple.
>>>>>>> feature1
```
Git用 <<<<<<<，=======，>>>>>>> 标记出不同分支的内容，我们修改如下后保存：
```shell
Creating a new branch is quick and simple.
```
再提交：
```shell
git commit -m "confict fixed"
[master 4de6f5f] confict fixed
```
用带参数的 `git log` 命令查看分支合并情况：
```shell
git log --graph --pretty=oneline --abbrev-commit
* 4de6f5f (HEAD -> master) confict fixed
|\
| * ddd008d (feature1) AND simple
* | 8368fab & simple
|/
* 14b0a6e modify readme.txt
```
最后删除 feature1 分支：
```shell
git branch -d feature1
Deleted branch feature1 (was ddd008d)
```
小结：

Git无法自动完成合并时，需要手动解决冲突，再提交，完成合并。
查看分支合并图可以使用如下命令：
```shell
git log --graph 
```

### 11 分支管理策略
#### 11.1 禁用 Fast Forward 合并模式
Git 默认采用的是快进合并模式，快进合并模式下，删除分支后，会丢掉分支信息。如果要强制禁用 Fast forward 模式， Git 在 merge 时会产生一个新的 commit ，这样在分支历史上就可以看出分支信息，接下来实验一下。

首先创建并切换到分支 dev :
```shell
git checkout -b dev
Switched to a new branch 'dev'
```
在 readme.txt 后增加一行内容：
```shell
Git fast forward merge.
```
并将修改提交：
```shell
git commit -m "no fast forward merge"
[dev 44e4f6f] no fast forward merge
1 file changed, 1 insertion(+), 1 deletion(-)
```
现在切换回 master 分支：
```shell
git checkout master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 13 commits.
    (use "git push" to publish your local commits)
```
然后用 `--no-ff` 模式将 dev 分支合并到 master 分支：
```shell
git merge --no-ff -m "merge with --no-ff" dev
Merge made by the 'recursive' strategy.
readme.txt | 2 +-
1 file changed, 1 insertion(+), 1 deletion(-)
```
Tips：因为本次合并要新建一个 commit 所以带上 `-m` 参数添加提交说明

然后用 `git log` 命令查看日志:
```shell
git log --graph --pretty=oneline --abbrev-commit
* df99854 (HEAD -> master) merge with --no-ff
|\
| * 44e4f6f (dev) no fast forward merge
|/
* 9e4a116 merge with --no-ff model
```
日志图形结果显示了分支的历史信息。

小结：

使用 `git merge` 命令带上 `--no-ff` 参数就表示用普通模式进行合并，合并后的历史有分支，能看出来曾经做过合并，而 Fast forward 模式则看不出来曾经做过合并。
#### 11.2 Bug分支
每一个 Bug 都可以通过一个新的临时分支来修复，然后在合并分支，最后将临时分支删除。假设正在 dev 分支上工作，对 readme.txt 文件修改工作，但接到任务说 master 分支上有一个名为 issue-001 的 Bug 被发现了，需要紧急修改，此时看一下 dev 分支的工作状态：
```shell
git status
On branch dev
Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

            modified: readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```
结果显示 dev 分支上还有没有提交的修改，但是修改只进行了一半，无法立刻提交，而现在又要去修改 Bug ，该怎么处理，好在 Git 有一个 `git stash` 命令可以将工作现场暂时保存起来，等待以后恢复现场继续工作：
```shell
git stash
Saved working directory and index state WIP on dev: df99854 merge with --no-ff
```
再查看工作区的状态：
```shell
git status
On branch dev
nothing to commit, working tree clean
```
结果显示工作区是干净的。接下来就可以到 master 分支上去修复 Bug 了。 切换到 master 分支，再该分支上创建并切换到名为 issue-001 的分支来修复 Bug ：
```shell
git checkout master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 15 commits.
    (use "git push" to publish your local commits)
git checkout -b issue-001
Switched to a new branch 'issue-001'
```
现在修复 Bug 需要删除 readme.txt 的最后一行，修改完毕后添加并提交：
```shell
git add readme.txt
git commit -m "fixed issue-001"
[issue-001 3dfa376] fixed issue-001
1 file changed, 1 insertion(+), 1 deletion(-)
```
然后切换到 master 分支，完成合并，并删除 issue-001 分支：
```shell
git checkout master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 15 commits.
(use "git push" to publish your local commits)
git merge --no-ff -m "merge bug fix issue-001" issue-001
Merge made by the 'recursive' strategy.
 readme.txt | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
git branch -d issue-001
Deleted branch issue-001 (was 3dfa376).
```
Bug 修复完毕，回到 dev 分支继续工作，查看工作区状态：
```shell
git status
On branch dev
nothing to commit, working tree clean
```
结果显示工作区是空的，使用 `git stash list` 查看工作现场的保存地址：
```shell
git stash list
stash@{0}: WIP on dev: df99854 merge with --no-ff
``` 
恢复工作现场的两种方式：

使用 `git stash apply stash@{0}` ，但是恢复后，stash 的内容并不会被删除，需要使用 `git stash drop stash@{0}` 命令来删除 stash 内容。
使用 `git stash pop` 命令直接恢复并删除 stash 内容：
```shell
git stash pop
On branch dev
Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)
    
           modified: readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
Dropped refs/stash@{0} (eb9fe8bbd71a8abc5a6e3badaa6a526d6c774ee8) 
```
可以看见工作现场恢复了，而且使用 `git stash list` 命令查看不到任何 stash 内容。

可以多次保存 stash ，然后用 `git stash list` 查看，然后用 `git stash apply stash@{number}` 来恢复指定的现场。

小结：

- 修复 Bug 时，新建临时分支，解决完 Bug 后，合并分支，最后删除分支。
- 使用 `git stash` 可以保存工作现场，然后去完成其工作，最后回来恢复现场继续工作。
- 要丢弃一个没有合并的分支，可以使用 `git branch -D name` 强行铲除分支。

### 12 多人协作
#### 12.1 远程仓库信息
当从远程仓库 clone 时， Git 实际上把本地的 master 分支和远程 master 分支关联起来，并且远程仓库的默认名称为 origin ，要查看远程仓库信息。

用 `git remote` 命令：
```shell
git remote
origin
```
或者用 `git remote -v` 查看远程仓库更详细的信息：
```shell
git remote -v
origin git@github.com:StrivePy/learngit.git (fetch)
origin git@github.com:StrivePy/learngit.git (push)
```
上面显示了抓取和推送 origin 的地址。如果没有推送权限，就看不见 origin 的推送地址。

#### 12.2 推送分支
推送分支，就是把本地分支上的所有提交推送到远程仓库。推送时加上分支名，Git就把该本地分支上的所有提交推送到对应的远程分支上：
```shell
git push origin master
```
要推送其它分支，使用对应的分支名称即可：
```shell
git push origin dev
```
并不是所有的分支都需要推送到远程仓库，需要推送到远程仓库的分支:

master 分支作为主分支，需要时刻与远程仓库同步。
dev 作为开发分支，团队成员都需要在上面工作，也需要时刻与远程仓库同步。
bug 分支只在本地修改 bug ，不需要推送到分支。
feature 分支是否推送到远程仓库，取决于是否和团队成员在该分支上合作开发。
#### 12.3 抓取分支
多人协作时，团队成员都会时不时往 master 分支和 dev 分支推送自己的修改。

在本地 learngit 仓库同级目录下新建一个 cooperation 目录来模拟另一个团队成员，进入 cooperation 目录将远程 learngit 仓库克隆下来：
```shell
git clone git@github.com:StrivePy/learngit.git
Cloning into 'learngit'...
remote: Counting objects: 81, done.
remote: Compressing objects: 100% (49/49), done.
remote: Total 81 (delta 28), reused 77 (delta 25), pack-reused 0
Receiving objects: 100% (81/81), 7.71 KiB | 281.00 KiB/s, done.
Resolving deltas: 100% (28/28), done.
```
默认情况下， clone 完成后只能看见 master 分支：
```shell
git branch
* master
```
想要在 dev 分支上进行开发，就必须创建远程 origin 的 dev 分支到本地：
```shell
git checkout -b dev origin/dev
Switched to a new branch 'dev'
Branch 'dev' set up to track remote branch 'dev' from 'origin'.
```
此时就能 dev 分支上进行工作了：
```shell
git branch
* dev
master
```
先查看 readme.txt 的内容：
```shell
cat readme.txt
git is a distributed version control system
git is a free software distributed under GPL
git has a mutable index called stage
git tracks changes of files
test git merge
create a new branch is simple And quick
add merge
test stash
add usr/bin/evn
```
将最后一行修改为：
```shell
add usr/bin/evn/test
```
然后提交并推送到远程仓库：
```shell
git add readme.txt
git commit -m "add usr/bin/evn/test"
[dev 62c782a] add usr/bin/evn/test
1 file changed, 1 insertion(+), 1 deletion(-)
git push origin dev
Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 293 bytes | 293.00 KiB/s, done.
Total 3 (delta 2), reused 0 (delta 0)
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
To github.com:StrivePy/learngit.git
    23625fd..62c782a dev -> dev
```
此时，回到本地非 clone 的 learngit 仓库，查看 readme.txt 内容：
```shell
cat readme.txt
git is a distributed version control system
git is a free software distributed under GPL
git has a mutable index called stage
git tracks changes of files
test git merge
create a new branch is simple And quick
add merge
test stash
add usr/bin/evn/
```
将最有一行修改为：
```shell
add usr/bin/evn/tests
```
然后提交修改并尝试推送到远程仓库：
```shell
git commit -m "add usr/bin/evn/tests"
[dev 9c1fa27] add usr/bin/
1 file changed, 1 insertion(+), 1 deletion(-)
git push origin dev
To github.com:StrivePy/learngit.git
! [rejected] dev -> dev (fetch first)
error: failed to push some refs to 'git@github.com:StrivePy/learngit.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```
结果因为另外的远程推送导致本次推送失败，你可能需要先整合（合并）远程修改再进行推送，所以使用 `git pull` 命令拉一下远程仓库：
```shell
git pull
remote: Counting objects: 3, done.
remote: Compressing objects: 100% (1/1), done.
remote: Total 3 (delta 2), reused 3 (delta 2), pack-reused 0
Unpacking objects: 100% (3/3), done.
From github.com:StrivePy/learngit
　　9c1fa27..1dacdbc dev -> origin/dev
Auto-merging readme.txt
CONFLICT (content): Merge conflict in readme.txt
Automatic merge failed; fix conflicts and then commit the result.
```
`git pull` 失败，结果提示合并时，在 readme.txt 文件中有冲突，查看 readme.txt 文件：

```shell
cat readme.txt
git is a distributed version control system
git is a free software distributed under GPL
git has a mutable index called stage
git tracks changes of files
test git merge
create a new branch is simple And quick
add merge
test stash
<<<<<<< HEAD
add usr/bin/evn/tests
=======
add usr/bin/evn/test
>>>>>>> 1dacdbc0ca9202c1e11c366ce6b9c13f44cb5b47
```
将最后一行修改为：
```shell
add usr/bin/evn/
```
然后提交到本地仓库，再远程推送：
```shell
git add readme.txt
git commit -m "merge and fixed"
[dev 1a26ae2] merge and fixed
git push origin dev
Counting objects: 6, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (6/6), done.
Writing objects: 100% (6/6), 572 bytes | 286.00 KiB/s, done.
Total 6 (delta 4), reused 0 (delta 0)
remote: Resolving deltas: 100% (4/4), completed with 2 local objects.
To github.com:StrivePy/learngit.git
    1dacdbc..1a26ae2 dev -> dev
```
结果显示推送成功，远程仓库中的 readme.txt 文件以解决冲突后文件为准。

小结：

- 查看远程库信息，使用命令：
```shell
git remote -v
```
本地创建的分支，如果不推送到远程，对其他人是不可见的。
- 多人协作的工作模式：
  - 首先可试图用命令 `git push origin branch_name` 远程推送对自己的修改。
  - 如果推送失败，则因为远程分支要比本地分支更新，用命令 `git pull` 拉取一下试图合并，如果拉取时提示 no tracking information ，用命令 `git branch --set-upstream=origin/remote_branch your_branch` 建立本地分支和远程分支的联系。
  - 如果合并失败，手动解决冲突，并在本地提交。
  - 没有冲突或者解决冲突后，使用命令 `git push origin your_branch` 推送。
  
### 13 标签管理
#### 13.1 创建标签
要创建标签，首先切换到要打标签的分支：
```shell
git branch
* dev
master
git checkout master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.
```
然后使用 `git tag tag_name` 创建标签：
```shell
git tag v1.0
```
可以使用 `git tag` 命令查看所有标签：
```shell
git tag
v1.0
```
默认标签是打在最新提交的 commit 上的，可以使用 `git tag tag_name commit_id` 命令对任何提交打上标签：
```shell
git log --pretty=oneline --abbrev-commit
1ba8102 (HEAD -> master, tag: v1.0, origin/master) delete dev
cd05c0d merge bug fix issue-001
3dfa376 fixed issue-001
df99854 merge with --no-ff
44e4f6f no fast forward merge
9e4a116 merge with --no-ff model
3a6b35d merge test..
ce6ca91 merge test
3f0ad45 conflict fixed
4de6f5f confict fixed
8368fab & simple
```
如果相对 fixed issue-001 这次提交打上标签：
```shell
git tag v0.9 3dfa376
```
用 `git show tag_name` 查看标签信息：
```shell
git show v0.9
commit 3dfa3760095617a41a068cf8b839c2a278e249c4 (tag: v0.9)
Author: StrivePy <1013974267@qq.com>
Date: Mon Mar 19 20:19:37 2018 +0800

        fixed issue-001

diff --git a/readme.txt b/readme.txt
index 23580ea..294d3b9 100644
--- a/readme.txt
+++ b/readme.txt
@@ -3,4 +3,4 @@ Git is free software distributed under the GPL.
 Git has a mutable index called stage.
 Git tracks changes of files.
 Creating a new branch is quick and simple.
-Git fast forward merge.
+
```
标签确实打在 fixed issue-001 这次提交上。

可以使用 `git tag -a tag_name -m "information" commit_id` 打上带说明信息的标签， `-a` 表示标签名， `-m` 表示说明信息：
```shell
git tag -a v0.1 -m "version 0.1 released" cd05c0d
```
查看标签信息：
```shell
git show v0.1
tag v0.1
Tagger: StrivePy <1013974267@qq.com>
Date: Thu Mar 22 19:40:32 2018 +0800

version 0.1 released

commit cd05c0d4223a76abfa4dd6d55fb1dcc7eac8c10c (tag: v0.1)
Merge: df99854 3dfa376
Author: StrivePy <1013974267@qq.com>
Date: Mon Mar 19 20:23:09 2018 +0800
    
            merge bug fix issue-001    
```
小结：

- 对默认的最新提交打标签使用命令：
```shell
git tag tag_name
```
- 要标签上带上说明信息，使用如下命令：
```shell
git tag -a tag_name -m "information" commit_id
```
- 用PGP签名标签，使用如下命令：
```shell
git tag -s tag_name -m "information" commit_id 
```
- 显示所有标签，使用命令：
```shell
git tag
```
- 显示标签详细信息，使用命令：
```shell
git show tag_name 
```
#### 13.2 操作标签
想要删除一个标签，使用 `git tag -d tag_name` 命令：
```shell
git tag -d v0.1
Deleted tag 'v0.1' (was 4630d67)
```
推送本地标签到远程：
```shell
git tag
v0.9
v1.0
git push origin v0.9
Total 0 (delta 0), reused 0 (delta 0)
To github.com:StrivePy/learngit.git
* [new tag]         v0.9 -> v0.9    
```
或者使用如下命令，将本地所有未推送的标签一次性推送到远程仓库：
```shell
git push origin --tags
Total 0 (delta 0), reused 0 (delta 0)
To github.com:StrivePy/learngit.git
* [new tag]         v1.0 -> v1.0
```
如果标签已经推送到了远程仓库，可以用以下步骤删除远程仓库标签：

先删除本地标签：
```shell
git tag -d v0.9
Deleted tag 'v0.9' (was 3dfa376)
```
再删除远程仓库的标签：
```shell
git push origin :refs/tags/v0.9
To github.com:StrivePy/learngit.git
- [deleted]         v0.9
```
登陆GitHub查看该标签确实被删除了。

小结：

- 推送一个本地标签，使用命令：
```shell
git push origin tag_name
```
- 推送本地所有未推送的标签，使用命令：
```shell
git push origin --tags
```
- 删除一个本地标签，使用命令：
```shell
git tag -d tag_name
```
- 删除一个远程标签，使用命令：
```shell
git push origin :refs/tags/tag_name 
```