---
title: "Using FractionallySizedBox in Combination with Wrap"
seoTitle: "Optimize Wrap with FractionallySizedBox"
seoDescription: "Learn to easily create responsive UI layouts in Flutter using FractionallySizedBox with Wrap, a powerful alternative to Bootstrap's column wrapping"
datePublished: Sun Apr 07 2024 11:23:51 GMT+0000 (Coordinated Universal Time)
cuid: clupfr5h8000008jv0ust5obw
slug: using-fractionallysizedbox-in-combination-with-wrap
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1712488966330/e7c621ea-2a74-491e-b88c-6f14e29bebe1.webp
tags: flexbox, flutter, frontend-development

---

Flutter มีวิดเจ็ตมากมายสำหรับการสร้าง UI ที่ถูกออกแบบมาเพื่อตอบโจทย์แบบจำเพาะของทุกการออกแบบ UI หนึ่งในการออกแบบที่ท้าทายใน Flutter, ซึ่งสามารถทำได้อย่างง่ายดายบนเว็บไซต์ด้วย Bootstrap, คือการจัดการกับการแบ่งคอลัมน์ในลักษณะที่ทำให้คอลัมน์สุดท้ายสามารถขึ้นบรรทัดใหม่ได้เอง หากมีการเพิ่มเข้ามา ตัวอย่างเช่น:

```xml
<div class="row">
    <div class="col-3"></div>
    <div class="col-3"></div>
    <div class="col-3"></div>
    <div class="col-3"></div>
    <div class="col-3"></div> <!-- This will automatically move to a new line -->
</div>
```

ในตัวอย่างด้านบน, แต่ละ `div` มีความกว้าง 25% ของพื้นที่ของมัน, และ `div` ตัวสุดท้ายจะถูกย้ายไปยังบรรทัดใหม่โดยอัตโนมัติ แต่เมื่อพยายามทำสิ่งเดียวกันใน Flutter, วิดเจ็ตแรกที่เรานึกถึงคือ `Row` ซึ่งสามารถใส่ `Flex` หรือ `Expanded` วิดเจ็ตได้ อย่างไรก็ตาม, ทั้งหมดจะยังคงอยู่ในบรรทัดเดียวกันโดยไม่ขึ้นบรรทัดใหม่:

```dart
Row(
    children: [
        Expanded(),
        Expanded(),
        Expanded(),
        Expanded(),
        Expanded(),
    ],
),
```

**เพื่อทำให้ได้ผลลัพธ์ที่คล้ายคลึงกับตัวอย่างใน Bootstrap, เราสามารถใช้วิดเจ็ต** `Wrap` **ร่วมกับ** `FractionallySizedBox` **ใน Flutter**

วิดเจ็ต `Wrap` ทำงานคล้ายกับ `Row` แต่มีความสามารถพิเศษในการย้ายวิดเจ็ตลูกที่มีขนาดเกินไปยังบรรทัดใหม่ นี่เป็นการทำงานที่เหมาะสำหรับการออกแบบที่มีความยืดหยุ่นและมีโอกาสที่จะไม่ครบแถวอย่างพอดี และ `FractionallySizedBox` จะสามารถกำหนดขนาดของวิดเจ็ตในรูปแบบสัดส่วน (fraction) ตามขนาดของวิดเจ็ตแม่ไว้ได้:

```dart
Wrap(
    children: [
        FractionallySizedBox(widthFactor: 0.25),
        FractionallySizedBox(widthFactor: 0.25),
        FractionallySizedBox(widthFactor: 0.25),
        FractionallySizedBox(widthFactor: 0.25),
        FractionallySizedBox(widthFactor: 0.25),
    ],
),
```

จากโค้ดทั้งหมดด้านบนจะเป็นโค้ดอย่างง่ายเพื่อให้เข้าใจ concept เท่านั้น ถ้าเอาไปรันจริงๆจะเห็นเป็นแค่เส้นๆอย่างเดียว ผมได้ทำโค้ดตัวอย่างเพื่อสาธิตการใช้งาน **Wrap** และ **FractionallySizedBox** วิดเจ็ตไว้ด้านล่างเพื่อให้ไปลองเล่นกันได้เหมือนเดิม หวังว่าจะมีประโยชน์กับผู้อ่านทุกคนนะครับ

### โค้ดตัวอย่าง

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1712488182992/1e7996de-f1da-48f8-8ec8-d6de15a02d22.gif align="center")

%[https://gist.github.com/Khanin-devmode/be715f7ae0a583aafa684ec9bc420a53]