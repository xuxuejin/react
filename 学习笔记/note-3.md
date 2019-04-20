# 基于webpack的开发模式
## 1.模块化发展回顾
以前的开发模式是直接手动引入文件，有一些问题：a.路径容易写错;b.文件之间有些依赖，尤其是大项目。后来出现了模块化,如：sea.js/require.js，应用最好的是nodeJs，node使用的是commonjs规范，后来浏览器出现了browserify,它是浏览器端的模块化实现，与sea.js/require.js相比，browserify非常贴切nodeJs的模块化实现，但是代码已更改，浏览器就要刷新。随后就出现了watchify，它是browserify的一个封装，带监听功能。但是以上的这些模块化都是针对js，为什么css，图片不能实现模块化呢？能不能把所有的资源都当模块来处理，实现模块化。这时候webpack就应运而生了。webpack是一个模块加载器，对它来说，一切都是模块。
## 2.webpack组成 
1.入口、出口
2.loaders（加载器）
3.plugins（插件）
