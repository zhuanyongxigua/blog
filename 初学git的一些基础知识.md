---
title: 初学git的一些基础知识
date: 2017-02-22 21:34:40
---

### 版本控制

​	提到git就不得不说版本控制，与git相关的系统都是版本控制系统，他们用于记录文件的更改以便随时可以返回需要的特定版本。它的发展历史，概括的说就是从可操作文件一个到多个，从单人工作到多人协同，从集中式到分布式。

### git工作原理

​	git是一个DRCS（distributed revision control system），它来自偶然，产于赌气。git 的工作原理也是它与其他版本控制软件的本质区别，每一次提交都相当于一个**快照**，改动的文件做记录，未改动的文件做链接。如下图，每一个纵向的**version**就是一个快照。

![Git stores data as snapshots of the project over time.](https://git-scm.com/book/en/v2/images/snapshots.png)

### git工作流程

​	git有三种状态，**Committed**（已提交），**Modified**（已修改）和**Staged**（已暂存）。已提交指的是文件已经老老实实地躺在你的数据库里面了；已修改是指已经变动但是并没有提交；已暂存的标示你已经**标记**这一次修改，但是标记的快照并没有合并到你的数据库里面。三种状态分别对应git的三个工作区域：**the Git directory**（本地仓库）, **the working tree**（工作目录）, **and the staging area**（暂存区域）。

![点击查看源网页](http://images0.cnblogs.com/blog2015/512650/201508/181930031754207.png)

所以，当我们使用git 的时候，我们的工作流程是这样的：

1.  修改工作目录中的文件；
2.  标记快照暂存；
3.  提交到库。

### 参考文件

<http://ericsink.com/vcbe/html/history_of_version_control.html>

<https://git-scm.com/book/en/v2/Getting-Started-Git-Basics>
