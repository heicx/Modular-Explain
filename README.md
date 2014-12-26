Modular-Explain
===============

# 前端模块化 前端项目构建脚手架 CSS3实战

----

# 为什么要前端模块化？
* 全局变量命名冲突
* 依赖关系繁琐
* 业务逻辑模块化，页面内容碎片化
* 合理抽象模块公共化

===

# 前端模块化的好处有哪些？
* 通过异步加载模块，提升页面性能。
* 模块职责单一，规范模块命名，有利于代码的维护
* 跨环境共享，模块化遵循AMD或CMD规则
* 通过脚手架实现模块化后的代码版本管理

===

# AMD 、 CMD 是什么，有何区别？
> 对于没有Class，没有Package标准的Javascript脚本而言，他们是前端的模块化规范，各有优势。

### 发展背景：伟大的CommonJS社区
> CommonJS社区由许多致力于标准化开源的牛人组建，CommonJS的前身原本叫ServerJS，在2009年-2010年期间推出了Modules/1.0规范后，在Node.js等环境下的运用中取得了突出的[成就](http://wiki.commonjs.org/wiki/Special:WhatLinksHere/Modules/1.0 )。


### 在CommonJS规范的不断完善过程中，发展并衍生出三大派系
* 在了解CommonJS派系之前，让我先看一下它有哪些规范？

===

# CommonJS规范
> AMD( Asynchronous Module Definition )

* AMD是Require.js在推广过程中对模块定义的规范化而产出
* AMD模块推崇依赖前置，所有模块在闭包模块load module时提前加载
* 闭包模块的依赖异步加载

> 强者的妥协，在CommonJS发展至Modules/2.0后，Require.js也支持了CMD-Modules/Wrapping 的规范写法。但原理上依然保持依赖前置。

===

# CommonJS规范
> CMD( Common Module Definition )

* CMD是sea.js在推广过程中对模块定义的规范化而产出
* CMD模块推崇依赖就近，用需加载

---
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

===

# CommonJS Modules的衍生派系
* [Modules/2.0]( http://www.seajs.org ) 流派 代表.FlyScript Sea.js
```javascript
     define(id?, deps?, factory); // Modules/CMD
     // FlyScript 的模块化语法糖.
     module.declare(function(require, exports, module){
        var a = require("a");
        exports.foo = a.name; 
     });
```

===

# 如何将现有的模块改为CMD模块
## 改造主流模块

```javascript
	if ( typeof module === "object" && module && typeof module.exports === "object" ) {
		// 如果当前已经是一个 基于 CommonJS的模块，直接通过exports接口暴露引用
		module.exports = jQuery;
	} else {
		// 设置全局
	    window.jQuery = window.$ = jQuery;

	    // 监测是否支持define关键字 监测是否支持amd或cmd规范
	    if ( typeof define === "function" && define.amd ) {
	        define( "jquery", [], function () { return jQuery; } );
	    }
	}
```     

===

# 如何将现有的模块改为CMD模块
### 改造普通模块，比如现有文件 util.js

```javascript
	window.util = window.util || {};
	util.addClass = function() {
	  window.css();
	};
	util.queryString = function() {};
```     

```javascript
	define(function(require, exports, module) {
	  // 引入依赖
	  var css = require('css');
	  util.addClass = function() {
	    css();
	  };
	  util.queryString = function() {};

	  // 暴露对应接口
	  module.exports = util;
	});
``` 

===

# 目前主流的模块化框架有哪些？
* mod.js
!["mod.js"](/images/fis.png)
* sea.js
!["sea.js"](/images/seajs.png)
* require.js
!["require.js"](/images/require.png)
* flyscript( Modules/Wrappings )

===

# sea.js API 只需看一遍
* seajs.config 
* seajs.use
* define
* require
* require.async
* exports
* module.exports

===

# sea.js 应用场景与实战

!["iautos"](/images/iautos.png)
!["iautos"](/images/iautos_2.png)

===

# 前端脚手架的使用 —— Grunt.js

!["grunt"](/images/grunt.png)


# Grunt.js 是什么？

* 前端javascript任务运行器
* 基于Node环境
* 通过npm平台安装搭建
* 庞大的生态圈，提供上千种插件
* 支持基于CommonJS规范的前端项目**自动化**打包

  
### 有哪些常用插件？
* grunt-cmd-transport
* grunt-contrib-uglify
* grunt-contrib-concat
* grunt-contrib-jshint
* grunt-contrib-clean
* grunt-contrib-cssmin
...

===

# Gruntfile.js 配置
  
```javascript
	var transport = require('grunt-cmd-transport');
	module.export  = function(grunt){
		grunt.initConfig({
		    	pkg: grunt.file.readJSON('package.json');
		    	transport : {
		    		options : {},
		    		app : {},
		    		...
			},
			cssmin : {
				expand: true,
				cwd: 'css/',
				src: '*.css',
				dest: 'dist/css/',
				ext: '.css'
			},
			concat ：{},
			uglify : {}
			...
	 	});

		grunt.loadNpmTasks('grunt-cmd-transport');
		grunt.loadNpmTasks('grunt-contrib-concat');
		grunt.loadNpmTasks('grunt-contrib-clean');
		grunt.loadNpmTasks('grunt-contrib-uglify');
		grunt.loadNpmTasks('grunt-contrib-cssmin');

		grunt.registerTask('build-libs', [ 'clean:libs','transport:libs', 'concat:libs','uglify:libs']);
		grunt.registerTask('build-app', ['clean:app','transport:app', 'concat:app', 'uglify:app']);
		grunt.registerTask('build-config', ['clean:config','transport:config', 'concat:config', 'uglify:config' ]);
		grunt.registerTask('build-css', ['clean:css','cssmin' ]);

		// grunt.registerTask("defualt", [ 'clean']);
	}
```
===

# Package.json 配置
  
```json
	{
		"name": "3g.iautos.cn",
		"version": "1.0.1",
		"author": "wukong_sudo",
		"static_path": "/static2015/",
	  	"spm": {
	    		"alias": {
			      "zepto": "js/libs/zepto.js",
			      "render": "js/config/render",
			      "base": "js/config/base",
			      "juicer": "js/libs/juicer",
			      "swipe": "js/libs/swiper"
		    	}
		},
	  	"devDependencies": {
			"grunt": "~0.4.5",
			"grunt-contrib-jshint": "^0.10.0",
		    	"grunt-contrib-uglify": "^0.5.1",
		    	"grunt-contrib-clean": "^0.6.0",
		    	"grunt-contrib-concat": "^0.5.0",
		    	"grunt-timestamp": "0.0.8",
		    	"grunt-contrib-cssmin": "^0.10.0"
		}
	}
```

