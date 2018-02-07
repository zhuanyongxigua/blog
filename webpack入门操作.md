---
title: webpack入门操作
date: 2017-05-01 21:34:40
---
## 起步

**在文件夹创建一个package.json**

~~~
npm init
~~~

**安装webpack**

~~~
// 安装Webpack
npm install --save-dev webpack
~~~

**新建一个webpack.config.js的文件并配置如下**

~~~
module.exports = {
  entry:  __dirname + "/app/main.js",//已多次提及的唯一入口文件
  output: {
    path: __dirname + "/public",//打包后的文件存放的地方
    filename: "bundle.js"//打包后输出文件的文件名
  }
}
~~~

> __dirname是当前文件夹所在的位置，也就是当前目录的绝对路径，它是一个变量，如果把这个目录放到其他的地方它会自动变化。

## 加入一些基础功能

**在package.json里面加入如下配置。**

~~~
{
  "name": "webpack-demo-practice",
  "version": "1.0.0",
  "description": "webpack-demo-practice",
  "scripts": {
    "start": "webpack" //配置的地方就是这里啦，相当于把npm的start命令指向webpack命令
  },
  "author": "zhang",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^1.12.9"
  }
}
~~~

package.json中的脚本部分已经默认在命令前添加了`node_modules/.bin`路径，所以无论是全局还是局部安装的Webpack，你都不需要写前面那指明详细的路径了。

**在webpack.config.js文件中加入source maps**，目的是找到代码的错误，方便我们改错

~~~
module.exports = {
  devtool: 'eval-source-map',      //配置生成Source Maps，选择合适的选项
  entry:  __dirname + "/app/main.js",
  output: {
    path: __dirname + "/public",
    filename: "bundle.js"
  }
}
~~~

eval-source-map只是source maps中的一个选项，它更适用于开发阶段，至于source maps有哪些选项，需要自己去查一下了。

**用webpack构建本地服务器**，目的是在没有后端的情况下也可以模拟后端来帮助前端调试

先安装webpack-dev-server

~~~
npm install --save-dev webpack-dev-server
~~~

然后把配置加到webpack.config.js文件之中

~~~
module.exports = {
  devtool: 'eval-source-map',

  entry:  __dirname + "/app/main.js",
  output: {
    path: __dirname + "/public",
    filename: "bundle.js"
  },

  devServer: {
    contentBase: "./public",//本地服务器所加载的页面所在的目录
    colors: true,//终端中输出结果为彩色
    historyApiFallback: true,//不跳转
    inline: true//当改变代码是可以自动刷新页面
  }
}
~~~

## 加入loader

loader是webpack的加载器，它的功能有点**类似于**插件（plugin），他与webpack正牌的插件（plugin）是有区别的，loader主要用于加载资源，也可以说是转化资源，把各类语法转化在同一个文件之中。plugin的功能则更加丰富。

**安装可以转换json的loader**

~~~
npm install --save-dev json-loader
~~~

修改webpack.config.js文件，把json加载器放到module里面

~~~
module.exports = {
  devtool: 'eval-source-map',

  entry:  __dirname + "/app/main.js",
  output: {
    path: __dirname + "/public",
    filename: "bundle.js"
  },

  module: {//在配置文件里添加JSON loader
    loaders: [
      {
        test: /\.json$/,    //正则表达式，用于匹配loader所处理文件的拓展名(必须)
        loader: "json"      //loader的名称(必须)
      }
    ]
  },

  devServer: {
    contentBase: "./public",
    colors: true,
    historyApiFallback: true,
    inline: true
  }
}
~~~

**可以编译JavaScript的loader，babel**

~~~
npm install --save-dev babel-core babel-loader babel-preset-es2015 babel-preset-react
~~~

在webpack.config.js中加入配置

~~~
module.exports = {
  devtool: 'eval-source-map',

  entry:  __dirname + "/app/main.js",
  output: {
    path: __dirname + "/public",
    filename: "bundle.js"
  },

  module: {
    loaders: [
      {
        test: /\.json$/,
        loader: "json"
      },
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'babel',
        query: {
          presets: ['es2015','react']
        }
      }
    ]
  },

  devServer: {
    contentBase: "./public",
    colors: true,
    historyApiFallback: true,
    inline: true
  }
}

~~~

另外，由于babel的配置一般会比较多，所以webpack支持单独创建文件配置babel，webpack会自动调用，方法是创建一个`.babelrc`的文件，用于单独配置babel。

单独配置babel的 情况下，webpack.config.js就要改为

~~~
// webpack.config.js
module.exports = {
  devtool: 'eval-source-map',

  entry:  __dirname + "/app/main.js",
  output: {
    path: __dirname + "/public",
    filename: "bundle.js"
  },

  module: {
    loaders: [
      {
        test: /\.json$/,
        loader: "json"
      },
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'babel'
      }
    ]
  },

  devServer: {
    contentBase: "./public",
    colors: true,
    historyApiFallback: true,
    inline: true
  }
}
~~~

新文件.babelrc的内容为

~~~
//.babelrc
{
  "presets": ["react", "es2015"]
}
~~~

**安装处理css的loader**

~~~
//安装
npm install --save-dev style-loader css-loader
~~~

配置webpack.config.js

~~~
//使用
module.exports = {
  devtool: 'eval-source-map',

  entry:  __dirname + "/app/main.js",
  output: {
    path: __dirname + "/build",
    filename: "bundle.js"
  },

  module: {
    loaders: [
      {
        test: /\.json$/,
        loader: "json"
      },
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'babel'
      },
      {
        test: /\.css$/,
        loader: 'style!css'//添加的配置再这里
      }
    ]
  },

  devServer: {
    contentBase: "./public",
    colors: true,
    historyApiFallback: true,
    inline: true
  }
}
~~~

其中`!`可以理解成“和”，css文件同时使用这两个加载器处理

css文件也需要放到入口文件里面，现在这个例子的入口文件是main.js

~~~
//main.js
import './main.css';//导入css文件
~~~

## 插件（plugin）

插件需要使用npm安装

安装之后同样在webpack.config.js里面配置

~~~
//使用
module.exports = {
  devtool: 'eval-source-map',

  entry:  __dirname + "/app/main.js",
  output: {
    path: __dirname + "/build",
    filename: "bundle.js"
  },

  module: {
    loaders: [
      {
        test: /\.json$/,
        loader: "json"
      },
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'babel'
      },
      {
        test: /\.css$/,
        loader: 'style!css'//添加的配置再这里
      }
    ]
  },

  plugins: [
    new webpack.BannerPlugin("Copyright Flying Unicorns inc.")
  ],

  devServer: {
    contentBase: "./public",
    colors: true,
    historyApiFallback: true,
    inline: true
  }
}
~~~
