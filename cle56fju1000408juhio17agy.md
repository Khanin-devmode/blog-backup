---
title: "Implement Firebase Authentication, Google Sign-in, in Flutter."
datePublished: Wed Feb 15 2023 04:32:12 GMT+0000 (Coordinated Universal Time)
cuid: cle56fju1000408juhio17agy
slug: implement-firebase-authentication-google-sign-in-in-flutter
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1677249734793/f2bb3825-c96f-45ad-b492-985e980854d7.png
tags: flutter, google-sign-in, firebaseauth

---

Implementing Firebase in Flutter is a straightforward process, you install firebase core, config it and then install Firebase Authentication. However, in my experience, I found documentation is a separate piece that I have to connect I have to connect them myself. I will list the step I took and links to official/unofficial docs to implement and solve problems I found in this article.

### Content

1. Install Firebase Core
    
    * Configure Firebase for your flutter project
        
2. Install Firebase Auth and implement it with Riverpod.
    
3. Implement Google Sign-in
    
    * Fix an error on debug version of Android.
        

---

### Install Firebase Core

* Install Firebase core and init firebase project in Flutter.
    
    * The first guide to follow: [https://firebase.google.com/docs/flutter/setup?platform=ios](https://firebase.google.com/docs/flutter/setup?platform=ios)
        
    * Problem found when running: `flutterfire configure`
        
        * Flutterfire not found. Need to specify a new path.
            
    * `flutterfire configure` will generate config files for each platform.
        
        * need to run again whenever adding a new platform or adding more firebase packages.
            

---

### Installing firebase auth.

[https://firebase.google.com/docs/auth/flutter/start](https://firebase.google.com/docs/auth/flutter/start)

* Implement firebase\_auth with Riverpod guide: [https://bishwajeet-parhi.medium.com/firebase-authentication-using-flutter-and-riverpod-f302ab749383](https://bishwajeet-parhi.medium.com/firebase-authentication-using-flutter-and-riverpod-f302ab749383)
    

---

### Implement sign-in with google.

* [https://pub.dev/packages/google\_sign\_in](https://pub.dev/packages/google_sign_in),
    
    * Follow each step carefully, as they have a specific setup for each platform.
        
    * on iOS, however, it is required that app must have a 'sign-in with apple' option to be approved on the app store.
        
        * [https://pub.dev/packages/sign\_in\_with\_apple](https://pub.dev/packages/sign_in_with_apple)
            
* **Problem found**
    
    * The application throws an error when finishing sign-in with google screen in Android.
        
    * ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675489868236/d4ba5d5e-5607-40bb-8196-20a377fe31cf.png align="center")
        
    * This happens in debug version on Android. While do not have any issues on iOS.
        
    * [https://stackoverflow.com/questions/54557479/flutter-and-google-sign-in-plugin-platformexceptionsign-in-failed-com-google](https://stackoverflow.com/questions/54557479/flutter-and-google-sign-in-plugin-platformexceptionsign-in-failed-com-google)
        
    * The root cause is, We need to add a "debug" version of the fingerprint in the Firebase console android app. The one that we successfully created is only for the production version of android.
        
    * ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676435616375/62ce2846-5e92-4c84-89e2-cb60da6cabb2.png align="center")
        
    * [https://developers.google.com/android/guides/client-auth?authuser=0&hl=en](https://developers.google.com/android/guides/client-auth?authuser=0&hl=en)
        
        * Get debug certificate fingerprint.
            
        * Problem found: cannot run keytool command, need to install java...
            
            * install java... from www.java.com
                
        * Following the guide, I use this command to get the debug.keystore. Since my app already publishes I can get the production keystore directly from Google Play Console.
            

```plaintext
  keytool -list -v \
  -alias androiddebugkey -keystore ~/.android/debug.keystore
```

* Then add debug keys from the result to SHA certificate fingerprint for Android application in Firebase. The app now sign-in successfully with Google on debug version of Android.
    

The most tedious process for me was to figure out what was the real problem that the error occurs. As the requirement is not clearly state in google\_sign\_in package but in google play service client auth guideline which is right. However a link as useful resources in package would save so much time from googling. Hope this article helpful for who might try implementing Firebase Auth for very first time. Cheers!