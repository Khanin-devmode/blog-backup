---
title: "วิธีการควบคุมการเข้าถึง class ใน package ของเรา"
seoTitle: "ควบคุมการเข้าถึง class ใน package"
seoDescription: "ควบคุมการเข้าถึง class ในแพ็กเกจ Flutter ด้วยการวางไฟล์ในโฟลเดอร์ lib/src, ส่งออกไฟล์จาก lib โดยใช้คำสั่ง show และปฏิบัติตามแนวทางการ import"
datePublished: Sun Jan 21 2024 13:36:33 GMT+0000 (Coordinated Universal Time)
cuid: clrnjl7qa000209kz469i26ev
slug: controlling-what-to-expose-as-public-api-in-flutter-package-th
canonical: https://blog.nintech.dev/controlling-what-to-expose-as-public-api-in-flutter-package
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1705844125323/e583018e-9395-4881-88d4-cda4531079d2.png
tags: flutter

---

TLDR จาก [**https://docs.flutter.dev/packages-and-plugins/developing-packages**](https://docs.flutter.dev/packages-and-plugins/developing-packages).

> ***By convention, implementation code is placed under lib/src. Code under lib/src is considered private; other packages should never need to import*** `src/...`***. To make APIs under lib/src public, you can export lib/src files from a file that’s directly under lib.***

ในการพัฒนาแพ็กเกจ Flutter การจัดการกับการเปิดเผยคลาสต่อผู้ใช้และการควบคุมวิธีการนำเข้าคลาสนั้นสำคัญมาก หากไม่มีการควบคุมดังกล่าว การอัปเดตแพ็กเกจอาจกลายเป็นเรื่องที่ยุ่งยากและเสียเวลามาก

จากประสบการณ์ในโปรเจ็กต์ที่มีแพคเกจที่เขียนขึ้นเองค่อนข้างเยอะ บางไฟล์เปิดขึ้นมาบางทีเจอบรรทัด import เกือบจะเต็มหน้าแรกก่อนที่จะเลื่อนลง สิ่งนี้ทำให้เกิดปัญหาในการในการบำรุงรักษาโค้ดและก็มีผลกับความสวยงามของโค้ดด้วย ตัวอย่างเช่น, หากมีน้องโปรแกรมเมอร์ใหม่เข้ามาร่วมโปรเจ็กต์และพบกับบรรทัดการนำเข้า 30 บรรทัด และตอนที่น้องแค่ต้องการเปลี่ยนชื่อหรือย้ายไฟล์แอปที่มีการเรียกใช้แพ็กเกจนี้ก็จะพังทันที

ปัญหาหลักเกิดจากการนำเข้าคลาสส่วนใหญ่จะเป็น widget โดยตรงจากไฟล์เลย ทำให้มีบรรทัด import หลายบรรทัด หากเราไม่มีการเซ็ตอัพที่ถูกต้อง ผู้ใช้จะสามารถ importโดยตรงเนื่องจากคลาสใน Dart ถูกเปิดเผยเป็นสาธารณะโดยอัตโนมัติ ซึ่งปัญหาเหล่านี้จริงๆแล้วสามารถป้องกันได้ง่ายมาก เดี๋ยวเราลองดูตัวอย่างจากโครงสร้างโปรเจคด้านล่างนี้นะครับ

```dart
ui_package_example_1/
├── lib/
│   ├── widgets/
│   │   ├── custom_button.dart
│   │   └── custom_text_field.dart
│   │   └── custom_icon.dart
│   ├── custom_widgets.dart
├── pubspec.yaml
└── README.md
```

```dart
// custom_widgets.dart
export '../lib/widgets/custom_button.dart';
export '../lib/widgets/custom_text_field.dart';
export '../lib/widgets/custom_icon.dart';
```

พอมาถึงตรงนี้เราอาจจะคิดว่าการสร้างไฟล์สำหรับ export ซึ่งก็คือ custom\_widgets.dart ในโปรเจคนี้จะแก้ปัญหาได้ คำตอบคือใช่แต่เป็นเพียงบางส่วนเท่านั้น custom\_widgets.dart จะช่วยให้ผู้ใช้ import ไฟล์เดียวเพื่อใช้วิดเจ็ตจากแพ็คเกจทั้งหมดได้ทีเดียว. แต่ต้นตอของปัญหาไม่ได้อยู่ที่การเพิ่มวิธีการนำเข้าเพิ่มเติม แม้มีไฟล์ export นี้ ผู้ใช้งานแพ็คเกจยังสามารถนำเข้าจากไฟล์วิดเจ็ตโดยตรงได้อยู่ดี **ปัญหาจริงๆก็คือไฟล์เหล่านี้ยังถูกเปิดเผยต่อผู้ใช้และ IntelliSense ใน IDE ของเราก็ยังแสดงเส้นทางการนำเข้าโดยตรงอยู่ดี และถ้ายังมีเส้นทางให้มีการ import ตรงได้ผู้ใช้งานก็จะทำ ส่วนใหญ่น่าจะไม่ได้ตั้งใจด้วยซ้ำแต่เพราะ IntelliSense มันแนะนำให้เอง**

การแก้ปัญหาที่จริงแล้วง่ายมาก เพียงแค่วางไฟล์ที่เราไม่ต้องการเปิดเผยอัตโนมัติในโฟลเดอร์ `src` ตามโครงสร้างโปรเจคด้านล่างนี้

```dart
ui_package_example_2/
├── src/
│   ├── widgets/
│   │   ├── custom_button.dart
│   │   └── custom_text_field.dart
│   │   └── custom_icon.dart
│   ├── custom_widgets.dart
├── pubspec.yaml
└── README.md
```

อัพเดต custom\_widgets.dart ด้วย

```dart
// custom_widgets.dart
export '../lib/src/custom_button.dart';
export '../lib/src/custom_text_field.dart';
export '../lib/src/custom_icon.dart';
```

เพื่อความเรียบง่ายเราแค่เปลี่ยนโฟลเดอร์ widgets เป็นโฟลเดอร์ src ซึ่งในโปรเจ็กต์จริงเราก็ควรสร้างโครงสร้างโฟลเดอร์ที่มีความหมายสำหรับโปรเจ็กต์ในโฟลเดอร์ src อีกที

เพียงเท่านี้คนใช้งานแพ็คเกจก็จะสามารถ import เฉพาะสิ่งที่เราต้องการจะ export ใน custom\_widgets.dart ได้เท่านั้น ซึ่งจริงๆก็คือง่ายมากอย่างที่เห็น แต่ถ้าเราไม่รู้เรื่องนี้แล้วมาแก้ทีหลัง ก็งานเข้ากันไปจ้า

# **Good Practices เพิ่มเติม**

## ใช้ `show` และไม่ใช้ `part`

นอกจากการใช้โครงสร้างโฟลเดอร์ src แล้วหากไฟล์ของคุณมีคลาสหลายตัวและคุณต้องการส่งออกเพียงบางส่วนคุณสามารถใช้คำสั่ง `show` เหมือนในตัวอย่างด้านล่างนี้

```dart
// custom_widgets.dart
export '../lib/src/custom_button.dart' show CustomButton, buttonStyleEnum;
export '../lib/src/custom_text_field.dart' show CustomTextFied;
export '../lib/src/custom_icon.dart' show CustomIcon;
```

คุณอาจเคยได้ยินเกี่ยวกับการใช้ `part` ในการจัดการไลบรารีเพิ่มเติมจากการใช้ `show` ซึ่งแนวทางปฏิบัติของ Dart แนะนำให้หลีกเลี่ยงการใช้ `part`

> ***Note: You may have heard of the*** `part` ***directive, which allows you to split a library into multiple Dart files. We recommend that you avoid using*** `part` ***and create mini libraries instead.***

## การ Import ภายในแพ๊คเกจกันเอง

Dart มีการแนะนำตามข้อความด้านล่างนี้ และผมก็เห็นด้วยที่จะปฏิบัติตามเพราะรู้สึกแปลกที่ต้องนำเข้าแพ็คเกจของตัวเองเพื่อที่จะใช้ในแพ็คเกจนั้นโดยการใช้การนำเข้าผ่านเส้นทางแพ็คเกจ

> ***When importing a library file from your own package, use a relative path when both files are inside of lib, or when both files are outside of lib. Use*** `package:` ***when the imported file is in lib and the importer is outside.***

# สรุป

อย่างที่ได้เห็นกันนะครับว่าการแก้ปัญหาสำหรับปัญหานี้ง่ายมาก แต่ผมคิดว่ายังต้องการแชร์อยู่ดีเพราะผมเห็นผลกระทบต่อโค้ดของเราโดยเฉพาะในโปรเจ็กต์ขนาดใหญ่ ถึงแม้ว่ามันจะได้รับการระบุไว้อย่างชัดเจนในเอกสารทางการของ Dart แล้วก็ตาม ผมต้องการจะแชร์ถึงผลกระทับของมันถ้าหากเราลืมหรือมองผ่านแล้วไม่ทันสังเกตุในจุดนี้ขึ้นมา ขอบคุณทุกคนที่อ่านถึงจุดนี้และหวังว่าจะช่วยแก้ปัญหาที่เจอได้นะครับ ขอบคุณครับ

# อ้างอิง

* [**https://dart.dev/guides/libraries/create-packages**](https://dart.dev/guides/libraries/create-packages)