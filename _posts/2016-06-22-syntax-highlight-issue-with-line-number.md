---
layout: post
title:  "Markdown code does not supports line number"
date:   2016-06-22
excerpt: ""
tag:
- jekyll
- syntax
- blog
- code
- linenumber
---

## ความเป็นมา

โพสต์นี้ถือว่าเป็นโพสต์แรกที่ย้ายการเขียน blog จาก wordpress, medium.com, storylog มาใช้ github page แทน เนื่องจากต้องการเก็บ content ไว้ใน repository ส่วนตัว เอาไว้ดูในอนาคต และคีย์สำคัญของการมาเขียนที่นี่ แทนคือ เราสามารถควบคุม สภาพแวดล้อมที่เราอยากให้เป็นได้สะดวกกว่า หรืออีกทำนองนึง คือ อยาก ปรับเปลี่ยนอะไรได้ตามใจชอบ

## ปัญหา

ปัญหาที่พบของการมาสร้าง page ด้วยการย้ายมาเขียน ที่ github.io นั่น คือ เราต้องติดตั้งเองทุกขั้นตอน เนื่องจากว่ามันปรับแต่งได้อิสระ จึงมองหาตัวช่วยที่ง่ายในการเริ่มต้น ก็มาพบกับ Jekyll

แต่...ก็ต้องมาพบกับ technical terms มากมาย จนทำให้เกิด learning curve ตามมา ในชั่วโมงแรกที่ทำความเข้าใจกับมัน

## Jekyll คืออะไร?

> Transform your plain text into static websites and blogs

จุดเด่นของเจ้า Jekyll คือ Simple-Static-Blog-Aware ด้วยความที่มัน minimal มันจึงทำงานไว เล็ก กระชับ และ อิสระ

ไม่มี database นั่นหมายความว่า มันสามารถลด I/O ไปได้พอสมควรในการอ่าน เขียน

1 file / 1 posts นั่น คือสิ่งที่ตอบโจทย์ ของบล็อกเล็กๆ หรือที่ทาง github เรียก markdown

[Jekyll offical page](https://jekyllrb.com/)

## Markdown คืออะไร?

> Markdown is a lightweight and easy-to-use syntax for styling all forms of writing on the GitHub platform.

ผมขอนิยามว่า small syntax ที่ง่าย สำหรับจัดการเนื้อหา ในการเขียนบทความใน ระบบของ Github

[Mastering Markdown](https://guides.github.com/features/mastering-markdown/)

## กลับมาที่ปัญหาที่พบ

Syntax highlight หรือ markdown code ไม่รอบรับการแสดงแถวของตัวโค๊ด มัน คือ ปัญหาหลักเลยก็ว่าได้ ถ้าหากมีโค๊ดที่ยาวมาก มันยากที่จะอ้างอิง ถึงบรรทัดที่เราต้องการได้

แต่ทางออกยังมี เนื่องจาก Rouge และ Pygments ซึ่งเป็น syntax highlighter ที่มาช่วยให้ Jekyll สามารถแสดง code บนเว็บ และสามารถแสดงเลขบรรทัดได้

[Jekyll markdown code](https://jekyllrb.com/docs/templates/#code-snippet-highlighting)

ด้วยวิธีนี้ ใส่ `linenos` เข้าไปดัง ตัวอย่างข้างล่าง

```
{% highlight ruby linenos %}
def foo
  puts 'foo'
end
{% endhighlight %}
```

แต่เมื่อนำมาใช้จริงพบว่า page ไม่สามารถแสดงผลได้ถูกต้องได้ stylesheet ดันมา overide กับ theme ที่ใช้อยู่ซ่ะงั้น
