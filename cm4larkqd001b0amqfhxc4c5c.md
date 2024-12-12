---
title: "ทำความเข้าใจ FilterQuality ของ Image ใน Flutter ผ่าน Demo แอพ"
seoTitle: "Flutter Image FilterQuality Guide"
seoDescription: "เรียนรู้เกี่ยวกับ FilterQuality สำหรับการเรนเดอร์รูปภาพใน Flutter พร้อมตัวอย่างแอปพลิเคชั่นและแนวทางการใช้งานที่ดีที่สุด"
datePublished: Thu Dec 12 2024 12:30:11 GMT+0000 (Coordinated Universal Time)
cuid: cm4larkqd001b0amqfhxc4c5c
slug: filterquality-image-flutter-demo
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1733991392056/b957c167-1cc7-44bf-9693-f0579d508bb6.png
tags: flutter, mobile-development

---

รูปภาพเป็นส่วนประกอบที่สำคัญของทุกแอพพลิเคชั่น หากเราเข้าใจการเรนเดอร์รูปภาพใน Flutter จะทำให้เราเข้าใจส่วนประกอบที่ต้องใช้ในการเรนเดอร์รูปภาพให้ชัดเจนบนทุกอุปกรณ์ เพื่อให้ประสบการณ์การใช้งานของผู้ใช้แอพพลิเคชั่นไม่ถูกด้อยด้วยภาพที่ไม่ชัด ซึ่งถือว่าเป็นพื้นฐานในการพัฒนาแอพพลิเคชั่นใดๆก็ตาม  
ในบทความนี้เราจะพูดถึง property ตัวนึงของ Image ที่ใช้ในการ render นั่นก็คือ FilterQuality ซึ่งเป็น property ที่ช่วยเสริมคุณภาพของรูปภาพ ซึ่งจะมีการปรับค่าระหว่าง `none`, `low`, `medium` และ `high` ซึ่งแต่ละค่าจะให้ผลต่างกันอย่างไร เดี๋ยวเราจะไปดูผ่าน demo application เพื่อช่วยให้เห็นภาพจากแอพพลิเคชั่นจริง พร้อมสรุปแนวทางการนำไปใช้ตอนท้ายนะครับ

## ทำไมถึงต้องมี FilterQuality

จากที่ได้เกริ่นไปในบทนำ FilterQuality เป็นเพียงตัว “เสริม” คุณภาพของรูปภาพเท่านั้น ซึ่งหมายความว่า ถ้ารูปภาพที่นำไปใช้นั้นมีความละเอียด pixels ตรงกับขนาดพื้นที่แสดงรูปภาพ การใช้ `FilterQuality.none` ก็เพียงพอที่จะทำให้ภาพที่แสดงมีคุณภาพสูงสุดแล้ว โดยอ้างอิงจาก [Flutter API Docs](https://api.flutter.dev/flutter/widgets/Image/filterQuality.html)

> If the image is of a high quality and its pixels are perfectly aligned with the physical screen pixels, extra quality enhancement may not be necessary. If so, then [FilterQuality.none would be the most](https://api.flutter.dev/flutter/dart-ui/FilterQuality.html) efficient.

แต่เนื่องจากแอพพลิเคชั่นของเราต้องรองรับอุปกรณ์หลายๆขนาด ซึ่งจะมีปัจจัยที่สำคัญมากอันนึงที่จะต่างกันระหว่างอุปกรณ์ซึ่งก็คือ Device Pixel Ratio (DPR) ที่จะเป็นตัวกำหนดขนาดของพื้นที่ที่ต้องใช้แสดงรูปภาพ ทำให้มีโอกาสสูงมากที่ขนาด pixels ของรูปภาพมีขนาดไม่ตรงกับขนาดของพื้นที่แสดงรูปภาพ ทั้งนี้ก็รวมถึงการใช้งานที่พื้นที่แสดงรูปภาพเล็กกว่าขนาดของรูปภาพด้วย

FilterQuality จะเป็นตัวช่วยทำให้การเรนเดอร์รูปภาพดีขึ้นในกรณีที่พื้นที่แสดงกับขนาด pixels ของรูปภาพไม่ตรงกัน แต่จะมีผลมากน้อยขนาดไหน ต่อไปเราจะไปดูที่ demo application กันเลยนะครับ

## Demo แอพพลิคเชั่น

จุดประสงค์ของแอพพลิเคชั่น คือช่วยให้เห็นความสัมพันธ์ของ **ขนาดไฟล์ตั้งต้น x ขนาดพื้นที่แสดง(Container) x FilterQuality** ได้แบบจับต้องได้ผ่านอุปกรณ์จริงหรือ simulator ซึ่งถ้าดูด้วยตาเปล่าจะแนะนำให้ดูจากอุปกรณ์จริงจะดีกว่า simulator ที่มีเรื่องความละเอียดของหน้าจอคอมพิวเตอร์เรามาเป็นอีกปัจจัยหนึ่ง โค้ดของ demo นี้สามารถ copy ได้จากท้ายบทความเลยนะครับ

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733901979230/4298bc29-2773-4eb7-9802-e442438fbb81.gif align="center")

ในส่วนต่อไปจะสรุปประเด็นหลักที่เกี่ยวกับเรียกใช้ Image และ FilterQuality ใน Flutter พร้อมกับภาพประกอบจาก demo แอพพลิเคชั่นเพื่อยืนยันประเด็นที่จะพูดถึงต่อไปไ

แต่ว่าในรูปประกอบจะมีการซูมเพื่อ pixels peeping จากภาพ screenshot ผ่าน ​ipad นะครับเพราะถ้าใช้ขนาดปกติเป็นรูปภาพประกอบในบทความจะเห็นความแตกต่างได้ไม่ชัด แนะนำว่าให้ลอง copy แอพพลิเคชั่นไปลองในอุปกรณ์จริงหากมีข้อสงสัยหรืออยากรู้ว่าของจริงจะมีผลกระทบมากน้อยขนาดไหนครับ

## ขนาดภาพที่ต้องใช้เพื่อรูปภาพที่ชัดที่สุด

สมมุตว่าเรามี Container ขนาด 50×50 pixels และรูปภาพ ขนาด 50×50 pixels นี่คือสิ่งที่เราได้

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733905504870/f5eec1ef-bc77-47c6-91fd-b1f6daa810da.png align="center")

จะสังเกตุได้ว่าภาพที่ได้ไม่ว่าจะมีค่า FilterQuality เท่าไหร่ ภาพที่แสดงก็ไม่คมชัด ส่วนการที่เพิ่ม FilterQuality เข้าไปเป็นเพียงการช่วยให้ภาพดูแตก(pixelate) น้อยลงเท่านั้น

ทำไมภาพขนาด 50×50 pixels ถึงมีขนาดไม่เพียงพอกับ Container และทำให้ภาพแตก? เพราะมีอีกสิ่งหนึ่งที่เราต้องการคำนึงถึงนั่นก็คือค่า DPR

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733905970156/2979d854-e156-4d25-b6a5-ce5f6611aa38.png align="center")

ซึ่งสำหรับ ipad 11” M3 จะมีค่า DPR อยู่ที่ 2.0 ซึ่งค่านี้ จะถูกนำไปคูณกับค่า logical pixel ของ container ทำให้รูปที่ต้องใช้ใน Container นี้ จะมีขนาดเท่ากับ (50×50)\*2.0 pixels หรือเท่ากับ 100×100 pixels นั่นเอง ซึ่งจากเดโม่แอพก็จะเห็นได้ชัดว่าที่ Container ขนาด 50×50 ภาพตั้งต้นขนาดตั้งแต่ 100×100 จะภาพชัดเจนกว่าภาพตั้งต้นขนาด 50×50 อย่างเห็นได้ชัด

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733906592887/2dc3c098-d01f-40cd-8582-fd7e184c9daa.png align="center")

และในเมื่อแอพพลิเคชั่นของเราต้องรองรับหลายๆอุปกรณ์ เราสามารถนำข้อสรุปนี้ไปใช้หาขนาดภาพที่ต้องใช้ในโปรเจคได้ง่ายๆคือ

> ขนาดภาพตั้งต้น = ขนาดพื้นที่แสดงรูปภาพ \* Device Pixel Ratio (ที่สูงสุดของอุปกรณ์ที่เราต้องการรองรับ)

## ภาพถูกบีบอัดมากไปก็แตกได้

จากภาพด้านล่างจะเห็นได้ว่าที่ขนาด container 50×50 ภาพตั้งต้นขนาด 200×200 มีอาการแตกแบบการโดนบีบอัด (ไม่เหมือนการแตกจาก pixelate ที่แตกแบบกระจายทั้งภาพ)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733907489280/cdac0cb1-7a05-44ec-bee1-580bcec7a6c1.png align="center")

เราสามารถแก้ไขได้ง่ายโดยใช้ `FilterQuality.medium` โดยอ้างอิงจาก [Flutter API docs](https://api.flutter.dev/flutter/dart-ui/FilterQuality.html)

> When scaling down, [medium provi](https://api.flutter.dev/flutter/dart-ui/FilterQuality.html)des the best quality especially when scaling an image to less than half its size or for animating the scale factor between such reductions. Otherwise, [low an](https://api.flutter.dev/flutter/dart-ui/FilterQuality.html)d [high pro](https://api.flutter.dev/flutter/dart-ui/FilterQuality.html)vide similar effects for reductions of between 50% and 100% but the image may lose detail and have dropouts below 50%.

จากเอกสารทาง Flutter จะแนะนำให้ใช้ FilterQuality.medium ซึ่งจะทำให้ภาพชัดมากกว่า low และ high ที่ให้คุณภาพใกล้เคียงกันเมื่อใช้ในพื้นที่ที่เล็กกว่า ซึ่งจะมีผลลัพธ์ตามรูปภาพจากเดโม่แอพด้านล่าง

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733908821781/5ec3b52f-b5ee-446c-83f9-4290f0dc2b02.png align="center")

แต่โดยส่วนตัวผมเองนั้นแยกไม่ค่อยออกในจุดนี้ซักเท่าไหร่เพราะมันใกล้เคียงกันมากแต่เอาเป็นว่าเราใช้ค่าตามที่ Flutter แนะนำดีกว่า ซึ่งถ้าเอาสรุปมาใช้ในการทำงานจริงผมขอสรุปตามนี้ไปเลยว่า

> ในตอนเรียกใช้ Image ให้เซตค่า `FilterQuality.medium` ไว้เป็น default value

## สรุป

หลังจากที่ได้ลองเล่นเดโม่แอพพลิเคชั่นจะมีสองประเด็นที่ต้องจำไว้ใช้ในเวลาที่พัฒนาแอพพลิเคชั่นนั่นก็คือ อย่าลืมคำนึงถึงค่า DPR ของอุปกรณ์ที่ต้องรองรับ และ เซตค่า default value ของ FilterQuality เป็น FilterQuality.medium เพื่อเป็นค่ากลางที่ช่วยแสดงผลภาพได้ดีครอบคลุมหลายการใช้งาน

หวังว่าบทความนี้จะทำให้เข้าใจถึงจุดประสงค์ของ FilterQuality และหลักการใช้งานของ FilterQuality ได้มากขึ้นนะครับ และถ้าอยากลองนำโค้ดไปทดลองด้วยตัวเองสามารถ clone จากที่ลิงค์ Github ด้านล่างได้เลยครับ

## References

* Github: [https://github.com/Khanin-devmode/flutter-filter-quality-example.git](https://github.com/Khanin-devmode/flutter-filter-quality-example.git)
    
* [https://api.flutter.dev/flutter/widgets/Image/filterQuality.html](https://api.flutter.dev/flutter/widgets/Image/filterQuality.html)
    
* [https://api.flutter.dev/flutter/dart-ui/FilterQuality.html](https://api.flutter.dev/flutter/dart-ui/FilterQuality.html)