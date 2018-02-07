---
title: 用vultr建立网站
date: 2017-04-20 21:34:40
---
内容会运用到Linux、docker

如果有visa，可以直接到[vultr官网](www.vultr.com)付款购买服务器。如果没有，某宝上面有代沟。

我在这里使用的配置是，Tokyo Ubuntu16.04 25GB SSD，也就是通常能买到的最低的配置

我是在某宝上面代购的，谈好价钱后，对方会给你一个ip地址和用户名还有密码。

![某宝](http://o7w6l6vti.bkt.clouddn.com/Screenshot_20_04_2017__8_59_PM.png)

之后打开terminal

~~~
ssh root@45.76.222.131
~~~

terminal会提示输入密码，直接输入收到的密码就好。

接着就要输入以下的命令了。

生成一个ssh key

~~~
ssh-keygen
~~~

显示公钥

~~~
cat ~/.ssh/id_rsa.pub
~~~

把生成的公钥放到GitHub上面

接着回来搭建基本的开发环境

安装vim

~~~
apt-get install -y vim git
~~~

安装docker

~~~
curl -SsL https://get.docker.com | sh
~~~

查看docker中的容器

~~~
docker ps -a
~~~

docker中安装node6.8

~~~
docker pull node:6.8
~~~

docker中安装mongodb:3.0

~~~
docker pull mongo:3.0
~~~

docker运行node

~~~
docker run -itd -v /root/.ssh/:/root/.ssh --name node --net=host node:6.8 bash
~~~

进入docker

~~~
docker exec -it node bash
~~~

从GitHub克隆过来自己写好的网页应用

~~~
git clone git@github.com:zhuanyongxigua/notepad-online.git
~~~

列出所有

~~~
ls -all
~~~

**出容器**（另开一个terminal重新进）运行mongodb

~~~
docker run -itd --name mgo --net=host mongo:3.0
~~~

然后就可以cd到克隆过来的文件里面使用`npm install`并`npm start`了

如果运行出错，需要停掉程序

~~~
docker stop node
~~~

这里面的node就是上面创建的那个，因为上面创建的时候的name规定的是node

改好错之后就继续运行node

~~~
docker start node
~~~

如果需要向mongodb post数据

~~~
curl -i -H "Content-Type:application/json" -XPOST -d '这里是JSON数据' url
~~~
