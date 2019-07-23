# Gulp 
gulp 基础应用

## 1.gulp介绍
gulp是前端开发过程中一种基于流的代码构建工具，是自动化项目的构建利器；
她不仅能对网站资源进行优化，而且在开发过程中很多重复的任务能够使用正确的工具自动完成；
使用她，不仅可以很愉快的编写代码，而且大大提高我们的工作效率。

项目构建是指项目上线之前对项目源代码进行一系列处理，使其以最佳的形式运行于线上服务器。
常见处理任包括以下几方面：

1. 模块化开发可以实现功能的复用并解决模块间的依赖关系，但带来好处的同时也使得功能代码的碎片化（若干文件）程度增加。
2. 使用less、sass等预处理器，可以降低CSS的维护成本，最终需要将这些预处理器编译成css文件；
3. 对静态资源（css、js、html、images）压缩合并可以提升网页打开速度，提高性能；

以上任务完如果完全靠手动来完成是非常耗时耗力的且容易出错，实际开发通常借助构建工具来实现。
所谓构建工具是指通过一系简单配置就可以帮我们实现合并、压缩、校验、预处理等一系列任务的软件工具。
常见的构建工具包括：`Grunt、Gulp、F.I.S（百度出品）、webpack`等。

Gulp是基于Nodejs开发的一个构建工具，借助gulp插件可以实现不同的构建任务，
其以简洁的配置和卓越的性能成为目前主流的构建工具。

```
## npm安装命令解释
- npm i --production 只安装"dependencies"里面的内容
- npm i -S 安装到"dependencies"
- npm i -D 安装到"devDependencies"
```
## 2.文档

- 官方：http://gulpjs.com/
- 中文官网：http://www.gulpjs.com.cn/
- npm：https://www.npmjs.com/package/gulp
- Github：https://github.com/gulpjs/gulp
- Gitbook：https://wizardforcel.gitbooks.io/gulp-doc/content/2.html

---

## 3.环境

> 官方文档：https://github.com/gulpjs/gulp/blob/master/docs/getting-started.md

一：Install the gulp command

在项目中使用 gulp 首先需要确保全局有 gulp-cli 环境，如果有就不需要执行下面的命令了。

```bash
# npm install --global gulp-cli
yarn global add gulp-cli
```

二：Install gulp in your devDependencies

```bash
# npm install --save-dev gulp
yarn add -D gulp
```

三：Create a file called gulpfile.js in your project root with these contents:

```bash
var gulp = require('gulp');

gulp.task('default', function() {
  console.log('hello gulp')
})
```

四：Test it out: Run the gulp command in your project directory:

```bash
gulp
```

---

## 4.API文档

> 官方文档：https://github.com/gulpjs/gulp/blob/master/docs/API.md

- gulp.task
- gulp.src
- gulp.dest
- gulp.watch

### gulp.task(name [, deps] [, fn])

作用：定义各种不同的任务

- gulp.task(name, fn)
- gulp.task(name, deps, fn)
- gulp.task(name, fn(cb))
- gulp.task(name, deps, fn(cb))

一：普通任务

```js
gulp.task('a', function () {
  console.log('1 aaa')
})

gulp.task('b', function () {
  console.log('2 bbb')
})
```

二：任务之间的依赖

```js
gulp.task('a', function (cb) {
  setTimeout(function () {
    console.log('1 aaa')
    cb()
  }, 1000)
})

// b 任务依赖的 a 任务中的回调函数如果不调用，b 任务是不会执行的
gulp.task('b', ['a'], function () {
  console.log('2 bbb')
})
```

三：gulp 流控制

```js
gulp.task('a', function () {
  // 当任务中是一个 gulp 流的时候则需要通过 return 来保证依赖中的执行顺序
  return gulp.src()
    .pipe()
    // ...
})

gulp.task('b', ['a'], function () {
  // doSomething
})
```

### gulp.src(globs[, options])

> gulp教程之gulp中文API：http://www.ydcss.com/archives/424

作用：根据路径（字符串或数组）读取需要构建的资源

#### globs

需要处理的源文件匹配符路径。

类型(必填)：String or StringArray，通配符路径匹配示例：

- `src/a.js` 指定具体文件；
- `*` 匹配所有文件    例：`src/*.js` (包含src下的所有js文件)；
- `**` 匹配0个或多个子文件夹    例：`src/**/*.js` (包含src的0个或多个子文件夹下的js文件)；
- `{}` 匹配多个属性    例：`src/{a,b}.js` (包含a.js和b.js文件)  src/*.{jpg,png,gif}(src下的所有jpg/png/gif文件)；
- `!` 排除文件    例：`!src/a.js` (不包含src下的a.js文件)；

#### options.base

options.base：类型：String  设置输出路径以某个路径的某个组成部分为基础向后拼接，具体看下面示例：
- 可以为数组 gulp.src([读取的文件，读取的文件/!读取文件（排除文件）])

```js
保留读取路径
gulp.task('static', ['clear'], function () {
  return gulp.src(paths.staticPath, {
      base: './src/' // 这里，你想保留哪一级路径，就配置该路径的上一级目录即可
    })
    .pipe(gulp.dest(paths.dist))
})

```

```js
gulp.src('client/js/**/*.js') 
  .pipe(minify())
  .pipe(gulp.dest('build'))  // Writes 'build/somedir/somefile.js'
 
gulp.src('client/js/**/*.js', { base: 'client' })
  .pipe(minify())
  .pipe(gulp.dest('build'))  // Writes 'build/js/somedir/somefile.js'
```

### gulp.dest(path[, options])

作用：构建任务完成后资源存放的路径

### gulp.watch(glob[, opts], tasks)

> 监视指定资源的改动，然后可以调用响应的任务处理

### gulp.watch(glob [, opts, cb])

---

## 常用插件

|          插件名称          |                        作用                        |                     链接              |
|----------------------------|----------------------------------------------------|-------------------------------------|
| del                        | 删除文件或文件夹                                   | https://www.npmjs.com/package/del     |
| gulp-less                  | 编译LESS文件                                       |                                       |
| gulp-rname                 | 重命名文件                                         |                                        |
| gulp-imagemin              | 图片压缩                                           |                                        |
| gulp-uglify                | 压缩Javascript                                     |                                       |
| gulp-concat                | 合并 js 文件                                       |                                       |
| gulp-concat-css            | 合并 css 文件                                      |                                       |
| gulp-cssnano               | 压缩 css                                           |                                       |
| gulp-htmlmin               | 压缩HTML                                           |                                         |
| gulp-nunjucks              | 模板引擎                                           | https://github.com/sindresorhus/gulp-nunjucks |
| gulp-rev                   | 添加版本号                                         |                                              |
| gulp-rev-collector         | 内容替换                                           |                                             |
| gulp-useref                | gulp-if                                            |                                             |
| gulp-load-plugins          | 依赖自动加载                                       |                                               |
| gulp-useref                | 自动合并打包处理                                   |                                              |
| gulp-wrap                  | 包装内容                                           |                                             |
| gulp-angular-templatecache | AngularJS 模板缓存                                 |                                             |
| browser-sync               | 和 gulp 配合使用实现文件改变执行某个任务后自动刷新 |                                                 |
| yargs                      | 获取命令行参数                                     |                                              |
| gulp-if                    | 根据判断执行某个插件                               |                                               |
| http-proxy-middleware      | http 代理插件                                      |                                              |
|                            |                                                    |                                             |

## 5.gulp 实战之：高级写页面

实现 HTML 模板功能，例如公共 HTML 头部和底部，提供可维护性，
以及实现 HTML 自动压缩，css 压缩，js 压缩，或者合并。

## 6.gulp实例

一： 全局安装 gulp：

```bash
$ npm install --global gulp
```
二： 作为项目的开发依赖（devDependencies）安装：

```bash
$ npm install --save-dev gulp

```
三： 在项目根目录下创建一个名为 gulpfile.js 的文件：

```bash
var gulp = require('gulp');
 
gulp.task('default', function() {
  // 将你的默认的任务代码放在这
});
```

四： gulp基本命令
- gulp.task(任务名,依赖项,callback)

  新建任务，有依赖项，先执行依赖项中的方法/任务
  
  ```
  gulp.task('b',function(callback){
  
  })
  
  gulp.task('a',['b','c'],function(){
  
  })
  
  执行a时，首先依赖b和c，在异步时候当定义callback时候，必须等到b执行完 才能执行a，实现流程控制
  ```

- gulp.src

  用来读文件
  
  ```bash
  读取除了index.css以外的所有css文件
  gulp.src(['./css/**/*.css','./css/index.css'])
  
  代表只读取main.css和index.css文件
  gulp.src(['./css/main.css','./css/index.css'])
  
  ```
    
- gulp.dest

  用来写文件

```bash
var gulp=require('gulp')
gulp.task('copy-index',function(){
  gulp.src('./src/index.html')
    .piple(gulp.dest('./dist/))
})

```
- **/*.js

通配符指该目录下所有js

五： gulp-less

  用来编译less文件
  

```bash

var gulp = require('gulp')
var less = require('gulp-less')

gulp.task('less', function () {
  gulp.src('./src/less/**/*.less') // 产生一坨数据
    .pipe(less()) // 通过 pipe 将上一步 src 读取到的数据进入 less 插件，less 插件将处理过后的数据再次吐出来
    .pipe(gulp.dest('./dist/css')) // 这里通过 pipe 管道接收上一个 less 吐出来的数据，经由 gulp.dest() 写入指定的目录路径
})

// 该任务用来监视某个文件，当文件发生变化，则执行响应的任务
gulp.task('watch-less', ['less'], function () {
  gulp.watch('./src/less/**/*.less', ['less']) //监控这些less文件  当发生改变时 执行less方法
})

```

六： gulp-cssnano

  压缩css（用法同理less）
  
七： gulp-del

  删除文件（每当提交任务时，需要删除dist文件，防止冲突）
  
```bash
  var gulp=require('gulp')
  var cssnano = require('gulp-cssnano')
  var del=require('gulp-del')
  
  gulp.task('clear',function(callback){
    del('./dist/')
      .then(function(){
        callback()
      })
  })
  gulp.task('less',['clear'],function(){
  
  //这里return 指执行cssnano方法是  需要等待less执行完才可执行 与callback含义一样
  //return针对gulp命令流程控制 ， callback针对列如setTimeout这种异步的流程控制
  
   return gulp.src('./src/less/**/*.css',function(){
      .pipe(less())
      .pipe(gulp.dest('./dist/css/'))
    })  
  })
  
  gulp.task('cssnano',['less'],function(){
    gulp.src('./dist/css/**/*.css)
      .pipe(cssnano())
      .pipe(gulp.dest('./dist/css/min/))
 
  })
```

八：  gulp-nunjucks 模板引擎

```
	<!DOCTYPE html>
	<html lang="en">
	
	<head>
	  <meta charset="UTF-8">
	  <title>Document</title>
	  {% block style %}
	  {% endblock %}
	</head>
	
	<body>
	  {% include "./header.html" %}
	  {% block body %}
	  {% endblock %}
	  {% include "./footer.html" %}
	  {% block script %}
	  {% endblock %}
	</body>
	
	</html>
```
```
	{% extends "./layout/layout.html" %}

	{% block style %}
	<link rel="stylesheet" href="css/main.css">
	<style>
	  /*body {
	    background-color: pink;
	  }*/
	</style>
	{% endblock %}
	
	{% block body %}
	<ul>
	  <li><a href="login.html">去登陆</a></li>
	  <li><a href="register.html">去注册</a></li>
	</ul>
	<h1>我是挺坑的 body</h1>
	{% endblock %}
```

九：brower-sync

```
// 1. 多个页面公共的头部和底部
// 2. less 构建
// 3. js 构建
// 4. 当 HTML 变化了，当 less 变化了、当 js 变化了，刷新浏览器

var gulp = require('gulp')
var nunjucks = require('gulp-nunjucks')
var browserSync = require('browser-sync').create()
var less = require('gulp-less')

// 1. 构建 HTML
gulp.task('html', function () {
  return gulp.src('./src/*.html')
    .pipe(nunjucks.compile())
    .pipe(gulp.dest('./dist'))
})

gulp.task('less', function () {
  return gulp.src('./src/less/**/*.less')
    .pipe(less())
    .pipe(gulp.dest('./dist/css'))
})

gulp.task('less-watch', ['less'], function (callback) {
  browserSync.reload()
  callback()
})

gulp.task('html-watch', ['html'], function (callback) {
  browserSync.reload()
  callback()
})

// 2. 当 HTML 构建完成，开启服务器（驱动 dist 目录），自动打开浏览器，
gulp.task('serve', ['html', 'less'], function () {
  browserSync.init({
    server: {
      baseDir: "./dist/"
    }
  })
  gulp.watch(["./src/*.html", './src/layout/*.html'], ['html-watch'])
  gulp.watch('./src/less/**/*.less', ['less-watch'])
})

gulp.task('default', ['serve'])


```


## 7.browsersync

http://www.browsersync.cn/docs/gulp/
