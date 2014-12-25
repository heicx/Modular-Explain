title: 前端模块化与CSS3实战
speaker: heicx_sudo

[slide]

# 前端模块化与CSS3实战
## 演讲者: 黑晨轩

[slide]


# 为什么要前端模块化？
* 全局变量命名冲突
* 依赖关系繁琐
* 业务逻辑模块化，页面内容碎片化。
* 合理抽象模块公共化。  

[slide]

# 前端模块化的好处有哪些？
* 通过异步加载模块，提升页面性能。
* 模块职责单一，规范模块命名，有利于代码的维护。
* 跨环境共享。模块化遵循AMD或CMD规则。
* 通过脚手架实现模块化后的代码版本管理。

[slide]

# AMD 、 CMD 是什么，有何区别？
> 对于没有Class，没有Package标准的Javascript脚本而言，他们是前端的模块化规范，各有优势。
     
## 发展背景：伟大的CommonJS社区
> CommonJS社区由许多致力于标准化开源的牛人组建，CommonJS的前身原本叫ServerJS，在2009年-2010年期间推出了Modules/1.0规范后，在Node.js等环境下的应用中取得了很好的[成就](http://wiki.commonjs.org/wiki/Special:WhatLinksHere/Modules/1.0 )。

## 在CommonJS规范的不断完善过程中，发展并衍生出三大派系
> 在了解CommonJS派系之前，让我先看一下它有哪些规范？

[slide]

# CommonJS规范
> AMD( Asynchronous Module Definition )

* AMD是Require.js在推广过程中对模块定义的规范化而产出。
* AMD模块推崇依赖前置，所有模块在闭包模块load module时提前加载。
* 闭包模块的依赖异步加载。

> 强者的妥协，在CommonJS发展至Modules/2.0后，Require.js也支持了CMD-Modules/Wrapping 的规范写法。但原理上依然保持依赖前置。

---
> CMD( Common Module Definition )

* CMD是sea.js在推广过程中对模块定义的规范化而产出。
* CMD模块推崇依赖就近，用需加载。

> 闭包模块异步加载，闭包内的require依赖为同步执行，require.async解决异步问题，更灵活更方便。

[slide]


# CommonJS Modules的衍生派系
* [Modules/1.x]( http://wiki.commonjs.org/wiki/Modules/1.1 ) 流派 代表.Node.js
```javascript
     define(id, deps, factory); // Modules/Transport
     var a = require("./a") // 执行到此处时，a.js 才同步下载并执行
```
* [Modules/Async]( http://www.requirejs.org ) 流派  代表.Require.js
```javascript
     define(id?, deps, factory); // Modules/AMD
     define(["config"], function(config) {
          // 在这里，模块config 已经下载并执行好.
          var a = require("./a") // 此处仅仅是获取模块 a 的 exports.
          config.init(); // 此处通过config模块的依赖进行初始化.
     }
     
     define define(id?, deps?, factory); // Modules/AMD support Simplified CommonJS Wrapper
```
* [Modules/2.0]( http://www.seajs.org ) 流派 代表.FlyScript Sea.js
```javascript
     define(id?, deps?, factory); // Modules/CMD
     // FlyScript 的模块化语法糖.
     module.declare(function(require, exports, module){
        var a = require("a"); 
        exports.foo = a.name; 
     });
```

[slide]


# 目前主流的模块化框架有哪些？
* mod.js 
![mod.js](/images/mod.jpg "mod.js")
* sea.js
![sea.js](/images/sea.jpg "sea.js")
* require.js
![require.js](/images/require.jpg "require.js")
* flyscript( Modules/Wrappings )
![flyscript](/images/flyscript.jpg "flyscript")

[slide]

# sea.js 与 require.js的比较

[slide]

# sea.js的优势有哪些？

[slide]


7.  sea.js API 只需看一遍
* seajs.config 
* seajs.use
* define
* require
* require.async
* exports
* module.exports

[slide]

# sea.js 应用场景与实战

[slide]


