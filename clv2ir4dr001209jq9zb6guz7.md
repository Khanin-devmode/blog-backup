---
title: "แนะนำการใช้ Bloc ด้วยการสร้างเคาน์เตอร์แอพในหน้าเดียว แบบจบๆ"
seoTitle: "สร้างแอพเคาน์เตอร์ด้วย Bloc ง่ายๆ"
seoDescription: "Learn how to use the Bloc library for efficient state management in Flutter through a simplified single-page counter app example"
datePublished: Tue Apr 16 2024 15:08:49 GMT+0000 (Coordinated Universal Time)
cuid: clv2ir4dr001209jq9zb6guz7
slug: bloc-example-counter-app
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1713279638229/e8503ac3-89a1-4c2e-b7e9-168691cb9695.webp
tags: flutter, flutter-bloc

---

## Bloc คืออะไร?

Bloc (Business Logic Component) เป็น library สำหรับ state management รูปแบบหนึ่งทีนิยมใช้กันใน Flutter จุดประสงค์ของการใช้ state management หลักๆก็คือการอ่านและเขียน state ที่ใช้ร่วมกันในหลายๆวิดเจ็ตได้สะดวกและมีประสิทธิภาพมากขึ้น เพราะไม่เช่นนั้นการอัพเดต state จะต้องเป็นการส่งต่อจากวิดเจ็ตแม่ลงมาเรื่อยๆ ซึ่งยากต่อการเขียนและแก้ไขอย่างมาก

Bloc library เองจริงๆก็มีตัวอย่าง ในการใช้ Bloc กับเคาน์เตอร์แอพอยู่แล้ว ซึ่งโค้ดที่เป็นตัวอย่างในบทความนี้ก็จะคัดลอกมาจากตัวอย่างจากทางเอกสารนี้เลย แต่จะคัดเลือกและเรียบเรียงให้สั้นและเข้าใจง่ายขึ้นและรวบให้อยู่ในไฟล์เดียวเพื่อความสะดวกในการเรียนรู้(โค้ดอยู่ที่ท้ายบทความ) หากนำไปใช้จริงเราต้องแยกไฟล์ตามโครงสร้างที่โปรเจคใช้ด้วย อย่างเช่น Clean Architecture

Bloc library จะมีวิธีการสร้างสองรูปแบบคือรูปแบบที่เป็นแบบมาตรฐานของ Bloc ที่เป็น event-driven และอีกรูปแบบนึงที่เป็น Cubit ที่เป็น function-driven ซึ่ง Cubit จะมีการใช้งานที่ตรงไปตรงมาคือเรียก function เพื่ออัพเดต state เลย การเลือกใช้ระหว่าง event-drive และ funciton-drive มักจะขึ้นอยู่กับความซับซ้อนของแอพพลิเคชั่นและความถนัดของนักพัฒนา และในตัวอย่างจากเอกสารก็จะเป็นการใช้ Cubit เพราะความเรียบง่ายในการใช้งานนั่นเอง ต่อไปเราจะเริ่มการใช้ Bloc library โดยตามขั้นตอนต่อไปนี้

## Import package

เพิ่ม bloc และ flutter\_bloc ใน pubspec.yaml

```dart
dependencies:
  bloc: ^8.1.0
  flutter:
    sdk: flutter
  flutter_bloc: ^8.1.1
```

## สร้าง Cubit เพื่อกำหนด state

Cubit จะเป็น class ที่เราใช้เก็บค่า state รวมถึง function ในการแก้ไขเปลี่ยนแปลงอัพเดต state ของเรา เนื่องจากแอพพลิเคชั่นเคาน์เตอร์ไม่ได้มีความซับซ้อนจะมีแค่เพียงสอง function ในการอัพเดต state ในส่วนที่ลูกศรชี้เป็นจุดหลักที่เราต้องเข้าใจในการสร้าง Cubit

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1713238563704/417315f0-0edd-4165-9a36-908c9beae021.png align="center")

หาก Cubit ที่เราต้องการใช้ในอนาคตมีความซับซ้อนหรือต้องเก็บหลายค่า เราจะต้องสร้าง class ใหม่มาแล้วนำมา cast type ใน Cubit (จากตัวอย่างจะเป็น type int) และ การ emit ค่าจะต้องเป็นการ emit ค่าใหม่ในรูปแบบ immutable (เป็น instance ใหม่ ที่ไม่ใช่ตัวแปรเดิมแต่เปลี่ยนค่า) เท่านั้น

## ยัดเยียด Cubit ลงไปใน widget tree ด้วย BlocProvider

`BlocProvider` จะทำให้วิดเจ็ตที่อยู่ภายใต้ widget tree ของ `BlocProvier` สามารถเข้าถึง state ใน Cubit ได้ ผ่านวิดเจ็ตของ Bloc ต่างๆอย่างเช่น `BlocBuilder` ถ้าเราไม่กำหนดว่าเราจะให้ Cubit ไปอยู่ที่ไหนเราก็ไม่สามารถสร้างวิดเจ็ตจาก state ของ Cubit ในขั้นตอนต่อไปได้

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1713152046991/f5c00894-8ac2-4023-84f7-9442c00d2290.png align="center")

ในตัวอย่งจะเป็นการครอบ widget tree ที่หน้า `MyHomePage` จริงๆเราสามารถประกาศ `BlocProvider` ไว้เหนือ widget tree กว่านี้ได้ แต่ให้ระวังว่าหากแอพพลิเคชั่นขยายขึ้นการครอบ `BlocProvider` ไว้บนสุดทั้งหมดจะไม่ใช่แนวทางที่ดีที่สุด เราควรจะครอบเฉพาะในส่วนที่ใช้จะเหมาะสมในการใช้งานมากกว่า

ถ้าเราไม่กำหนด `BlocProvider` ใน widget tree ของเรา จะเกิด error ขึ้น ข้อเสียจุดนึงในความคิดเห็นส่วนตัวก็คือ เราจะไม่เห็น error ในส่วนนี้ได้ถ้าเราลืมใส่ `BlocProvider` ใน widget tree ได้ทันทีจะเห็นก็ต่อเมื่อรันแอพแล้วเท่านั้น เพราะฉะนั้นเมื่อเห็น error ตอนที่เราเรียกใช้ `BlocBuilder` หรือ วิดเจ็ตของ Bloc ต่างๆ ให้เช็คให้แน่ใจว่าเรามี `BlocProvider` ครอบในส่วนของ widget tree ที่เราใช้งานแล้วรึยัง

## สร้างวิดเจ็ตจาก state ใน Cubit ด้วย BlocBuilder

หลังจากที่เราครอบ widget tree ด้วย `BlocProvider` แล้ว เราก็จะเข้าถึง state ใน Cubit ของเราด้วยวิดเจ็ตของ Bloc ต่างๆ ซึ่งในที่นี้จะเป็น `BlocBuilder` เพื่อไปสร้างหรือรีเทิร์นวิดเจ็ต Text ที่แสดงผล state ใน `CounterCubit` ของเรา

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1713238801170/0c545454-c8e6-4d06-8965-d385ef7f26d7.png align="center")

ในตัวอย่าง `BlocBuilder` เป็นเพียงวิดเจ็ตหนึ่งเท่านั้นในการนำ state ไปใช้ ใน bloc library ยังมีอีกหลายวิดเจ็ตให้การนำ state ไปใช้ขึ้นอยู่กับรูปแบบของ state ที่เราใช้และจุดประสงค์ในการใช้งานด้วย อย่างเช่น เราใช้เพื่อที่จะอัพเดต UI ด้วยหรือว่าอัพเดต state ไว้ก่อนแล้วให้วิดเจ็ตอื่นอัพเดต UI อีกที ตรงนี้เป็นจุดหนึ่งที่ต้องตระหนัก เพราะถ้าเราไม่เข้าใจเราก็จะใช้แต่ `BlocBuilder` ตามตัวอย่างแต่เพียงอย่างเดียวซึ่งจะส่งผลต่อประสิทธิภาพแอพของเราได้ รายละเอียดของวิดเจ็ตทั้งหมดสามารถดูได้จากลิงค์ด้านล่างนี้  
[https://bloclibrary.dev/flutter-bloc-concepts/#bloc-widgets](https://bloclibrary.dev/flutter-bloc-concepts/#bloc-widgets)

### สิ่งที่ต้องระวัง: อย่าหลง context

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1713276760152/a1a8394a-8855-420f-9bf1-e66640493e2c.png align="center")

หากเราต้องการเข้าถึง build context เช่นค่าจาก theme ต่างๆ ต้องเช็คให้ดีว่าเราใช้ context ถูกอันรึเปล่า เพราะ BlocBuilder จะใช้ context ของมันเองแยกจาก context หลักของเรา

## เรียกใช้ method ของ Cubit

การเรียกใช้ method ใน Cubit ของเราจะใช้ได้ก็ต่อเมื่อเราครอบ widget tree ด้วย `BlocProvider` แล้วเช่นเดียวกับ `BlocBuilder` จากตัวอย่างจะเป็นการเรียก method ผ่านปุ่ม `FloatingActionButton` สองปุ่มสำหรับเพิ่มและลดเคาน์เตอร์

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1713277925905/049cd57f-613d-4e6d-9179-371b9f9416f8.png align="center")

## สรุป

บทความนี้ต้องการจะให้คนที่เข้ามาสัมผัส Bloc libary ครั้งแรกสามารถเข้าใจได้ง่าย และนำไปใช้งานต่อได้ทันทีด้วยการนำเสนอ ยกตัวอย่าง และไฮไลท์จุดสำคัญที่ต้องใช้ในการใช้งาน Bloc library พร้อมยังมีโค้ดที่อยู่ในไฟล์เดียวให้นำไปทดลองรันเพื่อศึกษา แต่อย่าลืมว่าในโปรเจคจริงต้องทำตามโครงสร้างของโปรเจคแต่ละโปรเจคด้วย หวังว่าบทความนี้จะมีประโยชน์กับผู้ที่เริ่มใช้ครั้งแรกและสามารถนำไปต่อยอดใช้งานแบบลึกซึ้งขึ้นได้นะครับ

## แถม

### ทำไม Cubit ถึงชื่อ Cubit

คำว่า Cubit(สี่เหลี่ยมจิ๋ว คิวท์ๆ) ต้องการจะสื่อให้เหมือนชื่อเล่นหรือส่วนย่อที่ใช้งานได้ง่ายของ Block (Bloc) ที่หมายถึงของที่เป็นสี่เหลี่ยมขนาดใหญ่(กว่า cubit)

### BlocObserver ที่พูดถึงในเอกสารทางการคืออะไร

ในเอกสารของ Bloc library จะพูดถึง BlocObserver ก่อนเลย ซึ่งจริงๆแล้วไม่ใช่ตัวหลักที่ทำให้ bloc ทำงานแต่จะทำหน้าที่แค่เฝ้าดู(observe)ถึงการเปลี่ยนแปลงของ bloc เท่านั้น เหมาะนำไปใช้ในการ debug ค่า state ของเราว่ามีการเปลี่ยนแปลงอย่างไรบ้าง แต่ต้องยอมรับว่าตอนอ่านเอกสารแล้วเจอหัวข้อนี้อันแรกก็เหมือนโดนเบรคเบาๆเลย ทำให้รู้สึกว่า bloc ใช้ยาก ทั้งๆที่จริงไม่ใช่ประเด็นหลักในการใช้งาน Bloc library เลย

### References

* [https://bloclibrary.dev/tutorials/flutter-counter/](https://bloclibrary.dev/tutorials/flutter-counter/)
    

## Code

%[https://gist.github.com/Khanin-devmode/8a49ef8fdecccf12b397b84dbd8b6ec9]