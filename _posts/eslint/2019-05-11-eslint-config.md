---
title: eslint config 翻译
tags: eslint config 翻译
---

# 配置
```
eslint 是完全可配置的，意味着你可以关掉所有的验证规则，只要基本的语法是正确的，或者在项目中混合使用你定制的规则和绑定的规则。有2种方法配置eslint:
1.注释模式---使用js注释，直接将配置信息嵌入js文件
2.配置文件---使用js,json,yaml文件指定整个目录及其子目录的配置信息。可以是.eslintrc.*(*代表不同类型的文件后缀)或者在package.json中添加eslintConfig字段，这2种方法，eslint都会自动查找读取，或者也可以在命令行中指定一个配置文件；

如果在home目录下有一个配置文件（~/），eslint如果找不到其他的配置文件就会用这个；
下面这些内容都可以配置：

环境---脚本在哪些环境下运行，每一个环境都有一些预定义的全局变量；
全局--额外的全局变量，脚本咋执行时可以访问
规则--哪些规则是开启的，什么样的错误级别
```
## 规定解析选项
```
eslint 允许指定你想支持的js语言。默认是es5语法。你可以覆盖这个设置，通过使用解析选项以允许其他的es版本以及jsx。

注意，支持JSX语法不代表支持react,react使用jsx特定的语义，eslint识别不了。如果使用react建议使用eslint-plugin-react.
同样，支持es6不代表完全支持新的es6全部内容。对于es6语法，使用{"parseOptions":{"ecmaVersion":6}},新增的es6全局变量，
使用{"env":{"es6":true}},使用{"env":{"es6":true}}会自动开启es6语法，但是反之不行。.eslintrc.*文件中的parserOptions属性用于设置解析选项，
下面是可选的内容：

ecmaVersion-设为3，5，6，7，8，9，10指定需要使用的es语法版本，也可以使用年份，2015对应6
sourceType-"script" (default) or "module"(代码在esmodules里时使用)
ecmaFeatures-一个对象用于配置额外的特性，可能会经常使用的内容：
  globalReturn - 在全局作用域可以使用return语句
  impliedStrict - 允许使用全局的严格模式
  jsx - 使用jsx语法
```
设置parse options帮助eslint判断什么是解析错误。
下面是一个.eslintrc.json例子：
```
{
    "parserOptions": {
        "ecmaVersion": 6,
        "sourceType": "module",
        "ecmaFeatures": {
            "jsx": true
        }
    },
    "rules": {
        "semi": "error"
    }
}
```
###  parser 具体说明
```
eslint 默认使用 Espree作为解析器，可以更改parser，但是要满足下面的条件：
1.必须是一个本地安装的npm模块
2.必须兼容Esprima接口
3.必须产生兼容Esprima的AST和Etoken对象
即使上述条件都满足，也不能保证外部的解析器能正常和eslint配合工作；
更换解析器的语法：
{
    "parser": "esprima",
    "rules": {
        "semi": "error"
    }
}
Esprima，abel-ESLint ，@typescript都是可以和eslint兼容的；
```
###  环境说明
```
环境预定义了一些全局变量，下面是可以使用的环境：
browser - browser global variables.
node - Node.js global variables and Node.js scoping.
commonjs - CommonJS global variables and CommonJS scoping (use this for browser-only code that uses Browserify/WebPack).
shared-node-browser - Globals common to both Node.js and Browser.
es6 - enable all ECMAScript 6 features except for modules (this automatically sets the ecmaVersion parser option to 6).
worker - web workers global variables.
amd - defines require() and define() as global variables as per the amd spec.
mocha - adds all of the Mocha testing global variables.
jasmine - adds all of the Jasmine testing global variables for version 1.3 and 2.0.
jest - Jest global variables.
phantomjs - PhantomJS global variables.
protractor - Protractor global variables.
qunit - QUnit global variables.
jquery - jQuery global variables.
prototypejs - Prototype.js global variables.
shelljs - ShellJS global variables.
meteor - Meteor global variables.
mongo - MongoDB global variables.
applescript - AppleScript global variables.
nashorn - Java 8 Nashorn global variables.
serviceworker - Service Worker global variables.
atomtest - Atom test helper globals.
embertest - Ember test helper globals.
webextensions - WebExtensions globals.
greasemonkey - GreaseMonkey globals.

这些环境互相不排斥，可以一次定义多个；
环境可以在文件里指定或者配置文件里，也可以使用--env 命令行
注释的格式/* eslint-env node,macha */,允许使用node ,macha的环境；
配置文件实例：
{
    "env": {
        "browser": true,
        "node": true
    }
}
package.json实例：
{
    "name": "mypackage",
    "version": "0.0.1",
    "eslintConfig": {
        "env": {
            "browser": true,
            "node": true
        }
    }
}
YAMLA实例
  env:
    browser: true
    node: true

 如果想使用插件里的环境，要先使用plugins数组指定插件的名字，使用不带前缀的插件名/跟着环境名，格式 插件名/环境名，例如：
 {
    "plugins": ["example"],
    "env": {
        "example/custom": true
    }
}
package.json实例：
{
    "name": "mypackage",
    "version": "0.0.1",
    "eslintConfig": {
        "plugins": ["example"],
        "env": {
            "example/custom": true
        }
    }
}
YAMLA实例
  plugins:
    - example
  env:
    example/custom: true
```
Globals具体说明
```
在同一个文件里使用了没有定义的规则会警告，如果使用了全局的变量就不会；可以使用下面的方法定义全局变量：
1.在js文件内嵌：
/* global var1, var2 */
如果想让变量可写，这样定义
/* global var1:writable, var2:writable */
2.package.json
{
    "globals": {
        "var1": "writable",
        "var2": "readonly"
    }
}
3.yaml
globals:
    var1: writable
    var2: readonly

使用off值关闭对应的功能，下面的实例，可以使用大部分的es2015全局变量，但是不能使用promise:
{
    "env": {
        "es6": true
    },
    "globals": {
        "Promise": "off"
    }
}
```
###  插件配置
```
eslint支持第三方插件，但是使用前要先安装；
在配置文件里使用plugins字段，值是数组格式，或者列表形式，eslint-plugin-前缀可以省略；
{
    "plugins": [
        "plugin1",
        "eslint-plugin-plugin2"
    ]
}
yaml:
  plugins:
    - plugin1
    - eslint-plugin-plugin2
```
###  规则配置
```
ESLINT有大量的规则，你可以使用下面的值修改规则：
"off" or 0 - turn the rule off，关闭规则
"warn" or 1 - turn the rule on as a warning (doesn’t affect exit code)，警告
"error" or 2 - turn the rule on as an error (exit code is 1 when triggered)，报错

实例：
1.js内嵌：
/* eslint eqeqeq: "off", curly: "error" */
如果一个规则有多余的选项，可以使用数组指定，例如
/* eslint quotes: ["error", "double"], curly: 2 */
double也是应用到quotes上的，数组的第一个选项一般都是指定规则的研制程度，
2.package.json
{
    "rules": {
        "eqeqeq": "off",
        "curly": "error",
        "quotes": ["error", "double"]
    }
}
3.yaml
rules:
  eqeqeq: off
  curly: error
  quotes:
    - error
    - double

使用外部plugin
/* eslint "plugin1/rule1": "error" */

{
    "plugins": [
        "plugin1"
    ],
    "rules": {
        "eqeqeq": "off",
        "curly": "error",
        "quotes": ["error", "double"],
        "plugin1/rule1": "error"
    }
}   

```
### 内嵌格式，禁用规则
```
/* eslint-disable */
/* eslint-disable no-alert, no-console */

```
### 一组文件禁用一些规则
```
在配置文件里，使用overrides和files将一组文件禁用某些指定规则；
{
  "rules": {...},
  "overrides": [
    {
      "files": ["*-test.js","*.spec.js"],
      "rules": {
        "no-unused-expressions": "off"
      }
    }
  ]
}
```
### 添加共享设置
```
settings对象里设置的规则会被每个应用执行的规则上；
{
    "settings": {
        "sharedData": "Hello"
    }
}
  settings:
    sharedData: "Hello"
```
### 使用配置文件
```
有2种方法使用配置文件：
1.使用.eslintrc.* and package.json文件：
eslint会在要检查的文件目录下自动查询.eslintrc.* and package.json文件，随后继续再递归查找父目录直到文件系统的根目录(除非指定root:true，
eslint查到含有此字段的配置不再继续向上查找)；
当你想给一个项目的不同部分使用不同的配置时，或者当你想让其他人直接使用eslint而不用传递配置文件时，这个选项是有用的。
2.在命令行使用-c选项把文件位置传给cli，保存文件；
eslint -c myconfig.json myfiletotest.js
如果你想使用一个配置文件，并且忽略掉所有的.eslintrc.*文件，使用-c --no-eslintrc;
```
### 配置文件格式
```
JavaScript -  .eslintrc.js 导出一个包含配置的对象
YAML - use .eslintrc.yaml or .eslintrc.yml to define the configuration structure.
JSON - use .eslintrc.json to define the configuration structure. ESLint’s JSON files also allow JavaScript-style comments.
Deprecated - use .eslintrc, which can be either JSON or YAML.
package.json - create an eslintConfig property in your package.json file and define your configuration there
在同一个目录下，如果同时存在多个配置，只使用一个，优先级如下：
.eslintrc.js
.eslintrc.yaml
.eslintrc.yml
.eslintrc.json
.eslintrc
package.json
```
### 配置的层次和层叠
```
如果在你的home目录下有个配置文件，当前项目会在本地找不到配置文件的情况下使用这个默认配置，会应用到每一个目录下，包括第三方代码，会引起一些问题，
默认，eslint会一直查找直到根目录，为了防止上述情况发生，在eslintConfig中指定{root:true}阻止继续上溯；

完整的配置层次，优先级由高到低如下：
1.Inline configuration
/*eslint-disable*/ and /*eslint-enable*/
/*global*/
/*eslint*/
/*eslint-env*/
2.Command line options (or CLIEngine equivalents):
--global
--rule
--env
-c, --config
3.Project-level configuration:
  1）.eslintrc.* or package.json file in same directory as linted file
  2）Continue searching for .eslintrc and package.json files in ancestor directories (parent has highest precedence, then grandparent, etc.), up to and including the root directory or until a config with "root": true is found.
4. (1) 到 (3)都没有, fall back to a personal default configuration in ~/.eslintrc.

```
### 扩展的配置文件
```
一个配置文件可以从基础配置继承一些配置
extends属性的值可以是配置的字符串，或者数组形式的字符串；
rules属性可以以以下形式继承/覆盖规则集：
1.允许额外的规则
2.根据严重性，在不更改其选项的情况下更改继承规则
  Base config: "eqeqeq": ["error", "allow-null"]
  Derived config: "eqeqeq": "warn"
  Resulting actual config: "eqeqeq": ["warn", "allow-null"]
3.覆盖基础配置中的规则：
Base config: "quotes": ["error", "single", "avoid-escape"]
Derived config: "quotes": ["error", "single"]
Resulting actual config: "quotes": ["error", "single"]

```
### 使用eslint:recommended
```
使用推荐的配置时，在升级eslint版本后，可以使用--fix可以直接修复；
eslint --init 可以创建一个基于推荐配置的配置文件；
js格式的实例：
module.exports = {
    "extends": "eslint:recommended",
    "rules": {
        // enable additional rules
        "indent": ["error", 4],
        "linebreak-style": ["error", "unix"],
        "quotes": ["error", "double"],
        "semi": ["error", "always"],

        // override default options for rules from base configurations
        "comma-dangle": ["error", "always"],
        "no-cond-assign": ["error", "always"],

        // disable rules from base configurations
        "no-console": "off",
    }
}
```
### 使用可共享配置包
```
共享配置包是一个npm包，输出一个配置对象；要先安装；
extends属性可以省略包的eslint-config-前缀；
eslint --init命令会创建一个配置文件，你可以使用一些流行的风格，如eslint-config-standard；
实例：
extends: standard
rules:
  comma-dangle:
    - error
    - always
  no-empty: warn
```
### 使用插件中的配置
```
这个插件导出的也是规则，也要注意先安装后使用；
plugins属性可以省略包的eslint-config-前缀；
extends的值由以下部分组成：
1.plugin: 
2.省略前缀的包名
3./
4.配置名，如recommended
{
    "plugins": [
        "react"
    ],
    "extends": [
        "eslint:recommended",
        "plugin:react/recommended"
    ],
    "rules": {
       "no-set-state": "off"
    }
}
```
### 使用配置文件
```
extends属性的值可以是绝对路径，也可以是相对于配置文件的相对路径；

```
### 忽略文件和目录
```
在项目的根目录下建一个.eslintignore,罗列需要忽略检查的文件；
1.以#开头的被认为是注释，不会影响
2.路径是相对.eslintignore的路径，或者当前工作目录
3.以！开头的行是否定模式，会重新包含在前面否定的匹配
4.和.gitignore规范是一致的；
```