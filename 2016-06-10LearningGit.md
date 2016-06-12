一、分布式版本控制系统（Distributed Version Control System）
Git开发的初衷原则：
  ＃速度
  ＃简单的设计
  ＃对非线性开发模式的强力支持（允许上千个并发开发的支持）
  ＃完全分布式
  ＃有能力高效管理类似Linux内核一样的超大规模项目（速度和数据量）

二、Ｇit的基础要点
1､Ｇit直接取文件的快照，而非比较差异
2､近乎所有的操作都可以本地执行
3､时刻何持数据完整性
Ｇit使用SHA-1算法计算数据和校验和，通过文件的内容或目录的结构计算出一个SHA-1哈希值，作为指纹字
符串。该字符串由40个十六进制字符（0-f）组成。所有保存在Ｇit数据库的东西都是用些哈希值来索引的，
而不是靠文件名。
4､多数操作仅添加数据
5､对于任何文件只有三种状态：已提交（committed），已修改(modified)，已暂存(staged)
  #已提交表示该文件已经被安全地保存在本地数据库中
  ＃已修改表示修改了某个文件，但还没有提交保存
  ＃已暂存表示把已修改的文件放在下次提交的时要保存的清单中

三、安装Ｇit
官方网站
http://git-scm.com/download

2、初始化配置
Ｇit在开始使用前要做一些环境配置，Ｇit提供了git config工具管理Git在各个环节下的工作方式和行为
常归会有三个不同的环境变量，分别对应--system、--global、default
＃ /etc/gitconfig文件：系统中对所有用户都适用的配置。git使用--system选项读写该配置
＃ ~/.gitconfig文件：用户目录下的配置文件只适用于该用户。git使用--global修改该配置
＃ default应用于当前目录，也就是工作目录中的.git/config文件：这个文件仅对当前项目有效
每个级别的配置都会覆盖上层的相同配置。

3､配置用户信息
#两个必须配置的项目
git config --global user.name "lixueming"
git config --global user.email yshan0216@gmail.com

＃查看配置信息
git config --list


第二章 Ｇit基础知识
1､取得项目的Git仓库
＃从当前目录初始化
git init

#添加一些文件然后提交
git add *.c
git add README
git commit -m 'initial project version'

#从现有仓库克隆
git clone url
#在当前目录下创建grit的版本仓库
git clone git://github.com/chacon/grit.git
＃也可以自定义名称
git clone git://someremoete/grit.git mygrit

#检查当前文件版本的状态
git status

#跟踪新文件
git add README

#暂存已修改的文件
git add benchmarks.rb
git add有两种功能
1､开始文件跟踪
2､将变更的文件放在暂存区

＃忽备某些文件
一般我们总会有些文件无需纳入Ｇit的管理，也不希望他们总出现在未跟踪文件列表中。
通常是一些自动生成的文件，像日志或者编译过程中创建的等等。
我们可以创建一个.gitignore的文件列出要忽略的文件模式
cat .gitignore
*.［oa］
*~

#忽略＊.o或＊.a的文件
*.［oa］
＃忽略某些编辑软件产生的临时文件
*~
＃仅仅忽略项目跟目录下的TODO文件
/TODO
#忽略build目录下的所有文件
build/


.gitignore的编辑规范
1､所有空行或者注释符号＃开头的行都会被Ｇit忽略
2､可以使用标准的glob模式匹配,所谓的glob即是简化的正则表达式
3､匹配模式最后跟反斜杠（/）说明要忽略的是目录
4､要忽略指定模式以外的文件或目录，可以模式前加上惊叹号(!)取反

2､查看已暂存和未暂存的更新
git status
git diff

3、提交更新
git commit
git commit -m "Commit Information! Version Title"

#跳过使用暂存区域
#使用-a参数git自动把所有已跟踪过的文件暂存起来一并提交，从而跳过git add步骤
git commit -a -m "Commit information"

＃从Ｇit中移除某个文件，就必须从跟踪文件清单中移除，然后提交,该文件即不再纳入版本管理
git rm hello.java

＃移动文件或改名操作
git mv file_from file_to
#git mv相当于运行了以下3个命令
mv file_from file_to
git rm file_from
git add file_to

#查看提交历史
git log
#常用的一些参数
-p选项展开显示每次提交的内容差异
-2则公显示最近2次的更新
--stat显示简要的增改行情统计
--pretty使用完全不同于默认格式的方式展示提交历史，如oneline将每个提交放在一行显示，还有short
,full,fuller模式

git log --pretty=format:"%h - %an, %ar : %s"
选项 说明
%H 提交对象(commit)的完整哈希字串 %h 提交对象的简短哈希字串
%T 树对象(tree)的完整哈希字串
%t 树对象的简短哈希字串
%P 父对象(parent)的完整哈希字串 %p 父对象的简短哈希字串
%an 作者(author)的名字
%ae 作者的电子邮件地址
%ad 作者修订日期(可以用 -date= 选项定制格式) %ar 作者修订日期,按多久以前的方式显示
%cn 提交者(committer)的名字
%ce 提交者的电子邮件地址
%cd 提交日期
%cr 提交日期,按多久以前的方式显示
%s 提交说明

＃使用图形化工具查询提交历史
gitk

#＃撤消操作

＃修改最后一次提交
＃想要撤消刚才的提交操作，可以使用--amend选项重新提交
git commit --amend

#最消已暂存的文件
git reset HEAD <file>

#抛弃文件修改
git checkout -- benchmarks.rb

##远程仓库的使用
远程仓库是指托管在网络上的项目仓库，可能会有多个，其中一些只读，另外一些可读写
管理远程仓库的工作包括：
添加远程库
移除废弃的远程库，
管理各式远程库分支
定义是否跟踪这些分支

＃查看当前的远程库
git remote
在克隆完某个项目后，至少可以看到一个名为origin的远程库，Git默认使用这个名字来标识所克隆的
原始仓库
＃显示对应的远程地址-v(--verbose)
#如果有多个仓库可以全部列出
git remote -v
＃这样我们可以非常轻松地从这些用户的仓库中拉取他们的提交到本地

＃添加新的远程仓库
git remote add ［shortname］ ［url］:
git remote add pb git://github.com/paulboone/ticgit.git

＃抓取所有pb仓库有的，本地仓库没有的信息
git fetch pb

#从远程仓库抓取数据
#命令运行完成后，就可以在本地访问该远程创建中的所有分支，将其中某个分支  合并到本地，或者只是取
出某个分支
git fetch ［remote-name］
＃fetch只是将远端的数据拉取到本地仓库，并不自动合并到当前工作分支，只有当你确实准备好了，才能手工
合并

＃如果设置了某个分支用于跟踪远端仓库的分支，可以使用git pull命令自动抓取数据下来，然后将远端分支自动
合并到本地仓库的当前分支
git pull

#推送数据到远程仓库
git push ［remote-name］ ［branch-name］
#将本地的master分支推送到orgin服务器
git push orgin master

#查看远程仓库信息
git remote show orgin

#远程仓库的删除和重命名
git remote rename pb paul

#删除远程仓库
git remote rm paul

##打标签

#列出已有标签
git tag
git tag -l 'v1.4.2.*'

标签分为两类：
1､轻量级的(lightweight)，实际为一个指向特定提交对象的引用
2､含附注的(annotated)，实际为存储在仓库中的一个独立对象，有自身的校验和信息

＃创建含附注的标签
# 添加-a(annotated)参数即可
＃ -m 为标签的说明信息
git tag -a v1.4 -m 'my version 1.4'

＃签署标签
＃将 -a改为-s即可
git tag -s v1.5 -m 'my signed 1.5 tag'

＃创建轻量级标签
＃直接给出标签名即可
git tag v1.4-1w

＃验证已签署的标签
＃ -v verify
＃该参数会调用GPG验证签名，前提条件是有签署者的公钥存放在keyring中
git tag -v ［tag-name］

＃后期打标签
git log --pretty＝oneline
＃只需要在打标签的时候跟上对应提交对象的校验和或前几位字符
git tag -a v1.2 9fceb02

＃分享标签
＃默认情况下，git push并不会把标签传送到远端服务器上，只是通过显式命令才能分享标签到远端仓库。
git push origin v1.5
#一次推送所有标签上去
git push origin --tags

##git分支

git中的分支，其实本质上仅仅是个指向commit对象的可变指针，
git使用master做为分支的默认名字，它在每次提交的时候会自动向前移动

＃创建新的分支
git branch testing
＃运行git branch时仅仅只是创建一个新的分支，不会自动切换到分支里工作
＃要切换到其它分支上工作需要执行git checkout
git checkout testing

#如何知道自己的当前工作目录是在那个分支上工作的呢？
＃git使用HEAD的特别指针表示当前分支

git checkout master
命令完成了2件事
1､把HEAD指针移回到master分支
2、把工作目录下中的文件换成了master分支所指向的快照内容。

＃基本的分支与合并

＃基本分支
＃创建iss53分支，并将工作目录切换到分支iss53上
git checkout -b iss53

git branch iss53
git checkout iss53

git checkout master
＃对项目进行热修复
git checkout -b 'hotfix'
vim index.html
git commit -a -m 'fixed the broken email address'

git checkout master
＃将hotfix分支与master分支合并
git merge hotfix

＃删除分支
git branch -d hotfix

#回到iss53上继续工作
git checkout iss53
vim index.html
git commit -a -m 'finished the new footer ［issue53］'

＃三方合并
如果git遇到不是直接祖先的分支合并，则使用三方合并方案，找到两个分去的共同祖先，对这3个分支的快照
进行对比合并为一个新的快照，作为合并结果。
#合并完成后删除不必要的分支
git branch -d iss53

#冲突合并
如果在合并时修改了同一文件的同一部分，则Ｇit无法干净的将两者合并在一起，此时就无法提交，需要人工
解决冲突
任何未解决冲突都会以未合并(unmerged)状态列出
手动修改文件解决冲突或使用工具解决
git mergetool可以调用一个可视化的工具并引导解决所有冲突。

#分支管理
#列出当前所有分支的清单
git branch
分支前的＊表示当前所在的分支
#查看最后一次提交的信息
git branch -v
#查看当前那些分支已被并入当前分支
git branch --merged
#查看当前那些分支未被并入当前分支
git branch --no-merged
未合并的分支删除会丢失数据，所以默认删除会失败，可以强制删除
git branch -D testing

#分支式工作流程
#最佳实践：仅在master分支保留完全稳定的代码，即已经发布或即将发布的代码。与此同时，还有一个
名为develop或next的分支，专门用于后续的开发或仅用于稳定性测试，一但进入到某种稳定状态，便可以
将它合并到master分支中。
master
develop
topic
proposed建议分支
pu (proposed updates,建议更新)

＃特性分支topic
一个特性分支是指一个短期的，用于实现单一特性或与相关工作的分支

＃远程分支remote branch
remote branch是对远程仓库状态的索引，他们是一些无法移动的本地分支
，只有在进行git的网络活动时才会更新。远程分支就像书签，提醒着你上次连接远程仓库时上面各分支的位置

＃远程分支的表未方法
远程仓库名/分支名
git remote add teamone git://git.team1.ourcompany.com

#推送
要想和其他人分享某个分支，你需要把它推送到一个你拥有写权限的远程仓库。你的本地分支不会被自动同步到
你引入的远程分支中，除非你明确执行推送操作。换句话说，对于无意分享的，你尽可以保留为私人分支，而只
推送那些协同工作的特性分支。
#指定分支推送
#git push (远程仓库名) (分支名)
git push origin serverfix

#跟踪分支
从远程分支检出的本地分支，称为跟踪分支（track branch）
track branch是一种和远程分支有直接联系的本地分支
检出跟踪分支
git checkout --track origin/serverfix
git checkout -b sf origin/serverfix

#删除远程分支
git push ［远程名］ :［分支名］
git push origin :serverfix

#分支的衍合
把一个分支整合到另一个分支的办法有两种：merge（合并）和rebase（衍合）。
merge是将两个不同的分支合并为一个新的分支
rebase是将另一个分支所做的变更做为补丁更新到分支上，生成新的衍生分支

＃衍合过程
git checkout experiment
git rebase master

#衍合与三方合关的结果是一样的，但衍合能产生一个更为整结的提交历史，仿佛所有修改都是先后进行的。尽
管它们原来是同时发生的

git rebase --onto master server client
git checkout master
git merge client

＃先检出特性分支server，然后再主分支master上重演
git rebase 主分支 特性分支
git rebase master server

#快进主分支master
git checkout master
git merge server

#删除不必要的分支
git branch -d client
git branch -d server

##衍合的风险
永远不要衍合那些已经推送到公共仓库的更新


第三章 Git服务器
1､本地协议
远程仓库在该协议就是硬盘上的另一个目录。
常用操作
git clone /opt/git/project.git
git clone file:///opt/git/project.git
两者还有一些区别
只给出路径，Git会尝试使用硬链接或者直接复制它需要的文件。
使用file://，Git会调用它平时通过网络来传输数据的工序，效率相对较低。使用它的主要原因是当需要一个
不包含无关引用或对象的的干净仓库副本的时候，一般是从其它版本控制系统的导入之后或者类似的情形。

2､ssh协议
git clone ssh://user@server:project.git
#不指明某个协议时，默认使用ssh
git clone user@server@project.git

3、git协议
Ｇit协议是一个包含在Ｇit软件包中的特殊守护进程； 监听类似于ssh服务的9418端口，无需任授权

4､HTTP/S协议
cd /var/www/htdocs
git clone --bare /path/to/git_project gitproject.git
cd gitproject.git
mv hooks/post-update.sample hooks/post-update
chmod a+x hooks/post-update

#在服务器布署Git
git clone --bare my_project my_project.git
#类似于下面的操作
cp -Rf my_project/.git myproject.git
#将纯目录转移到服务器
scp -r my_project.git user@git.exeample.com:/opt/git
#上传完成后即其他对服务器有ssh访问权限并可以读取/opt/git的用户可以用以下命令克隆
git clone user@git.example.com:/opt/git/my_project.git

ssh git服务器
方法一：
给每个用户新建一个独立用户
useradd -g git username

方法二：
建立一个git用户
让每个需要写权限的人发送一个ssh公钥
然后将其加入git账户的〜/.ssh/authorized_keys文件

方法三：
ssh服务器通过ldap服务器集中授权制进行授权

创建基于公钥的git服务器
adduser git
su - git
mkdir .git
cat /tmp/id_rsa.john.pub >> ~/.ssh/authorized_keys
cat /tmp/id_rsa.tony.pub >> ~/.ssh/authorized_keys
cd /opt/git
mkdir project.git
cd project.git
git --bare init
#添加git的时候设置shell环境变量为/user/bin/git-shell
#which git-shell



#john's computer
cd my_project
git init
git add .
git commit -m "initial commit"
git remote add origin git@gitserver:/opt/git/project.git
git push origin master

#other developer
git clone git@gitmaster:/opt/git/project.git
vim README
git commit -am 'fix for the readme file'
git push origin master

#使用Ｗeb服务器搭建public git server
cd project.git
mv hooks/post-update.sample hooks/post-update
chmod a+x hooks/post-update

cat .git/hooks/post-update
#!/bin/sh
exec git-update-server-info
#功能是当通过ssh向服务器推送时，Ｇit将运行这个命令来更新http获取所需的文件

＃配置apache
<VirtualHost *:80>
  ServerName git.gitserver
  DocumentRoot /opt/git
  <Directory /opt/git/>
    Order allow, deny
    allow from all
  </Directory>
</VirtualHost>

#修改用户组
chgrp -R www-data /opt/git
#重启apache服务器
git clone http://git.gitserver/project.git

GitWeb,一个git自带的cgi脚本
























＃
