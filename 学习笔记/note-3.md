# 基于webpack的开发模式

## 1.模块化发展回顾
以前的开发模式是直接手动引入文件，有一些问题：

    a.路径容易写错
    b.文件之间有些依赖

为了解决这些问题，就出现了模块化，如：sea.js、require.js 等，其中应用的最好要属 nodeJs，node 中使用的是commonjs规范。

接着浏览器端出现了 browserify，它也是浏览器端的模块化实现，与 sea.js 和 require.js 相比，browserify 更加贴近 nodeJs 的模块化实现，但是代码更改之后，浏览器也要跟着刷新才行。随后就出现了 watchify，它是 browserify 的一个封装，带监听功能。

以上的这些模块化都是针对js，为什么css，图片等其它资源不能实现模块化呢，能不能把所有的资源都当模块来处理？webpack 就应运而生了，在 webpack 是一个模块加载器，对它来说，一切都是模块。

## 2.webpack组成 
    1.入口、输出
    2.loaders（加载器）
    3.plugins（插件）

## 3.安装webpack，配置react开发环境

### 安装webpack
    npm i webpack@3.11.0 webpack-dev-server@2.11.5 -g    
上面安装的是 webpack 的命令环境，之所能直接执行 webpack 命令，是因为全局安装了 webpack。在安装 webpack 和 webpack-dev-server 的时候，一定要注意版本问题，不同的版本不仅配置不一样，用法可能也有所不同。webpack -> 3.11.0 和 webpack-dev-server -> 2.11.5 经过测试没有问题。

### 配置webpack
    entry: './index.js',
    output: {
        filename: './bundle.js',
    }

### 运行webpack
    webpack        普通打包
    webpack -w     持续监听，并且打包
    webpack -p     压缩打包
    webpack -pw    持续监听，并且压缩打包
    
### 安装模块
webpack 2.x 默认支持模块化，但是非 js 模块并不能处理，需要借助各种 loader 来处理。对于 css 文件而言，需要 style-loader 和 css-loader 两个模块。在安装模块之前，最好使用 npm init -y 来初始化生成工程化文件，这样在团队协作的时候，只需要执行 npm i 就可以安装全部包文件，使用脚手架工具会自动生成工程文件。

    npm i style-loader css-loader —D

    —D 是--save     的简写，会写入到 package.json 里边的 dependencies 里边
    -S 是--save-dev 的简写，会写入到 package.json 里边的 devDependencies 里边

模块下载完成后，开始配置文件

    module: {
        rules:[
            {
                test: /\.css$/,
                use:['style-loader', 'css-loader']
            }
        ]
    }
    
### 配置babel
要想使 webpack 支持 jsx 语法，需要安装 babel 来处理，需要安装如下几个模块：

1.babel-core            babel的核心

2.babel-loader          babel模块处理器(注意版本区别，具体查看官网介绍)

3.babel-preset-es2015   babel处理 es6 的预设

    {
        test: /\.js$/,
        exclude: /node_modules/,
        use:['babel-loader']
    }

仅配置 babel-loader 还不够，一定要把处理 es6 的预设加上，配置预设有两种方式：
1.参数

    {
        test: /\.js$/,
        exclude: /node_modules/,  // 不希望 babel 转换的目录
        use:{
            loader: 'babel-loader',
            options: {
                presets: ['es2015']
            }
        }
    }
    
2.通过 .babelrc 文件进行配置

    {
        "presets":["es2015"]
    }

### 配置react
在配置 react 时，也有一些核心的模块需要安装：

1.react        react核心

2.react-dom    react处理虚拟DOM的核心

除此 react 本身，还需要安装处理 react 的 loader：

1.babel-preset-react   处理 react 的预设

2.react-hot-loader     热更新组件

安装完成之后，修改配置文件

    {
        test: /\.js$/,
        exclude: /node_modules/,
        use:['react-hot-loader/webpack','babel-loader']  
    }

### 启动服务器环境
已经全局安装了 webpack-dev-server，因此，除了使用 webpack 简单打包，还可以使用 webpack-dev-server 启动一个本地服务器，配置如下：

    devServer: {
        /*设置基本目录结构，也就是想要使用服务的目录地址，默认会启动目录中的 index.html*/
        contentBase: './src', 
        /*服务器的IP地址，可以使用IP也可以使用localhost*/
        host:'localhost',
        /*配置服务端口号，建议别用80*/
        port:9090,
        //hot: true,
        inline: true,
        /*是否自动打开浏览器*/
        open: true              
    }
更多配置信息参考官网：https://webpack.docschina.org/configuration/dev-server/#devserver-inline

上面的配置中，如果不加 hot 选项，默认使用了热更新。如果加上 hot 选项，还需要配合使用 HotModuleReplacementPlugin 插件，这个插件是 webpack 自带的，本地目录想使用的话，就必须引入 webpack 模块，但是本地项目没有没有安装 webpack 模块。所以，使用之前先安装 webpack 模块：

    npm webpack -D
    
安装完成之后，在 webpack.config.js 文件中要引入 webpack：

    const webpack = require('webpack');

    plugins: [
        new webpack.HotModuleReplacementPlugin()
    ]

但是，本地测试发现，添加插件，只是不报错而已，并不能实现热更新，可能更版本有关系。使用 webpack 3.11.0 和 webpack-dev-server 2.11.5 这样的组合，不写 hot 选项，不需要引用 webpack，也不需要添加插件选项，就可以实现热更新的操作。

☆注意：关于 webpack 在配置时，按照网上的教程一步一步来，也不一定能跑起来，这是就要看看是不是版本差异造成的，文档可能写得比较早，等你按照文档实现的时候，很多接口或者配置已经更改了，所以最好还是能查看官方的手册进行配置。

之前在运行 webpack-dev-server 时就遇到以下问题：

1.运行时提示安装 webpack-cli

2.运行时提示 Invalid configuration object

这两个问题都是 webpack-dev-server 版本引起的，换成版本 2.11.5 就解决了。
