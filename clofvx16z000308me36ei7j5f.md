---
title: "Working with minLines and maxLines properties in Flutter TextField"
seoTitle: "Flutter TextField: minLines, maxLines Properties"
seoDescription: "Master Flutter TextField: minLines & maxLines properties, single/multi-line fields, expandable input."
datePublished: Wed Nov 01 2023 15:00:24 GMT+0000 (Coordinated Universal Time)
cuid: clofvx16z000308me36ei7j5f
slug: working-with-minlines-and-maxlines-properties-in-flutter-textfield
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1707666441389/910a900d-4f3a-4788-bcd8-ad40a049797d.webp
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1698850780773/c7ac0b59-5330-49b1-990b-007a18b4120e.gif
tags: flutter

---

Text input in Flutter is simple to use. But when we want certain behaviors to occur each property, e.g. maxline and minline, when combined, has some unpredictable behavior when we try to understand their usage for the first time. In this article, I will provide you with examples for each behavior and its code example.

## Behavior 0: Single-line text field.

This is the default behavior when not supplying neither `maxLines` on `minLines` . The text field will stay in one line.

## Behavior 1: Multi-line text field.

Expand the text input field to numbers of `maxLine` parameters.

```dart
TextFormField{
    maxLines: 3
}
```

## Behavior 2: Expandable text field.

This could be what most people looking for, the text field shows as a single-line text field, `minLines: 1`, but will expand as users type beyond a line and capped out at a number of lines indicated at `maxLines`.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1698850212371/3e2e12a0-facd-477d-b934-da03d4b0535b.gif align="center")

```dart
TextFormField{
    minLines: 1,
    maxLines: 3
}
```

## Assert error: when `minLine` &gt; `maxLine`.

The compiler will give us an error when trying to use **only** `minLine` or **when** `minLine` **is higher than** `maxLine` which conflicts with each other.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1698850348701/dfb9c60f-d6c7-447a-8b79-428b13e78191.png align="center")

Hope this article helps save you some time in customizing your text field to match the desired behavior. Cheers, and happy coding.