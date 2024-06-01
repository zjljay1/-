[Git 命令大全「全面且实用，值得收藏」_git命令大全_八了个戒的博客-CSDN博客](https://blog.csdn.net/XH_jing/article/details/121900458)
<a name="ZekeV"></a>
## git推送代码流程

1. git add ....(代码添加到暂存区)
2. git commit -m <描述>  （将暂存区的代码提交到本地仓库上）
3. git push origin <远程分支>	(将本地分支的代码提交到远程仓库分支上)
<a name="hWnNg"></a>
#### 分支推送：
场景：远程仓库分支：dev(主分支) f-dev(我的分支) h-dev(其他人的分支)。<br />本地仓库分支：dev(主分支) f-dev(我的分支) h-dev(其他人的分支)

1. 在你的当前分支推送代码
```powershell
git add .... <将代码添加到暂存区>
git commit -m <描述> （将暂存区的代码提交到本地仓库上）
git push origin <远程分支（f-dev）>(将本地分支的代码提交到远程仓库分支上)
```

2. 切换分支，提交代码
```powershell
git checkout <分支名称> dev   		# 切换到你要合并的分支
git pull												#	更新这个分支的最新代码
git merge <合并的分支名称> f-dev #	合并f-dev分支到当前分支(dev)
git push origin dev 						# 推送代码到dev分支
```

3. 分支冲突

<a name="QtUa6"></a>
## 项目前的GIT配置
<a name="VAaXf"></a>
#### 1. 检查 git 版本
```
git --version
```
<a name="YkWDL"></a>
#### 2. 查看 git 相关命令
```
git --help
```
<a name="IBg79"></a>
#### 3. 查看当前的 git 配置信息
```bash
git config --list
```
<a name="jUkU4"></a>
#### 4. 查看 git 用户名 或 邮箱
```
# 查询git所使⽤的用户名
git config user.name
# 查询git所使⽤的email
git config user.email

# 注： --global 表示全局， 没有--global表示只查询在当前项目中的配置
git config --global user.name
git config --global user.email

```
<a name="DJADV"></a>
#### 5. 全局配置用户名(设置 git 使⽤者名称)
```
git config --global user.name "username"
```
<a name="doejN"></a>
#### 6. 设置 （配置）全局邮箱
```
git config --global user.email "eamil@qq.com"
```
<a name="D9qWJ"></a>
## Git对项目代码进行管理
<a name="bioEI"></a>
#### 1. 初始化 git 储存
```
git init
```
<a name="cCYKN"></a>
#### 2. 需要提交的所有修改放到暂存区（Stage）
```
git add *  # 将工作区所有修改添加到暂存区
git add .  # 将工作区所有修改添加到暂存区
git add <file-name>  # 将指定文件添加到暂存区
git add *.js  # 提交所有 .js 格式文件
git add -f <file-name>  # 强制添加 指定文件添加到暂存区

# 注：<file-name> 指的是文件的名称
```
<a name="Hg8DS"></a>
#### 3. 将暂存区的文件恢复到工作区
```
git reset <file-name>   # 从暂存区恢复指定到工作区
git reset -- .          # 从暂存区恢复所有文件到工作区
git reset --hard        # 把暂存区的修改退回到工作区
```
<a name="NJW6J"></a>
#### 4. 查看工作区、暂存区的状态
```
git status
```
<a name="uzK9l"></a>
#### 5. 移除暂存区的修改
```
git rm --cached <file-name> # 将本地暂存区的内容移除暂存区
```
<a name="RP5Ip"></a>
#### 6. 将缓存区的文件，提交到本地仓库（版本库 ）
```
git commit <file-name> ... "相关的记录信息"   # 将缓存区的指定文件提交到本地仓库
git commit -m "相关的记录信息"   	  # 将缓存区的所有文件提交到本地仓库
git commit -am '相关的记录信息'  	  # 跳过暂存区域直接提交更新并且添加备注的记录信息
git commit --amend '相关的记录信息'  # 使用一次新的commit，替代上一次提交，如果代码没有任何新变化，则用来修改上一次commit的提交记录信息

```
<a name="ai24J"></a>
#### 7. 撤销 commit 提交
```
git revert HEAD    # 撤销最近的一个提交(创建了一个撤销上次提交(HEAD)的新提交)
git revert HEAD^   # 撤销上上次的提交

```
<a name="U5pGR"></a>
## 查看日志
<a name="o1ULe"></a>
#### 1. 查看历史提交(commit)记录
```
git log    # 查看历史commit记录
git log --oneline  # 以简洁的一行显示，包含简洁哈希索引值
git log --pretty=oneline # 查看日志且并且显示版本
git log --stat     # 显示每个commit中哪些文件被修改,分别添加或删除了多少行
# 注：空格向下翻页，b向上翻页，q退出

```
<a name="e6sPa"></a>
#### 2. 查看分支合并图
```
git log --graph

```
<a name="OJqVp"></a>
#### 3. 查看版本线图
```
git log --oneline --graph
```
<a name="IZsm0"></a>
## Git版本控制
<a name="GYovA"></a>
#### 1. 回到指定哈希值对应的版本
```
git reset --hard <Hash>  # 回到指定 <Hash> 对应的版本
# 注: <Hash> 是版本的哈希值
git reset --hard HEAD    # 强制工作区、暂存区、本地库为当前HEAD指针所在的版本

```
<a name="y4uBl"></a>
#### 2. 版本回退
```
git reset --hard HEAD~1  # 后退一个版本
# 注：~ 后面的数字表示回退多少个版本

```
<a name="QN9kC"></a>
## 分支管理
<a name="mft31"></a>
#### 1. 查看分支
```
git branch   				#查看本地所有分支
git branch -r   		#查看所有远程分支
git branch -a 			#查看所有远程分支和本地分支
git branch --merged	#查看已合并的分支 
```
 
<a name="SB8w0"></a>
#### 2. 创建分支（依然停留在当前的分支）
```
git branch <branch-name> #  创建分支，依然停留在当前的分支
# 注: <branch-name> 是分支的名称
```
<a name="Qq2hF"></a>
#### 3. 切换分支
```
git checkout <branch-name>   # 切换到指定分支，并更新工作区
git checkout -         		 # 切换到上一个分支
```
<a name="Ok6tg"></a>
#### 4. 创建并切换分支（创建一个新的分支，并切换到这个新建的分支上）
```
git checkout -b <branch-name>  # 创建一个新的分支，并切换到这个新建的分支上
```
<a name="SYVM0"></a>
#### 5. 合并分支（合并某一个分支到当前分支）
```
git merge <branch-name>  # 合并<branch-name>分支到当前分支
```
<a name="V5LHI"></a>
#### 6. 删除分支
```
git branch -d <branch-name>    # 只能删除已经被当前分支合并的分支
git branch -D <branch-name>    # 强制删除分支
```
<a name="gE9HG"></a>
#### 7. 删除远程分支
```
git push origin --delete  <remote-branch-name>  
# 注：<remote-branch-name> 远程分支名
```
<a name="xuL9A"></a>
## 远程仓库
<a name="KjSpu"></a>
#### 克隆远程仓库（从远程仓库拉取代码）
```
git clone <url>
注：<url>为远程仓库的地址
```
<a name="uGCVO"></a>
#### 2. 本地库与远程库进行关联
```
git remote add origin <url>
```
<a name="iuu5V"></a>
#### 3. 查看远程仓库地址别名
```
git remote -v
```
<a name="PvTv9"></a>
#### 4. 新建远程仓库地址别名
```
git remote add <alias> <url>
# 注: <alias> 远程仓库的别名

```
<a name="OWmjy"></a>
#### 5. 删除本地仓库中的远程仓库别名
```
git remote rm <alias>
# 注: <alias> 远程仓库的别名
```
<a name="VrMQp"></a>
#### 6. 重命名远程仓库地址别名
```
git remote rename <old-alias> <new-alias>
# 注：<old-alias> 旧的远程仓库，<new-alias> 新的远程仓库
```
<a name="ZAEpO"></a>
#### 7. 把远程库的修改拉取到本地
```
git fetch <alias/url> <remote-branch-name>   # 抓取远程仓库的指定分支到本地，但没有合并
git merge <alias-branch-name>                # 将抓取下来的远程的分支，跟当前所在分支进行合并
git pull <alias/url> <remote-branch-name>    # 拉取到本地，并且与当前所在的分支进行合并
# 注: <alias/url> 远程仓库的别名 或者是 远程仓库地址
# <remote-branch-name> 远程分支名
```
<a name="U6MBD"></a>
#### 8. 将本地的分支推送到远程仓库
⚠️ 在推送前要先拉取哦 git pull
```
git push <alias/url> <branch-name>   # 将本地的每个分支推送到远程仓库
git push <alias/url> --force         # 强行推送 当前分支到远程仓库，即使有冲突
git push <alias/url> --all           # 推送所有本地分支到远程仓库
# 注: <alias/url> 远程仓库的别名 或者是 远程仓库地址
#     <branch-name> 本地分支名
```


