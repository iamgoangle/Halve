---
layout: post
title:  "npm install แต่ละ option ต่างกันอย่างไร?"
date:   2016-06-22
excerpt: ""
tag:
- nodejs
- npm
- dependency
---

## npm คืออะไร
npm คือ package manger command ที่ใช้ในการ install package, manage package ต่างๆ ที่เกี่ยวข้องกับ NodeJS ถือเป็นคำสั่งที่ให้เราเข้าถึง package ต่างๆบน repository ของ NodeJS

## การติดตั้ง package
ปกติแล้วจะใช้คำสั่ง ดังนี้

> npm install \<package name\>

Ref:[https://docs.npmjs.com/getting-started/installing-npm-packages-locally](https://docs.npmjs.com/getting-started/installing-npm-packages-locally)

## จะหา package ได้ที่ไหน
[https://www.npmjs.com/browse/star](https://www.npmjs.com/browse/star)

## npm install package --save | -global | --save-dev | --save-optional ต่างกันอย่างไร

จากหัวข้อ เราสามารถแยก การติดตั้ง package ออกเป็น 4 รูปแบบ

### 1. npm install package --save

การใช้ npm แบบนี้ ผลลัพธ์ที่ได้ คือ package ใดๆก็ตามที่เลือก จะถูกติดตั้งลงในเครื่องของเรา และ add package นี้ลงใน [package.json](https://docs.npmjs.com/getting-started/using-a-package.json) ในส่วนของ "dependencies" โดยอัตโนมัติ

ยกตัวอย่าง

**package.json** ก่อนใช้ --save
{% highlight json %}
{
  "name": "my_package",
  "version": "1.0.0"
}
{% endhighlight %}

**package.json** หลังใช้ --save
{% highlight json %}
{
  "name": "my_package",
  "version": "1.0.0",
  "dependencies": {
    "my_dep": "^1.0.0"
  }
}
{% endhighlight %}

สังเกตุว่ามี property ใหม่มา นั่นคือ dependencies

>dependencies property คือ package ในส่วนของ environment production (Package will appear in your dependencies)

### 2. npm install package -global

การใช้ npm แบบนี้ผลลัพธ์ที่ได้ จะตรงข้ามกับหัวข้อที่ผ่านมา คือ เป็นการระบุ package ที่ต้องการให้ติดตั้ง ในเครื่องเราเท่านั้น ไม่ต้องเพิ่ม dependency ลง ใน package.json

> -g flag is global only to your local machine
Ref: [http://stackoverflow.com/a/25092832/6495718](http://stackoverflow.com/a/25092832/6495718)

### 3. npm install package --save-dev

ผลลัพธ์ที่ได้จากการติดตั้ง package แบบนี้ package ที่ถูก install ลงในเครื่องเรา จะถูกเพิ่มใน package.json ด้วย ใน property devDependencies

### 4. nom install package --save-optionalDependencies

package จะถูกติดตั้งในเครื่องของเรา รวมไปถึงการเพิ่ม dependency ลง package.json ในส่วนของ property optionalDependencies

## Deployment

Production Environment

`npm install –production`

Developer Environtment

npm install –dev`

## Useful Links
1. [http://blog.nodejitsu.com/npm-cheatsheet](http://blog.nodejitsu.com/npm-cheatsheet)
2. [http://blog.martroutine.com/2013/08/%E0%B9%80%E0%B8%A3%E0%B8%B7%E0%B9%88%E0%B8%AD%E0%B8%87%E0%B8%99%E0%B9%88%E0%B8%B2%E0%B8%A3%E0%B8%B9%E0%B9%89%E0%B9%80%E0%B8%81%E0%B8%B5%E0%B9%88%E0%B8%A2%E0%B8%A7%E0%B8%81%E0%B8%B1%E0%B8%9A-package-js/](http://blog.martroutine.com/2013/08/%E0%B9%80%E0%B8%A3%E0%B8%B7%E0%B9%88%E0%B8%AD%E0%B8%87%E0%B8%99%E0%B9%88%E0%B8%B2%E0%B8%A3%E0%B8%B9%E0%B9%89%E0%B9%80%E0%B8%81%E0%B8%B5%E0%B9%88%E0%B8%A2%E0%B8%A7%E0%B8%81%E0%B8%B1%E0%B8%9A-package-js/)
3.  [http://nextflow.in.th/2015/better-node-js-module-install/](http://nextflow.in.th/2015/better-node-js-module-install/)
4. [http://stackoverflow.com/questions/25092617/npm-install-bower-using-g-vs-save-dev](http://stackoverflow.com/questions/25092617/npm-install-bower-using-g-vs-save-dev)
