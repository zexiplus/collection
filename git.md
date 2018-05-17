## git command

```shell
### 配置git

# 新建ssh key....
ssh-keygen -t rsa -C 'zexiplus@outlook.com'

# 新增远程仓库地址
git remote add origin git@github.com:zexiplus/oo.git         

# 显示密钥
cat /c/Users/username/.ssh/id_rsa.pub 

# 设置账户密码

git config --global user.name 'username'
git config --global user.email 'username@tt.com'

###

# 显示所有仓库信息
git branch -a  

# 显示远程仓库信息
git branch -r

# 显示本地仓库信息
git branch                                   

# 新建A分支
git branch A === git checkout -b A 		      

# 从远程拉取本地不存在的分支
git checkout -b 本地分支名 origin/远程分支名    

# 删除A分支
git branch -d A                              

# 切换到A分支(从远程拉取A分支)
git checkout A                                

# 切换回上一分支
git checkout -                                

#把A分支合并到当前分支并做记录
git merge -no-off A							  

#修改上一条commit 记录
git commit --amend                            

#图形化查看历史记录
git log --graph                               

# 回滚    
git reset --hard '#hash'                             

# 查看回滚日志
git reflog                                    

# 查看工作区和暂存区差别
git diff                                     

# 查看工作区和最新提交区别(git commit后)
git diff HEAD                                 

# 推送至远程仓库
git push -u [origin master]                   

# 从远程仓库拉取到本地仓库并合并到当前分支
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
# 把所有文件修改，删除，增加添加到暂存区
git add . 

# 把某个文件从暂存区移除（工作区该文件还存在，会影响之后的本地仓库提交）
git rm --cached filePath 

# 把所有文件修改，删除，增加添加到暂存区
git add . 

# 把某个文件从暂存区的修改移除
git reset filePath 

# 只恢复某个文件至某个历史版本
git checkout ${commit} filePath 
```



## git 四个阶段的撤销

**1**.查看工作区和暂存区差别  (只修改了没有git add )

git diff

撤销工作区和暂存区差别

git checkout **.    **或  git reset –-hard

------

**2**.查看暂存区和本地仓库差异（已经git add 没有 git commit）

git diff –-cached

撤销修改

git reset    // 把更改撤销至暂存区 及git add . 之前的状态（git add 的反向操作） 

git checkout . // 完全还原至上次的commit 

或 git reset –-hard

------

**3**.查看本地仓库和远程仓库差异（已提交未推送, git commit后还没有git push）

git diff master origin/master

撤销修改

git reset –hard origin/master

------

**4**.已经推送到远程仓库撤回(git push)

git diff master origin/master

撤回

git reset –hard HEAD^ 

git push -f

------







