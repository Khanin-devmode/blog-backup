---
title: "แนะนำการใช้ Bloc ด้วยเคานท์เตอร์แอพแบบต้นฉบับด้วย event-driven ในไฟล์เดียวจบๆ (เหมือนเดิม)"
seoTitle: "ตัวอย่างการใช้ Bloc กับ Counter App แบบ Event-Driven"
seoDescription: "Explore the simplicity of using the Bloc library with an event-driven counter app, comparing it to the Cubit approach for clarity"
datePublished: Sun Apr 28 2024 14:15:48 GMT+0000 (Coordinated Universal Time)
cuid: clvjm55pe000h0al3d5z1gpas
slug: bloc-example-counter-app-event-driven
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1714215494856/b1563d2e-ef16-4eaf-b9b2-5a1113f8c986.webp
tags: flutter, bloc, flutter-examples

---

ต่อจาก[บทความที่แล้ว](https://blog.nintech.dev/bloc-example-counter-app) เกี่ยวกับการใช้ Bloc library ด้วย Cubit ในบทความนี้จะเป็นบทความการใช้ Bloc library เช่นเดียวกันแต่เป็นรูปแบบ event-base ที่เป็น Bloc pattern แบบเต็มตัว จุดประสงค์คือให้เห็นการใช้งานที่ง่ายที่สุดในเคาน์เตอร์แอพ และเปรียบเทียบกับการใช้แบบ Cubit ได้ชัดเจน เพื่อให้เห็นข้อดีและข้อเสีย เพื่อเป็นข้อมูลในประกอบการตัดสินใจเลือกว่าจะใช้วิธีไหนในการพัฒนาแอพพลิเคชั่น

อย่าลืมนะครับว่านี่เป็นการรวมไฟล์มาในหน้าเดียวเพื่อให้ง่ายต่อการเรียนรู้ การใช้งานจริงต้องวางตาม structure ของโปรเจคด้วยนะครับ

## การใช้งานในส่วนที่เหมือนกันกับ Cubit

ในหัวข้อนี้ผมจะไม่ได้ยกโค้ดขึ้นมาเป็นตัวอย่างเพราะกลัวว่าจะทำให้ซับสนและซ้ำซ้อนกับของเดิมได้ อยากให้โฟกัสกับจุดที่ต่างกันเท่านั้นจริงๆดีกว่า ซึ่งหัวข้อที่เหมือนกันในการใช้ก็คือ

* [การ Import package](https://blog.nintech.dev/bloc-example-counter-app#heading-import-package)
    
* [การยัดเยียด state ไปใน widget tree ด้วย BlocProvider](https://blog.nintech.dev/bloc-example-counter-app#heading-cubit-widget-tree-blocprovider)
    
* [สร้างวิดเจ็ตจาก state ด้วย BlocBuilder](https://blog.nintech.dev/bloc-example-counter-app#heading-state-cubit-blocbuilder)
    

## การใช้งานที่แตกต่างจาก Cubit

ในส่วนหัวข้อด้านล่างนี้จะเป็นส่วนที่แตกต่างจากการใช้งาน Bloc ด้วย Cubit ทีเป็น function-driven โดยที่ตัวอย่างที่เราใช้ในบนความนี้จะเป็น Bloc library แบบ event-driven โดยที่ event จากแอพเคาน์เตอร์ก็จะมีเพียงแค่สอง event สำหรับเพิ่มและลดเคาน์เตอร์

### สร้าง Bloc class และกำหนด event

เมื่อเทียบกับ Cubit แล้ว Bloc จะมีการกำหนด Event ขึ้นมาแทนที่จะเรียกผ่าน method ด้วยตรง ในการที่จะ update state ต้องมีการเรียก event เพื่อ `emit` ค่าใหม่ออกไป

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1714279221787/2eb32939-93c5-45b3-b50e-a252ac037ed8.png align="center")

โดยที่ `CounterEvent` ของเราจะเป็น abstract class ซึ่งหมายถึงว่าคลาสนี้จะเป็นคลาสที่ไม่ได้มีการเรียก constructor เพื่อสร้าง instance ได้โดยตรง แต่จะเป็นคลาสที่จะต้องไป extends เพื่อประกาศเป็น class ใหม่ ซึ่งทำให้ class ที่ extends class นี้ไป (`IncrementEvent` และ `DecrementEvent`) ยังถือว่าเป็น class เดียวกัน ซึ่งก็คือ `CounterEvent` class

### การเรียก event เพื่ออัพเดตค่า state

ในการเรียก event ก็แทบจะคล้ายกับการเรียก method เลยแต่แทนที่จะเรียก method โดยตรง แต่จะเป็นการ add event เข้าไปที่ Bloc คล้ายๆกับการใส่ event เข้าไปใน stack แล้ว Bloc จะเช็คว่ารับ event อะไรมาแล้ว update state ตามเงื่อนไขที่เขียนไว้

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1714313139677/22901263-b029-45e4-936c-b203a87ffc10.png align="center")

## สรุป

ในบทความนี้ได้มีการแสดงการเขียนเคาน์เตอร์แอพด้วย Bloc แบบ event-driven เพื่อให้เห็นข้อแตกต่างที่ชัดเจนกับแบบ function-driven ด้วย Cubit ที่เขียนในบทความที่แล้ว จะเห็นว่าในการเขียนสิ่งเดียวกันการเขียนแบบ Bloc ปกติจะใช้มีโค้ดที่ต้องเตรียมไว้ (boiler plate) มากกว่าเพื่อจะซัพพอร์ตการเรียก event ต่างๆ โดยตามที่เอกสารแนะนำก็คือให้เลือกใช้แบบ Bloc ในแอพที่ต้องการสเกลแล้วต้องการรับกับ event ที่ซับซ้อนในแอพพลิเคชั่น แต่ก็ยังทิ้งท้ายไว้อีกว่าสุดท้ายก็ขึ้นอยู่กับทางทีมนักพัฒนาเองที่สบายใจหรือถนัดที่จะใช้แบบไหนมากกว่ากัน ส่วนตัวผมยังคงไปทางการใช้ Cubit มากกว่าเพราะการใช้งานที่ตรงไปตรงมาและแอพพลิเคชั่นที่ทำเป็นโปรเจคส่วนตัวก็ไม่ได้มี event อะไรซับซ้อนด้วย หวังว่าบทความนี้จะเป็นจุดเริ่มต้นให้ผู้ที่สนใจใช้ Bloc library เริ่มใช้งานได้ง่ายขึ้นนะครับ

## Source Code

%[https://gist.github.com/Khanin-devmode/a2b03574ad7ed3de88c366e455def5c8]