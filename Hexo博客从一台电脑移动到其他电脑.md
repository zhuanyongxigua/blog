---
title: Hexo博客从一台电脑迁移到其他电脑
date: 2017-05-20 16:18:46
tags:
---
hexo官方给了一些迁移的方法，不过它上面介绍的方法都是把博客文章从hexo系统迁移到其他博客系统的方法。然而我们这里要讨论的是：

**当我们更换电脑的时候我们应该怎么办？**

所以默认你已经成功利用hexo和github发布博客，如果还没有，可以看一下[教程](http://lixiaolai.com/2016/06/22/makecs-build-a-blog-with-hexo-on-github/)

具体的思路是：在生成的已经推到github上的hexo静态代码出建立一个分支，利用这个分支来管理自己hexo的源文件。

如果能在刚刚配置hexo的时候就想好以后的迁移的问题就太好了，可以省掉很多麻烦，可实际使用中，刚刚配置hexo的时候，好多人都是初学，不会想到以后的问题，我就是这样的。

具体的操作：

克隆gitHub上面生成的静态文件到本地

~~~
git clone https://github.com/yourname/hexo-test.github.io.git
~~~

把克隆到本地的文件除了git的文件都删掉，找不到git的文件的话就到删了吧。不要用`hexo init`初始化。

将之前使用hexo写博客时候的整个目录（所有文件）搬过来。把该忽略的文件忽略了

~~~
touch .gitignore
~~~
创建一个叫hexo的分支

~~~
git checkout -b hexo
~~~

提交复制过来的文件到暂存区

~~~
git add --all
~~~

提交

~~~
git commit -m "新建分支源文件"
~~~

推送分支到github

~~~
git push --set-upstream origin hexo
~~~

到这里基本上就搞定了，以后再推就可以直接`git push`了，hexo的操作跟以前一样。

今后无论什么时候想要在其他电脑上面用hexo写博客，就直接把创建的分支克隆下来，`npm install`安装依赖之后就可以用了。

克隆分支的操作

~~~
git clone -b hexo https://github.com/yourname/hexo-test.github.io.git
~~~
因为上面创建的是一个名字叫hexo的分支，所以这里`-b`后面的是hexo，再把后面的gitHub的地址换成你自己的hexo博客的地址就可以了。

这样做完了以后，每次写完博客发布之后不要忘了还要`git push`把源文件推到分支上。
