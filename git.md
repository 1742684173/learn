https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013743256916071d599b3aed534aaab22a0db6c4e07fd0000

# **1､创建版本库(repository)**

版本库可以理解成一个目录，这个目录里面的所有的文件都可以被git管理起来，每个文件的修改、删除，git都能跟踪，以拿任何时刻都可以追踪历史，或者还原

创建目录：mkdir learngit

进入目录：cd learngit

显示目录：pwd learngit

初始化：git init 



# **2､添加提交内容**

添加readme：git add readme.txt  可以添加一个或多个 git add file1  file2

提交文件：git commit -m ''    一次提交全部的

查看状态：git status 能过改变readme.txt的内容 

查看具体改变什么：git diff

注意： 

工作区

add是把文件添加到暂存区

commit是把暂存区的所有内容提交到当前分支

 **2､版本回滚**

查看日志：git log  或 git log --protty==oneline

回滚：git reset --hard HEAD^   注意：上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100

回到指定版本：git reset --hard 版本id  注意：只需要前四位就行

查看之前每一个命令：git reflog

撤消工作区：git checkout -- <file>

撤消暂存区(add)到工作区：git reset HEAD <file>

**场景1**：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

**场景2**：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD <file>，就回到了场景1，第二步按场景1操作。

**场景3**：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考[版本回退](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013744142037508cf42e51debf49668810645e02887691000)一节，不过前提是没有推送到远程库。



# **3､删除文件**

删除工作区文件：rm <file>

确定删除：git rm <file> -----> git commit

误删：git checkout --  <file>



# **4､分支**

创建分支：git branch 分支名称

切换分支：git checkout 分支名称   

查看分支：git branch

创建并切换分支：git checkout -b 分支名称   

fast forward合并分支：git merge 分支名称  

普通合并：git merge --no-ff -m "merge with no-ff" dev   //合并要创建一个新的commit，所以加上-m参数，把commit描述写进去。

注意：普通合并合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并

删除分支：git branch -d 分支名称

查看分支合并情况：git log --graph --pretty=oneline --abbrev-commit



# **5､Bug分支**

**方法：**

修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再git stash pop，回到工作现场。

保存当存工作区：git stash

查看保存的工作区：git stash list

恢复但不删除工作区：git stash apply

删除工作区：git stash drop

恢复并删除工作区：git stash pop 

 

# **6､添加新功能分支**

开发一个新功能，新建一个分支，在分支上开发，再合并到主分支，然后删除；

如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。



# **7､多人协作**

查看远程库：git remote

查看远程详细库：git remote -v

推送到远程分支：git push origin <name>

在本地创建和远程分支对应的分支：git checkout -b <branch-name> origin/<branch-name>

建立本地分支和远程分支的关联：git branch --set-upstream <branch-name> origin/<branch-name>；

多人协作的工作模式通常是这样：

1.首先，可以试图用git push origin <branch-name>推送自己的修改；

2.如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；

3.如果合并有冲突，则解决冲突，并在本地提交；

4.没有冲突或者解决掉冲突后，再用git push origin <branch-name>推送就能成功！

如果git pull提示no tracking information，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream-to <branch-name> origin/<branch-name>。



# **8､在服务器上搭建自己的git(centos)**

https://www.cnblogs.com/liter7/p/6581344.html

**服务端：**

安装git：yum -y install git

创建裸仓库：git init --bare <仓库名称>

创建git用户：useradd git

给用户git密码：passwd git

赋予git权限：chown -R git:git <仓库名称>

禁用git用户shell登录：vi /etc/passwd   将不git用户修改如下（一般在最后一行）git:x:1000:1000::/home/git:**/usr/bin/git-shell**

**客户端：**

创建用户：

git config --global user.name "你的名字" git config --global user.email "你的邮箱"

创建密钥：ssh-keygen -t rsa -C "你的邮箱"

将公钥(/Users/zxl/.ssh/d_rsa.pub)复制内容加入服务器列表(/root/.ssh/authorized_keys)

克隆远程到本地：git clone git@101.101.101.101:/usr/local/git/learngit.git



# **9､tag**

创建标签：git tag <tagname>  或 git tag -a <tagname> -m "blablabla..."

查看标签：git tag

删除标签：git tag -d <tagname>

删除远程标签：git push origin :refs/tags/<tagname>

推送远程标签：git push origin <tagname>  

推送全部：git push origin --tags



# 10.本地配置

```
git config --global user.name username
git config --global user.email username@email.com
```