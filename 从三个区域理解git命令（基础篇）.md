---
title: 从三个区域理解git命令（基础篇）
date: 2017-03-20 21:34:40
---

# 2.1 获取库

在学习这些下面的命令之前，首先要清楚你的代码的三种状态，已修改、已暂存、以提交。以及他们对应的三个区域：工作目录、暂存区、库。

其实在除这三个区域之外还有一个区域，这个区域是在你的git系统之外的区域，暂且叫它**非跟踪区**，所以我们上面说的三个区域就可以统称为跟踪区了。

下面这个链接是一个git的操作教学。

[](https://try.github.io)

## 库从哪来？

### 两个来源，一个是你的电脑，一个是别人的电脑

**不管在哪，他们现在都在非跟踪区。**所以这一步的意义就是把代码从非跟踪区拉到你git管理的跟踪区。

**如果是你的电脑**，在你写的代码的那个**文件夹**右键，点击git bash here（前提是你要安装了git）；或者在terminal用cd命令到所在文件夹。

然后

```
$ git init
```

**如果是别人的电脑** （一般是在服务器），在你觉得方便的文件夹，比如右键你的`新建文件夹` ，点击git bash here，然后输入。

```
$ git clone https://github.com/libgit2/libgit2
```

这样在你的新建文件夹里面就有一个libgit2的文件夹了。

当然你也可在克隆的时候顺便把名字改了。

```
$ git clone https://github.com/libgit2/libgit2 mylibgit
```

# 开始编程

### 跟踪新文件（新增文件加入git管理范围）

在把需要的东西拉到跟踪区之后（本地库）之后，就可以开始写代码了。

如果你是新建了一个文件写，git不会主动把他划在**跟踪区**，所以你需要用`git add` ；

如果你是在已有的文件里面修改代码，修改好了之后想把它添加到**暂存区**，那你需要用`git add` ；

所以，`git add` 不但可以把文件从非跟踪区拉到工作目录，还可以把文件从工作目录拉到暂存区。 `git add` 还有其他的功能，总之，要把它理解成“**添加内容到下一次提交**”，不要理解成“**把文件增加到项目中**”。

具体举例：

```$ git add README~~~
$ git add CONTRIBUTING.md
```

一个一个添加麻烦？

~~~
$ git add -A
~~~

`A` 就是`all` ，无论文件在**非跟踪区**还是**工作区**，都通通拉到**暂存区**。

当然`git add` 还有很多其他的细节，具体可以看[这个](https://git-scm.com/docs/git-add)。

### 检查你文件的状态

用过`git add` 之后，或者没有用，无所谓，你都可以用`git staus` 查看你的**工作目录**的里面文件的状态，这个工作目录就是你用git初始化的文件夹，这个命令查看的区域可以包括**非跟踪区**、**工作目录**、**暂存区**。

~~~
$ git status
~~~

里面不但显示文件的状态，也会有相应的操作的提示。如果你觉得使用这个命令得到的信息过于冗长，那你就可以试着用`git status -s` 或者 `git status --short` 。

```
$ git status -s
 M README
MM Rakefile
A  lib/git.rb
M  lib/simplegit.rb
?? LICENSE.txt
```

 `??` 表示新的且**没有被缓存**的文件,

 `A` 新的且**已经被缓存**的文件,

 `M` 被修改的文件

`MM` 缓存之后再次修改的。

### 查看文件具体做了哪些修改

```
$ git diff
```

### 忽略文件

总是有一些文件你不想被git接手，那就手动忽略他们吧。

创建一个`.gitignore` 的文件，以存放你需要忽略的文件模式。

```
$ cat .gitignore
*.[oa]
*~
```

 “.o” or “.a”表示忽略以`.o`、`.a` 结尾的文件。

第二行的意思是忽略名字里面带有波浪线 (`~`)的文件。

### 提交更改

用了`git add` 把文件拉到**暂存区**之后，就可以提交了。

```
$ git commit
```

用 `commit` 加 `-m` 留下提交信息。

```
$ git commit -m "Story 182: Fix benchmarks for speed"
[master 463dc4f] Story 182: Fix benchmarks for speed
 2 files changed, 2 insertions(+)
 create mode 100644 README
```

#### 如果觉得麻烦，可以跳过暂存区

这个命令用到的`all` 与上面说的`git add` 的`all` 是有区别的，`git commit` 的`all` 不包括**非跟踪区**的文件

~~~
$ git commit -a
~~~

~~~
$ git commit -a -m`
~~~

#### 查看提交历史

~~~
$ git log
~~~

# 撤销

#### 修改已提交

**针对的是已提交的文件。**

```
$ git commit --amend
```

git commit --amend命令用来修复最近一次commit. 可以让你合并你缓存区的修改和上一次commit, 而不是提交一个新的快照. 还可以用来编辑上一次的commit描述.记住amend不是修改最近一次commit, 而是整个替换掉他. 对于Git来说是一个新的commit. 如果文件没有变，那就可以用它来修改上一次的描述。

#### 取消已缓存

**在已缓存区**，可以把不想缓存的文件拉出来

```
$ git reset HEAD CONTRIBUTING.md
```

#### 撤销已更改

**针对的文件是工作目录里面的文件**，如果是没有缓存的文件，退回到的是上次提交的状态；如果是缓存了之后再次修改的文件，那退回的是缓存之后的状态。这个命令使用要慎重，一旦退回，无法恢复。

~~~
git checkout -- <file>
~~~

# 别名

用 `git config` 简化命令。

```
$ git config --global alias.co checkout
$ git config --global alias.br branch
$ git config --global alias.ci commit
$ git config --global alias.st status
```

执行了上述的命令后，checkout就变成了co，branch就变成了br，其他两个同理。

如果是想替换`reset HEAD` 这样多单词的命令：

```
$ git config --global alias.unstage 'reset HEAD --'
```
