Modular-Explain
===============

# 前端模块化 前端项目构建脚手架 CSS3实战


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

强者的妥协，在CommonJS发展至Modules/2.0后，Require.js也支持了CMD-Modules/Wrapping 的规范写法。但原理上依然保持依赖前置。

> CMD( Common Module Definition )

* CMD是sea.js在推广过程中对模块定义的规范化而产出
* CMD模块推崇依赖就近，用需加载

> 闭包模块异步加载，闭包内的require依赖为同步执行，require.async解决异步问题，更灵活更方便。

===

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

## 改造普通模块，比如现有文件 util.js

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
!["mod.js"](/doc/images/fis.png)
* sea.js
!["sea.js"](/doc/images/seajs.png)
* require.js
!["require.js"](/doc/images/require.png)
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

!["iautos"](/doc/images/iautos.png)
!["iautos"](/doc/images/iautos_2.png)

===

# 前端脚手架的使用 —— Grunt.js

!["grunt"](/doc/images/grunt.png)

===

# Grunt.js 是什么？

* 前端javascript任务运行器
* 基于Node环境
* 通过npm平台安装搭建
* 庞大的生态圈，提供上千种插件
* 支持基于CommonJS规范的前端项目**自动化**打包

## 有哪些常用插件？
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

#CSS3实战

1.  什么是cs3？
  CSS3是CSS2的升级版本，3只是版本号，它在CSS2.1的基础上增加了很多强大的新功能。 目前主流浏览器chrome、safari、firefox、opera、甚至360都已经支持了CSS3大部分功能了，IE10以后也开始全面支持CSS3了。

  在编写CSS3样式时，不同的浏览器可能需要不同的前缀。它表示该CSS属性或规则尚未成为W3C标准的一部分，是浏览器的私有属性，虽然目前较新版本的浏览器都是不需要前缀的，但为了更好的向前兼容前缀还是少不了的。
-webkit-flex，-webkit-transform等等。



2.  css3能做什么？
  CSS3简化了前端开发工作人员的设计过程，使代码更简洁、更高效。可以极大的提高工作效率，打造更高级的用户体验，加快页面载入速度。使web应用的界面设计进入一个新的台阶。

选择器
  以前我们通常用class、 ID 或 tagname 来选择HTML元素，CSS3的选择器强大的难以置信。它们可以减少在标签中的class和ID的数量更方便的维护样式表、更好的实现结构与表现的分离。

圆角效果
  以前做圆角通常使用背景图片，或繁琐的元素拼凑，现在很简单了 border-radius 帮你轻松搞定。

块阴影与文字阴影
  可以对任意DIV和文字增加投影效果。

色彩
  CSS3支持更多的颜色和更广泛的颜色定义。新颜色CSS3支持HSL ， CMYK ，HSLA and RGBA。

渐变效果
  以前只能用Photoshop做出的图片渐变效果，现在可以用CCS写出来了。IE中的滤镜也可以实现。

个性化字体
  网页上的字体太单一？使用@Font-Face 轻松实现定制字体。

多背景图
  一个元素上添加多层背景图片。

边框背景图
  边框应用背景图片。

变形处理
  你可以对HTML元素进行旋转、缩放、倾斜、移动、甚至以前只能用JavaScript实现的强大动画。

多栏盒布局
  可以让你不用使用多个div标签就能实现多栏布局。浏览器解释这个属性并生成多栏，让文本实现一个仿报纸的多栏结构。

媒体查询
  针对不同屏幕分辨率，应用不同的样式。


3.  边框
  圆角效果：border-radius
 /* 所有角都使用半径为10jpx的圆角 */
border-radius:10px;
 /* 四个半径值分别是左上角、右上角、右下角和左下角，顺时针 */
border-radius:5px 4px 3px 2px;

总结：实心圆的圆角度数最大为高度的一半；如果为实心上半圆，设定1,2位属性即可。

  阴影效果：box-shadow
box-shadow是向盒子添加阴影。支持添加一个或者多个。

/* 支持内外阴影写法，1，2位为x，y轴必填偏移量，3位表示阴影半径，4位表示颜色，默认黑色，5位表示投影方式，默认为外阴影 */
box-shadow: 4px 2px 4px #ccc inset, 4px 2px 2px #fff; 

  边框图片：border-image
顾名思义就是为边框应用背景图片，它和我们常用的background属性比较相似。例如：
  background:url(xx.jpg) 10px 20px no-repeat;

/* 如果应用了边框图片，图片会自动飞切割分成四等分，分别用于四个边 
    如果一张图片像素为210*210，图中边框最小有效边框高度为20px，得出
    border-image:url(xx.jpg) 20 repeat/round/strech;
    末位参数为图片延伸方式，默认为stretch。
*/

  border-image:url(xx.jpg) 50 repeat/round/strech;

4.  颜色
  RGBA
  RGB是一种色彩标准，是由红(R)、绿(G)、蓝(B)的变化以及相互叠加来得到各式各样的颜色。RGBA是在RGB的基础上增加了控制alpha透明度的参数。
/* 末位参数为透明度，区间0-1 */
  background-color:rgba(255,255,255,0.5);

总结：基本与rgb的用法一致。

  
渐变色背景: linear-gradient
  /* 第一参数表示渐变方向，从左上角到又下角为 to right bottom */
  background-image: linear-gradient(to right bottom, red, blue);
  

5.  文字与字体
text-overflow 与 word-wrap

text-overflow表示是否使用一个省略号标示对象内的文本溢出。
word-wrap表示当前行超过容器宽度时是否断行。

/* clip表示剪切  ellipsis表示省略标记 */
text-overflow: clip | ellipsis

/* normal表示连续文本换行  break-word表示内容在边界内换行( 主要用于单词或URL地址换行 ) */
word-wrap: normal | break-word

注意：text-overflow在使用过程中还需要定义文本在一行内显示(white-space:nowrap)及溢出内容为隐藏(overflow:hidden)，只有这样才能实现溢出文本显示省略号的效果。

@font-face

  @font-face能够加载服务器端的字体文件，让浏览器端可以显示用户电脑里没有安装的字体。 

@font-face{
     font-family: 字体名称;
     src: 字体文件在服务器上的相对或绝对路径;
}

总结：字体库文件需要正确引入并文件格式正确可用, otf格式。

text-shadow

/*
     X-Offset：表示阴影的水平偏移距离，正值时阴影向右偏移，反之向左。
     Y-Offset：表示阴影的垂直偏移距离，正值时阴影向下偏移，反之向上。
     Blur: 表示阴影模糊程度，数值越大越模糊，反之清晰。默认为0。
     Color：表示阴影颜色，可以使用rgba色。
*/
text-shadow: 2px 2px 0px rgba(100,200,0,0.6);


6.  背景
background-origin
background-origin可以设置元素背景图片的原始起始位置

/* 参数分别表示背景图片是从边框，还是内边距（默认值），或者是内容区域开始显示。 */
background-origin ： border-box | padding-box | content-box; 

eg: 

background:#ccc url(xxx.png) no-repeat;
background-origin: content-box;

总结：背景图片设置必须为no-repeat，反之background-origin的设置无效。


background-clip
background-clip用来将背景图片做适当的裁剪以适应实际需要。

/* 参数分别表示从边框、或内填充，或者内容区域向外裁剪背景。no-clip表示不裁切，和参数border-box显示同样的效果。backgroud-clip默认值为border-box。 */
background-clip ： border-box | padding-box | content-box | no-clip 

background-size
background-size用来设置背景图片的大小，以长度值(长宽像素值对)或百分比显示，还可以通过cover和contain来对图片进行伸缩。  

/*   auto：默认值，不改变背景图片的高度和宽度 
     长度值：成对出现，如200px 50px，如果只写一个值，将按照图片宽度等比缩放
     百分比：0-100%之间任何值。
     cover：等比缩放以填充满整个容器。
     contain：等比缩放至某一边紧贴容器边缘。
*/
background-size: auto | <长度值> | <百分比> | cover | contain 

multiple backgrounds
multiple backgrounds：多重背景，也就是CSS2里background的属性外加origin、clip和size组成的新background的多次叠加，缩写时为用逗号隔开的每组值；用分解写法时，如果有多个背景图片，而其他属性只有一个（例如background-repeat只有一个），表明所有背景图片应用该属性值。 

background ： [background-color] | [background-image] | [background-position][/background-size] | [background-repeat] | [background-attachment] | [background-clip] | [background-origin],…

 总结：用逗号隔开每组background的缩写值；如果有size值，需要紧跟position并且用"/"隔开；分解写法时，background-color只能设置一个。


7.  选择器
8.  变形与动画
9.  布局样式
10. Media Queries 与 响应设计
