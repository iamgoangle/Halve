---
layout: post
title:  "ติดตั้ง Karma เพื่อทำ TDD ด้วย Jasmine และ ออก report"
date:   2016-07-02
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

	คำสั่งข้างบน คือ การสร้าง config file สำหรับ test runner ของเรา ซึ่งเราสามารถสร้างได้เอง โดยศึกษาเพิ่มเติมที่ [http://karma-runner.github.io/0.12/intro/configuration.html](http://karma-runner.github.io/0.12/intro/configuration.html)

```
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

จาก Wizard ข้างบน ส่วนสำคัญ คือ locaton ที่เรากำหนดให้ test file ในตัวอย่างนี้ คือ tests/*.test.js

นั่นหมายความว่า เราจะต้องสร้าง test / spec ไว้ที่ folder ดังกล่าว

6. ทำการสร้างไฟล์ simple.test.js ไว้ใน folder/test
7. ใส่ spec เข้าไป แล้วทำการ save file

``` javascript
describe("A suite", function() {
    it("contains spec with an expectation", function() {
        expect(true).toBe(true);
    });
});
```

8. รัน karma เพื่อเริ่มต้น test spec ที่เราเขียนไป
`karma start karma.conf.js`

	![alt text](https://raw.githubusercontent.com/iamgoangle/iamgoangle.github.com/master/images/_posts/2016-07-02%2021_32_01-Command%20Prompt%20-%20karma%20%20start%20karma.conf.js.png "result run karma")

## Karma Reporter - Reference
[https://www.npmjs.com/browse/keyword/karma-reporter](https://www.npmjs.com/browse/keyword/karma-reporter)
[https://www.npmjs.com/package/karma-spec-reporter](https://www.npmjs.com/package/karma-spec-reporter)

## Karma - Configuration
[http://karma-runner.github.io/0.12/intro/configuration.html](http://karma-runner.github.io/0.12/intro/configuration.html)
