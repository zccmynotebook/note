---
title: splitChunks
tags:  webpack splitChunks
---
  ```
最初，chunks是webpack内部父子关系图引用的各种代码；CommonsChunkPlugin用于避免重复依赖，但是优化还不够；
webpakc4用optimization.splitChunks取代了它；

默认只影响按需加载的chunks,因为改变了原始chucks会影响html里script文件；
webpack会基于以下条件自动分块：
1.可以共享的新的chunk或者引用来自node_modules文件的模块；
2.大于30kb的块(压缩代码前)
3.按需加载的模块，并行请求的最大数目<=5
4.初始化页面时的请求最大并行请求数<=3

当满足最后2个条件时，webpack有可能受限于包的最大数量值，生成的代码体积往上增加；
```
## Configuration
```
module.exports = {
  //...
  optimization: {
    splitChunks: {
      chunks: 'async',
      minSize: 30000,
      maxSize: 0,
      minChunks: 1,
      maxAsyncRequests: 5,
      maxInitialRequests: 3,
      automaticNameDelimiter: '~',
      name: true,
      cacheGroups: {
        vendors: {
          test: /[\\/]node_modules[\\/]/,
          priority: -10
        },
        default: {
          minChunks: 2,
          priority: -20,
          reuseExistingChunk: true
        }
      }
    }
  }
};
```
## 代码拆分
```
拆分代码有3种场景：
1.入口代码;
2.使用SplitChunksPlugin阻止重复代码；
3.动态加载；
```
> 表示从哪些chunks里面抽取代码，除了三个可选字符串值 initial、async、all 之外，还可以通过函数来过滤所需的 chunks；
 - async表示只从异步加载的模块（动态加载import()）里面进行拆分
 - initial表示只从入口模块进行拆分
 - all表示以上两者都包括
minSize：表示抽取出来的文件在压缩前的最小大小，默认为 30000；
maxSize：表示抽取出来的文件在压缩前的最大大小，默认为 0，表示不限制最大大小；
minChunks：表示被引用次数，默认为1；
maxAsyncRequests：最大的按需(异步)加载次数，默认为 5；
maxInitialRequests：最大的初始化加载次数，默认为 3；
automaticNameDelimiter：抽取出来的文件的自动生成名字的分割符，默认为 ~；
name：抽取出来文件的名字，默认为 true，表示自动生成文件名；
## cacheGroups
> cacheGroups 才是我们配置的关键。它可以继承/覆盖上面 splitChunks 中所有的参数值，
除此之外还额外提供了三个配置，分别为：test, priority 和 reuseExistingChunk。
- test: 表示要过滤的modules，默认为所有的 modules，可匹配模块路径或 chunk 名字，当匹配的是 chunk 名字的时候，其里面的所有 modules 都会选中；
- priority：表示抽取权重，数字越大表示优先级越高。因为一个 module 可能会满足多个 cacheGroups 的条件，那么抽取到哪个就由权重最高的说了算；
- reuseExistingChunk：表示是否使用已有的 chunk，如果为 true 则表示如果当前的 chunk 包含的模块已经被抽取出去了，那么将不会重新生成新的。
- enforce 可接受的值只能是Boolean。默认是false，这个字段告诉webpack忽略掉splitChunks.minSize, splitChunks.minChunks, splitChunks.maxAsyncRequests和splitChunks.maxInitialRequests选项，总是是当前group去分离chunks。