### 简介

Git是由Linux的创始人Linus花费了两周时间，使用C语言写的一个分布式版本管理系统。

它的优点如下：

* 分布式
* 记录历史
* 团队协作

### 下载安装

对于Mac操作系统来说，可以通过Mac系统的包管理器HomeBrew来安装。注意：安装xcode会默认下载git。

> Mac下的终端可以通过Oh My Zsh插件来美化，当然你也可以直接使用iTerm终端。

### 初始化

```bash
git init					
```

### 配置

```bash
# 全局配置
git config --global user.name "ugu"
git config --global user.email "yinyun957@gmail.com"

# 针对当前项目
git config user.name "guyou"
git config user.email "guyou@qq.com"

# 查看所有配置
git config --list
```

注意：如果没有配置name和email，那么就无法进行commit操作。

### 分区

* 工作区
* 暂存区
* 历史区（版本库）

```shell
git status 								# 查看状态

git add */./filename/folder				# 添加到暂存区

git commit -m "commit msg"				# 提交到历史区

git log									# 查看commit日志

git remote add github address			# 关联远程服务器
git push origin master					# 推送到远程服务器，第一次推送建议加上-u

git remote -v							# 查看关联的远程服务器
```

当你将文件提交到暂存区，`.git`文件夹下面就会多出一个index文件，该文件就是暂存区，不要手动进行改动。然后当你把暂存区的内容commit到历史区时，暂存区不会被清空，便于你恢复之前版本。而如果你想直接从工作区提交到历史区，可以使用`git commit -a -m 'commit msg'`。

注意：`git add`操作不会添加空文件夹，如果你想要通过git管理这个文件，可以在里面添加一个`.gitkeep`文件即可。而如果你需要让git忽略某些文件，只需要创建`.gitignore`文件配置忽略规则即可。

### 删除暂存区

如果你想删除暂存区的内容，可以执行以下命令：

```bash
git rm --cached 1.txt				# 删除已提交在暂存区中的1.txt

git rm --cached . -r				# 清空暂存区内容，一定要加-r，表示递归删除
```

如果在删除暂存区内容的同时，删除工作区的内容，可以使用如下命令：

```bash
git rm "文件名"
```

注意：若本地文件存在，则不能删除，需要通过`-f`参数删除。

### diff

不同区的代码比较：

```bash
git diff									# 工作区和暂存区

git diff --cached(--staged)   				# 暂存区和历史区

git diff master(分支名)		      		  # 工作区和历史区			
```

### 撤销修改

如果你本次修改还未添加到暂存区，现在想要恢复到上一次提交的状态。可以使用如下命令，来从暂存区拉取上一次的代码来覆盖工作区的当前内容。

```bash
git checkout "文件名"

git checkout .
```

注意：此时你无法再回到修改后的内容，因为你修改的内容并未提交到Git中。

如果你修改的代码已经提交到了暂存区，但是想要放弃此次添加到暂存区的内容，而恢复到上一次暂存区的状态，就可以使用命令：

```bash
git reset HEAD "文件名"

git reset HEAD .
```

注意：上述操作只能回滚上一次，无法回滚到上上一次或之前。这样你就可以继续使用`git checkout "文件名"`来使用上一次暂存区的内容覆盖当前工作区的内容。

> 从上述操作可以看出，如果只是将错误代码提交到暂存区是没有关系的，但是如果提交到历史区就会被永久性的记住。

### 修改commit

```shell
git commit --amend		# 进入vim编辑器，修改最后一次提交。按c进入编辑状态，按两次大写Z退出编辑状态

git log					# 最后一次commit已经修改掉
```

如果修改的不是最后一次commit，而是之前的commit信息，需要使用`git rebase`指令。

```shell
git rebase -i HEAD~3/$head			#选择要修改的commit

# 按下i，选择的commit信息pick改为edit。按esc退出编辑状态，按shift+:输入wq，保存退出

git commit --amend					

# 进入vim编辑器，修改commit信息。esc退出编辑状态，shift+:输入wq，保存退出

git rebase --continue				#完成更改commit
```

### 回滚版本

使用`git log`查看commit日志时，你会发现每个版本都对应一个40位的唯一版本ID，通过这个ID可以回滚版本。

```bash
git reset --hard 版本号		# 版本号不必全写，但也不能写的特别少
```

注意：该指令会将指定版本内容覆盖暂存区和工作区。此时你继续使用`git log`只能看到本次回滚版本及其之前的提交历史，如果你想要在回滚到本次回滚版本之后的版本（未来的版本），可以使用`git reflog`来查看整个提交历史。

```bash
git reflog					# 查看所有的日志
```

这样你就可以回滚任意的版本。

另外，如果你想快速回滚到上次版本，可以直接使用如下指令。

```bash
git reset --hard HEAD^			# 回滚到上次版本

git reset --hard HEAD~3			# 回滚到三次版本之前
```

### 分支

```shell
git branch 						# 查看分支

git branch dev					# 创建分支
git checkout dev				# 切换分支

# 创建分支和切换分支可以一步完成
git checkout -b dev

git branch -d	dev				# 删除分支
git branch -D dev				# 强制删除
```

注意：删除分支不允许在当前分支删除。

当文件有修改时，此时是不允许切换分支的，因为你修改的内容可能会被覆盖，这是不安全的。所以你需要把代码暂存起来或者进行提交，当然更建议前者，因为你的代码可能还没写完，还不想提交。

```bash
git stash						# 暂存，其实就是用上一次暂存区的内容直接覆盖工作区，保证两者一致，这样就可以顺利切换分支
	
git checkout master				# 成功切换

git checkout dev				# 重新切换到开发分支

git stash pop					# 恢复暂存区的内容
```

#### 合并分支

```bash
git merge dev					# master分支合并dev分支
```

如果仅仅是dev分支发生改动，那么master合并dev分支是启用`fast-forward`模式进行快速合并。简单来说就是直接将箭头指向dev分支的最新提交。

#### 处理冲突

如果dev和master分支都发生变动，此时在master上合并dev分支就会有冲突，所以就不会采用`fast-forward`进行合并，而是会新建一个commit，将两次版本进行合并。如果此时两个分支修改了同一行代码，就会发生冲突，而git不会自动处理冲突，你需要手动来处理分支。

```bash
git checkout -b dev			# dev分支上修改index.css并提交
git add .
git commit -m "dev commit"


git checkout master			# master分支上修改index.css并提交
git add .
git commit -m "master commit"

git merge dev				# 此时会有冲突，你需要手动解决冲突。

# 解决冲突之后，你需要再做一次提交
git add .
git commit -m "merge"
```

如果你想要看到合并的过程，可以使用`git log --graph`来查看，如果你想要简洁的一行显示，可以再加上`--oneline`参数。

### 远程服务器

```bash
# 远程仓库别名一般起origin，便于记忆
git remote add origin https://github.com/youzizi1/git-demo.git


git remote -v	# 查看远程仓库信息

git remote rm origin（远程仓库别名）	# 删除远程仓库origin
```

git只会将历史库推送到远程服务器上，工作区和暂存区不会推送。

#### 推送分支到远程

```bash
git checkout -b "gh-pages"		

touch index.html

git commit -a -m "add gh-pages branch"

git push origin gh-pages		# 推送分支到远程服务器
```

注意：gh-pages分支是github自带的分支，该分支提供一个github pages来给项目添加静态页面，用于描述该项目。

#### issue和pull-request

* issue：如果你在使用某个开源项目的时候，遇到问题或bug可以向作者提出一个issue
* pull-request：如果你想要对某个开源项目贡献代码时，可以向作者发出一个pull-request。前提是你需要先将项目fork到自己账户下，然后对源代码进行修改，最后向原仓库提出合并请求

#### 添加贡献者

作为项目拥有者，如果你觉得某人每次都要通过pull-request贡献代码很麻烦，可以在settings中找到collaboration中添加贡献者，这样贡献者可以直接操作原仓库。

#### 合并到本地

```bash
git fetch								# 拉取
git diff master origin/master			# diff
git merge origin/master					# 合并

# 上面操作可以合并为一个操作
git pull								# 拉取并合并
```

### tag版本

```bash
git tah v1.0
```

### 持续集成

travis cli是一个免费的持续集成工具，  用来构建及测试在GitHub托管的代码。它简单易学，你只需要配置`.travis.yml`配置文件即可使用。

```yml
# 示例
language: node_js

node_js: stable

cache:
  directories:
    - node_modules

before_install:
  - npm install -g hexo-cli

install:
  - npm install

script:
  - hexo clean
  - hexo g

after_script: # 最后执行的命令
  - cd ./public
  - git init
  - git config user.name "ugu"
  - git config user.email "yinyun957@gmail.com"
  - git add .
  - git commit -m "Travis CI Auto Builder :$(date '+%Y-%m-%d %H:%M:%S' -d '+8 hour')"
  - git push --force --quiet "https://${blog_token}@${BLOG_REF}" master:master

branches:
  only:
    - master # 触发持续集成的分支

env:
  global:
    # Blog Pages
    - BLOG_REF: github.com/youzizi1/youzizi1.github.io.git

```

其中`blog_token`是在travel cli官网进行配置，用来访问github仓库的权限，这个权限令牌需要在github上通过`Personal access tokens`进行生成。

### gittalk

gittalk是基于github issues的评论工具，自带github登录系统，只需要配置`OAuth Apps`即可使用。

### 配合ESLint

* `pre-commit`
* `yorkie`
* `husky`

这些模块可以实现`git commit`之前`lint`代码。

> `vue`中的`yorkie`基于`husky`。

### 图标和徽章

通过`https://shields.io`进行生成Github的README.md的图标。