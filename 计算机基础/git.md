## 1. 工作区、暂存区和版本库
- 工作区： 电脑中能看到的目录
- 暂存区： stage、index 一般存放在.git 目录下的index文件
- 版本库： 工作区的隐藏目录.git,不算工作区，是git的版本库

1. 项目状态  
项目文件有4种状态，git可以管理三种状态：已提交(committed) 已修改(modified) 已暂存(staged) 第四个状态:未跟踪  
    - 已修改：表示修改了文件，但是还没保存到数据库中
    - 已暂存：表示对一个已修改文件的当前版本进行了标记，使之包含在下次提交的快照中
    - 已提交：表示数据已安全保持在本地数据库中(所以也可以称为未修改状态)
    - 未跟踪：新创建的文件如果没有添加到暂存区，那么git没法对其进行跟踪，故而是‘未跟踪’状态
    

## 2. git 配置和结构  

1. git config，配置或读取相应的工作环境变量
    - /etc/gitconfig 系统对所有用户都普遍适用的配置， git config --system  
    - ~/.gitconfig 用户目录下的配置文件，只适用于该用户 git config --global

2. 结构  
![](https://www.runoob.com/wp-content/uploads/2015/02/git-command.jpg)  
![](https://img2020.cnblogs.com/blog/1003703/202011/1003703-20201127190554776-322950002.png)

## 3. git 基本操作

- workspace 工作区
- staging eara 暂存区、缓冲区
- local reponsitory 本地仓库
- remote repository 远程仓库

1. git diff 
    比较文件的不同，即暂存区和工作区的差异
2. git reset 
    回退版本
3. git remove
    删除工作区文件
4. git move
    移动或重命名工作区文件
5. git blame \<file>
    以列表形式查看指定文件的历史修改记录

## 4. git 分支管理
1. git branch (branchName)
    创建分支
2. git checkout (branchName)
    切换分支
3. git merge (branchName)
    合并分支
4. git branch -d (branchNmae)
    删除分支
5. git checkout -b (branchName)
    创建并切换到新分支

6. git合并分支，冲突处理

## 5. git 标签
git tag -a <tagName> -m "runoob"  
git log --decorate

## 6. git 文件跟踪
git rm -r --cached .  //取消跟踪，但不删除本地文件
git rm -r -f . //取消跟踪，并删除本地文件


## 参考链接
https://www.runoob.com/git/git-workspace-index-repo.html