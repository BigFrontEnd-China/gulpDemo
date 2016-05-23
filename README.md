# gulpDemo
需要先安装nodejs（<a href="https://nodejs.org/en/">https://nodejs.org/en/</a>）</br>
具体安装方式请您移步我的博客<a href="http://www.cnblogs.com/web-Development/p/5164266.html">http://www.cnblogs.com/web-Development/p/5164266.html</a></br>

这边以webstorm为例：安装gulp插件</br>

<code>npm install -g gulp</code>  安装gulp全局安装包</br>

在环境变量path进行配置（注意此环节）</br>

切换到本地项目路径在本地项目路径中安装gulp</br>

<code>npm install -save-dev gulp</code></br>

安装gulp相关组件(相关组件有很多目前只需要安装我们常用的)</br>

<code>npm install -save-dev gulp-jshint</code></br>
<code>npm install -save-dev gulp-concat</code></br>
<code>npm install -save-dev gulp-imagemin</code></br>
<code>npm install -save-dev gulp-minify-css</code></br>
<code>npm install -save-dev gulp-minify-html</code></br>
<code>npm install -save-dev gulp-uglify</code></br>
<code>npm install -save-dev gulp-rename</code></br>

在项目里新建gulpfile.js进行相关配置

``` js
/**
 * Created by China-Boy-1985.
 *
 * gulp.task(name, fn) - 定义任务，第一个参数是任务名，第二个参数是任务内容。
 *gulp.src(path) - 选择文件，传入参数是文件路径。
 *gulp.dest(path) - 输出文件
 *gulp.pipe() - 管道，你可以暂时将 pipe 理解为将操作加入执行队列
 */
//引入gulp
var gulp = require('gulp');
//引入组件
var jshint = require('gulp-jshint');//进行文件路径检查
var concat = require('gulp-concat');//进行文件合并
var imageMin = require('gulp-imagemin');//进行图片压缩
var minifyCss = require('gulp-minify-css');//进行样式压缩
var minifyHtml = require('gulp-minify-html');//进行HTML压缩
var uglify = require('gulp-uglify');//进行js文件压缩
var rename = require('gulp-rename');//修改文件名称

//配置主路径
var config = {
    path:'userDefinedForm/'
};

//检查脚本路径文件是否正确
gulp.task('lint',function(){
    gulp.src(config.path+'js/*.js').pipe(jshint()).pipe(jshint.reporter('default'));
});

//进行js文件压缩合并
gulp.task('script',function(){
   gulp.src(config.path+'js/*.js')
       .pipe(concat('all.js'))  //进行文件合并
       .pipe(gulp.dest(config.path+'js/dist'))//存储到dist目录
       .pipe(rename('all.min.js'))//进行文件重命名
       .pipe(uglify())//进行文件压缩
       .pipe(gulp.dest(config.path+'js/dist'));//压缩文件另存
});

//进行css文件压缩合并
gulp.task('minifyCss',function(){
    gulp.src(config.path+'css/*.css')
        .pipe(concat('all.css'))
        .pipe(gulp.dest(config.path+'css/dist'))
        .pipe(rename('all.min.css'))
        .pipe(minifyCss())
        .pipe(gulp.dest(config.path+'css/dist'));
});

//进行html文件压缩合并
gulp.task('minifyHtml',function(){
    gulp.src(config.path+'html/*.html')
        .pipe(concat('all.html'))
        .pipe(gulp.dest(config.path+'html/dist'))
        .pipe(rename('all.min.html'))
        .pipe(minifyHtml())
        .pipe(gulp.dest(config.path+'html/dist'))
});
//进行图片文件压缩
gulp.task('imageMin',function(){
    gulp.src(config.path+'image/*')
        .pipe(imageMin({
            progessive:true
        }))
        .pipe(gulp.dest('image/dist'));
});
//js文件修改时执行
gulp.task('auto',function(){
    gulp.watch(config.path+'js/*.js',['script']);
});


//默认gulp命令
gulp.task('default',function(){
  gulp.run('script','minifyCss','minifyHtml');//进行任务执行
  gulp.watch(config.path+'js/*.js',config.path+'css/*.css',config.path+'html/*.html',function(){
      gulp.run('script','minifyCss','minifyHtml');//监控执行任务
  });
});
```
配置完毕后切换到项目路径执行<code>gulp default</code>  

