---
title: "Stream Firestore data with Riverpod in Flutter"
datePublished: Mon Feb 27 2023 08:40:48 GMT+0000 (Coordinated Universal Time)
cuid: clemklgpg00060alcf79y69a3
slug: stream-firestore-data-with-riverpod-in-flutter
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1677249463678/92fe2f68-c5c9-4cb0-b0f2-e24d7a6450da.png
tags: flutter, firestore, riverpod, streamprovider

---

Flutter and Firebase are widely used together. However, when adding state management such as Riverpod there are not many examples available to demonstrate how to implement them together. In this blog, I will share how I stream data from Firebase with Riverpod as state management in Flutter. It might not be the best implementation but this works well enough and is easy to understand and use. Also, as usual, I will provide the link to the resources I referenced in the credits section.

### What I wanted to achieve

In the Coolors Clone app, users can save their favorite color and be able to use it later. A very simple feature.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677483326537/15e6fc5e-98e2-4616-b89f-1e1295c9bea3.png align="center")

Collection and field store in Firestore.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677483578841/f0aa45ae-9c1c-4612-999e-05d79ebae22b.png align="center")

### Using Riverpod `StreamProvider` with Firebase query snapshot.

I create `StreamProvider` in one of the logic files called *db\_logic.dart.* with explanation comments in another code snippet.

```dart
final savedColorStreamProvider = StreamProvider<List<ColorDoc>>((ref) async* {

  final FirebaseFirestore firestore = FirebaseFirestore.instance;
  var allColorDoc = const <ColorDoc>[];

  final user = ref.watch(authStateProvider).value;

  await for (var snapshot in firestore
      .collection(kSavedColors)
      .where('createdBy', isEqualTo: user!.uid)
      .snapshots()) {
    allColorDoc = [];

    for (DocumentSnapshot colorDoc in snapshot.docs) {
      Color color = hexToColor(colorDoc.get('colorHex'));

      var savedColor = ColorDoc(colorDoc.id, color);

      allColorDoc = [...allColorDoc, savedColor];
      yield allColorDoc;
    }
  }
});
```

Next, with comments explanation.

```dart
//Create stream provider that return stream list of ColorDoc objects. ColorDoc is a model class in models.dart.
final savedColorStreamProvider = StreamProvider<List<ColorDoc>>((ref) async* {

  //create instance of Firestore and variable to hold return data.
  final FirebaseFirestore firestore = FirebaseFirestore.instance;
  var allColorDoc = const <ColorDoc>[];

  //get auth state from auth state provider, which is another provider in the app.
  final user = ref.watch(authStateProvider).value;

  //Each Snapshot contain list of documents queried from Firestore.
  await for (var snapshot in firestore
      .collection(kSavedColors)//- kSavedColors = 'SavedColors', which I used as constants through out the app to minimize type error.  
      .where('createdBy', isEqualTo: user!.uid) //Query snapshot with user id
      .snapshots()) {
    //everytime data is updated new snapshot will be created.
    allColorDoc = [];

    //iterate through each documents in a snapshot
    for (DocumentSnapshot colorDoc in snapshot.docs) {
      Color color = hexToColor(colorDoc.get('colorHex'));

      var savedColor = ColorDoc(colorDoc.id, color);

      allColorDoc = [...allColorDoc, savedColor];
      yield allColorDoc; //similar to return but will not terminate the function
    }
  }
});
```

### Create List&lt;widgets&gt; from `StreamProvider`

Now that we can stream saved colors from the provider we will create a list of widgets with this stream. To make code easier to read, I will remove code that is not related to this topic. If you want to see a full code I will provide a link to this repo at the end.

```dart
import 'package:coolors_inspired_flutter/logics/db_logic.dart';
import 'package:coolors_inspired_flutter/models.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';
import 'package:flutter/material.dart';

class ColorPickerTab extends ConsumerWidget {
  const ColorPickerTab({
    Key? key,
  }) : super(key: key);

  @override
  Widget build(BuildContext context, WidgetRef ref) {

    //call provider and store stream value, ref.watch will watch for changes and return new value.
    final userSavedColors = ref.watch(savedColorStreamProvider);

    return Center(
      child: userSavedColors.when(//return listview when data is available.
        data: (savedColors) {
          return ListView.builder(
            reverse: false,
            itemCount: savedColors.length,
            itemBuilder: (context, index) {
              ColorDoc color = savedColors[index];
              return SavedColorRow(
                  //Custom Widget
                  color: color.color,
                  notifier: colorlistNofifier,
                  activeIndex: activeIndex,
                  docId: color.docId,
                  db: db);
            },
          );
        },
        error: (error, stackTrace) => Text(error.toString()),
        loading: () => const CircularProgressIndicator(),
      ),
    );
  }
}
```

Link to the files in git:

* [https://github.com/Khanin-devmode/coolors\_inspired\_flutter/blob/main/lib/logics/db\_logic.dart](https://github.com/Khanin-devmode/coolors_inspired_flutter/blob/main/lib/logics/db_logic.dart)
    
* [https://github.com/Khanin-devmode/coolors\_inspired\_flutter/blob/main/lib/components/color\_picker\_tab.dart](https://github.com/Khanin-devmode/coolors_inspired_flutter/blob/main/lib/components/color_picker_tab.dart)
    

The solution might be overkilled for just simply getting saved data from the database. But I think it is more convenient that we do not need to worry to update the UI every time data is changed or calling the data every time that we build the UI. I am sure that this code is not perfect but it could give you a general idea and example code of how to integrate Flutter, Firestore and Riverpod, at least as a starting point.

### Credits

Below are the resources I referenced to develop this solution.

* [https://mypace-co-ltd.medium.com/how-to-use-a-stream-which-is-listening-firestore-collection-in-multiple-screens-with-minimal-e06fde2f7912](https://mypace-co-ltd.medium.com/how-to-use-a-stream-which-is-listening-firestore-collection-in-multiple-screens-with-minimal-e06fde2f7912)
    
* [https://riverpod.dev/docs/providers/stream\_provider](https://riverpod.dev/docs/providers/stream_provider)
    
* [https://parthpanchal53.medium.com/flutter-firebase-firestore-crud-app-using-riverpod-981811e2a73d](https://parthpanchal53.medium.com/flutter-firebase-firestore-crud-app-using-riverpod-981811e2a73d)
    
* [https://www.appbrewery.co/p/flutter-development-bootcamp-with-dart](https://www.appbrewery.co/p/flutter-development-bootcamp-with-dart) on **Section 15: Flash Chat - Flutter x Firebase Cloud Firestore**