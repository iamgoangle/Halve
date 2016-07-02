---
layout: post
title:  "ติดตั้ง Karma เพื่อทำ TDD ด้วย Jasmine และ ออก report"
date:   2016-06-28
excerpt: ""
tag:
- npm
- js
- tdd
- karma
- test
---

## ติดตั้ง Karma เพื่อทำ test runner Jasmine และออก Report สวยๆเพื่อให้เข้าใจง่าย

###### Pre-Condition
* ควรติดตั้ง Node.JS ในเครื่องให้เรียบร้อย
* สร้าง project folder /tdd ขึ้นมา
* ไปที่ folder /tdd และใช้คำสั่ง `npm init` เพื่อสร้าง npm ให้กับโปรเจคนี้

---

1. ติดตั้ง Karma แบบ global

	`npm install karma-cli -g`

2. ติดตั้ง Karma แบบ devDependencies

	`npm install karma --save-dev`

3. ติดตั้ง Karma - Jasmine แบบ devDependencies

	`npm install karma-jasmine --save-dev`

4. ติดตั้ง Chrome ให้กับ Karma

	`npm install karma-chrome-launcher --save-dev`

5. สร้างไฟล์ karma.conf.js

	`karma init karma.conf.js`

ผลลัพธ์จากการรันคำสั่งนี้จะเข้าสู่โหมด Wizzard

``` javascript
Creating karma config file
> karma init karma.conf.js
Answer the prompts as follows:
Which testing framework do you want to use?
> Hit return to accept the default value i.e. jasmine.
Do you want to use Require.js ?
> Hit return to accept the default value i.e. no.
Do you want to capture any browsers automatically ?
> Hit return to accept the default value i.e. Chrome.
What is the location of your source and test files ?
Enter the following value:
> tests/*.test.js
```
