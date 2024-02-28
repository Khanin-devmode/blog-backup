---
title: "FormState vs FormFieldState in Flutter"
seoDescription: "FormState validates entire forms in Flutter; FormFieldState targets individual fields"
datePublished: Wed Feb 28 2024 12:00:41 GMT+0000 (Coordinated Universal Time)
cuid: clt5qwaus000s09l98l3o0lyx
slug: formstate-vs-formfieldstate-in-flutter
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1709110485164/b4ed22f4-be0d-4b93-90a7-c61e7a6883d4.webp
tags: forms, flutter, form-validation

---

*TLDR;* `FormState` *will store the state such as validation for the entire form, but* `FormFieldState` *will store the state for only one*`TextFormField`*.*

When we code a form in our application, we will need to use `GlobalKey` cast with `FormState`, `GlobalKey<FormState>`, or cast with `FormFieldState`, `GlobalKey<FormFieldState>` to check the state of our form status, such as validating or not validating. Then how do these two classes differ and when are they used? Let's see from the examples below.

Actually, these two classes have specific uses: `FormState` is used for the `Form` widget to validate every `FormField` under the `Form` only. If `FormState` is used with `TextFormField`, the currentState will immediately be null because FormState only accepts types that are `Form`. We can easily see this from the class definition of `FormState`.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1709106516410/260e5b16-a765-4d16-b5fb-ca15a4ba1cc0.png align="center")

Similarly, FormFieldState is specifically used for FormField type widgets only, which means `TextFormField`, being a class built from FormField, can use FormFieldState. And if we use FormState with TextFormField, the currentState of FormState will also be null because it’s not used with the correct type.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1709106239195/1c2c10ab-7b6d-41b8-9421-3c94d3dfa988.png align="center")

The purpose of using both types is similar, for methods of State such as validate or reset. The difference is just that FormState is used for Forms that encompass every FormField, while FormFieldState is used for a single FormField only. However, even though the purpose of use is similar, they cannot be used interchangeably.

# Bonus: TextField vs. TextFormField

*TLDR;* `TextFormField` *is used with* `Form` *for validation. If you want a regular input text, use*`TextField`*.*

Suppose we don't have the `TextFormField` widget for creating a `Form`. To validate the `TextField`, we must encapsulate the `TextField` widget with another `FormField` widget. Since validating `TextField` is a frequent necessity in every app, Flutter has integrated it into `TextFormField` for the convenience of Flutter developers. In summary, if we want the `TextField` to have validation or be used in a form, use `TextFormField`. If not, a regular `TextField` will suffice.

# Summary

I hope this article helps you understand the use of widgets and classes with similar names, like FormState vs. FormFieldState or TextField vs TextFormField, more easily. Because everyone who seeing these widget groups for the first time will wonder how much they differ since their names are very similar. Let's summarize the points again:

* `FormState` is specifically used with `Form` to validate all `FormFields` within the Form.
    
* `FormFieldState` is used with `FormField` widgets and widgets that incorporate `FormField`, such as `TextFormField`.
    
* `TextFormField` is used alongside `Form` for validation.
    
* `TextField` is used when there is no need for validation.