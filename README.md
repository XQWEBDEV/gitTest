---

---

## Git的基本使用方法

### 1.项目的初始话

使用git init命令来初始话项目

```
git init
```

### 2.建立与远程仓库的链接

```
git remote add origin 远程仓库的SSH地址
```

**发现没有没有权限需要公钥**：

ssh-keygen -t rsa -C "you@example.com" (一直回车)

找到c盘下Administrator中.ssh文件夹，用记事本打开id_rsa.pub文件，复制里面的内容。

**在gitHub中添加权限**

找到settings->SSH and GPG keys->new ssh keys

设置title，将刚才复制的公钥粘贴到key中。点击`Add SSH key`

### 3.状态的查看

git status

### 4.本地工作空间,暂存区,本地仓库

本地工作空间是指我们的资源管理器你的编辑文件修改的可见的我们的工作环境，

暂存区是我们在将工作空间里的文件添加的等待提交的空间，每提价一次，暂存区就会清理一次，变为空

本地仓库是暂存区提交的代码的

### 5.将本地空间的内容添加到暂存区

```
git add 添加的文件或者目录
```

```
git add -A`和 `git add .   git add -u在功能上看似很相近，但还是存在一点差别
```

git add . ：他会监控工作区的状态树，使用它会把工作时的**所有变化提交**到暂存区，包括文件内容修改(modified)以及新文件(new)，但不包括被删除的文件。

git add -u ：他仅监控**已经被add的文件**（即tracked file），他会将被修改的文件提交到暂存区。add -u 不会提交新文件（untracked file）。（git add --update的缩写）

git add -A ：是上面两个功能的合集（git add --all的缩写）

### 6.更改的撤销

1.当我们在工作环境中修改后，没有推送到暂存，查看状态时就会出现本地空间与本地仓库出现差异

```
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   git/git.basic.md

no changes added to commit (use "git add" and/or "git commit -a")

```

提示表示可以提交到暂存区，也可以撤销更改

如果将修改保存到本地仓库，就先推送到暂存区，再提交到本地仓库

推送到暂存区

```
git add .
```

提交到本地仓库

```
git commit -m "修改git.basic.md文件"
```

如果不想提交到本地仓库就可以放弃或丢弃对文件的修改

```
git checkout -- <file>
```

2.当你更改了工作空间，并之前又推送到了暂存区时。执行撤销时，要先清空暂存区对应文件**git reset HEAD \<file\>**，再执行**git checkout -- \<file\> **

重置暂存区的文件

```
git reset HEAD <file>
```

查看状态

```
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)

Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        deleted:    git/test.txt

```

可以看到我们的修改并没有撤销，因为commit监听就是对文件的修改。对文件的删除是监听到的，所以撤销需要使用**git checkout --hard \<file\>**

```
git checkout --hard <file>
```

### 7.分支

在我们实际开发的时候都是在新的分支上对进行。当开发一个功能后再与主分支master合并。

```
//分支的创建
git bransh dev
//分支的切换
git checkout dev
```

创建新的分支并切换到该分支

```
git checkout -b dev
```

合并分支

```
git merge dev
```

删除分支

```
git branch -D dev
```

### 8.分支管理策略

合并分支是，加上`--no--ff`参数就可以用普通模式合并，合并的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并

注意：使用`--no-ff`合并是会自动创建一个新的commit，所以要加上-m并写上描述。

### 9.解决分支冲突

例如当我们在dev分支进行了test.txt文件的修改并提交，回到主分支master，在master分支上对test.txt文件进行修改并提交。在完成自动合并时就会发生合并冲突，需要手动的修改更改的文件并提交再进行分支的合并

### 10.Bug分支

在开发分支时没有完成不能提交，但是要保存工作环境就可以使用git stash

```
git stash
```

当我们完成处理其他bug时。再回到当前的dev分支时使用git stash pop时就会回到之前的开发环境

```
git stash pop
```

**git stash apply**恢复

**git stash drop**删除

**git stash pop**恢复并删除

### 11.标签管理

当我们要发布一个版本时，我们通常先在版本库中打一个标签（tag），这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就死把那个标签的时刻的历史版本取取出来。所以标签也是版本库的一个快照。

#### 11.1 创建标签

- 命令：`git tag <name>`用于新建一个标签，默认为HEAD，也可以指定一个commit id;
- `git tag -a <tagname> -m "描述"`可以指定标签信息
- `git tag -s <tagname> -m "描述"可以用PGP签名标签`
- 命令`git tag`可以查看所有的标签
- 用命令`git show <tagname>`可以看到指定标签的说明文字

#### 11.2 操作标签

因为创建标签都只存储在本地，不会自动推送到远程，所以，打上的标签可以在本地安全删除。

- 删除标签`git tag -d <tagname>`

- 如果要推送标签到远程，使用命令`git push origin <tagname>`
  或者一次性推送所有的尚未推送要远程的标签`git push origin --tag`

- 如果标签已经推送到远程，需要删除远程标签：先从本地删除：

  ```js
  $ git tag -d <tagname>
  ```

  然后在删除远程，使用命令push，但是格式如下:

  ```js
  $ git push origin :refs/tags/<tagnaem>
  ```

  需要产看远程标签是否删除，登录gitHub查看

### 12 版本回退

通过**git log** 来查看我们的提交记录的相关信息，最前面的就是我们每次的commit 一个标识

**git log --pretty=oneline --abbrve-commit**可以让提交记录在一行并进行缩略显示

**版本回退**

```
git reset "+commit标识"
```

### 13 删除

删除命令

```
git rm <file>
```

在对文件进行删除时也是对文件的修改，所以要回退也是可以通过**git checkout --hard \<file\>**

### 14 . pull远程库

```
git pull origin master
```

出现问题输入i再esc，再输入：wq回车

### 15. push到远程库

```js
git push origin master
```

### 