---
layout: post
title:  "Frontend Engineer - Thinking about performance"
date:   2016-07-22
excerpt: ""
tag:
- performance
- browser
- javascript
---

## เราให้ความสำคัญกับ Performance มากแค่ไหน?
ทุกวันนี้อัตราความเร็วเครื่องคอมพิวเตอร์ หรือ มือถือ รวมไปถึง Smart devices ต่างๆที่สามารถเชื่อมต่ออินเตอร์เนตได้
กำลังแข่งขันกันในเรื่องความเร็วในการประมวลผล แน่นอนว่าความเร็วที่แข่งขันกันอยู่นั้น มันก็เป็นผลดีต่อผู้บริโภค
ที่สามารถมีตัวเลือกมากมาย

ผู้บริโภคโดยส่วนใหญ่ใช้อุปกรณ์เหล่านี้ในการเข้าเวปไซต์ ใช่ไหมครับ? คำตอบ คือ ใช่ และเป็นที่แน่นอนว่า
การใช้ทรัพยากรของเครื่อง เพื่อเปิดสักเวปไซต์นึงนี่ มันกินทรัพยากรไม่มากหรอก

มันก็จริง! แต่นั่นมันอาจทำให้เราไม่ได้ใส่ใจกับเรื่อง Performance เพราะ เราคิดไปเองว่าคอมพิวเตอร์เดียวนี้มันแรง

แต่อย่าลืมว่า แม้ว่าเครื่องของผู้ใช้จะแรงเพียงใด แต่การออกแบบ Frontend ที่ไม่ได้ใส่ใจกับเรื่อง Performance
มันก็ตอบสนองผู้ใช้ช้าอยู่ดี และรวมไปถึง User Experience ที่แย่อีกด้วย

## ถ้าเราจะเริ่มต้น Frontend Optimization เราควรมองปัจจัยอะไรบ้าง?
มีคำที่กล่าวว่า "Personal experience all tell us the fast is best" ดังนั้น เราลองมาพิจารณาเรื่องเล็กน้อยที่เราควรปรับปรุงเวปไซต์ของเราให้ดีขึ้น เช่น
่

1. ความเร็วในการแสดงผล

2. ความเร็วในการตอบสนอง

3. การทำงานเบื้องหน้า เบื้องหลังต้องดีเยี่ยม

4. ลด HTTP Request ที่ไม่จำเป็น

5. เข้าใจธรรมชาติของ Web Browser

## เข้าใจธรรมชาติของ Browser กันแบบคร่าวๆกันก่อน
ก่อนที่ Web Browser สักหนึ่งตัวจะแสดงผล HTML ที่เราเขียน ออกมาบนหน้าจอ Browser มีสิ่งที่เรียกว่า
Browser Rendering Engine หรือ มันคือเครื่องมือที่ Browser ใช้ตีความ และทำความเข้าใจ HTML ที่เขียนไว้

```
Different browsers use different rendering engines: Internet Explorer uses Trident, Firefox uses Gecko, Safari uses WebKit. Chrome and Opera (from version 15) use Blink, a fork of WebKit.

WebKit is an open source rendering engine which started as an engine for the Linux platform and was modified by Apple to support Mac and Windows. See webkit.org for more details.
```
Ref: [http://www.html5rocks.com/en/tutorials/internals/howbrowserswork/](http://www.html5rocks.com/en/tutorials/internals/howbrowserswork/)

คราวนี้มาดูขั้นตอนการทำงานคร่าวๆกัน

1. Rendering engine ทำการรับ content html ที่ download มาผ่านทาง Network Layer จากนั้น ทำการแบ่งข้อมูลออกเป็นก้อนๆ
ตีความว่า ก้อนละ 8kb มองเป็นเค้กก้อนละ 8kb ก็ได้

2. Parsing HTML และแปลงเป็น DOM เพื่อให้ ออกมาอยู่ในรูปแบบ Tree Structure หรือ โครงสร้างต้นไม้

3. มาถึงจุดสำคัญละครับ !!! ในจุดนี้เองถ้า Rendering Engine พบว่า มี External resource
เช่น `<script src="">` หรือ `<link rel="">` หรือสคริปจำพวกการโหลดไฟล์ ตัว Rendering Engine จะหยุด หยุดจริงๆครับ และทำการ Execute ตีความจนเสร็จ

4. เมื่อเสร็จสิ้นกระบวนการเกี่ยวกับ Javascript, Stylesheet ตัว Rendering engine จะทำการสร้าง Render Tree

5. จากนั้นก็ Parsing html to DOM ไปเรื่อยๆจนเสร็จ

6. ยังไม่จบ!! Rendering Engine จะเข้าสู่กระบวนการต่อ ที่เรียกว่า Layout process เพื่อบอก Browser ว่า element ต่อไปนี้ ควรอยู่จัดใดของหน้าเว็ป

7. และกระบวนการสุดท้าย ก็คือ นำ Render Tree มาผนวกกับ Layout process เพื่อรวม Style และแสดงผลบนหน้าจออย่างสวยงาม

Ref: [http://www.html5rocks.com/en/tutorials/internals/howbrowserswork/](http://www.html5rocks.com/en/tutorials/internals/howbrowserswork/)
[https://armno.github.io/2016/01/28/how-web-browsers-work](https://armno.github.io/2016/01/28/how-web-browsers-work)

## และแล้วเราก็เจอปัญหาจนได้
จากการทำงานคร่าวๆของ Browser เราจับใจความได้ว่า มันทำงาน แบบ Synchronously execute นี่หว่า แม่งศัพท์เทคนิคอีกละ !! ก็คือ มันหยุดรอครับ เมื่อเจอ script เกี่ยวกับ JavaScript, CSS แล้วจะเกิดอะไรขึ้นที่หน้าจอของผู้ใช้ นั่นก็คือ หน้าขาวๆ นิ่งๆ ที่รอโหลด

สิ่งนี้เรียกว่า `Browser render-blocking-resources` แต่ก็อย่างที่บอกครับ โลกของการทำให้ Performance ในส่วนของ Frontend ดีขึ้น ไม่ใช่แค่ แก้เรื่อง Browser render-blocking-resources ครับ

## Solutions
1. Browser render blocking resources
 [CSS](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/render-blocking-css?hl=en) [JS](https://developers.google.com/speed/docs/insights/BlockingJS)

2. Make Fewer Requests

3. Maximising parallelisation

4. ใช้ CDN (Content Delivery Network) แทนการอ้างสคริปภายใต้ domain web server ของเรา

5. HTTP requests and DNS lookups

6. DNS prefetching

7. Make JavaScript Asynchronous

7. Make Inline CSS/Javascript เท่าที่จำเป็น เฉพาะส่วนที่ critical กับผู้ใช้จริงๆ

8. Minify HTML/CSS/JS

9. RESTFul APIs need to be only return minimal value via header instead body
