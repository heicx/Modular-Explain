1.  为什么要前端模块化？
     1.1.  全局变量命名冲突
     1.2.  依赖关系繁琐
     1.3.  业务逻辑模块化，页面内容碎片化。
     1.4.  合理抽象模块公共化。  
2.  前端模块化的好处有哪些？
     2.1  通过异步加载模块，提升页面性能。
     2.2  模块职责单一，规范模块命名，有利于代码的维护。
     2.3  跨环境共享。模块化遵循AMD或CMD规则。
     2.4  通过脚手架实现模块化后的代码版本管理。
3.  AMD 、 CMD 是什么，有何区别？
     对于没有Class，没有Package标准的Javascript脚本而言，他们是前端的模块化规范，各有优势。
     发展背景：伟大的CommonJS社区
     CommonJS社区由许多致力于标准化开源的牛人组建，CommonJS的前身原本叫ServerJS，在2009年-2010年期间推出了Modules/1.0规范后，在Node.js等环境下的应用中取得了很好的成就(http://wiki.commonjs.org/wiki/Special:WhatLinksHere/Modules/1.0 )

     在CommonJS规范的不断完善过程中，发展并衍生出三个派系：
     1.  Modules/1.x 流派( http://wiki.commonjs.org/wiki/Modules/1.1 )
     2.  Modules/Async 流派 Require.js ( http://www.requirejs.org )
     3.  Modules/2.0 流派 FlyScript Sea.js( http://www.seajs.org )  
     
     3.1  AMD( Asynchronous Module Definition )
     AMD是Require.js在推广过程中对模块定义的规范化而产出。
     AMD模块推崇依赖前置，所有模块在闭包模块load module时提前加载。
     闭包模块及依赖异步加载。
     强者的妥协，在CommonJS发展至Modules/2.0后，Require.js也支持了CMD-Modules/Wrapping 的规范写法。但原理上依然保持依赖前置。
     3.2  CMD( Common Module Definition )
     CMD是sea.js在推广过程中对模块定义的规范化而产出。
     CMD模块推崇依赖就近，用需加载。
     闭包模块异步执行，require引入依赖为同步执行，require.async解决异步问题。
     
4.  目前主流的模块化框架有哪些？
mod.js sea.js require.js flyscript( Modules/Wrappings )

5.  sea.js 与 require.js的比较

6.  sea.js的优势有哪些？
7.  sea.js API 只需看一遍
seajs.config 
seajs.use
define
require
require.async
exports
module.exports

8.  sea.js 应用场景与实战


