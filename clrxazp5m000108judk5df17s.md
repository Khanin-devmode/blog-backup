---
title: "วิธีการดู Widget Tree ของ Widget ที่มาจาก Package ที่เรียกใช้ใน Flutter"
seoTitle: "วิธีดู Widget Tree จาก Package ใน Flutter"
seoDescription: "ดู Widget Tree ใน Flutter: ใช้ Widget Inspector, ตั้งค่า path, และ clone package"
datePublished: Sun Jan 28 2024 09:33:34 GMT+0000 (Coordinated Universal Time)
cuid: clrxazp5m000108judk5df17s
slug: flutter-package-widget-tree
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1706434332479/5150567d-6b2c-47c4-a621-6d5ccb6b4f3e.png
tags: flutter, flutter-insector

---

Widget Inspector ของ Flutter เป็นเครื่องมือที่ขาดไม่ได้เลยในตอนที่เราพัฒนาแอพพลิเคชั่นด้วย Flutter พอเราใช้ไปจนชินจะสังเกตุได้ว่าในบางครั้งที่เราจะต้องการดูโครงสร้าง widget ลึกๆ เราจะไม่สามารถเห็น widget tree ของ widget ที่เรา import มาจาก package อื่นและยังไม่สามารถเลือกที่จะ inspect ส่วนประกอบของ widget นั้นได้อีกด้วย เราต้องไปไล่ดูในไฟล์เอาเองว่ามีโครงสร้างอย่างไร การที่เราดูสามารถ widget tree ของ widget ได้โดยตรงนี้ทำให้เราเข้าใจได้มากขึ้นว่า widget นึ้ถูกเขียนมาอย่างไรและบางครั้งเราอาจจะ reverse engineer มาสร้าง widget ที่เราต้องการได้ด้วย

# Widget Tree ตามปกติ

ตัวอย่าง Widget Tree ตามปกติ

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706192914306/b5382fa6-54af-4b96-9899-7d626bdf6aba.png align="center")

จากรูปตัวอย่างจะเป็นการเข้าไป inspect widget ได้ในระดับปกติซึ่งจะเห็นแค่ Elevated Button และ Text เท่านั้น

จุดประสงค์ที่ทางทีม Flutter ตั้งใจจะปิดการเข้าถึงนี้เพราะทางทีม Flutter เห็นว่า widget tree จะดูซับซ้อนเกินไปถ้าไม่ปิดไว้ และเป็นอุปสรรคในการใช้งาน Widget Inspector มากกว่าที่จะเป็นประโยชน์ แต่เนื่องจากมีคนรีเควสท์ตรงส่วนนี้เข้ามาจึงได้ เพิ่มวิธีที่จะเข้าถึง widget tree จาก แพ๊คเกจได้ อ้างอิงจากลิงค์ในหัวข้อ References ด้านล่าง

# Widget Tree ที่เข้าถึง widget ใน Package

ภาพตัวอย่างที่มีแก้ config ค่าให้เห็น Widget Tree จาก pacakgeแล้ว

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706193549966/e60c998a-bff9-4d14-9eb8-89be5ac9e69d.png align="center")

จากภาพตัวอย่าง จะเห็นได้ว่า ใน Elevated Button เดิมๆนั้น ภายในมี Widget Tree ที่หลายชั้นมาก ซึ่งนี่เป็นเหตุผลว่าทำไม Flutter Dev Tool ถึงปิดกั้นการเข้าถึงเป็นค่าตั้งตั้น เพราะไม่เช่นนั้นแล้วจะดูซับซ้อนมากเกินความจำเป็นหากยิ่งเป็นสำหรับผู้เริ่มต้นด้วย แต่สำหรับผู้ที่เริ่มมีประสบการณ์สิ่งนี้จะมีประโยชน์มากหากเราต้องการเรียนรู้ถึงโครงสร้างของ Widget ที่เราใช้

# วิธีการตั้งค่า

วิธีการก็คือใส่ path ของ package ที่อยู่ในเครื่องของเราเข้าไปใน setting ของ widget inspector ซึ่งหมายความว่าเราจะต้อง clone package ลงมาไว้ในเครื่องของเราก่อนด้วย

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706433683402/0b6bd837-56ae-497c-a1cf-75f4803c4746.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706434041299/2e0568b4-f96b-41e2-91f3-de81f46f2245.png align="center")

เพียงเท่านี้เราก็ดูรายละเอียกของ widget ได้ทั้งหมดและที่ดีกว่านั้นก็คือตอนที่เราเลือก inspect widget ตัว editor ก็จะชึ้ไปยังไฟล์และตำแหน่งโค้ดของ widget ตัวนั้นให้โดยอัตโนมัติเหมือนตอนที่เรา inspect code ที่เรากำลังเขียนอยู่ได้เลย หวังว่าข้อมูลนี้จะเป็นประโยชน์กับทุกคนไม่มากก็น้อยนะครับ ขอบคุณครับ

# References

* [https://github.com/flutter/devtools/issues/3919](https://github.com/flutter/devtools/issues/3919)
    
* [https://github.com/flutter/devtools/issues/3941](https://github.com/flutter/devtools/issues/3941)