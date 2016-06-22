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

การใช้ npm แบบนี้ ผลลัพธ์ที่ได้ คือ package ที่เลือก จะถูกติดตั้งลงในเครื่องของเรา และ add package นี้ลงใน [package.json](https://docs.npmjs.com/getting-started/using-a-package.json) ในส่วนของ "dependencies" โดยอัตโนมัติ

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
