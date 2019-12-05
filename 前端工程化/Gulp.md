## 自动化构建

构建就是将源代码转换成发布到线上可执行的代码，包括如下：

* 代码转换：ES6+转换ES5，Sass转CSS
* 文件优化：压缩代码和图片
* 代码分割：提取页面公共代码，提取首屏不需要执行部分的代码让其异步执行
* 模块合并：避免多次HTTP请求
* 自动刷新：监听本地源代码变化，自动重新构建、刷新浏览器
* 代码校验：校验代码规范以及单元测试是否通过
* 自动发布：自动构建生产代码并提交给发布系统
* ......

## Gulp

### 概述

Gulp和Grunt一样，都是一个自动任务运行器。但是相较于Grunt，它具有速度快，效率高和上手易等优点。打开Gulp的官网，会发现这样一段描述：

> Gulp.js是一个自动化构建工具，开发者可以使用它在项目开发过程中自动执行常见任务。Gulp.js是基于Node.js构建，利用Node.js流的威力，让你可以快速构建项目并减少频繁的IO操作。Gulp.js源文件和用来配置任务的gulpfile.js文件都是通过JavaScript来编写的。

简单来说，Gulp是在项目开发过程中自动执行任务的一个工具，通过它可以方便地在开发时，对目标文件的内容进行I/O操作。由于Gulp是基于流的，它对文件内容的操作就像是水槽对于水流，水流流经水槽，水槽将水流塑造成不同的形状。

### stream流

在计算机系统中文件的大小是变化很大的，有的文件内容非常多非常大，而Node.js里的文件操作是异步的。例如：将一个文件的内容写入到另一个文件中，这个过程就可以将文件内容看做是水流，从一个文件流入另一个文件。而在这个流入的过程中，可以对文件内容进行一定的操作后再写入目标文件。

### 安装

Gulp和它的所有插件都是通过JavaScript编写并依托Node.js平台，所以在学习Gulp之前需要先安装好Node.js环境。接着，在当前项目根目录键入如下指令安装Gulp。

```shell
npm init						#生成package.json
npm install -g gulp				#全局安装
npm install --save-dev gulp		#安装本地依赖
```

安装好Gulp之后，就可以安装不同的插件来执行不同的任务，键入如下指令安装相应的模块。

```shell
npm install --save-dev gulp-uglify		#安装gulp-uglify插件
npm install --save-dev gulp-jshint		#安装gulp-jshint插件
```

### gulpfile文件

项目根目录下的gulpfile.js文件是Gulp的配置文件，构建任务都定义在这个文件中。

```javascript
var gulp = require('gulp');				//加载gulp模块
var uglify = require('gulp-uglify');	//加载gulp-uglify模块
gulp.task('minify', function () {		//gulp.task()方法用于定义任务
  gulp.src('js/app.js')					//gulp.src()方法用来指定要处理的文件
    .pipe(uglify())						
    .pipe(gulp.dest('build'))			//gulp.dest()方法将处理后的文件写入build文件夹
});
```

### 基于流的Gulp

既然Gulp是基于流的，就先了解它是如何控制和操作流。

```javascript
//gulpfile.js
var gulp = require('gulp');
gulp.task('sync',function(){
	console.log("我是一个同步任务");
});
gulp.task('async',function(){
	setTimeout(function(){
		console.log("我是一个异步任务");
	},2000);
});
```

从上面gulpfile.js文件可以看出，Gulp是基于任务的，通过`gulp.task`可以定义一个任务。然后就可以执行`gulp <task>`指令执行该任务。

```shell
gulp sync				#执行sync任务
# Starting 'sync'...
# 我是一个同步任务
# Finished 'sync' after 2.29 ms
gulp async				#执行async任务
# Starting 'async'...
# Finished 'async' after 1.91 ms
# 我是一个异步任务
```

Gulp中的任务可以是同步和异步，在异步任务中确定任务完成，可以通过调用函数参数done来实现。另外，`gulp.task`方法还允许接受第二个可选数组参数，表示第一个参数的依赖任务。该方法常用来将任务组合起来执行。注意：数组中的任务是并行处理，所以不能保证这些任务的执行顺序。

```javascript
var gulp = require('gulp');
gulp.task('sync1',function(){
	console.log("同步任务1");
});
gulp.task('sync2',function(){
	console.log("同步任务2")
});
gulp.task('async1',function(){
	console.log("hello world");
	setTimeout(function(){
		console.log("异步任务1")
	},2000);
});
//组合任务
gulp.task('concatTask',['async1','sync1','sync2']);

/*键入 gulp concatTask指令输出如下：
# Starting 'async1'...                                
hello world                                                    
# Finished 'async1' after 2.26 ms                     
# Starting 'sync1'...                                 
同步任务1                                                          
# Finished 'sync1' after 918 μs                       
# Starting 'sync2'...                                 
同步任务2                                                          
# Finished 'sync2' after 566 μs                       
# Starting 'concatTask'...                            
# Finished 'concatTask' after 678 μs                  
异步任务1   
*/
//注意：concatTask任务是否在这些前置依赖的任务完成之前就运行了？所以一定要确保依赖的任务都使用了正确的异步执行方式：使用一个回调函数，或者返回一个promise或stream。
```

以上是Gulp基本的任务模型，而在实际开发过程，你定义的任务通常是用来操作文件输入和输出流，因此Gulp封装了批量操作文件流的API。

```javascript
var gulp = require('gulp');

gulp.task('test',function(){
	gulp.src('./demo3.js')
	.pipe(gulp.dest('./build'));
});
/* gulp test
demo3.js输出到build文件夹下
*/
```

### gulp模块的方法

#### src()

`gulp.src`方法的参数表示要处理的文件，这些指定的文件会转换成数据流。该方法的参数一般有如下写法：

- app.js：指定确切的文件名
- js/*.js：指定js文件夹下的所有js文件
- js/*/.js：指定js文件下的所有子文件夹中的js文件
- !app.js：除了指定文件外的所有文件

另外，该方法的参数还可以是一个数组，用来指定多个成员。

```javascript
gulp.src(['js/**/*.js', '!js/**/*.min.js'])
```

#### dest()

`gulp.dest`方法用于将管道的输出写入文件，同时将这些输出继续输出，因此可以调用多次该方法将输出写入多个目录。如果指定目录不存在，则会新建该目录。

```javascript
gulp.src('./demo.js')
	.pipe(uglify())
	.pipe(gulp.dest('./build'))
	.pipe(jshint())
	.pipe(gulp.dest('./dist'))

```

另外，该方法还可以接受第二个可选参数，表示配置对象。

```javascript
gulp.dest('build', {
  cwd: './app',		//cwd字段用来指定写入路径的基准路径，默认是当前目录
  mode: '0644'		//mode字段指定写入文件的权限，默认是0777
})

```

#### task()

`gulp.task`方法用于定义具体的任务，该方法第一个参数是任务名，第二个参数是回调函数。

```javascript
gulp.task('greet', function () {
   console.log('Hello world!');
});

```

如果某个任务的名称为default，就表示为默认任务，就可以直接键入gulp指令执行该任务。

#### watch()

`gulp.watch`方法用于指定需要监视的文件，一旦这些文件发生变动，就运行指定任务。

```javascript
gulp.task('watch', function () {
   gulp.watch('js/a.js', ['build']);		//一旦a.js发生变化，就执行build任务
});

```

该方法第二个参数还可以是回调函数，用来代替指定的任务。

```javascript
gulp.task('watch', function () {
   gulp.watch('js/a.js', function(e){
   		console.log(e.type)    
   });		
});

```

因为监听文件发生变化会触发change事件，所以上述代码可以改写成如下：

```javascript
var watcher = gulp.watch('./demo.js', ['test']);
watcher.on('change', function (e) {
   console.log(e.type);
});

```

除了change事件之外，当文件发生变化过程中还会触发如下事件：

- end：回调函数运行完毕时触发。
- error：发生错误时触发。
- ready：当开始监听文件时触发。
- nomatch：没有匹配的监听文件时触发。

另外，watcher对象还包含其他一些方法。

- watcher.end()：停止watcher对象，不会再调用任务或回调函数。
- watcher.files()：返回watcher对象监视的文件。
- watcher.add(glob)：增加所要监视的文件，它还可以附件第二个参数，表示回调函数。
- watcher.remove(filepath)：从watcher对象中移走一个监视的文件。

### gulp-load-plugins插件

一般情况下，gulpfile.js中的模块需要一个个加载。

```javascript
var gulp = require('gulp'),
    jshint = require('gulp-jshint'),
    uglify = require('gulp-uglify'),
    concat = require('gulp-concat');

```

这样的加载方式十分繁琐，因此需要使用gulp-load-plugins插件，来加载package.json文件中所有的gulp模块。

```
var gulp = require('gulp'),
    gulpLoadPlugins = require('gulp-load-plugins'),
    plugins = gulpLoadPlugins();
gulp.task('js', function () {
   return gulp.src('js/*.js')
      .pipe(plugins.jshint())
      .pipe(plugins.jshint.reporter('default'))
      .pipe(plugins.uglify())
      .pipe(plugins.concat('app.js'))
      .pipe(gulp.dest('build'));
});

```

### 关于return的问题

如果你定义多个任务，通常需要它们按照一定的顺序执行。

```javascript
var gulp = require('gulp')
gulp.task('task1',function(){
    setTimeout(function(){
        console.log('task1')
    },2000);
})
gulp.task('task2',function(){
	setTimeout(function(){
    	console.log('task2')
	},1000);
})
gulp.task('default',['task1','task2'],function(){
    console.log('default task')
})

```

当执行default任务时，你希望task1和task2已经执行完成，但是事实并非如下，返回的结果是`default task->task2->task1`。这是因为task1和task2都是异步任务，而gulp无法判断这些异步任务是否已经完成，除非使用如下三种方法：

- 使用回调函数通知gulp任务完成。当定义任务时，gulp会传入一个回调函数作为参数，当任务完成时，调用这个回调函数，就可以通知gulp这个任务确定完成了。

  ```javascript
  var gulp = require('gulp')
  gulp.task('task1',function(done){
      setTimeout(function(){
          console.log('task1')
  		done();
      },2000);
  })
  gulp.task('task2',function(done){
  	setTimeout(function(){
      	console.log('task2')
        	done()
  	},1000);
  })
  gulp.task('default',['task1','task2'],function(){
      console.log('default task')
  })
  /*等待两秒后输出 task1
    等待一秒后输出 task2
    接着输出		default task
  */
  
  ```

- 使用return让任务返回流。如果定义的任务返回的是流，那么gulp也可以确定任务何时完成，从而确保任务的先后执行顺序。注意：通常gulp的插件都是返回流，这样才可以级联起来。

- 让任务返回Promise。如果定义的任务，返回的是Promise对象，那么gulp也可以确定任务何时执行完成，从而确保执行顺序。

  ```javascript
  var gulp = require("gulp");
  var Promise = require("bluebird");
  gulp.task("task1",function(){
    return new Promise(function(resolve,reject){
      setTimeout(function(){
        console.log("task1");  
        resolve();
      },2000);
    })
  });
  gulp.task("task2",["task1"],function(){ 
    return new Promise(function(resolve,reject){
      setTimeout(function(){
        console.log("task2");  
        resolve();
      },1000);  
    });
  });
  gulp.task("default",["task1","task2"],function(){
    console.log("default task");
  });
  
  ```

另外，当gulp启动多个任务时，它默认是并发执行。如果希望各个任务以你希望的方式串行执行，必须显式地使用上面三种方式。