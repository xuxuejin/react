# 脚手架开发React
手动配置 webpack + react，真是劳神又伤身，其实在实际项目开发中，大多数都使用脚手架来搭建 React 项目。在 vue 中使用 vue-cli 来生成项目文件，而在 react 中使用 create-react-app 来生成项目。

## 1.安装脚手架命令环境
    npm i create-react-app -g
验证有没有安装成功：create-react-app --version

## 2.创建项目
    create-react-app <项目名称>
    
## 3.进入到项目目录
    cd <项目名称>
    
## 4.启动项目
    npm start
 
除了启动项目之外，还有其他几个命令，都有各自的用途：
    
    npm start
        Starts the development server.

    npm run build
        Bundles the app into static files for production.

    npm test
        Starts the test runner.

    npm run eject
        Removes this tool and copies build dependencies, configuration files
        and scripts into the app directory. If you do this, you can’t go back!
        （将封装在 create-react-app 中的配置全部反编译到当前项目，用户就能完全取得 webpack 文件的控制权，更改 webpack 配置。）
