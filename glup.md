#引入gulp和gulp插件

//引入gulp和gulp插件<br>
var gulp = require('gulp'),<br>
    runSequence = require('run-sequence'),<br>
    rev = require('gulp-rev'),<br>
    revCollector = require('gulp-rev-collector');<br>
//gulp-rename(文件更名)<br>
var rename      = require('gulp-rename');<br>
//使用gulp-clean-css压缩css文件<br>
var cssmin = require('gulp-minify-css');<br>
//给css文件里引用文件加版本号（文件MD5）<br>
var cssver = require('gulp-make-css-url-version');<br>
//压缩js文件<br>
var uglify = require('gulp-uglify');<br>
//图片压缩<br>
var smushit = require('gulp-smushit');<br>
//清空文件夹<br>
var clean = require('gulp-clean');<br>
// 缓冲内容<br>
var cached = require('gulp-cache');<br>

var jshint = require('gulp-jshint');<br>

//定义css、js文件路径，是本地css,js文件的路径，可自行配置<br>
var cssUrl = './css/*.css',<br>
    jsUrl = './js/*.js',<br>
    imgUrl='./images/**/*.*';<br>

//清空目标文件夹<br>
gulp.task('clean',function(){<br>
    //read参数为false表示不读取文件的内容<br>
    return gulp.src(['./dist/','./rev'],{read:false})<br>
        .pipe(clean());<br>
});<br>


gulp.task('clearCache', function (done) {<br>
    return cache.clearAll(done);<br>
});<br>


//JShint检查<br>
gulp.task('jshint', function () {<br>
    return gulp.src(jsUrl)<br>
        .pipe(jshint())<br>
        .pipe(jshint.reporter('default'));<br>
});<br>


//CSS生成文件hash编码并生成 rev-manifest.json文件名对照映射<br>
gulp.task('buildCss', function(){<br>

    return gulp.src(cssUrl)<br>
       // .pipe(cached('buildCss'))<br>
        //.pipe(rev()) //- 文件名加MD5后缀<br>
        //.pipe(rename({suffix: '.all'})) //- 文件名更名<br>
        .pipe(gulp.dest('dist/css')) //- 文件名加MD5后缀<br>
        .pipe(rev()) //- 文件名加MD5后缀<br>
        .pipe(cssmin()) //- 压缩处理成一行<br>
       // .pipe(cssver()) //给css文件里引用文件加版本号（文件MD5）<br>
        //.pipe(rename({suffix: '.min'})) //- 文件名更名<br>
        .pipe(gulp.dest('dist/css'))  //- 输出文件本地<br>
        .pipe(rev.manifest())  //- 生成一个rev-manifest.json<br>
        .pipe(gulp.dest('rev/css')); //- 将 rev-manifest.json 保存到 rev 目录内<br>
});<br>




//Html更换css文件版本
gulp.task('revHtmlCss', function () {<br>
    return gulp.src(['./rev/css/*.json', './html/*.html'])  /*是本地html文件的路径，可自行配置*/<br>
        .pipe(revCollector({replaceReved: true}))<br>
        .pipe(gulp.dest('./dist/html'));  /*Html更换css、js文件版本,WEB-INF/views也是和本地html文件的路径一致*/<br>
});<br>



//CSS生成文件hash编码并生成 rev-manifest.json文件名对照映射<br>
gulp.task('buildJs', function(){<br>
    return gulp.src(jsUrl)<br>
        //.pipe(cached('buildJs'))<br>
        //.pipe(rev()) //- 文件名加MD5后缀<br>
        //.pipe(rename({suffix: '.all'})) //- 文件名更名<br>
        .pipe(gulp.dest('dist/js'))//- 输出文件本地<br>
        .pipe(rev()) //- 文件名加MD5后缀<br>
        .pipe(uglify())  //- 压缩处理成一行<br>
        //.pipe(rename({suffix: '.min'})) //- 文件名更名<br>
        .pipe(gulp.dest('dist/js'))//- 输出文件本地<br>
        .pipe(rev.manifest()) //- 生成一个rev-manifest.json<br>
        .pipe(gulp.dest('rev/js'));//- 将 rev-manifest.json 保存到 rev 目录内<br>
});

//Html更换css文件版本<br>
gulp.task('revHtmlJs',['buildJs'], function () {<br>
    return gulp.src(['rev/js/*.json', './dist/html/*.html'])  /*WEB-INF/views是本地html文件的路径，可自行配置*/<br>
        .pipe(revCollector())
        .pipe(gulp.dest('./dist/html'));  /*Html更换css、js文件版本,WEB-INF/views也是和本地html文件的路径一致*/<br>
});



/*压缩图片且添加MD5后缀*/<br>
gulp.task('buildImg', function () {<br>
    return gulp.src(imgUrl)     //读取的图片位置<br>
        //.pipe(cached('buildImg'))<br>
        .pipe(smushit({<br>
            verbose: true<br>
        }))<br>
        .pipe(rev())//- 文件名加MD5后缀<br>
        .pipe(gulp.dest('dist/images')) //输出图片的位置。<br>
        .pipe(rev.manifest()) //- 生成一个rev-manifest.json<br>
        .pipe(gulp.dest('rev/img')); //输出JSON文件的位置<br>
});<br>




/*css文件图片MD5资源改变*/<br>
gulp.task('revImgCss', function () {<br>
    gulp.src(['./rev/img/*.json', './dist/css/*.css'])<br>
        //- 读取 rev-manifest.json 文件以及需要进行js名替换的文件<br>
        .pipe(revCollector())<br>
        //- 执行文件内css名的替换<br>
        .pipe(gulp.dest('./dist/css'));<br>
        //- 替换后的文件输出的目录<br>
});<br>



//Html更换css、js文件版本<br>
//gulp.task('revcss', function() {<br>
//    gulp.src(['./rev/css/*.json', './html/**.html'])<br>
//        //- 读取 rev-manifest.json 文件以及需要进行css名替换的文件<br>
//        .pipe(revCollector())<br>
//        //- 执行文件内css名的替换<br>
//        .pipe(gulp.dest('./dist/html'));<br>
//    //- 替换后的文件输出的目录<br>
//});<br>





//Html更换css、js ,image文件版本<br>
//gulp.task('revHtml', function () {<br>
//    return gulp.src(['rev/**/*.json', './html/*.html'])  /*WEB-INF/views是本地html文件的路径，可自行配置*/<br>
//        .pipe(revCollector())
//        .pipe(gulp.dest('./dist/html'));  /*Html更换css、js文件版本,WEB-INF/views也是和本地html文件的路径一致*/<br>
//});

//开发构建<br>
gulp.task('dev', function (done) {<br>
    condition = false;<br>
    runSequence(<br>
        ['clean'],<br>
        ['buildCss'],<br>
        ['revHtmlCss'],<br>
        ['buildJs'],<br>
        ['revHtmlJs'],<br>
        ['buildImg'],<br>
        ['revImgCss'],<br>
        done);});


gulp.task('default', ['dev']);<br>