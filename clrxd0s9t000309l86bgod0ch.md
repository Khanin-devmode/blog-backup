---
title: "Expanded widget tree of widget from packages in Flutter's Widget Inspector"
seoTitle: "Expanded Flutter Widget Tree Inspector Package"
seoDescription: "Use Flutter's Widget Inspector for enhanced widget tree access, in-depth analysis of package widgets, and better app development"
datePublished: Sun Jan 28 2024 10:30:24 GMT+0000 (Coordinated Universal Time)
cuid: clrxd0s9t000309l86bgod0ch
slug: expanded-widget-tree-of-widget-from-packages-in-flutters-widget-inspector
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1706437739552/c63f16d4-83bd-4420-b6a1-a48cd2ae6b91.png
tags: flutter, flutter-insector

---

Flutter's Widget Inspector is an indispensable tool when we develop applications with Flutter. Upon frequent usage, we will notice that sometimes when we want to see the deep structure of a widget, we cannot see the widget tree of the widget that we import from another package, nor can we choose to inspect the components of that widget. We have to go and look in the file ourselves to see what structure it has. The fact that we can directly view the widget tree of a widget helps us to understand more about how the widget is written and sometimes we might be able to reverse engineer to create the widget we want.

# Normal Widget Tree

Example of a Normal Widget Tree.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706437649950/79b4da54-d807-42fd-9dd3-a4791461e250.png align="center")

From the example picture, you can inspect the widget at a normal level, seeing only the Elevated Button and Text.

The purpose of the Flutter team deciding to block this access was because they saw that the widget tree would be too complex if not blocked and would hinder the use of Widget Inspector more than it would be beneficial. However, due to requests in this area, a way to access the widget tree from the package was added, as referenced in the links under the References section below.

# Widget Tree Accessing Widgets in Package

Example picture with config changed to see Widget Tree from package

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706437659493/4e720e2b-e05d-4157-8991-9a0e562f2ea0.png align="center")

From the example picture, you can see that within the original Elevated Button, there is a complex widget tree, which is why the Flutter Dev Tool initially blocks access. Otherwise, it would be overly complex, especially for beginners. But for those with experience, this can be very beneficial if we want to learn about the structure of the widgets we use.

# How to Set Up

The way is to put the path of the package located on our machine into the settings of the widget inspector, which means we have to clone the package to our machine first.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706437672983/c843dcbe-6b55-4626-a9b1-141a7c329001.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706437690963/4420c87f-df12-405a-9616-48b00cf69c0f.png align="center")

With this, we can see all the details of the widgets and better yet, when we select to inspect a widget, the editor will automatically navigate to the file and position of the widget's code, just like when we inspect the code we are writing. I hope this information will be useful to everyone, even if just a little. Thank you.

# References

* [https://github.com/flutter/devtools/issues/3919](https://github.com/flutter/devtools/issues/3919)
    
* [https://github.com/flutter/devtools/issues/3941](https://github.com/flutter/devtools/issues/3941)