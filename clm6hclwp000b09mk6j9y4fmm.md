---
title: "Easy Dark Mode Setup in Flutter: Counter App Example"
seoTitle: "Dark Mode Flutter: Counter App Example"
seoDescription: "Implement dark mode in Flutter counter app using themes, toggle states, Riverpod state management; easy setup, minimal code alterations"
datePublished: Tue Sep 05 2023 15:43:16 GMT+0000 (Coordinated Universal Time)
cuid: clm6hclwp000b09mk6j9y4fmm
slug: easy-dark-mode-setup-in-flutter-counter-app-example
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1702539341299/07fa19c1-1ff0-4c05-ae3f-71001994a6b7.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1693929475990/3b70c15c-84d6-4c04-bbcd-523d2d730176.png
tags: flutter, dark-mode

---

Dark mode in Flutter is very simple to implement. Specifies themes for each dark and light mode and then toggles states between the two. The example below shows a minimalistic setup starting from the counter app.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693928480174/541ff2d1-132d-4b01-ae7f-b5b6449744cd.gif align="center")

## Specify themes for both light and dark modes.

In `MaterialApp` widget specifies a `ThemeData()` for both to `theme:` and `darkTheme:` which corresponds to light and dark mode respectively. Also

```dart
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData.light(), // <-- Specify light ThemeData().
      darkTheme: ThemeData.dark(), // <-- Specify dark ThemeData().
      home: const MyHomePage(title: 'Flutter Demo Home Page'),
    );
  }
}
```

In the example, I use named constructors `ThemeData().light()` and `ThemeData().dark()` for convenience. But in real projects, we would create custom ThemeData and specify them here.

To switch between light and dark themes. We could add `themeMode:` properties like the following.

*main.dart*

```dart
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData.light(), 
      darkTheme: ThemeData.dark(), 
      //Add themeMode: property, which need isLightTheme state.
      themeMode: isLightTheme ? ThemeMode.light : ThemeMode.dark,
      home: const MyHomePage(title: 'Flutter Demo Home Page'),
    );
  }
}
```

But to make the above code work without using a third-party state management library can be a bit tedious. We have to change MyApp to StatefulWidget and pass down the function to toggle state down the widget tree.

# Manage state for changing themes.

In this example, I will use Riverpod library for state management, which helps lift the state up and allow widgets in the tree to access and update the state without passing any functions down the widget tree. This might look like a lot more work to do but actually can be easily done in very few lines.

I will show the final result along with step by step guide that associates with comments in the code.

Made sure to add Riverpod package with `flutter pub add flutter_riverpod`

1. **Add provider scope** to let the package know where will be the scope that we will cover with state management.
    
2. **Create a provider** for switching the theme state.
    
3. Change `MyApp` from Stateless widget to **Consumer widget**. This allows the widget to reference states in the provider. Also, add `WidgetRef ref` argument in the build method.
    
4. **Listen to the provider** with `ref.watch`. Any state change will update the widget.
    
5. Change `MyHomePage` from Stateful widget to **ConsumerStateful** widget.
    
6. We will also **listen to the provider**, so we can apply the current state to the switch widget.
    
7. Add the Switch widget to the app bar and supply a notifier to **update the state** in `onChanged` function.
    
8. \[opitional\] I removed the background-color property of the Scaffold widget for readability between two modes.
    

*main.dart*

```dart
import 'package:flutter/material.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';

void main() {
  runApp(const ProviderScope(child: MyApp())); // [1]
}

final isDarkModeProvider = StateProvider<bool>((ref) => true); //[2]

class MyApp extends ConsumerWidget { // [3]
  const MyApp({super.key});

  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final isDarkMode = ref.watch(isDarkModeProvider); // [4]

    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData.light(),
      darkTheme: ThemeData.dark(),
      themeMode: isDarkMode ? ThemeMode.dark : ThemeMode.light,
      home: const MyHomePage(title: 'Flutter Demo Home Page'),
    );
  }
}

class MyHomePage extends ConsumerStatefulWidget { //[5]
  const MyHomePage({super.key, required this.title});

  final String title;

  @override
  MyHomePageState createState() => MyHomePageState();
}

class MyHomePageState extends ConsumerState<MyHomePage> {
  int _counter = 0;

  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    final isDarkMode = ref.watch(isDarkModeProvider); // [6]

    return Scaffold(
      appBar: AppBar(
        title: Text(widget.title),
        actions: [
          Switch(
              value: isDarkMode,
              onChanged: (newValue) => ref 
                  .read(isDarmModeProvider.notifier)
                  .update((state) => newValue)) //[7]
        ],
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            const Text(
              'You have pushed the button this many times:',
            ),
            Text(
              '$_counter',
              style: Theme.of(context).textTheme.headlineMedium,
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _incrementCounter,
        tooltip: 'Increment',
        child: const Icon(Icons.add),
      ),
    );
  }
}
```

The code might look a bit long but we only added about 10 lines to the original counter app. And below is a cat gif as your reward.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693926821881/579967fb-a1a4-4024-9a1c-ae2b30d53a81.gif align="center")

We added themes for both light and dark modes. Adding state to toggle between dark and light mode and using Riverpod as state management. Setting up themes itself is simple While state management with Riverpod might be a bit overkill in this example I think it makes the result cleaner and it is also a minimal example to get started with Riverpod or state management Flutter as well. I will be adding more articles regarding themes and Riverpod in the future. Let's keep in touch. Cheers.