---
layout: post
title:  "เริ่มต้น Saas CSS preprocessor"
date:   2016-06-23
excerpt: ""
tag:
- css
- saas
- preprocessor
---

## Saas คืออะไร
Saas คือ CSS Preprocessor ที่มี feature ต่างๆ ที่ช่วยให้การเขียน CSS มีความกระชับ ลดความซ้ำซ้อน
ลดความซับซ้อนของตัว CSS ได้

## อะไรคือ CSS Preprocessor?
คือ การเขียน CSS อีกรูปแบบหนึ่ง ที่ให้เราสามารถเขียน CSS แบบ Nesting, Variables, Mixins and Operators
ก่อน แล้วจึง processor ให้เป็น .css file

## ทำไมต้อง CSS Preprocessor?
โดยปกติแล้ว CSS style ที่เราเขียนกันทุกวันนี้ มันก็โอเคอยู่แล้ว ถ้าโปรเจคเรามีขนาดเล็ก มีความซับซ้อนน้อย
หรือ เราเทพจริงๆในการลดความซ้ำซ้อนของ selector, id, class แต่ในความเป็นจริง คือ ไม่ใช่!!

นั่นจึงเป็นเหตุผลให้เกิดเครื่องมือ หรือมาตรฐานการเขียน css แบบ preprocessor

## Saas มี features อะไรบ้าง?
อย่างที่กล่าวไป Saas ช่วยให้เราสามารถทำ Nesting, Variables, Mixins and Operators ได้ ดังนั้น มาดูกันว่า แต่ละตัว
เขียนอย่างไร และแนวคิด หลักการทำงาน มันเป็นอย่างไร

### Nesting
เป็นการเขียน saas code ในรูปแบบ ลำดับชั้น (Heirarchy code) ถ้าหากเราคุ้นเคยกับ HTML syntax
การทำความเข้าใจ saas nesting คงทำได้ไม่ยาก ดังตัวอย่าง

**Saas syntax**

`ปล.ผมเลือกการเขียนแบบ non-curley brackets หรือ {} อีกกา เพราะว่า มันเท่ดี :)`

{% highlight css %}
nav
  ul
    margin: 0
    padding: 0
    list-style: none

  li
    display: inline-block

  a
    display: block
    padding: 6px 12px
    text-decoration: none
{% endhighlight %}

ผลลัพธ์ เมื่อผ่านการ processor to .css

{% highlight css %}
nav ul {
  margin: 0;
  padding: 0;
  list-style: none;
}

nav li {
  display: inline-block;
}

nav a {
  display: block;
  padding: 6px 12px;
  text-decoration: none;
}
{% endhighlight %}

จะเห็นได้ว่า เราลดความซ้ำซ้อน ตอนที่เราเขียน แบบ Saas ไปได้ ด้วยการประกาศ nav เพียงจุดเดียว

* * *
test
