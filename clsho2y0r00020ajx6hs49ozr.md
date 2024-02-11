---
title: "ตัวอย่างการใช้ Custom Painter และ Path พร้อมภาพประกอบ ด้วยการสร้างวิดเจ็ตตั๋วหนัง"
seoTitle: "Custom Painter, Path example, create movie ticket"
seoDescription: "สร้างวิดเจ็ตตั๋วหนังด้วย Custom Painter และ Path ใน Flutter พร้อมภาพประกอบ และโค้ดตัวอย่าง เพิ่มความสวยงามและความสามารถในแอพของคุณ"
datePublished: Sun Feb 11 2024 15:35:24 GMT+0000 (Coordinated Universal Time)
cuid: clsho2y0r00020ajx6hs49ozr
slug: custom-painter-path
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1707665459955/d56625fe-8097-4aad-a0a0-0daff0e3caa8.webp
tags: flutter, mobile-development, flutter-examples

---

ตอนที่ผมช่วยทางลูกค้าพัฒนาแอพพลิเคชั่น มีวิดเจ็ตตัวนึงที่น่าสนใจ เห็นแล้วคันมืออยากลองเขียนก็คือวิดเจ็ตที่มีหน้าตาเหมือนตั๋วหนัง ผมได้ลองทำวิดเจ็ตตัวหนังเวอร์ชั่นของผมแล้วเอามาแชร์ให้ทุกคนดู พร้อมทั้งอธิบายด้วยรูปภาพในการใช้ CustomPaint และ Path มาประกอบกันเพื่อให้ทุกคนเห็นภาพจากโค้ดที่เขียนได้ง่ายขึ้น และสามารถนำไปต่อยอดเขียน Widget ที่มีหน้าตาซับซ้อนมากกว่าเดิม เพื่อตอบสนองไอเดียวของดีไซเนอร์ได้อีกด้วย

ตัวอย่างที่ผมสร้างขึ้นจะไม่ได้ลงลึกถึงรายละเอียดความสวยงามแต่จะเป็นคอนเซปการเพนท์ง่ายๆ เพื่อให้เข้าใจง่ายและโค้ดไม่ยาวจนเกินไปและอยู่ในไฟล์เดียวได้ ซึ่งผมได้แนบโค้ดไฟล์เดียวเพื่อให้นำไปลองบิวด์ในเครื่องของตัวเองดูได้เลยนะครับ

# รูปร่างของตั๋วที่สร้าง

จากรูปด้านล่างตั๋วที่สร้าง(ไม่สวยนะครับ โปรดมองผ่าน ฮ่าๆๆ) ทั้งหมดสามตั๋วเป็นวิดเจ็ตเดียวกัน แต่ต่างกันที่ขนาดของหัวตั๋วและความสูง โดยที่ ShapeBorder ที่ใช้ในการสร้าง Border ทุกตั๋วเป็น class เดียวกันทั้ง รวมถึงด้านซ้ายและขวาก็ใช้คลาสเดิมเช่นเดียวกัน

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1707658668687/fededd99-b281-40e7-9efc-0acc40cb740c.png align="center")

ส่วนหัวจะเป็น SizedBox และส่วนหางคือ Expanded ที่อยู่ใน Row เดียวกันโดยที่ child ของทั้งสอง widget คือ CustomPaint

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1707661528719/0a3f212b-bad3-4896-ab06-bf43b34d054b.png align="center")

ใน CustomPaint จะมี parameter 'paint' ให้เราส่ง CustomPainter เข้าไป ซึ่ง CustomPainter จะเป็น abstract class ซึ่งหมายความว่าเราเอา class นั้นมาใช้ตรงๆไม่ได้ แต่ต้องทำการสร้าง class ใหม่ต่อยอดจาก class นั้น ซึ่งในที่นี้คือ class InvertedBorderPainter

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1707661925976/f7933dd4-333a-40e5-b263-db80f936926e.png align="center")

ใน class นี้เองที่เราจะกำหนด path ใน paint method ซึ่งเป็น method มาจาก class แม่ซึ่งก็คือ CustomPainter ซึ่ง paint method จะให้ข้อมูลที่จำเป็นกับเราซึ่งก็คือ canvas และ size ให้เราเอามาใช้ path ที่ต้องการได้ ซึ่ง path นี้ก็จะเป็นรูปร่างของ widget เรานั่นเอง

ทีนี้เรามาดูกันว่าเราจะวาด path ใน Flutter อย่างไร ผมจะแนบภาพประกอบพร้อมกับโค้ดที่บอกลำดับขั้นตอนที่ flutter ใช้สร้าง path มาให้ดูเลย เพราะน่าจะเข้าใจได้ดีกว่า

```dart
class InvertedBorderPainter extends CustomPainter {
  final double borderWidth;
  final Color borderColor;
  final Color backgroundColor;
  final double radius;
  final double ticketHeadWidth;

  InvertedBorderPainter({
    this.borderWidth = 2,
    this.borderColor = Colors.black,
    this.backgroundColor = Colors.black12,
    this.radius = 20.0,
    this.ticketHeadWidth = 50,
  });

  @override
  void paint(Canvas canvas, Size size) {
    Path borderPath = Path()
      ..moveTo(0, radius)
      ..arcToPoint(
        Offset(radius, 0),
        clockwise: false,
        radius: Radius.circular(radius),
      )
      ..lineTo(size.width - radius, 0)
      ..arcToPoint(
        Offset(size.width, radius),
        clockwise: false,
        radius: Radius.circular(radius),
      )
      ..lineTo(size.width, size.height - radius)
      ..arcToPoint(
        Offset(size.width - radius, size.height),
        clockwise: false,
        radius: Radius.circular(radius),
      )
      ..lineTo(radius, size.height)
      ..arcToPoint(
        Offset(0, size.height - radius),
        clockwise: false,
        radius: Radius.circular(radius),
      )
      ..close();

    Paint bgPaint = Paint()
      ..color = backgroundColor
      ..style = PaintingStyle.fill
      ..strokeWidth = borderWidth;

    Paint borderPaint = Paint()
      ..color = borderColor
      ..style = PaintingStyle.stroke
      ..strokeWidth = borderWidth;

    canvas.drawPath(borderPath, bgPaint);
    canvas.drawPath(borderPath, borderPaint);

    // Inner fill or additional styling can be added here if needed
  }
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1707665206536/a99762c6-9c3e-4df4-a947-429ec8caaa46.png align="center")

จากรูปภาพด้านบนคือการสร้าง path ไปทีละจุดตามตำแหน่งแกน x และแกน y ซึ่งเราจะไม่ได้ฮาร์ดโค้ดลงไปเพื่อให้ path มีการปรับขนาดตามขนาดของ widget อย่างเช่น `size.width - radius` ค่อยๆต่อไปถึงจุดสุดท้ายและปิด path ด้วยคำสั่ง `close()`

หลังจากนั้นเราก็ paint สิ่งที่เราต้องการด้วย path ของเราซึ่งในที่นี้คือมี border และ background ซึ่งเราใช้ path เดียวกันแต่ paint กันคนละแบบ ด้วยคำสั่ง `canvas.drawPath`

การเข้าใจการใช้ Path สามารถช่วยให้เราออกแบบวิดเจ็ตที่หลายหลายได้มากขึ้น ไม่ว่าจะนำ Path ไปใช้กับ CustomPainter อย่างในตัวอย่าง หรือ CustomClipper ที่มีการใช้เหมือนกันแต่แทนที่จะเป็นการ paint จาก path เป็นการตัดออกด้วย path ซึ่งโดยเฉพาะกับ Flutter ที่มี concept ในการ render บน canvas เองแทบจะคล้ายๆกับการออกแบบในโปรแกรม vector based อย่าง Illustrator เลยทีเดียว เอาจริงๆแรงบันดาลใจที่อยากแชร์อีกอย่างก็คือผมเกือบจะลืมมันไปแล้วเพราะไม่ได้ใช้นาน เลยอยากบันทึกไว้สอนตัวเองทีหลังด้วยครับ หวังว่าจะมีประโยชน์กับทุกคนเหมือนเดิมนะครับ

# Example Code

%[https://gist.github.com/Khanin-devmode/7969d2d96d2064c7884fd8317c9cd9fb]