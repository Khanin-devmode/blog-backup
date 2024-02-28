---
title: "FormState vs. FormFieldState ใน Flutter"
seoTitle: "FormState vs. FormFieldState: Key Differences"
seoDescription: "Understand FormState for validating entire forms and FormFieldState for individual TextFormField in Flutter. Perfect for form management"
datePublished: Wed Feb 28 2024 12:00:30 GMT+0000 (Coordinated Universal Time)
cuid: clt5qw1zy000608l3fnpjc2nw
slug: formstate-vs-formfieldstate-flutter
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1709109997671/15b0bdcd-c97d-4e9f-a3d4-5d9aaeba2d0e.webp
tags: flutter, form-validation

---

*TLDR;* `FormState` *จะเก็บค่า state เช่นการ validate สำหรับฟอร์มทั้งฟอร์ม แต่* `FormFieldState` *จะเก็บค่า state สำหรับ* `TextFormField` *เพียงอันเดียว*

ในตอนที่เราโค้ดฟอร์มในแอพพลิเคชั่นของเรา เราจะต้องใช้ `GlobalKey` ที่ cast ด้วย `FormState` , `GlobalKey<FormState>`,หรือ `FormFieldState` , `GlobalKey<FormFieldState>` เพื่อใช้เช็ค state ของสถานะฟอร์มของเรา เช่นการ validate หรือ ไม่ validate แล้วสอง Class นี้มันใช้ต่างกันอย่างไรใช้ตอนไหน เราจะมาดูจากตัวอย่างด้านล่างกัน

จริงๆแล้วสอง Class นี้จะมีการใช้งานเฉพาะเลยก็คือ `FormState` ใช้สำหรับ `Form` วิดเจ็ต เพื่อ vaildate ทุก `FormField` ที่อยู่ใต้ `Form` เท่านั้น หาก ใน `FormState` ไปใช้กับ `TextFormField` ค่า currentState จะเป็น null ทันที เพราะ FormState จะรับ type ที่เป็น Form เท่านั้น เราสามารถดูได้ง่ายๆ จาก class definition ของ `FormState` ได้เลย

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1709106516410/260e5b16-a765-4d16-b5fb-ca15a4ba1cc0.png align="center")

ในทำนองเดียวกัน FormFieldState ก็จะเป็นการใช้เฉพาะสำหรับวิดเจ็ดประเภท FormField เท่านั้น ซึ่ง TextFormField ก็เป็น class ที่สร้างต่อมาจาก FormField ทำให้สามารถใช้ FormFieldState ได้ และหากเราใน FormState มาใช้กับ TextFormField ค่า currentState ของ FormState ก็จะเป็น null เช่นกัน เพราะใช้ไม่ถูกประเภทกับ Type ที่รองรับ

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1709106239195/1c2c10ab-7b6d-41b8-9421-3c94d3dfa988.png align="center")

ประโยชน์ในการใช้งานของทั้งสองประเภทก็คล้ายๆกันคือเพื่อใช้สำหรับ method ต่างๆของ State อย่างเช่น validate หรือ reset ต่างกันแค่เพียง FormState ใช้สำหรับ Form ที่ครอบทุก FormField ส่วน FormFieldState จะใช้สำหรับ FormField เดียวเท่านั้น แต่ถึงแม้ว่าจุดประสงค์การใช้งานจะคล้ายกันแต่ใช้สลับกันไม่ได้นะ

# แถม TextField vs. TextFormField

*TLDR;* `TextFormField` *ใช้ร่วมกับ* `Form` *เพื่อการ validate ถ้าต้องการ input text ธรรมดาใช้* `TextField`

สมมุติว่าเราไม่มีวิดเจ็ต `TextFormField` ในการสร้าง `Form` สิ่งที่เราต้องทำเพื่อให้ validate `TextField` ได้คือเราต้องครอบ `TextField` วิดเจ็ตด้วย `FormField` วิดเจ็ตอีกที และเนื่องจากการ validate `TextField` เป็นเรื่องที่ทุกแอพต้องใช้บ่อยๆ ทาง Flutter เลยรวมมาให้เป็น `TextFormField` เพื่ออำนวยความสะดวกให้กับ Flutter ไปเลย สรุปง่ายๆก็คือ ถ้าเราต้องการให้ `TextField` มีการ validate หรือใช้ในฟอร์มให้ใช้ `TextFormField` ถ้านอกเหนือจากนั้นก็ใช้ `TextField` ธรรมดาได้เลย

# สรุป

หวังว่าบทความนี้จะช่วยให้เข้าใจ การใช้งานของวิดเจ็ตและ class ที่มีชื่อใกล้กัน อย่าง FormState vs. FormFieldState หรือ TextField vs TextFormField ได้ง่ายขึ้น เพราะทุกคนที่เห็นวิดเจ็ตกลุ่มนี้ครั้งแรกๆก็จะมีความสงสัยว่ามันต่างกันมากขนาดไหนเพราะชื่อมันคล้ายกันมากๆเลย ของสรุปเป็นหัวข้อไว้อีกทีนะครับ

* FormState ใช้เฉพาะกับ Form เพื่อ validate FormField ใน Form ทั้งหมด
    
* FormFieldState ใช้กับ FormField วิดเจ็ต และวิดเจ็ตที่มี FormField เป็นส่วนประกอบอย่างเช่น TextFormField
    
* TextFormField ใช้คู่กับ Form เพื่อการ validate
    
* TextField ใช้เมื่อไม่มีความจำเป็นต้อง validate