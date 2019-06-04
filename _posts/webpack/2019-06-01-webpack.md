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