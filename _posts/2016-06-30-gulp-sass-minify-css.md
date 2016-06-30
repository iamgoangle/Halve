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
1. ทำการติดตั้ง Node.JS เพื่อให้ได้ npm (node package manager) (Download)[https://www.npmjs.com/]

2. ทำการติดตั้งติด Gulp แบบ Global

`$ npm install --global gulp-cli`

3. ไปที่ directory ของ project เราแล้วทำการ initial npm

`$ npm init`

4. ติดตั้ง Gulp ให้อยู่ใน project package devDependencies

`$ npm install gulp --save-dev`

5. ถึงขั้นตอนนี้เราสามารถใช้ npm และ gulp command line ได้แล้ว เพื่อให้เราสามารถใช้ความสามารถจาก คำสั่งนี้ได้อีก เราจำเป็นต้อง ติดตั้ง plugin เสริม

6. เนื่องจากผมชอบใช้ Ruby Sass เพราะบางที ผมชอบ compile sass ด้วย manual cli จึงเลือกใช้ Ruby Sass เป็นเครื่องมือหลักในการ compile สำหรับโปรเจคนี้

7. ติดตั้ง Ruby ไว้ในเครื่องเรา (Ruby Installer)[http://rubyinstaller.org/]

8. เปิด command line แล้วติดตั้ง Ruby Sass ด้วยคำสั่งนี้

`$ gem install sass`

9. เมื่อติดตั้งเสร็จแล้ว ลองใช้คำสั่ง

`$ sass -v`

เพื่อตรวจสอบเวอร์ชั่น ของ sass
