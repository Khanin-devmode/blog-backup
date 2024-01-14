---
title: "การคำนวณขนาดใน Flutter จากโค้ดถึงอุปกรณ์จริง"
seoTitle: "คำนวณขนาดใน Flutter จากโค้ด สู่อุปกรณ์จริง"
seoDescription: "คำอธิบาย Flutter: วิธีคำนวณขนาดจาก Logical Pixels, DPR สำหรับอุปกรณ์ เพื่อกานสื่อสารกับดีไซเนอร์ และพัฒนาแอพมือถือ"
datePublished: Sun Jan 14 2024 09:09:16 GMT+0000 (Coordinated Universal Time)
cuid: clrd9yior000a08jrd1eibedn
slug: flutter-logical-pixels-to-physical-pixels
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1705223185860/45a8df96-91a3-4e90-b19f-00349d89d6f6.png
tags: flutter

---

ในการสื่อสารกับดีไซเนอร์ระหว่างการที่เราพัฒนาแอพพลิเคชั่นบนมือถือ เรามักจะเจอคำถามว่า ฟอนท์ไซส์ ตอนที่ดีไซเนอร์ออกแบบ ตอนไปแสดงจริงในเครื่องจะมีขนาดเท่าไหร่ เครื่องเล็กจะเป็นอย่างไร เครื่องใหญ่จะเป็นอย่างไร ถึงแม้ว่า Flutter เองจะรองรับการคำนวนเพื่อให้แสดงผลได้อย่างถูกต้องในทุกอุปกรณ์แล้ว แต่หากเราเข้าใจวิธีการคำนวนจากค่าที่เราใส่เข้าไปจนแสดงผลออกมา จะทำให้เรามั่นใจและสื่อสารกับดีไซเนอร์ได้อย่างชัดเจนขึ้นด้วย

# หน่วยที่ Flutter ใช้ : Logical pixels

หน่วยที่เราใช้ใน Flutter จะเป็นขนาดที่เรียกว่า Logical pixels ซึ่งจะเป็นขนาดที่ ทั้ง Flutter (รวมถึง UI framework อื่น) ตั้งใจจะใช้เพื่อความสะดวกในการกำหนดขนาด ไม่ต้องกังวลเรื่องขนาดของอุปกรณ์ที่มีจำนวนนับไม่ถ้วนในปัจจุบัน Logical pixels จะถูกคำณวนเป็น Rendering pixels โดยอัตโนมัติในการที่แอพจะเรนเดอร์ UI ของเรา ซึ่งการคำนวณจะเป็นตามสูตรดังนี้

Rendering pixel = Logical Pixel \* DPR (Device pixel ratio)

โดยค่า DPR จะถูกตั้งค่ามาจากจากซอฟท์แวร์เพื่อให้มีค่าที่เหมาะสมสำหรับแต่ละอุปกรณ์ หรือคำนวนให้เหมาะสมอย่างอัตโนมัติโดยเวบบราวเซอร์ โดยอุปกรณ์ที่มีความละเอียดสูงมันจะมีค่า DPR ที่สูงตามไปด้วย

ยกตัวอย่าง หากอุปกรณ์ของเรามีค่า DPR อยู่ 3 และ container เราเขียนให้มีความกว้าง width = 150 Logical pixels. Container นี้จะมีขนาดความกว้าง Rendering pixel อยู่ที่ 450 pixels

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1705221558558/90bbb88e-ba35-4e40-83b9-733fc9473486.png align="center")

ในรูปภาพจะสังเกตุได้ว่าขนาด Logical pixels และ DPR เป็นของ iPhone 13 mini แต่ว่า Rendering pixels, 2436 x 1125, ไม่เท่ากับความละเอียดของ iPhone 13mini ที่เราอ่านได้จากคุณสมบัติอุปกรณ์ที่เราหาได้ทั่วไปซึ่งจะมีค่าเท่ากับ 2340 x 1080 pixels ทั้งนี้เป็นเพราะว่าผู้ผลิตอุปกรณ์มือถือจะมีการใช้เทคนิคในการเรนเดอร์ให้ใหญ่กว่าอุปกรณ์จริงเล็กน้อยแล้ว downscale ลง เพื่อให้ภาพดูชัดขึ้นอีก ซึ่งปัจจัยนี้จะขึ้นอยู่กับทางผู้ผลิตมือถือว่าจะออกแบบการ ​render กับอุปกรณ์อย่างไร

# การหาค่า Logical pixels ในอุปกรณ์ต่างๆ

จริงๆแล้วในการทำงานเราเพียงต้องหาขนาด Logical Pixels ของอุปกรณ์หลักที่ใช้ในการพัฒนาเพื่อนำไปสื่อสารกับดีไซเนอร์ก็เพียงพอกับการทำงานแล้ว เราสามารถหาค่า Logical pixels ง่ายๆใน Flutter ได้

## Log จาก context

ใน Widget ของเราเราสามารถหาขนาดของ BuildContext ได้โดยการเรียกผ่าน method ของ MediaQuery.

```dart
@override
Widget build(BuildContext context) {
    final size = MediaQuery.of(context).size;
    print(size.width);
    print(size.height);
}
```

## จาก Flutter Widget Inspector

วิธีนี้จริงๆจะสะดวกกว่าวิธี Log ออกมาด้วยซ้ำ โดยสามารถดูได้จาก Layout Explorer.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1705216123259/724a4c84-5d8a-4add-a444-d0a450c4491b.png align="center")

หรือจะดูจาก Widget Details Tree ซึ่งจริงๆแล้วก็คือข้อมูลของ MediaQueryData ตอนที่เรา log ออกมา แต่ในนี้เรายังสามารถดูค่าอื่นอย่างเช่น DPR ได้ในที่เดียวกัน

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1705216082390/71e379a6-f39d-40ee-8a79-9a54b93a23b0.png align="center")

ผมคิดว่าทุกคนน่าจะได้ประโยชน์จากการที่เข้าใจที่ไปที่มาของขนาดอุปกรณ์และสามารถไขข้อข้องใจเกี่ยวกับขนาดใน Flutter ได้และสามารถนำไปสื่อสารกับดีไซเนอร์หรือในทีมนักพัฒนาได้อย่างมั่นใจขึ้นนะครับ

#   
(แถม) ค่า Logical pixels และ DPR ในแต่ละอุปกรณ์

ข้อมูลในตารางเป็นค่า Logical pixels และ DPR ของอุปกรณ์ที่มักใช้ในการพัฒนาและเทสต์แอพพลิเคชั่น ทางผมรวบรวมไว้เพื่อความสะดวกในการใช้หาข้อมูลครับ

| อุปกรณ์ | ขนาด Logical Pixels (width x height) | DPR (Device Pixel Ratio) |
| --- | --- | --- |
| iPhone SE (2020) | 375 x 667 | 2 |
| iPhone 13 mini | 375 x 812 | 3 |
| iPhone 15 pro max | 430 x 932 | 3 |
| Galaxy A15 | 430 x 908\* | 3 |
| Samsung S23 pro | 480 x 1005\* | 3 |
| Galaxy Flip 5 | 360 x 856\* | 3 |
| Galaxy Fold 5 | 906 x 1028\* | 2 |

\*เป็นการหาค่าโดยการสร้างใน Android simulator ทำ spec ของอุปกรณ์จริงแล้วดูจาก Widget Inspector ความสูงอาจจะมีความคลาดเคลื่อนแต่ความกว้างสามารถใช้ได้แน่นอน โดยลองเช็คด้วยการสร้าง simulator ตามขนาดของ iPhone แล้วเทียบกับ iOS simulator จริงๆอีกที