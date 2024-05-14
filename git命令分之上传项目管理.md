# 前言

> 为什么要将代码放入GitHub进行项目管理
> GitHub是开源社区的重要平台，是基于 Git 的版本控制系统，这使得开发者可以轻松地跟踪和管理代码的变化。而且GitHub拥有强大的协作功能，可以进行代码审查，项目管理，和协作开发，同时提供了私有仓库功能，确保信息的安全。

# 一、如何创建Github项目

## 1、 我的github仓库：

<a href="https://github.com/Muying-Zhao?tab=repositories" target="_blank"><https://github.com/Muying-Zhao></a>

## 2、新建项目

*   注册登录账号，点击主页new新建项目
*   Repository name：muying-rollup（新建项目名）
*   Description (optional)：项目描述附加信息，会被放入md文件里
*   Public和Private，选择项目是否开源和私有
*   Add a README file：可勾选，自动添加项目的基本信息、安装说明、使用指南以及其它重要的细节。下期介绍如何使用md编写笔记。

> 项目简介：简要介绍项目的名称、用途和特点。
> 安装指南：提供安装项目的步骤，例如依赖项安装、环境配置等。
> 使用说明：提供项目的使用方法和示例。
> 特色功能：列出项目的特色功能或亮点。
> 示例：提供项目的示例代码或截图。
> 贡献指南：说明如何贡献代码或报告问题。
> 许可证信息：列出项目的许可证信息。

```md
# 忽略.DS_Store所有文件
.DS_Store
node_modules
# 忽略dist目录
/dist


# local env files
.env.local
.env.*.local

# Log files
npm-debug.log*
yarn-debug.log*
yarn-error.log*
pnpm-debug.log*

# Editor directories and files
.idea
.vscode
*.suo
*.ntvs*
*.njsproj
*.sln
*.sw?
```

*   Add .gitignore：让 Git 在提交时忽略特定类型的文件，如以下文件编译生成的文件（如 .class, .jar），可不选。一般会显示如下

> 日志文件（如 .log）
> 缓存文件（如 .cache）
> IDE 自动生成的文件或目录（如 .idea, .vscode）
> 依赖管理工具生成的文件或目录（如 node\_modules, venv，/dist）

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240419/1713522382207712324.png)

3、上传本地项目

> 点击Add file => upload files => choose yout files
> 打开本地项目输入路径或拖拽上传，
> 这种方法上传单个html文件，js，css等好使，但上传大型项目如文件夹不好使。故这种方法不推荐

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240419/1713523272327980090.png)

# 二、本地使用github项目的git命令(推荐)

## 1、设置全局的用户名和邮箱

* 输入你的用户名和邮箱

> 创建完muying-rollupvue项目使用一下git命令，

```shell
git config --global user.name "Muying-Zhao"
git config --global user.email "<2479377049@qq.com>"
```

*   检查这个命令是否生效

```shell
git config --global --get user.name
git config --global --get user.email
```

## 2、复制github上需要克隆文件的Https

> 如我使用的是vscode，打开vscode然后
> `ctrl+shift+p` 搜索`Git:clone` 远程源
> 复制github上需要克隆文件的Https
> `https://github.com/Muying-Zhao/muying-rollupvue.git`
> 选择桌面进行存储，最后将在桌面建立文件夹，并打开克隆的项目

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240419/1713524482406589830.png)

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240419/1713524702791945209.png)

# 三、本地项目上传至Github(推荐用六，目的功能是一样的)

## 1、首先github上已经存在空项目

## 2、使用命令行切换至上传项目

*   方法一

> 使用vscode打开需要上传的项目
> `ctrl+shift+p`搜索
> `git init`初始化项目
> 然后点击确认

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240419/1713526150722285028.png)

*   方法二

> 使用终端命令行工具

> 切换到你的项目目录

```shell
cd path/to/your/project
```

> 初始化 Git 仓库

```shell
git init
```

> 将本地仓库与远程仓库关联，github上空项目的https链接

```shell
git remote add origin https://github.com/Muying-Zhao/muying-rollupvue.git
```

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240419/1713527218611192083.png)

## 3、解决相同类似的报错

> 1、fatal: refusing to merge unrelated histories这通常表示你的本地仓库已经有一个名为 origin 的远程仓库关联了。表示你之前已经添加过远程仓库，或者克隆了一个仓库，此时远程仓库已经自动被命名为 origin

![1eaf8b2e876a95c4969d02bec47d1f3.png](https://bbs-img.huaweicloud.com/blogs/img/20240419/1713527330992497261.png)

> 2、如果你确实希望合并这两个分支的历史记录，可以通过添加 --allow-unrelated-histories 标志来强制执行合并操作,但是请注意，合并不相关历史可能导致混乱，因此在执行之前，请确保你知道正在做什么，并且理解合并带来的潜在后果。所以确保github上是空项目
> fetch获取远程仓库的更新

```shell
git fetch origin
git merge origin/main --allow-unrelated-histories
git push origin main
```

> 3、可以选择另建立一个新的分支
> 如果你当前在main主分支上,建立master分支，

    git fetch origin
    git checkout -b master origin/main
    git push -u origin master

## 4、网络原因引起的报错

> 这种可能是国内网络原因需要使用vpn处理，或者使用加速器
> 建议如果长期网速慢使用不上就使用gitee托管吧

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240419/1713528089760795343.png)

# 四、提交保存实时更新

## 1、本地文件添加到 Git 仓库并提交

> 将所有文件添加到暂存区

```shell
git add .
```

> 提交更改，并添加提交信息

```shell
git commit -m "你的提交信息"
```

## 2、使用vscode保存

> 每次修改完会有提示，即使填写提交的内容保存并上传，然后同步实时更改就成功了。

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240419/1713529747294313903.png)

# 五、修改默认分支方式

> Settings -> General -> Default branch

![image.png](https://bbs-img.huaweicloud.com/blogs/img/20240419/1713529910724772985.png)

# 六、本地项目上传至Github(推荐)

> 一步到位

*  初始化Git仓库

```shell
git init
```

*   添加文件到暂存区

> 如果想添加所有文件，可以使用`git add .`命令。

```shell
git add . 
```

> 如果只想添加特定文件，可以使用 `git add` 文件名 命令。

```shell
git add 文件名 
```

*   提交代码

> 为了给提交添加说明，可以使用 git commit -m “提交说明” 命令

```shell
git commit -m "提交说明" 
```

*   链接远程仓库

> 在Git仓库的远程服务器上创建一个新的仓库，并获取该远程仓库的URL

```shell
 git remote add origin 远程仓库的URL
```

*   推送代码

> 使用 git push -u origin master 命令将本地的代码推送到远程仓库。

```shell
git push -u origin master
```

* 查看推送分支

```shell
git branch
```

# 七、从git上下载又想推送到git项目上(项目已存在不为空)

```shell
git init
git config --global user.name "MuYing-Zhao"
git config --global user.email "2479377049@qq.com"
git add .
git commit -m "你的提交信息"
git remote add origin https://github.com/Muying-Zhao/muying-rollup-html.git
git branch
```

* 主分支名字不是 main，而是 master或其他

> 推送 master 分支到远程仓库的 main 分支(不推荐，分支一致比较好)

```shell
git push -u origin master:main
```

> 或者创建一个 main 分支，切换到这个main分支推送并跟踪(推荐)

```shell
git checkout -b main  
git push -u origin main
```

> 远程仓库有 main 分支，但你的本地 main 分支与远程的 main 分支没有建立跟踪关系

```shell
git branch --set-upstream-to=origin/main main
git push
```

* 本地的（如main）分支推送到远程仓库的 main 分支时遇到了问题

> 远程仓库的 main 分支上有一些更新（可能是其他人推送了代码），而这些更新在你的本地仓库中是不存在的。Git 拒绝了这个推送操作，因为它会覆盖远程仓库的更改。

所以需要拉取远程仓库的 main 分支到本地的一个临时分支

```shell
git fetch origin main:temp
```

> 切换本地(main) 分支，并尝试合并 temp 分支中的更改

```shell
git checkout main  
git merge temp
```

* --allow-unrelated-histories 选项来强制 Git 合并这些不相关的历史

```shell
git merge temp --allow-unrelated-histories
```

* 尝试推送你的本地 main 分支到远程仓库

```shell
git push -u origin main
```

* 删除本地仓库其他分支

```shell
git branch -D temp
```

# 八、远程拉取合并再推送(推荐用九，目的功能是一样的)

* 拉取远程更改

> 需要从远程仓库拉取最新的更改。这可以通过 git fetch 命令完成，它会获取远程仓库的最新数据，但不会合并或更改你当前的工作。

```shell
git fetch origin main
```

* 查看远程更改

> 合并之前，想要查看远程分支上的更改。可以使用 git log 命令来查看提交历史

```shell
git log origin/main..HEAD  
# 或者，如果你想查看所有差异  
git diff origin/main
```

* 合并或变基远程更改

> git merge origin/main

```shell
git merge origin/main
```

> 选择变基

变基可能会引入冲突，因为在改变提交的顺序。在解决任何冲突后，需要使用 git add 来标记冲突已解决，并使用 `git rebase --continue` 来继续变基过程。

```shell
git rebase origin/main
```

* 解决冲突

> 如果在合并或变基过程中遇到冲突，需要`手动解决`它们。这通常涉及到编辑文件以合并来自不同分支的更改，并使用 `git add` 命令来标记冲突已解决

* 推送更改

> 如果这是你第一次将 main 分支推送到远程仓库，并且远程仓库上还没有这个分支，你可能需要使用 -u 或 --set-upstream 选项来设置上游（即跟踪）分支

```shell
git push -u origin main
```

> 不是第一次的话可以直接使用

```shell
git push origin main
```

# 九、git pull(推荐)

> git pull实际上是git fetch和git merge（或git rebase）的组合。用于从另一个存储库或本地分支获取并集成（整合）改动。当你想要获取远程仓库的最新更改并合并到你的当前分支时

* 确保你当前在正确的分支上

> 使用 git status 查看你当前所在的分支，然后使用 git branch 查看所有分支并确认你所在的分支。如果你不在想要的分支上，使用 git checkout <branch-name> 切换到正确的分支。

```shell
git status
git checkout main  
```

* 拉取并合并远程分支的更改

> 运行 git pull 命令。默认情况下，git pull 会从名为 origin 的远程仓库拉取当前分支的更改，并尝试将这些更改合并到你的本地分支。

```shell
git pull origin main
```

* 处理合并冲突

> 如果在拉取和合并过程中遇到冲突，Git 会提示你哪些文件存在冲突，并标记这些文件。你需要`手动编辑`这些文件，解决冲突，然后使用 `git add` 命令标记这些文件为已解决冲突，最后运行 git commit 来提交合并的结果。
<conflicted-file> 为文件路径

```shell
# 解决合并冲突文件然后提交
git add <conflicted-file> 
git commit -m "Resolved merge conflict"
```

* 推送更改到远程仓库

> 合并后的更改进行了额外的提交

```shell
git push origin main
```

# 十、git删除分支命令

*   从本地仓库中删除分支

> git branch -d 或 git branch -D 命令。-d 选项用于删除已经合并到当前分支的分支，而 -D（或 --delete --force）选项则用于强制删除任何分支，无论它是否已合并。

```shell
git branch -d master
git branch -D master
```

*   检查这个命令是否生效

```shell
git config --global --get user.name
git config --global --get user.email
```


# 十一

```shell
git init   
git config --global user.name "MuYing-Zhao"
git config --global user.email "2479377049@qq.com"
git add .
git commit -m "save_one"
git checkout -b main 
git branch    
git merge master  
git branch -D master
git remote add origin 
https://github.com/Muying-Zhao/cartList-vue3.git
git push -u origin main
```
