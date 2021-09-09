##### 1.状态查看： 

git status
可以查看工作区，暂存区的状态

- untracked 在暂存区没有该文件
- modified 修改过
- staged  使用git add 暂存过

##### 2.添加操作：

git add [file name]
将工作区的新建/修改添加到暂存区

##### 3.提交操作

git commit -m "commit message" [file name]
将暂存区的内容提交到本地仓库

##### 4.查看历史记录

git log
多屏显示控制方式
空格向下翻页
b向上翻页
q退出
git log --pretty=online // 简洁方式
git log --oneline
git reflog

##### 5.前进后退

- 5.1基于索引值操作[推荐]
  git reset --hard [索引值]
- 5.2使用^符号：只能后退
  git reset --hard HEAD^(表示往后退一步,后退几步加几个^)
- 5.3使用~符号：只能后退
  git reset --hard HEAD~n(表示后退n步)

##### 6.reset命令的三个参数对比

- --soft 参数

  仅仅在本地库移动指针

- --mixed参数

  在本地库移动HEAD指针

  重置暂存区

- --hard参数（常用）

  在本地库移动HEAD指针

  重置暂存区

  重置工作区

##### 7.删除文件后找回

前提：删除前，文件存在的状态提交到了本地库

操作：git reset --hard[指针位置]

- 删除操作已经提交到本地库：指针位置指向历史记录
- 删除操作尚未提交到本地库：指针位置使用HEAD

##### 8.文件比较

- git diff [文件名]   将工作区中的文件和暂存区进行比较
- git diff  [本地库历史版本] [文件名] 将工作区中的文件和本地历史版本进行比较
- 不带文件名可以比较多个文件
- git diff --cached   比较缓存区与本地库最近一次commit内容
- git diff HEAD 比较工作区与本地最近一次commit内容
- git diff <commit ID> <commit ID>  比较两个commit之间的差异

##### 9.分支管理

- git branch 查看本地分支
- git branch -r 查看远程分支
- git branch [分支名] 新建一个分支
- git branch -d [分支名] 删除分支
- git checkout [分支名] 切换分支
- git checkout -b [分支名] 新建分支并切换到该分支
- git merge [分支名] 合并分支
- git branch --merged 查看哪些分支已经合并到当前分支
- git branch --no-merged 查看哪些分支没有合并到当前分支

##### 10.git远程库别名

git remote add [别名] [远程库地址]

##### 11.远程库修改的拉取

pull = fetch + merge

git pull [远程库地址别名] [远程库分支名]

git fetch [远程库地址别名] [远程库分支名]

##### 12.推送到远程库

git push [远程库地址别名] [远程库分支名]

##### 13.文件暂存

- git stash save -a "message" 添加改动到stash
- git stash drop ` <stash@(ID)>  `  删除暂存
- git stash list 查看stash列表
- git stash clear   删除全部暂存
- git stash pop `<stash@(ID)>` 恢复改动

##### 14.撤销操作

- git checkout -- <file> 撤销工作区修改
- git reset HEAD <file> 撤销暂存区文件（不覆盖工作区）
- git reset --(soft | mixed | hard) <HEAD -num> | <commit ID> 版本回退

##### 15.线上回滚操作

```git
方法一：（强制指针回移）
1.查看历史记录
git reflog
2.本地分支回滚到指定版本
git reset --hard [索引值]
3.强推到远程(直接push推不上去，需要强推，因为reset之后，远程分支比本地分支新)
git push -f origin [远程分支名]

方法二：(推荐)
1.查看历史记录
git reflog
2.反向新创建一个版本，这个版本的内容与我们回滚的版本内容一致，HEAD会指向这个新版本，而不是回退到之前版本
git revert [索引值]
3.直接推送到远程即可
git push origin [远程分支名]
```

##### 16.查看文档

git help
git help [命令]

git help -a  展示git命令大纲全部列表



![image-20201121205957445](/Users/admin/Desktop/code/my_study/笔记/前端架构/git/images/image-20201121205957445.png)