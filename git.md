# Git

> git 配置, 命令



## catalogue

[TOC]

##git config

* **新建ssh key**

  ```shell
  ssh-keygen -t rsa -C 'zexiplus@outlook.com'
  ```

* **新增远程仓库**

  ```shell
  git remote add origin git@github.com:zexiplus/oo.git
  ```

* **显示秘钥**

  ```shell
  cat /c/Users/username/.ssh/id_rsa.pub 
  ```

* **配置用户名密码**

  ```shell
  git config --global user.name 'username'
  git config --global user.email 'username@tt.com'
  ```



## git command

> git 常用命令, [中括号内]代表可省略

* **显示所有仓库列表**

  ```shell
  git branch -a
  ```

* **显示所有仓库列表**

  ```shell
  git branch -r
  ```

* **显示本地仓库列表**

  ```shell
  git branch
  ```

* **新建本地仓库分支**

  ```shell
  git branch A
  # or
  git checkout -b A
  ```

* **从远程拉取分支到本地分支(新建)**

  ```shell
  git checkout -b R origin/A
  ```

* **切换分支**

  ```shell
  git checkout A
  ```

* **切换回上次分支**

  ```shell
  git checkout -
  ```

* **删除分支**

  ```shell
  git branch -d A
  ```

* **把A分支合并到当前分支并作记录**

  ```shell
  git merge -no-off A
  ```

* **修改上一条commit记录**

  ```shell
  git commit --amend
  ```

* **图形化查看记录**

  ```shell
  git log --graph
  ```

* **回滚到记录**

  ```shell
  git reset --hard #hash
  ```

* **查看回滚记录**

  ```shell
  git reflog
  ```

* **查看工作区和暂存区差别**

  ```shell
  git diff
  ```

* **查看工作区和最新提交的差别**

  ```shell
  git diff HEAD
  ```

* **推送至远程仓库**

  ```shell
  git push -u [origin master]
  ```

* **从远程仓库拉取到本地仓库并合并到当前分支**

  ```shell
  git pull [origin master]
  ```

* **从远程仓库拉取到本地仓库不自动合并**



```shell
                                                                               


git pull [origin master]                      

# 从远程仓库拉取到本地仓库不自动合并
git fetch [origin master]                     

# 比较本地仓库master和拉取到的远程仓库master区别
git diff master origin/master                 

# 合并已拉取到的远程仓库
git merge origin/master                       

# 备份当前的工作区的内容，从最近的一次提交中读取相关内容(被强行commit可跳过)
git stash             

# 从Git栈中读取最近一次保存的内容，恢复工作区的相关内容
git stash pop 	      

# 更改仓库地址信息,之后添加
git config -e         
[user]
        name = shizx	
        email = shizx@balabala.com
# 存储提交人信息, 之后不用输入账户密码
[credential]
		helper = store
		
# 把所有文件修改，删除，增加添加到暂存区
git add . 

# 把某个文件从暂存区移除（工作区该文件还存在，会影响之后的本地仓库提交）
git rm --cached filePath 

# 把某个文件中从暂存区移除(本地还存在)
git rm -r --cached node_modules

# 把所有文件修改，删除，增加添加到暂存区
git add . 

# 把某个文件从暂存区的修改移除
git reset filePath 

# 只恢复某个文件至某个历史版本
git checkout ${commit} filePath 
```



## git 四个阶段的撤销

* **概念**
  * 工作区: 文件在硬盘上的操作记录, git add 之前的状态
  * 暂存区: 文件的暂存操作记录, git add 之后 git commit 之前
  * 本地版本库: 已经提交的文件记录, git commit 之后, git push 之前, 会有对应的hash版本号
  * 远程版本库: 已经推送到远程仓库的文件记录 , git push 之后

* **查看/撤销工作区文件变更  (只修改了没有git add )**

  ```shell
  # 查看差别
  git diff
  
  git checkout . 
  # or
  git reset –-hard
  ```

------

* **查看/撤销暂存区和本地仓库差异（已经git add 没有 git commit）**

  ```shell
  # 查看差别
  git diff –-cached
  
  # 还原暂存区(git add 反向操作)
  git reset
  
  # 还原工作区和暂存区
  git checkout .
  # or
  git reset --hard
  ```

------

* **查看/撤销本地仓库和远程仓库差异（已提交未推送, git commit后还没有git push）**

  ```shell
  # 查看差异
  git diff master origin/master
  
  # 撤销修改至远程版本库
  git reset --hard origin/master
  ```



------

* **已经推送到远程仓库撤回(git push)**

  ```shell
  # 恢复至上次提交 并推送重新覆盖
  git reset --hard HEAD^
  git push -f
  ```

------







