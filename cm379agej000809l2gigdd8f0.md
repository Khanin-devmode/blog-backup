---
title: "ถาม Chat มาให้ 
ค่า double แปลกๆอย่าง double.maxFinite double.minPositive คืออะไร"
seoTitle: "การใช้งานของ double ประเภทต่างๆ"
seoDescription: "เรียนรู้เกี่ยวกับค่า double ใน Dart เช่น double.maxFinite, double.minPositive, และ double.infinity กับการใช้งานและสถานการณ์ที่เหมาะสม"
datePublished: Thu Nov 07 2024 12:00:24 GMT+0000 (Coordinated Universal Time)
cuid: cm379agej000809l2gigdd8f0
slug: ask-chatgpt-about-constants-in-double
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1730730299906/2707cd77-59e3-4329-93c6-06b7a0d37f82.png
tags: flutter, chatgpt

---

ในตอนที่เราเรียกใช้ค่าอย่าง double เรามักจะเห็นแว็บๆว่า double นั้นมีค่าให้ใช้ไม่ไม่ค่อยคุ้นอย่างเช่น `double.maxFinite` และ `double.minPositive` ในขณะที่ `double.infinity` นั้นเองจะมีได้ใช้อยู่บ้างในการที่ต้องการขยาย `Container` ให้สุด แล้วค่า double ต่างๆนี้มันต่างกันอย่างไร ผมเลยหาข้อมูลด้วยการถาม ChatGPT ดูแล้วเอามาแชร์กันครับ

### คำถาม Prompt แรก

> ในภาษา Dart double.infinity, double.maxFinite, double.minPositive, double.nan และ double.negativeInfinity ต่างกันอย่างไรและใช้สำหรับทำอะไร

### คำตอบ Prompt แรก

> ในภาษา Dart ค่าคงที่ใน double ได้แก่ double.infinity, double.maxFinite, double.minPositive, double.nan, และ double.negativeInfinity มีหน้าที่และการใช้งานที่ต่างกันดังนี้:
> 
> 1\. double.infinity
> 
> • ใช้แทนค่า “อนันต์” ในเชิงบวก
> 
> • เหมาะสำหรับกรณีที่ต้องการค่าที่ใหญ่กว่าเลขจำนวนจริงใด ๆ เช่น ขยายขนาด widget ให้เต็มพื้นที่ที่รองรับ
> 
> • เช่นใน Flutter สามารถใช้กับ Container เพื่อให้มีขนาดกว้างที่สุดได้โดยตั้งค่าเป็น double.infinity
> 
> 2\. double.maxFinite
> 
> • ค่าที่ใหญ่ที่สุดในบรรดาตัวเลขที่เป็นจำนวนจริงที่ Dart สามารถเก็บได้โดยไม่ใช่อนันต์ (infinity)
> 
> • เหมาะสำหรับการกำหนดค่าขนาดใหญ่ที่สุดที่ยังคงเป็น finite (เช่น scrollable widget ที่ต้องการขยายพื้นที่เต็มโดยไม่เกินขอบเขต)
> 
> 3\. double.minPositive
> 
> • ค่าบวกที่เล็กที่สุดที่ไม่ใช่ศูนย์
> 
> • ใช้ในกรณีที่ต้องการค่าที่เล็กมาก ๆ เพื่อป้องกันข้อผิดพลาดจากการใช้ 0 ในการคำนวณหรือหาร เช่นกำหนดให้มีช่องว่างที่บางที่สุดที่ยังแสดงผลได้
> 
> 4\. double.nan
> 
> • แทนค่า “ไม่ใช่ตัวเลข” (Not-a-Number) มักเกิดจากการคำนวณที่ไม่สามารถระบุค่าได้ (เช่น 0/0)
> 
> • ใช้เพื่อตรวจสอบหรือระบุข้อผิดพลาดในกรณีที่การคำนวณล้มเหลว ซึ่ง double.nan ไม่เท่ากับค่าตัวเลขใด ๆ และแม้แต่ตัวมันเอง
> 
> 5\. double.negativeInfinity
> 
> • ค่าที่แทนความหมายของ “อนันต์ในเชิงลบ”
> 
> • ใช้ในกรณีที่ต้องการค่าที่เล็กกว่าเลขจำนวนจริงใด ๆ เช่น ในการคำนวณที่จำเป็นต้องกำหนดค่าต่ำสุดที่ไม่สิ้นสุด
> 
> **ตัวอย่างการใช้งานใน Flutter:**

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: Text('Double Constants in Dart')),
        body: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              // ตัวอย่างการใช้ double.infinity
              Container(
                color: Colors.blue,
                width: double.infinity,
                height: 50,
                child: Center(child: Text('Width: double.infinity')),
              ),
              SizedBox(height: 20),
              // ตัวอย่างการใช้ double.maxFinite
              Container(
                color: Colors.green,
                width: double.maxFinite,
                height: 50,
                child: Center(child: Text('Width: double.maxFinite')),
              ),
              SizedBox(height: 20),
              // ตัวอย่างการใช้ double.minPositive
              Container(
                color: Colors.red,
                width: 100,
                height: double.minPositive,
                child: Center(child: Text('Height: double.minPositive')),
              ),
              SizedBox(height: 20),
              // การใช้งาน double.nan
              Text(double.nan.toString() == 'NaN'
                  ? 'Result is NaN (not a number)'
                  : 'Result is a number'),
              // การใช้งาน double.negativeInfinity
              Container(
                color: Colors.purple,
                width: double.infinity,
                height: 50,
                child: Center(
                  child: Text(double.negativeInfinity.isNegative
                      ? 'Negative Infinity'
                      : 'Not Negative Infinity'),
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

> **สรุปการใช้งาน**
> 
> • double.infinity: ใช้เมื่อต้องการขยายขนาดให้มากที่สุดที่เป็นไปได้
> 
> • double.maxFinite: ใช้เมื่อจำเป็นต้องขยายเต็มพื้นที่แต่ไม่เกินขอบเขตที่ไม่เป็นอนันต์
> 
> • double.minPositive: ใช้เพื่อกำหนดค่าที่เล็กที่สุดที่ยังไม่ใช่ศูนย์
> 
> • double.nan: ใช้ระบุหรือเช็คข้อผิดพลาดที่ไม่สามารถระบุเป็นตัวเลขได้
> 
> • double.negativeInfinity: ใช้สำหรับค่าที่ต่ำที่สุดที่ไม่สิ้นสุด

จากตัวอย่างที่ให้มาก็ทำให้เห็นว่าค่าคงที่เหล่านี้ทำงานยังไงแต่ผมก็รู้สึกว่ายังไม่การใช้งานที่มีความจำเป็นจะต้องใช้ค่าพวกนี้ผมเลยลองถามต่อ

### คำถาม Prompt ที่สอง

> ตัวอย่างที่ให้มาทำให้นึกภาพได้ชัดเจนดี แต่ว่ามีตัวอย่างที่ทำให้ต้องจำเป็นใช้ค่าคงที่พวกนี้มั้ยแทนที่จะใช้ค่า double ธรรมดา ฉันนึกออกแต่ตอนที่ต้องการขยาย Container ให้กว้างที่สุดได้โดยใช้ double.infinity แต่นึกความจำเป็นของ double.maxFinite double.minPositive double.nan และ double.negativeInfinity ไม่ออก

### คำตอบ Prompt ที่สอง

> แน่นอนครับ ค่าคงที่ double.maxFinite, double.minPositive, double.nan, และ double.negativeInfinity มีประโยชน์ในกรณีเฉพาะเมื่อการใช้ค่า double ทั่วไปอาจไม่เพียงพอ ซึ่งเดี๋ยวเรามาดูตัวอย่างที่มีการใช้งานจริงที่เหมาะสมกับแต่ละค่ากันครับ
> 
> **1\. double.maxFinite - ใช้เมื่อต้องการค่า “สูงสุดที่เป็นไปได้” แต่ไม่เกินขอบเขตของ finite number**
> 
> • **สถานการณ์**: เมื่อใช้ในโปรแกรมที่คำนวณหาขนาดหรือระยะทางมากที่สุดโดยไม่ใช้อนันต์ (เช่น การกำหนดค่าเริ่มต้นในการเปรียบเทียบขนาด หรือความสูง/กว้างของ widget ที่ไม่ต้องการค่าต่ำกว่า double.infinity)
> 
> • **ตัวอย่าง**: การหาขนาด widget ที่ใหญ่ที่สุดโดยตั้งค่าเริ่มต้นเป็น double.maxFinite

```dart
List<double> widgetHeights = [150, 200, 450, 100];
double maxHeight = double.maxFinite;

for (var height in widgetHeights) {
  if (height < maxHeight) {
    maxHeight = height;
  }
}
print('Minimum height: $maxHeight');
```

> **2\. double.minPositive - ใช้เพื่อหลีกเลี่ยงค่าศูนย์ที่อาจทำให้เกิดข้อผิดพลาดทางคณิตศาสตร์**
> 
> • **สถานการณ์**: ใช้ในกรณีที่ต้องการค่าใกล้ศูนย์ที่สุดแต่ไม่ใช่ศูนย์ ซึ่งสามารถใช้ในทางการคำนวณที่ต้องการหลีกเลี่ยงการหารด้วยศูนย์ หรือการคำนวณเกี่ยวกับเลขทศนิยมที่เล็กมาก ๆ
> 
> • **ตัวอย่าง**: การหลีกเลี่ยงการหารด้วยศูนย์

```dart
double divisor = 0; // การทำแบบนี้อาจทำให้เกิด error ได้
double safeDivisor = divisor == 0 ? double.minPositive : divisor;
double result = 100 / safeDivisor;
print('Result: $result');
```

> **3\. double.nan - ใช้ในการตรวจสอบกรณีที่ค่าคำนวณผิดพลาดหรือไม่สามารถระบุได้**
> 
> • **สถานการณ์**: เมื่อต้องการตรวจสอบค่าที่อาจเป็นผลจากการคำนวณที่ผิดพลาด เช่น การคำนวณหารที่ไม่สามารถกำหนดผลลัพธ์ได้ เช่น 0 / 0 หรือ sqrt(-1)
> 
> • **ตัวอย่าง**: การตรวจสอบค่า NaN

```dart
double value = 0 / 0;
if (value.isNaN) {
  print('Error: Calculation resulted in NaN');
} else {
  print('Result: $value');
}
```

> **4\. double.negativeInfinity - ใช้เมื่อต้องการเริ่มต้นค่าต่ำสุดในเชิงลบ**
> 
> • **สถานการณ์**: ใช้ในการคำนวณหาค่าสูงสุดของลิสต์ เช่น หาค่า max จากลิสต์โดยไม่ต้องการค่าเริ่มต้นเป็นค่าตัวเลขที่จำกัด
> 
> • **ตัวอย่าง**: การหาค่า max โดยใช้ negative infinity

```dart
List<double> scores = [-10, 0, 25, -30, 15];
double maxScore = double.negativeInfinity;

for (var score in scores) {
  if (score > maxScore) {
    maxScore = score;
  }
}
print('Maximum score: $maxScore');
```

> **สรุปสั้น ๆ**
> 
> • double.maxFinite เหมาะกับการกำหนดค่าขนาดใหญ่สุดแบบ finite โดยไม่ต้องการค่า infinite
> 
> • double.minPositive เหมาะกับการกำหนดค่าใกล้ศูนย์ที่สุดที่ไม่ใช่ศูนย์เพื่อป้องกัน error
> 
> • double.nan ใช้ตรวจสอบค่าที่ไม่ใช่ตัวเลข ซึ่งอาจเกิดจากการคำนวณผิดพลาด
> 
> • double.negativeInfinity ใช้เพื่อเริ่มต้นค่าต่ำสุดในกรณีที่ต้องการหาค่าสูงสุด

## คำถาม Prompt ที่สาม

> มีตัวอย่างอื่นสำหรับ double.maxFinite มั้ย เพราะฉันคิดว่าไม่จำเป็นต้องใช้ double.maxFinite ก็สามารธรัน function นั้นได้เหมือนกัน ฉันเลยคิดว่าเป็นตัวอย่างที่ไม่ดีเท่าไร

## คำตอบ Prompt ที่สาม

> คุณเข้าใจถูกต้องครับ ในตัวอย่างที่ให้ไปสำหรับ double.maxFinite สามารถใช้ค่าตัวเลขทั่วไป (เช่น 100000) แทนได้ จึงอาจไม่ใช่ตัวอย่างที่ทำให้เห็นถึงความ “จำเป็น” ในการใช้ double.maxFinite ดังนั้น เรามาดูกรณีอื่นที่ double.maxFinite มีความจำเป็นและมีประโยชน์มากขึ้นครับ
> 
> **ตัวอย่างเพิ่มเติมที่ใช้ double.maxFinite**
> 
> 1\. **การจัดการการเลื่อนเนื้อหาใน Flutter**
> 
> • ใน Flutter มีบางครั้งที่เราต้องการขยายความกว้างหรือความสูงของ widget ให้เต็มพื้นที่ที่เลื่อนได้สูงสุด โดยไม่ให้มีการเลื่อนที่ไม่สิ้นสุด (infinite scroll)
> 
> • double.maxFinite สามารถใช้เป็นค่ากำหนดให้กับ widget เช่น ListView หรือ SingleChildScrollView เพื่อให้ความสูงจำกัดอยู่ในขอบเขตที่เป็นไปได้ (finite) โดยไม่เกิดข้อผิดพลาดที่เกิดจาก double.infinity

```dart
import 'package:flutter/material.dart';

class MaxFiniteExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Max Finite Example')),
      body: SingleChildScrollView(
        child: Container(
          width: double.infinity,
          height: double.maxFinite, // กำหนดความสูงสูงสุดที่เป็น finite
          color: Colors.blue,
          child: Center(child: Text('Scrollable Content within max finite height')),
        ),
      ),
    );
  }
}
```

> 2\. **จำกัดขนาดการแสดงผลในกรณีคำนวณ Layout ที่มีความกว้าง/ความสูงใหญ่พิเศษ**
> 
> • ในบางครั้งการใช้งาน double.infinity กับ widget บางตัวอาจเกิด error (เช่น Overflow error) โดยเฉพาะเมื่อ widget นั้นต้องการขนาด finite ในการแสดงผล ดังนั้น double.maxFinite จะช่วยให้ขนาดอยู่ในขอบเขตที่ Dart และ Flutter สามารถจัดการได้โดยไม่เกิดข้อผิดพลาด
> 
> • ตัวอย่างเช่น ในการกำหนดขอบเขตความกว้างสูงสุดสำหรับ Container หรือ ConstrainedBox ที่ต้องการขนาดแน่นอน แต่เราต้องการขนาดใหญ่ที่สุดที่ไม่เกินขอบเขต finite

```dart
import 'package:flutter/material.dart';

class MaxWidthExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Max Finite Width Example')),
      body: Center(
        child: ConstrainedBox(
          constraints: BoxConstraints(
            maxWidth: double.maxFinite, // ขอบเขตความกว้างสูงสุดแบบ finite
          ),
          child: Container(
            color: Colors.green,
            padding: EdgeInsets.all(16.0),
            child: Text(
              'This container has a max width of maxFinite, avoiding overflow issues with large sizes',
              style: TextStyle(color: Colors.white),
            ),
          ),
        ),
      ),
    );
  }
}
```

> 3\. **กรณีที่ต้องใช้ค่าขอบเขตสูงสุดในอัลกอริทึม**
> 
> • หากคุณเขียนอัลกอริทึมที่ต้องการค่าเริ่มต้นสูงสุด (เช่น การค้นหาค่าที่เล็กที่สุดในชุดข้อมูลที่มีขนาดไม่จำกัด) การใช้ double.maxFinite เป็นค่าเริ่มต้นจะช่วยให้แน่ใจว่าเราจะไม่หลุดออกจากขอบเขตของ double

```dart
List<double> prices = [150.0, 120.5, 99.99, 200.0];
double minPrice = double.maxFinite; // เริ่มต้นด้วยค่าสูงสุดที่ไม่เกินขอบเขต

for (var price in prices) {
  if (price < minPrice) {
    minPrice = price;
  }
}
print('Minimum price: $minPrice'); // ได้ผลลัพธ์เป็น 99.99
```

> ในกรณีเหล่านี้ double.maxFinite จะช่วยให้เราจัดการขนาดใหญ่ที่สุดที่สามารถจัดการได้แบบ finite โดยไม่เกิดข้อผิดพลาดที่เกิดจากการใช้ double.infinity ครับ

### สรุป

จากคำตอบทั้งหมดของ ChatGPT ผมคิดว่าผมได้คำตอบที่เข้าใจการใช้งานได้มากขึ้น ถ้าเอาสรุปสำหรับตัวผมเองผมก็จะจำไว้ว่า

* double.inifinity ใช้เมื่อตอนต้องการขยายสุดพื้นที่ ต้องการเล่นกับเลขอนันต์ในการคำนวณค่าต่างๆ
    
* double.maxPositive ใช้เมื่อต้องการขยายสุดพื้นที่เช่นกันแต่ว่าพื้นที่ดังกล่างไม่สามารถเป็นค่า infinite ได้ หรือถ้าใช้ double.inifinity แล้วเกิด error
    
* double.minPositive ใช้เมื่อต้องการค่าเหมือนเป็น 0 แต่ว่าไม่สามารถรับค่าเป็น 0 ได้ (ซึ่งอันนี้ยังคิดการใช้งานที่จำเป็นสุดๆไม่ได้เหมือนกัน)
    
* double.nan ใช้สำหรับเช็คค่า double
    
* double.negativeInfinity ใช้เหมือน infinity ในการเป็นค่าเริ่มต้นที่สามารถเป็นลบได้
    

ส่วนตัวผมเองชอบคำตอบที่ได้เกี่ยวกับ double.maxPositive มากที่สุดเพราะน่าจะเอาไปใช้ได้ดีอย่างวิดเจ็ตที่เราใช้บ่อยอย่างเช่น SingleChildScrollView และปัญา overflow ที่อาจจะเกิดขึ้นตอนเราพยายามใช้ double.infinity ได้ และนี่ก็เป็น prompt ที่ต้องการจะบันทึกไว้เพราะก็สงสัยทุกครั้งที่เจอและตั้งใจจะแชร์ให้ทุกคนได้อ่าน อย่างน้อยก็ช่วยประหยัดเวลาคนอ่านไม่ต้องไปนั่ง prompt เองอีกที หวังว่าจะเป็นประโยชน์ไม่มากก็น้อยนะครับ ขอบคุณครับ