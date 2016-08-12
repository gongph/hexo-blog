---
title: Gulp入门教程
date: 2016-07-06 15:48:22
tags:
- gulp
- npm
- 前端技术
categories:
- 前端技术
- gulp
---

### 来自官方的介绍
 基于流的自动化构建工具。利用 Node.js流的威力，你可以快速构建项目并减少频繁的 IO 操作。

### 来自其他解释
 gulp是前端开发过程中对代码进行构建的工具，是自动化项目的构建利器；她不仅能对网站资源进行优化，而且在开发过程中很多重复的任务能够使用正确的工具自动完成；使用她，我们不仅可以很愉快的编写代码，而且大大提高我们的工作效率。
 <!-- more -->

 gulp是基于Nodejs的自动任务运行器， 她能自动化地完成 javascript/coffee/sass/less/html/image/css 等文件的的测试、检查、合并、压缩、格式化、浏览器自动刷新、部署文件生成，并监听文件在改动后重复指定的这些步骤。在实现上，她借鉴了Unix操作系统的管道（pipe）思想，前一级的输出，直接变成后一级的输入，使得在操作上非常简单。

 gulp 和 grunt 非常类似，但相比于 grunt 的频繁 IO 操作，gulp 的流操作，能更快地更便捷地完成构建工作。

 ### 正文从这里开始
 在学习gulp开始之前，假设你电脑上已经安装了<code>node.js</code> <code>npm</code> .

 #### 全局安装gulp

 ```js
 npm install gulp -g
 ```

全局安装是为了执行gulp一系列任务命令。

#### 一个示例
本教程以压缩js，css为例，演示一下gulp的使用过程。
##### 新建项目
```cmd
mkdir gulp-intro
```

##### 初始化
```cmd
cd gulp-intro && npm init
```
目录结构如下：
```
gulp-intro
| —— app 
|    | —— js 
|    | —— css
| —— package.json
```

##### 本地安装gulp
```cmd
 gulp-intro > npm install gulp --save-dev
```
本地安装gulp是为了调用gulp插件

在项目的根目录新建 <code>gulpfile.js</code> 文件。
说明：gulpfile.js是gulp项目的配置文件，是位于项目根目录的普通js文件（其实将gulpfile.js放入其他文件夹下亦可）。

##### 安装 gulp-uglify 插件
<code>gulp-uglify</code> 是一款js压缩插件。
```cmd
gulp-intro > npm install gulp-uglify --save-dev
```

##### 安装 gulp-minify-css 插件
<code>gulp-minify-css</code> 是一款css压缩插件
```cmd
gulp-intro > npm install gulp-minify-css --save-dev
```

##### 安装 gulp-rename 插件
从插件名字上就能看出来，这是一个重命名插件。我们希望压缩后的js, css文件都以 <code>.min</code> 结尾，即： xxx.min.js  / xxx.min.css
```cmd
gulp-intro > npm install gulp-rename --save-dev
```

##### 编写测试代码

<code>js/gulp-intro.js</code>
```js
/*!
 * Test gulp v1.0.0
 * (c) 2016 gongph
 */
function gulpJsUglify () {
    console.log('test gulp js\'s uglify');
}
```

<code>css/gulp-intro.css</code>
```css
/* Test gulp minify */
h1 , h2{
    border-bottom: 1px solid #EFEAEA;
    padding-bottom: 3px;
    color: green;
}

.book .book-body .page-wrapper .page-inner section.normal {
    min-height:350px;
    margin-bottom: 30px;
}
```
<code>gulpfile.js</code>
```js
var gulp = require('gulp'), 
    uglify = require('gulp-uglify'), //js压缩插件依赖
    minifycss = require('gulp-minify-css'), //css压缩插件依赖
    rename = require('gulp-rename'); // 文件重命名插件依赖

// js压缩任务
gulp.task('jsmin', function () {
    gulp.src('app/js/gulp-intro.js') // 需要压缩的js文件
        .pipe(rename({suffix: '.min'})) // 指定后缀
        .pipe(uglify())// 执行压缩
        .pipe(gulp.dest('app/js')); //文件压缩后输出路径
});

// css压缩任务
gulp.task('cssmin', function () {
    gulp.src('app/css/gulp-intro.css') //需要压缩的css文件
        .pipe(rename({suffix: '.min'})) //指定后缀
        .pipe(minifycss()) // 执行压缩
        .pipe(gulp.dest('app/css')); //文件压缩后输出路径
});

// 通过默认任务来进行操作
gulp.task('default', ['jsmin', 'cssmin']);

```

至此，所有的测试代码已经编写完毕。让我们来测试一下gulp强大之处。

##### 执行gulp任务
通过命令行 <code>gulp 任务名称</code> 可以执行单个任务，比如执行 <code>gulp jsmin</code> 来进行js文件压缩。

当我们执行 <code>gulp default</code> 或 <code>gulp</code> 命令时，gulp会执行默认任务，即：
```js
gulp.task('default', ['jsmin', 'cssmin']);
```

我们来执行 <code>gulp</code> 来同时压缩js和css文件。

```cmd
gulp-intro > gulp
[17:11:24] Using gulpfile ~\gulp-intro\gulpfile.js
[17:11:24] Starting 'jsmin'...
[17:11:24] Finished 'jsmin' after 16 ms
[17:11:24] Starting 'cssmin'...
[17:11:24] Finished 'cssmin' after 6.01 ms
[17:11:24] Starting 'default'...
[17:11:24] Finished 'default' after 13 μs
```

ok, 让我们来看下现在的目录结构：
```
gulp-intro
| —— app 
|    | —— js
|        | —— gulp-intro.js
|        | —— gulp-intro.min.js
|    | —— css
|        | —— gulp-intro.css
|        | —— gulp-intro.min.css
| —— node_modules
    | —— gulp 
    | —— gulp-minify-css 
    | —— gulp-uglify 
    | —— gulp-rename
| —— package.json 
| —— gulpfile.js
```

查看下压缩后的文件是不是很神奇！

enjoy it ~ 

我是一枚喜欢玩前端的后端攻城狮，技术有限，代码如有错误之处，还请大神劈正！