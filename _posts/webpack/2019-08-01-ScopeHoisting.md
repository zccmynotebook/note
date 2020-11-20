---
title: Scope Hoisting
tags:  webpack 配置
---

开启 Scope Hoisting 后,代码体积更小，因为函数申明语句会 产生大量的代码;
代码在运行时因为创建的函数作用域 变少了 ，所以内 存开销也变小了;
Scope Hoisting 的 实现原理其实很简单:分析模块之间的依赖关系，尽可能将被打散的 模块合并到一个函数中，但前提是不能造成代码冗余 。 因此只有那些被引用了 一次的模块才 能被合井 。
由于 Scope Hoisting 需要分析模块之间的依赖关系，因此源码必须采用 ES6 模块化 语句， 不然它将无法生效。涉及配置：
```
const ModuleConcatenationPlugin = require (’webpack/lib/optimize/ModuleConcatenationPlugin ’);
module.exports = { 
  resolve: {
    //针对 Npm 中的第二方模块优先采用 jsnext :main 中指向的 ES6 模块化语法的文件 
    mainFields: [’jsnext:main’,’browser’,’main’]
  },
  plugins: [
    //开启 Scope Hoisting
    new ModuleConcatenationPlugin( ),]
}
```