---
title: "แนะนำการใช้ Bloc ด้วยเคานท์เตอร์แอพแบบ event-base ในไฟล์เดียวจบๆ (เหมือนเดิม)"
slug: bloc-event-base
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1714215494856/b1563d2e-ef16-4eaf-b9b2-5a1113f8c986.webp

---

ต่อจาก[บทความที่แล้ว](https://blog.nintech.dev/bloc-example-counter-app) เกี่ยวกับการใช้ Bloc library ด้วย Cubit ในบทความนี้จะเป็นบทความการใช้ Bloc library เช่นเดียวกันแต่เป็นรูปแบบ event-base ที่เป็น Bloc pattern แบบเต็มตัว จุดประสงค์คือให้เห็นการใช้งานที่ง่ายที่สุดในเคาน์เตอร์แอพ และเปรียบเทียบกับการใช้แบบ Cubit ได้ชัดเจน เพื่อให้เห็นข้อดีและข้อเสีย เพื่อเป็นข้อมูลในประกอบการตัดสินใจเลือกว่าจะใช้วิธีไหนในการพัฒนาแอพพลิเคชั่น

อย่าลืมนะครับว่านี่เป็นการรวมไฟล์มาในหน้าเดียวเพื่อให้ง่ายต่อการเรียนรู้ การใช้งานจริงต้องวางตาม structure ของโปรเจคด้วยนะครับ

## การใช้งานในส่วนที่เหมือนกันกับ Cubit

ในหัวข้อนี้ผมจะไม่ได้ยกโค้ดขึ้นมาเป็นตัวอย่างเพราะกลัวว่าจะทำให้ซับสนและซ้ำซ้อนกับของเดิมได้ อยากให้โฟกัสกับจุดที่ต่างกันเท่านั้นจริงๆดีกว่า ซึ่งหัวข้อที่เหมือนกันกับการใช้ก็คือ

* [การ Import package](https://blog.nintech.dev/bloc-example-counter-app#heading-import-package)
    
* [การยัดเยียด state ไปใน widget tree ด้วย BlocProvider](https://blog.nintech.dev/bloc-example-counter-app#heading-cubit-widget-tree-blocprovider)
    

## การใช้งานที่แตกต่างจาก Cubit

ในส่วนหัวข้อด้านล่างนี้จะเป็นส่วนที่แตกต่างจากการใช้งาน Bloc ด้วย Cubit ทีเป็น function-driven โดยที่ตัวอย่างที่เราใช้ในบนความนี้จะเป็น Bloc library แบบ event-driven โดยที่ event จากแอพเคาน์เตอร์ก็จะมีเพียงแค่สอง event สำหรับเพิ่มและลดเคาน์เตอร์

### สร้าง Bloc class และกำหนด event

เมื่อเทียบกับ Cubit แล้ว Bloc จะมีการกำหนด Event ขึ้นมาแทนที่จะเรียกผ่าน method ด้วยตรง ในการที่จะ update state ต้องมีการเรียก event เพื่อ emit ค่าใหม่ออกไป

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1714279221787/2eb32939-93c5-45b3-b50e-a252ac037ed8.png align="center")

โดยที่ CounterEvent ของเราจะเป็น abstract class ซึ่งหมายถึงว่าคลาสนี้จะเป็นคลาสที่ไม่ได้มีการเรียก constructor เพื่อสร้าง instance ได้โดยตรง แต่จะเป็นคลาสที่โดนส่วนใหญ่จะต้องไป extends เพื่อประกาศเป็น class ใหม่ ซึ่งทำให้ class ที่ extends class นี้ไป (IncrementEvent และ DecrementEvent) ยังถือว่าเป็น class เดียวกัน ซึ่งก็คือ CounterEvent class

### การเรียก event เพื่ออัพเดตค่า state

ในการเรียก event ก็แทบจะคล้ายกับการเรียก method เลยแต่แทนที่จะเรียก method โดยตรง แต่จะเป็นการ add event เข้าไปที่ Bloc คล้ายๆกับการใส่ event เข้าไปใน stack แล้ว Bloc จะเช็คว่ารับ event อะไรมาแล้ว update state ตามเงื่อนไขที่เขียนไว้

```dart
floatingActionButton: Column(
        mainAxisAlignment: MainAxisAlignment.end,
        crossAxisAlignment: CrossAxisAlignment.end,
        children: <Widget>[
          FloatingActionButton(
            child: const Icon(Icons.add),
            onPressed: () => context.read<CounterBloc>().add(IncrementEvent()),
          ),
          const SizedBox(height: 8),
          FloatingActionButton(
            child: const Icon(Icons.remove),
            onPressed: () => context.read<CounterBloc>().add(DecrementEvent()),
          ),
        ],
      ),
```

## สรุป

ในบทความนี้ได้มีการแสดงการเขียนเคาน์เตอร์แอพด้วย Bloc แบบ event-driven เพื่อให้เห็นข้อแตกต่างที่ชัดเจนกับแบบ function-driven ด้วย Cubit ที่เขียนในบทความที่แล้ว จะเห็นว่าในการเขียนสิ่งเดียวกันการเขียนแบบ Bloc ปกติจะใช้มีโค้ดที่ต้องเตรียมไว้ (boiler plate) มากกว่าเพื่อจะซัพพอร์ตการเรียก event ต่างๆ ซึ่งถึงจุดนี้หลายคนน่าจะมีคำถามว่าแล้วทำไมเราต้องใช้ event-driven ด้วยทั้งๆที่แบบ function-driven ใช้งานง่ายกว่าตั้งเยอะ ซึ่งผมก็มีคำถามนี้เหมือนกัน เพราะว่ายังไม่มีโอกาสได้ใช้แบบ event-driven แบบจริงจัง ผมเลยถาม ChatGPT เพื่อหาคำตอบนี้และคำตอบที่ได้ก็น่าสนใจทีเดียว เดี๋ยวผมจะนำมาแชร์ในบทความต่อไปนะครับ

## ตัวอย่าง Code

%[https://gist.github.com/Khanin-devmode/a2b03574ad7ed3de88c366e455def5c8]