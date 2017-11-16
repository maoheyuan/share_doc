#����gulp��gulp���

//����gulp��gulp���<br>
var gulp = require('gulp'),<br>
    runSequence = require('run-sequence'),<br>
    rev = require('gulp-rev'),<br>
    revCollector = require('gulp-rev-collector');<br>
//gulp-rename(�ļ�����)<br>
var rename      = require('gulp-rename');<br>
//ʹ��gulp-clean-cssѹ��css�ļ�<br>
var cssmin = require('gulp-minify-css');<br>
//��css�ļ��������ļ��Ӱ汾�ţ��ļ�MD5��<br>
var cssver = require('gulp-make-css-url-version');<br>
//ѹ��js�ļ�<br>
var uglify = require('gulp-uglify');<br>
//ͼƬѹ��<br>
var smushit = require('gulp-smushit');<br>
//����ļ���<br>
var clean = require('gulp-clean');<br>
// ��������<br>
var cached = require('gulp-cache');<br>

var jshint = require('gulp-jshint');<br>

//����css��js�ļ�·�����Ǳ���css,js�ļ���·��������������<br>
var cssUrl = './css/*.css',<br>
    jsUrl = './js/*.js',<br>
    imgUrl='./images/**/*.*';<br>

//���Ŀ���ļ���<br>
gulp.task('clean',function(){<br>
    //read����Ϊfalse��ʾ����ȡ�ļ�������<br>
    return gulp.src(['./dist/','./rev'],{read:false})<br>
        .pipe(clean());<br>
});<br>


gulp.task('clearCache', function (done) {<br>
    return cache.clearAll(done);<br>
});<br>


//JShint���<br>
gulp.task('jshint', function () {<br>
    return gulp.src(jsUrl)<br>
        .pipe(jshint())<br>
        .pipe(jshint.reporter('default'));<br>
});<br>


//CSS�����ļ�hash���벢���� rev-manifest.json�ļ�������ӳ��<br>
gulp.task('buildCss', function(){<br>

    return gulp.src(cssUrl)<br>
       // .pipe(cached('buildCss'))<br>
        //.pipe(rev()) //- �ļ�����MD5��׺<br>
        //.pipe(rename({suffix: '.all'})) //- �ļ�������<br>
        .pipe(gulp.dest('dist/css')) //- �ļ�����MD5��׺<br>
        .pipe(rev()) //- �ļ�����MD5��׺<br>
        .pipe(cssmin()) //- ѹ�������һ��<br>
       // .pipe(cssver()) //��css�ļ��������ļ��Ӱ汾�ţ��ļ�MD5��<br>
        //.pipe(rename({suffix: '.min'})) //- �ļ�������<br>
        .pipe(gulp.dest('dist/css'))  //- ����ļ�����<br>
        .pipe(rev.manifest())  //- ����һ��rev-manifest.json<br>
        .pipe(gulp.dest('rev/css')); //- �� rev-manifest.json ���浽 rev Ŀ¼��<br>
});<br>




//Html����css�ļ��汾
gulp.task('revHtmlCss', function () {<br>
    return gulp.src(['./rev/css/*.json', './html/*.html'])  /*�Ǳ���html�ļ���·��������������*/<br>
        .pipe(revCollector({replaceReved: true}))<br>
        .pipe(gulp.dest('./dist/html'));  /*Html����css��js�ļ��汾,WEB-INF/viewsҲ�Ǻͱ���html�ļ���·��һ��*/<br>
});<br>



//CSS�����ļ�hash���벢���� rev-manifest.json�ļ�������ӳ��<br>
gulp.task('buildJs', function(){<br>
    return gulp.src(jsUrl)<br>
        //.pipe(cached('buildJs'))<br>
        //.pipe(rev()) //- �ļ�����MD5��׺<br>
        //.pipe(rename({suffix: '.all'})) //- �ļ�������<br>
        .pipe(gulp.dest('dist/js'))//- ����ļ�����<br>
        .pipe(rev()) //- �ļ�����MD5��׺<br>
        .pipe(uglify())  //- ѹ�������һ��<br>
        //.pipe(rename({suffix: '.min'})) //- �ļ�������<br>
        .pipe(gulp.dest('dist/js'))//- ����ļ�����<br>
        .pipe(rev.manifest()) //- ����һ��rev-manifest.json<br>
        .pipe(gulp.dest('rev/js'));//- �� rev-manifest.json ���浽 rev Ŀ¼��<br>
});

//Html����css�ļ��汾<br>
gulp.task('revHtmlJs',['buildJs'], function () {<br>
    return gulp.src(['rev/js/*.json', './dist/html/*.html'])  /*WEB-INF/views�Ǳ���html�ļ���·��������������*/<br>
        .pipe(revCollector())
        .pipe(gulp.dest('./dist/html'));  /*Html����css��js�ļ��汾,WEB-INF/viewsҲ�Ǻͱ���html�ļ���·��һ��*/<br>
});



/*ѹ��ͼƬ�����MD5��׺*/<br>
gulp.task('buildImg', function () {<br>
    return gulp.src(imgUrl)     //��ȡ��ͼƬλ��<br>
        //.pipe(cached('buildImg'))<br>
        .pipe(smushit({<br>
            verbose: true<br>
        }))<br>
        .pipe(rev())//- �ļ�����MD5��׺<br>
        .pipe(gulp.dest('dist/images')) //���ͼƬ��λ�á�<br>
        .pipe(rev.manifest()) //- ����һ��rev-manifest.json<br>
        .pipe(gulp.dest('rev/img')); //���JSON�ļ���λ��<br>
});<br>




/*css�ļ�ͼƬMD5��Դ�ı�*/<br>
gulp.task('revImgCss', function () {<br>
    gulp.src(['./rev/img/*.json', './dist/css/*.css'])<br>
        //- ��ȡ rev-manifest.json �ļ��Լ���Ҫ����js���滻���ļ�<br>
        .pipe(revCollector())<br>
        //- ִ���ļ���css�����滻<br>
        .pipe(gulp.dest('./dist/css'));<br>
        //- �滻����ļ������Ŀ¼<br>
});<br>



//Html����css��js�ļ��汾<br>
//gulp.task('revcss', function() {<br>
//    gulp.src(['./rev/css/*.json', './html/**.html'])<br>
//        //- ��ȡ rev-manifest.json �ļ��Լ���Ҫ����css���滻���ļ�<br>
//        .pipe(revCollector())<br>
//        //- ִ���ļ���css�����滻<br>
//        .pipe(gulp.dest('./dist/html'));<br>
//    //- �滻����ļ������Ŀ¼<br>
//});<br>





//Html����css��js ,image�ļ��汾<br>
//gulp.task('revHtml', function () {<br>
//    return gulp.src(['rev/**/*.json', './html/*.html'])  /*WEB-INF/views�Ǳ���html�ļ���·��������������*/<br>
//        .pipe(revCollector())
//        .pipe(gulp.dest('./dist/html'));  /*Html����css��js�ļ��汾,WEB-INF/viewsҲ�Ǻͱ���html�ļ���·��һ��*/<br>
//});

//��������<br>
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