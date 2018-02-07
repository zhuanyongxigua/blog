---
title: npm网速慢用淘宝cnpm
date: 2017-04-26 21:34:40
---
npm网速慢

用[淘宝npm](https://npm.taobao.org/)镜像

## 1.继续用npm这个命令

临时使用

~~~
npm --registry https://registry.npm.taobao.org install <pkg>
~~~

长期使用

~~~
npm config set registry https://registry.npm.taobao.org
// 验证是否成功
npm config get registry
~~~

## 2.换用cnpm命令安装包

~~~
npm install -g cnpm --registry=https://registry.npm.taobao.org
~~~

安装的时候就是这样了

~~~
cnpm install <pgk>
~~~
