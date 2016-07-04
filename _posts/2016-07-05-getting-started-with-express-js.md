---
layout: post
title:  "Getting started with Express.JS"
date:   2016-07-05
excerpt: ""
tag:
- nodejs
- npm
- expressjs
- jade
---

## เริ่มต้นสร้าง Web Application project กับ Express.JS
	เราทราบกันดีว่า Node.JS ได้เข้ามามีบทบาทการสร้างเว็พแอพพลิเคชันในรูปแบบใหม่เป็นอย่างมาก
	จากโลกเดิมๆของการพัฒนา เราให้ความสำคัญกับคำนิยาม client-request-server และในทางกลับกัน
	server-reponse-client นั่นหมายความว่า client จะต้องร้องขอก่อนเสมอ

	แต่ Node.JS ไม่มองเช่นนั้น ลองสมมุติดูว่าถ้าเราเอา JS ไปรันบน server ได้ แล้วให้มันส่งผลลัพธ์มาที่
	client โดยที่ client ไม่ต้องร้องขอ ด้วยการ interval request หล่ะ จะเกิดอะไรขึ้น? นั่น คือ เราสามารถสร้าง
	Realtime web based application ได้

	อย่าลืมว่า JS เป็น single-thread ดังนั้น มันเร็ว และไม่เกิด blocking I/O

	และที่ผมกล่าวมาเป็นเพียงหนึ่งใน ในความสามารถของ Node.JS เท่านั้น ซึ่งเราสามารถค้นหา package ได้มากมาย
	ในเวป [https://www.npmjs.org/](https://www.npmjs.org/)

	ดังนั้นสรุปได้ว่า Node.JS ได้เปลี่ยนนิยามโลก JavaScript ใหม่ ไม่ใช่การใช้ทรัพยากรบนเครื่อง client เพื่อทำงาน
	แต่มัน คือ การให้ server สามารถเข้าใจ JS ได้ และทำงานได้

	หรือกล่าวได้ว่า Node.JS คือ platform นึง ระบบนึงที่อำนวยความสะดวกในการทำ web application ด้วยภาษา JS

## Express.JS
	ผมมองว่าเป็นเครื่องมือนึง ที่ช่วยให้เราทำเวปแอพพลิเคชั่น ที่รันอยู่บนพื้นฐาน Node.JS ได้อย่างสะดวก โดยสามารถทำ
	single-page-application ได้ ทำ page routing ได้ จำลองตัวเองเป็น web server ได้ และอื่นๆที่จำเป็นสำหรับการพัฒนา
	เวปแอพพลิเคชันขั้นพื้นฐาน

## ลืมพวก PHP, ASP.NET, JSP, หรือ server-side-script ไปก่อน
	Express.JS ให้เราเขียนเวปแอพพลิเคชันด้วยภาษา JavaScript

## เริ่มต้นติดตั้ง	
