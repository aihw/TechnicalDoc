# Git

### 一、常用命令

###### 1.创建用户名和邮箱

git config --global user.name "axs"
git config --global user.email "1120390855@qq.com"

###### 2.本地仓库初始化(进入到本地仓库目录)

git init

###### 3.工作区添加到暂存区

git add [文件名称]

###### 4.暂存区添加到本地库

git commit -m "备注信息" [文件名称]

###### 5.查看工作区和暂存区的状态

git status

###### 6.查看日志方式1

git log

###### 7.日志记录过多时,分页查看

下一页: 空格
上一页: b
到尾页,显示END
退出:q

###### 8.查看日志方式2

git log --pretty=oneline

###### 9.查看日志方式3

git log --oneline

###### 10.查看日志方式4

git reflog
多了信息:HEAD@{数字}
数字含义:指针回到这个历史版本需要走多少步

###### 11.前进或者后退历史版本

git reset --hard [索引](git reflog 查看的第一列就是索引)
hard参数会让本地库指针移动的同时,重置暂存区和工作区
mixed参数会让本地库指针移动的同时,重置暂存区,工作区不变
soft参数只会让本地库指针移动,暂存区和工作区不变

###### 12.删除、恢复文件

删除工作区 rm [文件名称]
删除暂存区 git add [文件名称]
删除本地库 git commit [文件名称]
找回删除文件 使用 git reset --hard [索引] 命令回到删除之前的历史版本
找回暂存区文件 使用上述方法

###### 13.比对工作区和暂存区中文件不同之处

git diff [文件名称](不加文件名称,默认比对所有的文件)
Git是按照行为单位管理数据

###### 14.比对暂存区和本地库中文件的不同之处

git diff [索引] [文件名称]

###### 15.查看分支

git branch -v

###### 16.创建分支

git branch [分支名称]

###### 17.切换分支

git checkout [分支名称]

###### 18.合并

进入主分支 git checkout master
合并到主分支 git merge [分支名称]

###### 19.解决主分支和其他分支冲突

决定留下想要的即可
删除完成 将文件添加到暂存区
然后进行commit操作 不可以带文件名称 否则出错  git commit -m [备注信息]
完成分支冲突解决 退出合并状态

###### 20.查看git远程仓库别名

git remote -v

###### 21.在git本地给远程仓库地址起别名(方便使用)

git remote add [别名] [远程仓库地址]

###### 22.本地库提交到远程库

git push [远程库别名] [分支]

###### 23.克隆远程仓库到本地

git clone [远程仓库地址]
克隆作用:初始化本地库、将远程仓库完整克隆到本地、给远程仓库期别名

###### 24.拉取操作pull = fetch + merge

git fetch [远程库别名] [分支]  只是将远程库内容下载到本地,工作区文件没有更新,还是原来的内容
git checkout [远程库别名]/[分支] 切换到远程仓库分支
git merge [远程库别名]/[分支] 合并操作

###### 25.免密操作

git命令行 cd ~ 进入用户主目录
ssh-keygen -t rsa -C [github注册邮箱]
.ssh 目录生成两个文件
复制id_rsa.pub文件的内容
登录github->setiing->SSh and GPG key-> New SSH Key

###### 26.将本地库和远程库合并操作，git需要添加一句代码

git pull --allow-unrelated-histories(告诉git允许不相关历史合并)