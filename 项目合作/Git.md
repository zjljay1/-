<a name="h6F6l"></a>
## 版本控制
什么是版本控制版本控制（Revision control）是一种在开发的过程中用于管理我们对文件、目录或工程等内容的修改历史，方便查看更改历史记录，备份以便恢复以前的版本的软件工程技术。

- 实现跨区域多人协同开发
- 追踪和记载一个或者多个文件的历史记录
- 组织和保护你的源代码和文档
- 统计工作量
- 并行开发、提高开发效率
- 跟踪记录整个软件的开发过程
- 减轻开发人员的负担，节省时间，同时降低人为错误

简单说就是用于管理多人协同开发项目的技术
<a name="yrjXu"></a>
### 常见的版本控制工具
主流的版本控制器有如下这些：

- **Git**
- **SVN**（Subversion）
- **CVS**（Concurrent Versions System）
- **VSS**（Micorosoft Visual SourceSafe）
- **TFS**（Team Foundation Server）
- Visual Studio Online

版本控制产品非常的多（Perforce、Rational ClearCase、RCS（GNU Revision Control System）、Serena Dimention、SVK、BitKeeper、Monotone、Bazaar、Mercurial、SourceGear Vault），现在影响力最大且使用最广泛的是Git与SVN
<a name="gpQB8"></a>
### 版本控制分类
<a name="eClL1"></a>
#### 本地版本控制
记录文件每次的更新，可以对每个版本做一个快照，或是记录补丁文件，适合个人用，如RCS。<br />![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1673094561695-a16be037-ba67-44fb-8bb0-1be85b2d6cba.png#averageHue=%23e2e1e1&clientId=u7c6b9d0a-ce7a-4&from=paste&id=ufc82e1cd&originHeight=523&originWidth=617&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=uf298f868-2431-4e6a-956c-965e51b75be&title=)
<a name="DLijy"></a>
#### 集中版本控制 SVN
所有的版本数据都保存在服务器上，协同开发者从服务器上同步更新或上传自己的修改<br />![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1673094585423-aceeac91-29da-43de-9cd3-3b17d75c2b11.png#averageHue=%23e3e2e2&clientId=u7c6b9d0a-ce7a-4&from=paste&id=u20a4a280&originHeight=608&originWidth=763&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u300f5150-b201-43df-b960-9a63b9827e1&title=)所有的版本数据都存在服务器上，用户的本地只有自己以前所同步的版本，如果不连网的话，用户就看不到历史版本，也无法切换版本验证问题，或在不同分支工作。而且，所有数据都保存在单一的服务器上，有很大的风险这个服务器会损坏，这样就会丢失所有的数据，当然可以定期备份。代表产品：SVN、CVS、VSS
<a name="Ivqi5"></a>
#### 分布式版本控制 Git
每个人都拥有全部的代码！安全隐患！<br />所有版本信息仓库全部同步到本地的每个用户，这样就可以在本地查看所有版本历史，可以离线在本地提交，只需在连网时push到相应的服务器或其他用户那里。由于每个用户那里保存的都是所有的版本数据，只要有一个用户的设备没有问题就可以恢复所有的数据，但这增加了本地存储空间的占用。<br />不会因为服务器损坏或者网络问题，造成不能工作的情况！<br />![](https://cdn.nlark.com/yuque/0/2023/jpeg/28163149/1673094621905-b5e6691b-502a-45d1-88d4-f9756f28c9de.jpeg#averageHue=%23efefef&clientId=u7c6b9d0a-ce7a-4&from=paste&id=u851a8c8a&originHeight=857&originWidth=765&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ub279feed-2fec-406b-9f9c-bd75cdecdc0&title=)
<a name="j4Eax"></a>
#### Git和SVN的主要区别
SVN是集中式版本控制系统，版本库是集中放在中央服务器的，而工作的时候，用的都是自己的电脑，所以首先要从中央服务器得到最新的版本，然后工作，完成工作后，需要把自己做完的活推送到中央服务器。集中式版本控制系统是必须联网才能工作，对网络带宽要求较高。<br />![](https://cdn.nlark.com/yuque/0/2023/jpeg/28163149/1673094655726-c8af3c5a-c863-403d-8708-e726f6302dfe.jpeg#averageHue=%23b9cf92&clientId=u7c6b9d0a-ce7a-4&from=paste&id=uce7d7794&originHeight=178&originWidth=1080&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u57edc8a4-6cbd-4edb-8e17-18f5d877b1a&title=)<br />Git是分布式版本控制系统，没有中央服务器，每个人的电脑就是一个完整的版本库，工作的时候不需要联网了，因为版本都在自己电脑上。协同的方法是这样的：比如说自己在电脑上改了文件A，其他人也在电脑上改了文件A，这时，你们两之间只需把各自的修改推送给对方，就可以互相看到对方的修改了。Git可以直接看到更新了哪些代码和文件！<br />**Git是目前世界上最先进的分布式版本控制系统。**
<a name="J7FjD"></a>
## Git历史
同生活中的许多伟大事物一样，Git 诞生于一个极富纷争大举创新的年代。<br />Linux 内核开源项目有着为数众广的参与者。绝大多数的 Linux 内核维护工作都花在了提交补丁和保存归档的繁琐事务上(1991－2002年间)。到 2002 年，整个项目组开始启用一个专有的分布式版本控制系统 BitKeeper 来管理和维护代码。<br />Linux社区中存在很多的大佬！破解研究 BitKeeper ！<br />到了 2005 年，开发 BitKeeper 的商业公司同 Linux 内核开源社区的合作关系结束，他们收回了 Linux 内核社区免费使用 BitKeeper 的权力。这就迫使 Linux 开源社区(特别是 Linux 的缔造者 Linus Torvalds)基于使用 BitKeeper 时的经验教训，开发出自己的版本系统。（2周左右！） 也就是后来的 Git！<br />**<br />Git是免费、开源的，最初Git是为辅助 Linux 内核开发的，来替代 BitKeeper！<br />![](https://cdn.nlark.com/yuque/0/2023/jpeg/28163149/1673094954158-ab8ef651-e0f2-4323-a11f-14cb57da0250.jpeg#averageHue=%23a87b4e&clientId=u7c6b9d0a-ce7a-4&from=paste&id=ufde5bb36&originHeight=332&originWidth=577&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ubc279b57-00c0-4e10-8ae2-c6c40f06f0d&title=)<br />Linux和Git之父李纳斯·托沃兹（Linus Benedic Torvalds）1969、芬兰
<a name="P9nr9"></a>
## Git环境配置
打开 git官网 [Git](https://git-scm.com/)，下载git对应操作系统的版本。<br />所有东西下载慢的话就可以去找镜像！<br />官网下载太慢，我们可以使用淘宝镜像下载：[CNPM Binaries Mirror](http://npm.taobao.org/mirrors/git-for-windows/)<br />![](https://cdn.nlark.com/yuque/0/2023/jpeg/28163149/1673096194188-8293d726-32ba-4cb7-8561-802f4411981c.jpeg#averageHue=%23e7e6de&clientId=u7c6b9d0a-ce7a-4&from=paste&id=u9891c773&originHeight=644&originWidth=1080&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u4068e4d1-c62b-42c1-8ca0-dc717f330d0&title=)<br />下载对应的版本即可安装！<br />安装：无脑下一步即可！安装完毕就可以使用了！
> 启动Git

安装成功后在开始菜单中会有Git项，菜单下有3个程序：任意文件夹下右键也可以看到对应的程序！<br />![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1673096233166-00b9571d-7d0b-4566-a5b8-ebaeac8ee49b.png#averageHue=%23626161&clientId=u7c6b9d0a-ce7a-4&from=paste&id=u9385e58d&originHeight=283&originWidth=405&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=uffee2761-602e-4a07-a343-4f7667f172e&title=)<br />**Git Bash：**Unix与Linux风格的命令行，使用最多，推荐最多<br />**Git CMD：**Windows风格的命令行<br />**Git GUI**：图形界面的Git，不建议初学者使用，尽量先熟悉常用命令
> <a name="g05u4"></a>
## 常用的Linux命令

1）、cd : 改变目录。<br />2）、cd . . 回退到上一个目录，直接cd进入默认目录<br />3）、pwd : 显示当前所在的目录路径。<br />4）、ls(ll):  都是列出当前目录中的所有文件，只不过ll(两个ll)列出的内容更为详细。<br />5）、touch : 新建一个文件 如 touch index.js 就会在当前目录下新建一个index.js文件。<br />6）、rm:  删除一个文件, rm index.js 就会把index.js文件删除。<br />7）、mkdir:  新建一个目录,就是新建一个文件夹。<br />8）、rm -r :  删除一个文件夹, rm -r src 删除src目录<br />rm -rf / 切勿在Linux中尝试！删除电脑中全部文件！<br />9）、mv 移动文件, mv index.html src index.html 是我们要移动的文件, src 是目标文件夹,当然, 这样写,必须保证文件和目标文件夹在同一目录下。<br />10）、reset 重新初始化终端/清屏。<br />11）、clear 清屏。<br />12）、history 查看命令历史。<br />13）、help 帮助。<br />14）、exit 退出。<br />15）、#表示注释
> Git配置

所有的配置文件，其实都保存在本地！
```git
git config -lgit
```
![](https://cdn.nlark.com/yuque/0/2023/jpeg/28163149/1673096298288-c4d4b2da-fd3d-4748-8e43-e87a413164cb.jpeg#averageHue=%23736e67&clientId=u7c6b9d0a-ce7a-4&from=paste&id=u04b032e7&originHeight=622&originWidth=1080&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u241e91ce-9049-4608-873b-b544c2f11dc&title=)<br />查看不同级别的配置文件：
```git
#查看系统config
git config --system --list

#查看当前用户（global）配置
git config --global  --list
```
**Git相关的配置文件：**<br />1）、Git\etc\gitconfig  ：Git 安装目录下的 gitconfig     --system 系统级<br />2）、C:\Users\Administrator\ .gitconfig    只适用于当前登录用户的配置  --global 全局<br />![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1673096346559-017932b9-ef13-4021-b1a1-1532ee1f223d.png#averageHue=%23f5f4f3&clientId=u7c6b9d0a-ce7a-4&from=paste&id=u02b36ee3&originHeight=269&originWidth=1080&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u888ddde7-f1cf-4cbf-9a90-654b2026be9&title=)<br />这里可以直接编辑配置文件，通过命令设置后会响应到这里
> <a name="VRUoO"></a>
## 设置用户名与邮箱（用户标识，必要）

当你安装Git后首先要做的事情是设置你的用户名称和e-mail地址。这是非常重要的，因为每次Git提交都会使用该信息。它被永远的嵌入到了你的提交中：
```git
git config --global user.name "kuangshen"  #名称
git config --global user.email 24736743@qq.com   #邮箱
```
只需要做一次这个设置，如果你传递了--global 选项，因为Git将总是会使用该信息来处理你在系统中所做的一切操作。如果你希望在一个特定的项目中使用不同的名称或e-mail地址，你可以在该项目中运行该命令而不要--global选项。总之--global为全局配置，不加为某个项目的特定配置。<br />![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1673096417516-0a8e92fa-3f00-4251-8d34-9e63a1a3e033.png#averageHue=%23454443&clientId=u7c6b9d0a-ce7a-4&from=paste&id=u4f700b1a&originHeight=169&originWidth=1080&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u1ab2c825-a443-4b97-bb33-3b8006ccbe5&title=)
<a name="kW8Do"></a>
## Git基本理论
<a name="SIVdd"></a>
### 三个区域
Git本地有三个工作区域：工作目录（Working Directory）、暂存区(Stage/Index)、资源库(Repository或Git Directory)。如果在加上远程的git仓库(Remote Directory)就可以分为四个工作区域。文件在这四个区域之间的转换关系如下：<br />![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1673099443158-39786604-dd31-4ec8-9b92-a4d6f3856f7f.png#averageHue=%23ece5d6&clientId=u7c6b9d0a-ce7a-4&from=paste&id=u8349f31f&originHeight=480&originWidth=500&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u343a4214-28dd-42e1-9df1-6c8796bfac3&title=)

- Workspace：工作区，就是你平时存放项目代码的地方
- Index / Stage：暂存区，用于临时存放你的改动，事实上它只是一个文件，保存即将提交到文件列表信息
- Repository：仓库区（或本地仓库），就是安全存放数据的位置，这里面有你提交到所有版本的数据。其中HEAD指向最新放入仓库的版本
- Remote：远程仓库，托管代码的服务器，可以简单的认为是你项目组中的一台电脑用于远程数据交换

本地的三个区域确切的说应该是git仓库中HEAD指向的版本：<br />![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1673099453704-e7d837e1-0b84-4838-abf3-0010b2c31851.png#averageHue=%23f4f4f4&clientId=u7c6b9d0a-ce7a-4&from=paste&id=ufa3d6a2b&originHeight=728&originWidth=1080&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u5b92dfdb-1d8d-4331-ac47-4f4828ca501&title=)

- Directory：使用Git管理的一个目录，也就是一个仓库，包含我们的工作空间和Git的管理空间。
- WorkSpace：需要通过Git进行版本控制的目录和文件，这些目录和文件组成了工作空间。
- .git：存放Git管理信息的目录，初始化仓库的时候自动创建。
- Index/Stage：暂存区，或者叫待提交更新区，在提交进入repo之前，我们可以把所有的更新放在暂存区。
- Local Repo：本地仓库，一个存放在本地的版本库；HEAD会只是当前的开发分支（branch）。
- Stash：隐藏，是一个工作状态保存栈，用于保存/恢复WorkSpace中的临时状态。
<a name="Jx8O8"></a>
### 工作流程
git的工作流程一般是这样的：<br />１、在工作目录中添加、修改文件；<br />２、将需要进行版本管理的文件放入暂存区域；<br />３、将暂存区域的文件提交到git仓库。<br />因此，git管理的文件有三种状态：已修改（modified）,已暂存（staged）,已提交(committed)<br />![](https://cdn.nlark.com/yuque/0/2023/jpeg/28163149/1673099481390-e0911c22-fc6e-4736-835d-ee6764e98308.jpeg#averageHue=%23f3f0ea&clientId=u7c6b9d0a-ce7a-4&from=paste&id=u01beb05b&originHeight=850&originWidth=1080&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=uedfe96e5-9ced-4777-a07f-a4d5e8fd354&title=)
<a name="aMwLm"></a>
## Git项目搭建
<a name="xcw4c"></a>
### 创建工作目录与常用指令
工作目录（WorkSpace)一般就是你希望Git帮助你管理的文件夹，可以是你项目的目录，也可以是一个空目录，建议不要有中文。<br />日常使用只要记住下图6个命令：<br />![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1673099534411-2312be7f-9bc8-402b-bbeb-354e5c818175.png#averageHue=%23e1e1da&clientId=u7c6b9d0a-ce7a-4&from=paste&id=uf9e3c333&originHeight=337&originWidth=1080&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u9c49e161-55a6-45da-883d-37e5c7db6fa&title=)<br />创建本地仓库的方法有两种：一种是创建全新的仓库，另一种是克隆远程仓库。
> 本地仓库搭建

1、创建全新的仓库，需要用GIT管理的项目的根目录执行：
```git

# 在当前目录新建一个Git代码库
$ git init
```
> 克隆远程仓库

1、另一种方式是克隆远程目录，由于是将远程服务器上的仓库完全镜像一份至本地！
```git
# 克隆一个项目和它的整个代码历史(版本信息)
$ git clone [url]  # https://gitee.com/kuangstudy/openclass.git
```
<a name="srr9S"></a>
## Git文件操作
<a name="juxO6"></a>
### 文件的四种状态
版本控制就是对文件的版本控制，要对文件进行修改、提交等操作，首先要知道文件当前在什么状态，不然可能会提交了现在还不想提交的文件，或者要提交的文件没提交上。

- Untracked: 未跟踪, 此文件在文件夹中, 但并没有加入到git库, 不参与版本控制. 通过git add 状态变为Staged.
- Unmodify: 文件已经入库, 未修改, 即版本库中的文件快照内容与文件夹中完全一致. 这种类型的文件有两种去处, 如果它被修改, 而变为Modified. 如果使用git rm移出版本库, 则成为Untracked文件
- Modified: 文件已修改, 仅仅是修改, 并没有进行其他的操作. 这个文件也有两个去处, 通过git add可进入暂存staged状态, 使用git checkout 则丢弃修改过, 返回到unmodify状态, 这个git checkout即从库中取出文件, 覆盖当前修改 !
- Staged: 暂存状态. 执行git commit则将修改同步到库中, 这时库中的文件和本地文件又变为一致, 文件为Unmodify状态. 执行git reset HEAD filename取消暂存, 文件状态为Modified
<a name="Xabzt"></a>
### 查看文件状态
上面说文件有4种状态，通过如下命令可以查看到文件的状态：
```git
#查看指定文件状态
git status [filename]

#查看所有文件状态
git status

# git add .                  添加所有文件到暂存区
# git commit -m "消息内容"    提交暂存区中的内容到本地仓库 -m 提交信息
```
<a name="shUVJ"></a>
### 忽略文件
有些时候我们不想把某些文件纳入版本控制中，比如数据库文件，临时文件，设计文件等<br />在主目录下建立".gitignore"文件，此文件有如下规则：

1. 忽略文件中的空行或以井号（#）开始的行将会被忽略。
2. 可以使用Linux通配符。例如：星号（*）代表任意多个字符，问号（？）代表一个字符，方括号（[abc]）代表可选字符范围，大括号（{string1,string2,...}）代表可选的字符串等。
3. 如果名称的最前面有一个感叹号（!），表示例外规则，将不被忽略。
4. 如果名称的最前面是一个路径分隔符（/），表示要忽略的文件在此目录下，而子目录中的文件不忽略。
5. 如果名称的最后面是一个路径分隔符（/），表示要忽略的是此目录下该名称的子目录，而非文件（默认文件或目录都忽略）。
```git
#为注释
*.txt        #忽略所有 .txt结尾的文件,这样的话上传就不会被选中！
!lib.txt     #但lib.txt除外
/temp        #仅忽略项目根目录下的TODO文件,不包括其它目录temp
build/       #忽略build/目录下的所有文件
doc/*.txt    #会忽略 doc/notes.txt 但不包括 doc/server/arch.txt
```
<a name="xPsej"></a>
## 使用码云
设置本机绑定SSH公钥，实现免密码登录！（免密码登录，这一步挺重要的，码云是远程仓库，我们是平时工作在本地仓库！)
```git
# 进入 C:\Users\Administrator\.ssh 目录
# 生成公钥
ssh-keygen
```
![](https://cdn.nlark.com/yuque/0/2023/jpeg/28163149/1673099764393-71689d77-0521-48c9-9f8b-757bb6a97264.jpeg#averageHue=%237c7871&clientId=u7c6b9d0a-ce7a-4&from=paste&id=ua6febc52&originHeight=734&originWidth=1080&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u647ccdae-c32b-4cc0-92d9-c17bda4d3ee&title=)<br />将公钥信息public key 添加到码云账户中即可！<br />![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1673099779751-a2a9e85b-408f-4482-b60f-c7668d9742ea.png#averageHue=%23e4eff3&clientId=u7c6b9d0a-ce7a-4&from=paste&id=u103ab174&originHeight=207&originWidth=1080&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=uf7fe5fe6-1ed6-43e0-be9f-0c1dec62ff0&title=)<br />使用码云创建一个自己的仓库！![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1673099788264-ccc37b20-9eac-454f-a007-59ced4278f43.png#averageHue=%23cfcabb&clientId=u7c6b9d0a-ce7a-4&from=paste&id=u64357d4a&originHeight=251&originWidth=1080&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u8db8a8a9-8bfc-4663-a0c1-19f98da0ce7&title=)<br />许可证：开源是否可以随意转载，开源但是不能商业使用，不能转载，...  限制！<br />![](https://cdn.nlark.com/yuque/0/2023/jpeg/28163149/1673099797088-903a10d8-ba60-4046-919d-fecf300671d4.jpeg#averageHue=%23fcfbfb&clientId=u7c6b9d0a-ce7a-4&from=paste&id=u14551d7b&originHeight=927&originWidth=1027&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u716a11cb-e83e-4ee1-ae46-23ad988ae6d&title=)<br />克隆到本地！![](https://cdn.nlark.com/yuque/0/2023/jpeg/28163149/1673099808101-758b39ba-c44c-4c67-bfd6-42364a6ce14a.jpeg#averageHue=%23f3ebe0&clientId=u7c6b9d0a-ce7a-4&from=paste&id=u7bd04422&originHeight=634&originWidth=1080&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=uea4acc3d-dfbc-431d-902e-9673e061470&title=)
<a name="o3Awz"></a>
## IDEA集成Git
1、新建项目，绑定git。![](https://cdn.nlark.com/yuque/0/2023/jpeg/28163149/1673099864707-fd981d46-d3c7-45dd-8179-c475c3fc0359.jpeg#averageHue=%23e9e7df&clientId=u7c6b9d0a-ce7a-4&from=paste&id=uca29140e&originHeight=544&originWidth=1070&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u0b92b233-1a0c-4266-bfb5-75822624bdc&title=)注意观察idea中的变化<br />![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1673099882742-8a16092f-4400-4f78-a842-1301e16a8352.png#averageHue=%23cecbcb&clientId=u7c6b9d0a-ce7a-4&from=paste&id=u15d721b9&originHeight=388&originWidth=1061&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u379f5a5c-6281-40a1-839b-e98f7217471&title=)<br />2、修改文件，使用IDEA操作git。

- 添加到暂存区
- commit 提交
- push到远程仓库

3、提交测试<br />![](https://cdn.nlark.com/yuque/0/2023/jpeg/28163149/1673099893709-f5e70cbe-2b82-40cc-bd24-f31d2b9e9410.jpeg#averageHue=%23f7e7d9&clientId=u7c6b9d0a-ce7a-4&from=paste&id=u4a7e3fe5&originHeight=636&originWidth=1080&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u3df1408a-bb47-4534-90cf-5d60701d2ab&title=)
<a name="rp4r2"></a>
## Git分支
分支在GIT中相对较难，分支就是科幻电影里面的平行宇宙，如果两个平行宇宙互不干扰，那对现在的你也没啥影响。不过，在某个时间点，两个平行宇宙合并了，我们就需要处理一些问题了！<br />![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1673099910064-39ebe731-768d-41c0-8132-f1c83ba5e755.png#averageHue=%23faf2f1&clientId=u7c6b9d0a-ce7a-4&from=paste&id=u1925130b&originHeight=333&originWidth=453&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=uad617ccd-0110-436c-8e4e-97cadc20684&title=)![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1673099913751-f69010c0-3190-44aa-b630-363bb71f6259.png#averageHue=%23f2f5ea&clientId=u7c6b9d0a-ce7a-4&from=paste&id=u6c502e00&originHeight=346&originWidth=729&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ua5bdd127-0e79-4baf-84ca-44c901dd980&title=)git分支中常用指令：
```git
# 列出所有本地分支
git branch

# 列出所有远程分支
git branch -r

# 新建一个分支，但依然停留在当前分支
git branch [branch-name]

# 新建一个分支，并切换到该分支
git checkout -b [branch]

# 合并指定分支到当前分支
$ git merge [branch]

# 删除分支
$ git branch -d [branch-name]

# 删除远程分支
$ git push origin --delete [branch-name]
$ git branch -dr [remote/branch]
```
IDEA中操作<br />![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1673099941505-36e01cd9-2f4b-4161-b4f6-9a3b37b21560.png#averageHue=%23f7f2f2&clientId=u7c6b9d0a-ce7a-4&from=paste&id=u59ac4e8e&originHeight=455&originWidth=619&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=uc84e1534-e56b-4d66-b4b7-49091361057&title=)<br />如果同一个文件在合并分支时都被修改了则会引起冲突：解决的办法是我们可以修改冲突文件后重新提交！选择要保留他的代码还是你的代码！<br />master主分支应该非常稳定，用来发布新版本，一般情况下不允许在上面工作，工作一般情况下在新建的dev分支上工作，工作完后，比如上要发布，或者说dev分支代码稳定后可以合并到主分支master上来。

 

