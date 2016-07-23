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
สำหรับเนื้อหาตรงนี้ ผมได้รวบรวมศัพท์เทคนิค (เผื่อเอาไป Search google หาข้อมูลเพิ่มเติม) สำหรับการเพิ่มประสิทธิภาพเพื่อช่วยให้ Frontend ทำงานได้ดียิ่งขึ้น โดยแบ่งออกเป็นหัวข้อต่างๆ ดังนี้

### Programming Technique

1. Prevent the browser render blocking resources

	Browser จะหยุดรอ ให้ JS/CSS พวกนี้ทำงานเสร็จก่อน ถึงจะทำการ Render page ได้ นั่นหมายความว่า จะเกิดเหตุการณ์ หน้าขาวเนื่องจากรอโหลด และทำให้ผู้ใช้ต้องรอ

	งั้นลองแก้ไขปัญหาแบบง่ายด้วย 2 วิธีนี้ดู
	- Inline JavaScript/CSS เฉพาะส่วนที่อยากให้แสดงเร็วๆ ทำงานทันที
	- Asynchronous loading js file แต่ต้องยอมรับว่าการใช้เทคนิคนี้ มันจะทำให้ Page render เลยใช่ไหมครับ แต่จะพบว่า Style มีจุดบกพร่องในการแสดงผล เนื้องจาก Style ถูกโหลดมาทีหลัง

	ถ้าสนใจลองศึกษา
	[https://github.com/typekit/webfontloader](https://github.com/typekit/webfontloader) สำหรับโหลด web font
	[https://github.com/filamentgroup/loadCSS](https://github.com/filamentgroup/loadCSS) สำหรับโหลด CSS
	[http://www.w3schools.com/tags/att_script_async.asp](http://www.w3schools.com/tags/att_script_async.asp) สำหรับโหลด JS

	ข้างบนทั้งหมด คือ การโหลดแบบ Asynchronous


	อ่านเพิ่มเติม:

	 [https://developers.google.com/web/fundamentals/performance/critical-rendering-path/render-blocking-css?hl=en](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/render-blocking-css?hl=en) [https://developers.google.com/speed/docs/insights/BlockingJS](https://developers.google.com/speed/docs/insights/BlockingJS)
	 [http://armno.github.io/2015/05/04/use-loadcss-to-improve-rendering-performance](http://armno.github.io/2015/05/04/use-loadcss-to-improve-rendering-performance)

2. JavaScript Uglify/Minify

	อีกวิธีการที่จะช่วยให้ Browser HTTP Request ไปยัง assets ไม่ว่าจะเป็น HTML, JS และ CSS เพื่อดาวน์โหลดได้ไวขึ้น นั่น คือ การทำ Uglify หรือ Minify เพื่อทำให้ไฟล์เหล่านี้ มีขนาดที่เล็กลง

	ผมใช้ gulp ในการ minify js และ css ทุกครั้งเมื่อมีการ save file

	อ่านเพิ่มเติม:

	[https://developers.google.com/speed/docs/insights/MinifyResources](https://developers.google.com/speed/docs/insights/MinifyResources)
	[http://codehangar.io/concatenate-and-minify-javascript-with-gulp/](http://codehangar.io/concatenate-and-minify-javascript-with-gulp/)
	[https://css-tricks.com/gulp-for-beginners/](https://css-tricks.com/gulp-for-beginners/)

3. CSS Uglify/Minify

4. Make inline css or javascript

5. RESTFul APIs only return minimal value via header instead body

6. Asynchronous loading the assets

### Network
7. Make Fewer Requests
	ง่ายที่สุดที่สำหรับวิธีนี้ คือ HTTP request ให้น้อยที่สุดเท่าที่เป็นไปได้

8. Maximising parallelisation / Parallel Downloads Across Domains
	โดยปกติแล้ว Browser สามารถทำสิ่งที่เรียกว่า Multiple (Parallel) HTTP Request at once หรือ Browser สามารถ HTTP Request พร้อมกันได้มากกว่า 1 request ในช่วงเวลานึง สำหรับ 1 domain name แต่!! ไม่เกิน 3-6 Requests และนี่ คือ ปัญหาคอขวดของ Web Browser

	อีกความหมาย คือ เมื่อ Browser พยายาม Request ไปยัง สักหนึ่งเวปไซต์ Browser สามารถโหลด Assets ต่างๆ เช่น รูป ไฟล์ พร้อมกันได้ สำหรับ 1 domain ในหนึ่งช่วงเวลา แต่มันก็ยังมีข้อจำกัดที่ 6 request อยู่ดี

	แต่เราสามารถใช้เทคนิค domain sharding ได้ ด้วยการ HTTP Request ไปยัง sub.domain หรือ across multiple domains

	หรือการ request ไปยัง sub domain มันจะทำให้ ในหนึ่งช่วงเวลา จากเดิม สมมุติเรา request = 6 เมื่อโค๊ดของเรามีการ request sub domain ด้วย เราก็จะได้อีก 6 ในช่วงเวลาเดียวกัน

	อ่านเพิ่มเติม:

	[http://csswizardry.com/2013/01/front-end-performance-for-web-designers-and-front-end-developers/#section:http-requests-and-dns-lookups](http://csswizardry.com/2013/01/front-end-performance-for-web-designers-and-front-end-developers/#section:http-requests-and-dns-lookups)
	[https://www.keycdn.com/blog/parallel-downloads-across-domains/](https://www.keycdn.com/blog/parallel-downloads-across-domains/)

9. ใช้ CDN (Content Delivery Network) แทนการอ้างสคริปภายใต้ domain web server ของเรา
	หากเราต้องการลดภาระของ web server ของเรา ในการ deliver assets เช่น js, css framework ให้กับ User เราสามารถใช้ CDN ที่เขา host file เหล่านี้ไว้ให้แล้ว

	และอีกสาเหตุหนึ่งที่เป็นหัวใจหลักของ CDN นั่นก็คือ ระบบ caching, shorted path ในการ deliver assets ที่ผมบอกข้างบน

	สมมุติว่า Web server ผมอยู่ที่ US แต่มี User ที่ใช้งานเวปไซต์ผมที่ Bangkok แน่นอนว่า ไกลกันมากในระยะทางการให้บริการ ถ้าหากเราใช้ CDN เช่น Google CDN ตัว server ของ google จะทำการหา CDN Node ที่ใกล้เรามากที่สุด สำหรับ assets นั้นๆ ทำให้เวปโหลดเร็วขึ้น และลดภาระการ Request จาก Web Server เราได้ด้วย

	อ่านเพิ่มเติม:

	[http://jojoee.com/why-use-a-cdn/](http://jojoee.com/why-use-a-cdn/)
	[https://developers.google.com/speed/libraries/](https://developers.google.com/speed/libraries/)
	[https://en.wikipedia.org/wiki/Content_delivery_network](https://en.wikipedia.org/wiki/Content_delivery_network)

10. HTTP requests and DNS lookups

11. DNS prefetching
	มาพูดถึง DNS กันก่อน เอาง่ายๆ ถ้าเรามี HTTP Request ไปยัง domain.com DNS ต้องทำการ Resolve URL to IP Address แปลง domain ไปเป็น IP Address นั่นเอง

	สมมุติว่าเรามีการ เรียกไฟล์ CSS ไปยังสัก domain.com มันต้องเสียเวลา resolve ใช่ไหมครับ และมันจะเกิดปัญหา Render blocking resource ด้วย

	ดังนั้น เราจะบอกให้ Browser ไป Resolve URL ล่วงหน้าเลย เมื่อถึงเวลาที่ต้องใช้ CSS เหล่านั้น ในตอน Rendering process จะได้นำไปใช้ได้เลย ไม่ต้องเสียเวลา Resolve URL

	แต่มันก็มีคำถามมาอยู่ดีว่า Resolve ก่อน หลัง มันก็ไม่ต่างกันอยู่ดีไม่ใช่หรอ คำตอบ คือ ไม่ครับ DNS Prefetching ทำงานแบบ Asynchronous ซึ่งนั่นมันจะไม่ทำให้เกิด Render blocking resource

	```
	<link rel="dns-prefetch" href="//example.com">
	```

	[http://www.siamhtml.com/speed-up-website-with-dns-prefetch/](http://www.siamhtml.com/speed-up-website-with-dns-prefetch/)
	[https://css-tricks.com/prefetching-preloading-prebrowsing/](https://css-tricks.com/prefetching-preloading-prebrowsing/)
	[https://www.chromium.org/developers/design-documents/dns-prefetching](https://www.chromium.org/developers/design-documents/dns-prefetching)
	[http://www.htmlgoodies.com/beyond/webmaster/how-your-browser-speeds-up-cross-domain-loading-using-dns-prefetching.html](http://www.htmlgoodies.com/beyond/webmaster/how-your-browser-speeds-up-cross-domain-loading-using-dns-prefetching.html)

### Image / Font
1. Lossless optimisation

2. Correct image dimension

3. Remove unused assets / Audit and Remove

4. Using the right file image

และนี่เป็นเพียงตัวอย่างคร่าวๆ ที่เหมือนเป็นเชคลิสต์ ทุกครั้งในการเริ่มต้นพัฒนา Frontend ในบทความหน้าผมจะเริ่มเจาะไปทีละตัวครับ
