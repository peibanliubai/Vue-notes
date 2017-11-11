## git本地操作
- 不停的创造文件进行备份(没有修改的提示)git可以帮我们管理我们的代码保证代码不丢失
- 记录历史只要代码提交到git上就永久不会丢失,可以随时'穿越';
- 团队协作  两个人修改了同一文件的同一行,也需要手动解决冲突,可以实现自动合并(模块化,组件化)
- git拥有强大的'分支'管理系统

##分布式
- svn(集中式),需要有一台中央服务器
- git的区别
- 速度比svn快
- svn中每个文件夹都有一个文件.svn文件,git有一个单独的文件夹.git文件夹

## git安装
- window 下载  下载地址 http://git-scm.com
- mac  下载   下载Homebrewhttp://brew.sh
- mac的包管理器
- http://ohmyz.sh/
- http://www.iterm2.com/

##配置用户(不配置用户不能提交代码)
```
git config --list
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"
```

## 初始化git
不要在文件夹里初始化,一个项目初始化一次,不能嵌套的
```
git init 告诉git哪个文件夹被git所管理
git status 查看所在状态  红色在 工作区  绿色在 暂存区/(过渡区)    nothing to commit历史区/(版本库)
```
##git提交
- git add .或者-A  提交到 暂存区/(过渡区)
- git commit -m"first commit(注释内容)"  提交到 历史区/(版本库)
- git commit -a -m"注释内容" 直接从工作区提交到历史区(文件至少需要提交过一次)
查看版本号 git log
- 一个版本库对应一个版本号
暂存区的特点:过渡的作用,避免误操作，保护工作区和历史区,分支处理;

##删除

```
git rm "文件名" 删除暂存区中的内容,并且保证工作区中的内容已经不存在
若本地文件存在则不能删除,需要通过-f参数删除
git rm --cached 文件名 -r 删除暂存区

```
##git diff不同区的代码比较
- git diff 工作区和暂存区
- git diff master(分支名) 工作区和版本库
- git diff --cached（--staged）暂存区和历史区

##git撤销
- git reset HEAD "文件名" 撤销回git add的内容撤回文件(回到上一次暂存区)
- git checkout "文件名" 从暂存区撤销,暂存区无内容,从历史区域撤销
- git commit --amend 有的时候我们希望提交时合并到上一次的提交

##git 恢复
- git checkout commit_id filename(文件名)  恢复某个版本文件

- git reset --hard commit_id(版本号)  通过版本id恢复

- git reflog 查看当时回滚时的版本 恢复未来

快速版本回退
- git reset --hard HEAD^ 回到上一个
- git reset --hard HEAD~3 回到某个

##git 分支
- git branch 查看分支
- git branch "分支名"  创建分支
- git checkout "分支名" 切换到某个分支上
- git checkout -b "分支名" 创建并切换到当前分支
- git branch -D "分支名"  删除分支  删除分支时用户不能在当前分支上
添加文件到历史区两个分支才没有关系;
- git stash 暂存文件,文件有修改时切换分支,分支不(提交/暂存)不能切换,(这里的暂存是用暂存区覆盖掉工作区)
- git stash pop 还原暂存的内容

##git merge 分支合并
- git merge "分支名" 在master分支上合并此分支

##直接输入文件内容
- echo "内容" > "文件名"  一个'>'表示写入,两个'>>'表示追加写入


##添加忽略文件
$ touch .gitignore
$ echo .DS_Store
$ echo node_modules
$ echo .idea


##linux命令
- pwd :print working directory
- rm -rf 文件夹名 删除文件夹
- rm 文件名  删除文件
- mkdir 文件夹名   创建目录
- cd 目录名 change directory
- ls -al 显示一个文件夹下所有的文件
- touch 文件名 创建文件
- cat 文件名 查看文件
- vi 文件名 进入编辑 按i键:编辑模式  退出编辑模式:ESC 然后:q!强制退出
:wq保存并退出

http://man.linuxde.net/vi


