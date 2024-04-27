---
title: "แนะนำการใช้ Bloc ด้วยเคานท์เตอร์แอพแบบ event-base ในไฟล์เดียวจบๆ (เหมือนเดิม)"
slug: bloc-event-base

---

ต่อจากบทความที่แล้ว เกี่ยวกับการใช้ Bloc library ด้วย Cubit ในบทความนี้จะเป็นบทความการใช้ Bloc library เช่นเดียวกันแต่เป็นรูปแบบ event-base ที่เป็น Bloc pattern แบบเต็มตัว จุดประสงค์คือให้เห็นการใช้งานที่ง่ายที่สุดในเคาน์เตอร์แอพ และเปรียบเทียบกับการใช้แบบ Cubit ได้ชัดเจน เพื่อให้เห็นข้อดีและข้อเสีย เพื่อเป็นข้อมูลในประกอบการตัดสินใจเลือกว่าจะใช้วิธีไหนในการพัฒนาแอพพลิเคชั่น

อย่าลืมนะครับว่านี่เป็นการรวมไฟล์มาในหน้าเดียวเพื่อให้ง่ายต่อการเรียนรู้ การใช้งานจริงต้องวางตาม structure ของโปรเจคด้วยนะครับ

## การใช้งานในส่วนที่เหมือนกันกับ Cubit

ในหัวข้อนี้ผมจะไม่ได้ยกโค้ดขึ้นมาเป็นตัวอย่างเพราะกลัวว่าจะทำให้ซับสนและซ้ำซ้อนกับของเดิมได้ อยากให้โฟกันกับจุดที่ต่างกันเท่านั้นจริงๆดีกว่า ซึ่งหัวข้อที่เหมือนกันกับการใช้ก็คือ

* การ import package
    
* การยัดเยียด state ไปใน widget tree ด้วย BlocProvider
    

## การใช้งานที่แตกต่างจาก Cubit

### สร้าง Bloc class และกำหนด event

```dart
// Bloc Implementation
class CounterBloc extends Bloc<CounterEvent, int> {
  CounterBloc() : super(0) {
    on<IncrementEvent>((event, emit) => emit(state + 1));
    on<DecrementEvent>((event, emit) => emit(state - 1));
  }
}

// Bloc Event
abstract class CounterEvent {}
class IncrementEvent extends CounterEvent {}
class DecrementEvent extends CounterEvent {}
```

เมื่อเทียบกับ Cubit แล้ว Bloc จะมีการกำหนด Event ขึ้นมาแทนที่จะเรียกผ่าน method ด้วยตรง ในการที่จะ update state ต้องมีการเรียก event เพื่อ emit ค่าใหม่ออกไป

### การเรียก event เพื่ออัพเดตค่า state

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

ในการเรียก event ก็แทบจะคล้ายกับการเรียก method เลยแต่แทนที่จะเรียก method โดยตรง แต่จะเป็นการ add event เข้าไปที่ Bloc คล้ายๆกับการใส่ event เข้าไปใน stack แล้ว Bloc จะเช็คว่ารับ event อะไรมาแล้ว update state ตามเงื่อนไขที่เขียนไว้

## สรุป

ในบทความนี้ได้มีการแสดงการเขียนเคาน์เตอร์แอพด้วย Bloc แบบ event-driven เพื่อให้เห็นข้อแตกต่างที่ชัดเจนกับแบบ function-driven ด้วย Cubit ที่เขียนในบทความที่แล้ว จะเห็นว่าในการเขียนสิ่งเดียวกันการเขียนแบบ Bloc ปกติจะใช้มีโค้ดที่ต้องเตรียมไว้ (boiler plate) มากกว่าเพื่อจะซัพพอร์ตการเรียก event ต่างๆ ซึ่งถึงจุดนี้หลายคนน่าจะมีคำถามว่าแล้วทำไมเราต้องใช้ event-driven ด้วยทั้งๆที่แบบ function-driven ใช้งานง่ายกว่าตั้งเยอะ ซึ่งผมก็มีคำถามนี้เหมือนกัน เพราะว่ายังไม่มีโอกาสได้ใช้แบบ event-driven แบบจริงจัง ผมเลยถาม ChatGPT เพื่อหาคำตอบนี้และคำตอบที่ได้ก็น่าสนใจทีเดียว เดี๋ยวผมจะนำมาแชร์ในบทความต่อไปนะครับ

## ตัวอย่าง Code

%[https://gist.github.com/Khanin-devmode/a2b03574ad7ed3de88c366e455def5c8]