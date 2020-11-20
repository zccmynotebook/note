---
title: tree shaking
tags:  webpack 配置
---

>
Tree Shaking 可以用 来剔除 JavaScript 中用 不上的死代码。它依赖静态的 ES6 模块化 语 法，例如通过import和export导入、导出。 
需要注意，要让 Tree Shaking 正常工作的前提是，提交给 Webpack的JavaScript代码必须采用了ES6 的模块化语法，因为 ES6模块化语法是静态的(在导入、导出语句中的路径 必须是静态的字符串，而且不能放入其他代码块中)，这让 Webpack 可以简单地分析出哪些 export 的被 import 了。如果采用了 ES5 中的模块化，例如 module.export={ ... }、 require (x+y)、 if (x) {require (’./util ’)}，则 Webpack 无法分析出可以剔除哪些 代码。

### 配置Tree Shaking生效 

>
首先，为了将采用 ES6 模块化的代码提交给 Webpack，需要配置 Babel 以让其保留 ES6 模块化语句。修改 .babelrc 文件如下:
”modules ”: false 的含义是关闭 Babel 的模块转换功能，保留原本的 ES6 模 块化语法。

```
{
    "presets": [
      [
        "@babel/preset-env",
        {
          "modules": false,      
        }
      ]
    ],
  }

```
若打开Webpack输出的bundle.js文件并查看， 则会发现用不上的代码还在里面,
要剔除用不上的代 码， 则还得经过 UglifyJS 处理一遍。

在项目中使用大量的第 三方库时，我们会发现 Tree Shaking 似乎不生效了，原因是大部 分 Npm 中的代码都采用了 CommonJS 语法，这导致 Tree Shaking 无法正常工作而降级处理。 但幸运的是，有些库考虑到了这一点，这些库在发布到 Npm 上时会同时提供两份代码，一份采 用 CommonJS 模块化语法，一份采用 ES6 模块化语法。并且在 package.json 文件中 分别指出这两份代码的入口。
```
{
  ”main”:”lib/index.j5”，//指明采用 CommonJS模块化的代码入口
  ”jsnext:main,,:”es/index.js” //指明采用 ES6模块化的代码入口
}
```
为了 让 Tree Shaking 对 redux 生效，需要配置 Webpack 的文件寻找规 则 如下 :

```
module.exports = {
  resolve: {
    //针对 Npm 中的第三方模块优先采用 jsnext:main 中指向的 ES6 模块化语法的文件
        mainFields: [’jsnext:main’,’browser’,’main’]
  }
}
```
以上配置的含义是优先使用 jsnext:main作为入口，如果不存在， jsnext:main就会采用 browser 或者 main 并将其作为入口 。 
目前越来越多的 Npm 中的第三方模块都考虑到了 Tree Shaking， 并对其提供了支持。 采用jsnext:main 作为 ES6 模块化代码的入口是社区的一个约定.