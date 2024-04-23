---
title: "แนะนำการใช้ Bloc ด้วยเคานท์เตอร์แอพแบบ event-base ในไฟล์เดียวจบๆ (เหมือนเดิม)"
slug: bloc-event-base

---

ต่อจากบทความที่แล้ว เกี่ยวกับการใช้ Bloc library ด้วย Cubit ในบทความนี้จะเป็นบทความการใช้ Bloc library เช่นเดียวกันแต่เป็นรูปแบบ event-base ที่เป็น Bloc pattern แบบเต็มตัว จุดประสงค์คือให้เห็นการใช้งานที่ง่ายที่สุดในเคาน์เตอร์แอพ และเปรียบเทียบกับการใช้แบบ Cubit ได้ชัดเจน เพื่อให้เห็นข้อดีและข้อเสีย เพื่อเป็นข้อมูลในประกอบการตัดสินใจเลือกว่าจะใช้วิธีไหนในการพัฒนาแอพพลิเคชั่น

อย่าลืมนะครับว่านี่เป็นการรวมไฟล์มาในหน้าเดียวเพื่อให้ง่ายต่อการเรียนรู้ การใช้งานจริงต้องวางตาม structure ของโปรเจคด้วยนะครับ

หัวข้อที่แตกต่างกันจะเป็นในส่วนการสร้าง class ที่เก็บ state จาก Cubit เป็น Bloc การเรียกใช้ state ใน Bloc

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

เมื่อเทียบกับ Cubit แล้ว Bloc จะมีการกำหนด Event ขึ้นมาแทนที่จะเรียกผ่าน method ด้วยตรง

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

%[https://gist.github.com/Khanin-devmode/a2b03574ad7ed3de88c366e455def5c8]