---
title: webpack-dev-server开启服务监测js、HTML、CSS并自动刷新网页的套路详细步骤并附代码
date: 2017-05-21 10:27:37
tags:
---
在一个合适的文件夹下npm初始化

~~~
npm init
~~~
安装webpack

~~~
cnpm install webpack --save-dev
~~~

创建webpack.config.js文件，**也可以鼠标右键创建**（下同，省略）。

~~~
touch webpack.config.js
~~~

建立一个app文件夹

~~~
mkdir app
~~~

在app文件夹中建立一个main.js的文件和一个index.html文件

~~~
cd app            			// 进入新创建的app文件夹
touch main.js
touch index.html
cd ..						// 退出app文件夹，到上一层
~~~



在main.js中随便写点什么，比如

~~~
var app = document.getElementById("app");
app.innerHTML = "This is an example";
~~~



在index.html中随便写点什么，其中id要与上面的代码对应

~~~
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>This is an example</title>
    </head>
    <body>
        <div class="">
            <p id="app"></p>
        </div>
        <script type="text/javascript" src="../build/bundle.js"></script>
    </body>
</html>
~~~

在webpack.config.js文件中加入如下代码，其中其中publicPath是后面配置webpack.dev.server的时候用的。

~~~
var path = require("path");
module.exports = {
  entry: {
    app: ["./app/main.js"]
  },
  output: {
    path: path.resolve(__dirname, "build"),
    publicPath: "/assets/",
    filename: "bundle.js"
  }
};
~~~

运行以下webpack，先编译下试试，因为只是局部安装，所以运行的时候要这样

~~~
node_modules/.bin/webpack
~~~

运行完之后就可以在terminal中看到运行的结果，工作目录中也会自动创建了一个build的文件夹，里面有一个bundle.js的文件，这些都是在上面webpack.config.js里面配置的结果。

浏览器打开index.html看看

先进入app文件夹

~~~
cd app
~~~

打开index.html的三种方法

~~~
open index.html										 // 默认浏览器打开
open -a /Applications/Google\ Chrome.app index.html  // chrome打开，前提是你的chrome放在了Applications里面
open -a "Google Chrome" index.html 					 //你也不知道chrome在哪，就用这个
~~~



安装webpack-dev-server，**下面cd的命令就不写了**

~~~
cd ..						// 因为上一步进入app文件夹了，所以要退出来
npm install webpack-dev-server --save-dev
~~~

把index中的`../build/bundle.js`改成`/assets/bundle.js`这个与上面设置的publicPath有关。如果没有publicPath，那这个路径就直接写成`bundle.js`

在命令行输入

~~~
node_modules/.bin/webpack-dev-server --content-base app/
~~~



运行了之后命令行会提示端口的号码，这个时候这个命令行在启动服务，在想用命令行打开浏览器就不方便了，所以手动到浏览器输入`localhost:8080/index.html`；

{% asset_img Jietu20170520-222013.jpg port %}

另一种iframe模式的操作的区别只在输入的网址：`localhost:8080/webpack-dev-server/index.html`

以上两种打开网页的方式选用一种即可。

这个时候要注意的是，生成的bundle.js文件在内存中，我们是看不到的。

以后还没多次的使用开启服务的命令，总是这样输入大串的命令实在不合适，所以，到npm初始化时候创建的package.json里面的scripts中加一行代用的命令

~~~
"dev": "webpack-dev-server --content-base app/"
~~~

{% asset_img Jietu20170520-222619.jpg npmscripts %}

再运行的时候就这样

~~~
npm run dev
~~~

运行了服务并打开了网页之后，再改动main.js文件，浏览器就可以自动刷新了。

#### 自动刷新HTML

安装HtmlWebpackPlugin插件

~~~
npm install --save-dev html-webpack-plugin
~~~



在webpack.dev.server中添加配置

~~~
var path = require("path");
var HtmlWebpackPlugin = require('html-webpack-plugin'); // 添加在这里
module.exports = {
  entry: {
    app: ["./app/main.js"]
  },
  output: {
    path: path.resolve(__dirname, "build"),
    publicPath: "/assets/",
    filename: "bundle.js"
  },
  plugins: [new HtmlWebpackPlugin({					//添加在这里
    template: path.resolve(__dirname, 'app/index.html'),
    filename: 'index.html',
    inject: 'body'
  })]
};
~~~



运行一下，改动html就可以自动刷新了。

#### 自动刷新css

安装style-loader和css-loader

~~~
npm install --save-dev style-loader css-loader
~~~

在main.js中引入style.css，由于main.js上面指定的配置中的入口，所以不再main.js中引入style.css的话，webpack是找不到这个文件的。

~~~
import './style.css';
~~~

在webpack.dev.server中添加配置，添加在output下面，plugins上面。

~~~
  module: {
    loaders: [
        {
            test: /\.css$/,
            loader: 'style-loader!css-loader'
        }
      ]
  },
~~~

创建一个css，随便写点什么

~~~
p {
    background-color: blue;
}
~~~

在次再运行，修改css就可以自动刷新了。



完整代码在[github](https://github.com/zhuanyongxigua/routine.git)

参考：

[WEBPACK DEV SERVER](https://webpack.github.io/docs/webpack-dev-server.html)

[HtmlWebpackPlugin](https://webpack.js.org/plugins/html-webpack-plugin/)
