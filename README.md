## 本地->github
- 先有github账号
## 本地提交
- README.md

- 创建一个.gitignore
- git不会上传空文件夹添加.gitkeep在空文件夹内

## 关联远程仓库
```
git remote add origin 地址
```

## 删除关联
```
git remote rm 名字
```

## 推送代码
```
git push origin master
```
##拉去最新的代码
```
git pull origin master (fetch+merge)
```

##gh-pages 分支来发布我们的静态页
- 在项目中创建一个gh-pages的分支
- 将分支提到线上仓库
-找到提供给你的网址 settings github-pages
```
        git checkout -b gh-pages
        touch index.html
        git add .
        git commit -m 'xxx'
        git push origin gh-pages

```
## issue 问题
- 可以提交项目,自己的信息

## 更改别人代码
- fork是在当前项目下克隆了一份,如果代码更新,你的不会随之更新
- 拉取到本地
```
git clone 项目地址 项目别名

```
>默认就是git仓库而且有origin 地址,可以将代码提交到自己的仓库上
>只有fork的关系才能发送 pull request请求

## collaborators
- 添加贡献者 ,被添加的人拥有最大的权限

##GUI界面
- 1.add commit

