---
layout: post
title:  "Setting up the project to support js task runner with Gulp Sass and Minify CSS"
date:   2016-06-28
excerpt: ""
tag:
- gulp
- js
- sass
- minify
- css
---

## Gulp คืออะไร?
`Automate and enhance your workflow` นี่คือ คำนิยามจาก Official website (http://gulpjs.com/)[http://gulpjs.com/]
เท่าที่ใช้งานมา ผมมองว่ามัน คือ JavaScript Task Runner โดยใช้ความสามารถของ NodeJS มาช่วย เพื่อให้สามารถ watching เหตุการณ์ หรือ งานที่เราสนใจ และ ให้ทำสิ่งที่เรากำหนดไว้ได้

## วันนี้จะทำอะไร?
ผมจะใช้ความสามารถของ Gulp ทำสิ่งต่อไปนี้ให้ผม
1. ผมมีไฟล์ .scss หรือ Sass Project ของผม ซึ่งเป็น CSS Preprocessor

2. ทันทีที่ผมเซฟไฟล์ใน Editor ผมอยากให้ Gulp compile .scss to .css

3. โดยที่ .css ที่ผมได้ จะต้องทำการ minify .css ด้วย

## มาเริ่มต้นกัน
1. ทำการติดตั้ง Node.JS เพื่อให้ได้ npm (node package manager) [Download](https://www.npmjs.com/)

2. ทำการติดตั้งติด Gulp แบบ Global
`$ npm install --global gulp-cli`

3. ไปที่ directory ของ project เราแล้วทำการ initial npm
`$ npm init`

4. ติดตั้ง Gulp ให้อยู่ใน project package devDependencies
`$ npm install gulp --save-dev`

5. ถึงขั้นตอนนี้เราสามารถใช้ npm และ gulp command line ได้แล้ว เพื่อให้เราสามารถใช้ความสามารถจาก คำสั่งนี้ได้อีก เราจำเป็นต้อง ติดตั้ง plugin เสริม

6. เนื่องจากผมชอบใช้ Ruby Sass เพราะบางที ผมชอบ compile sass ด้วย manual cli จึงเลือกใช้ Ruby Sass เป็นเครื่องมือหลักในการ compile สำหรับโปรเจคนี้

7. ติดตั้ง Ruby ไว้ในเครื่องเรา [Ruby Installer](http://rubyinstaller.org/)

8. เปิด command line แล้วติดตั้ง Ruby Sass ด้วยคำสั่งนี้
`$ gem install sass`

9. เมื่อติดตั้งเสร็จแล้ว ลองใช้คำสั่ง
`$ sass -v`
เพื่อตรวจสอบเวอร์ชั่น ของ sass

10. มาถึงขั้นตอนนี้ เราสามารถ รัน sass command ได้แล้ว โดยไม่จำเป็นต้องมี npm มาเกี่ยวข้องก็ได้ แต่เราจะเอาความสามารถ ของ gulp + sass มารวมกันแล้ว ดูข้อมูลเพิ่มเติมเกี่ยวกับ sass command ได้ที่ [http://sass-lang.com/install](http://sass-lang.com/install)

11. ทำการติดตั้ง gulp-ruby-sass [https://github.com/sindresorhus/gulp-ruby-sass](https://github.com/sindresorhus/gulp-ruby-sass)
[https://www.npmjs.com/package/gulp-ruby-sass/](https://www.npmjs.com/package/gulp-ruby-sass/)

`$ npm install --save-dev gulp-ruby-sass`

เพื่อให้อยู่ใน devDependencies

11. กลับมาที่ directory ของ project เราต่อ สิ่งที่เราต้องทำต่อมานั่นก็ คือ สร้างไฟล์ที่ชื่อว่า `Gulpfile.js` เพื่อกำหนด task ที่จำเป็น ตามที่ เรากำหนด Requirement ไว้

12. เปิด Gulpfile.js ที่ได้สร้างไว้ และใส่ code เหล่านี้ลงไป

{% highlight js %}
var gulp = require('gulp');
var sass = require('gulp-ruby-sass');
var mininfyCSS = require('gulp-clean-css');
var rename = require('gulp-rename');

/* create sass task */
gulp.task('sass', function () {
    return sass('sass/*.scss', {
        style: 'expanded'
    })
    .on('error', sass.logError)
    .pipe(gulp.dest('result'));
});

/* create mininfy css task */
gulp.task('minify-css', function() {
    return gulp.src('result/*.css')
    .pipe(mininfyCSS(
        {
            debug: true,
            keepBreaks: false
        }, function(details) {
        console.log(details.name + ': ' + details.stats.originalSize);
        console.log(details.name + ': ' + details.stats.minifiedSize);
    }))
    .pipe(rename({ suffix: '.min' }))
    .pipe(gulp.dest('dist'));
});

/* create watcher */
gulp.task('watch', function(){
    gulp.watch('sass/*.scss', ['sass'], function (){
        //console.log('[Sass task] is done.');
    });

    gulp.watch('result/*.css', ['minify-css'], function (){
        //console.log('[Minify CSS] task is done.');
    });
});

/* default task */
gulp.task('default', ['sass', 'watch', 'minify-css']);
{% endhighlight %}

13. วิธีการรัน Gulpfile ตัวนี้ ให้เปิด command แล้วใช้คำสั่ง `$ gulp` เพื่อทำการสั่งให้ gulp ทำงานตามที่เรากำหนด

### บทความหน้าผมจะพูดถึงตัวโค๊ดว่ามันทำงานอย่างไรครับ 
