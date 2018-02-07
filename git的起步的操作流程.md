---
title: git的起步的操作流程
date: 2017-05-18 21:34:40
---
先初始化

~~~
git init
~~~
加入一个README文件

~~~
git add README.md
~~~

第一次提交

~~~
git commit -m "first commit"
~~~



链接远程仓库（已经在远程创建了一个未初始化的仓库）

~~~
git remote add origin https://github.com/yourproject/-.git
~~~

第一次push

~~~
git push -u origin master
~~~

这个时候这个已经被git的文件夹里面只有一个README.md的文件，接下来就可以写自己的代码了，在第一次需要提交自己的代码之前，先要规定需要忽略的文件

建立.gitignore文件

```
touch .gitignore
```

在创建的文件里面加入需要忽略的文件或文件夹例如`node_modules/`，具体的过滤选项

```
*.a       # 忽略所有 .a 结尾的文件
!lib.a    # 但 lib.a 除外
/TODO     # 仅仅忽略项目根目录下的 TODO 文件，不包括 subdir/TODO
build/    # 忽略 build/ 目录下的所有文件
doc/*.txt # 会忽略 doc/notes.txt 但不包括 doc/server/arch.txt
```

然后在`git add --all`和`git push`。
