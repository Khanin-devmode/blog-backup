---
title: "Hands-on with Image FilterQuality in Flutter Through a Demo App"
seoTitle: "Exploring Image FilterQuality in Flutter"
seoDescription: "Explore how FilterQuality in Flutter enhances image rendering across devices through a detailed demo app and practical insights"
datePublished: Thu Dec 12 2024 12:30:08 GMT+0000 (Coordinated Universal Time)
cuid: cm4larig2000509mkhiuh8dpq
slug: hands-on-with-image-filterquality-in-flutter-through-a-demo-app
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1733991649533/55788d60-7567-4dd1-8aae-06d40daadcb5.png
tags: flutter

---

Images are an essential component of every application. Understanding how to render images in Flutter will help us understand the components needed to render images clearly on all devices, ensuring that the user experience is not compromised by blurry images, which is fundamental in developing any application. In this article, we will discuss a property of the Image widget used in rendering, which is FilterQuality. This property enhances the quality of images by adjusting between `none`, `low`, `medium`, and `high`. We will explore how each setting affects the image through a demo application to provide a clearer understanding from a real application, along with a summary of how to apply it at the end.

## **Why FilterQuality is Necessary**

As mentioned in the introduction, FilterQuality is merely an "enhancement" to image quality. This means that if the image used has pixel resolution matching the display area, using `FilterQuality.none` is sufficient to achieve the highest image quality, as referenced from the [Flutter API Docs](https://api.flutter.dev/flutter/widgets/Image/filterQuality.html)

> If the image is of a high quality and its pixels are perfectly aligned with the physical screen pixels, extra quality enhancement may not be necessary. If so, then [FilterQuality.none would be the most](https://api.flutter.dev/flutter/dart-ui/FilterQuality.html) efficient.

However, since our application must support multiple device sizes, a significant factor that varies between devices is the Device Pixel Ratio (DPR), which determines the size of the area needed to display the image. This makes it highly likely that the pixel size of the image does not match the display area size, including cases where the display area is smaller than the image size.

FilterQuality helps improve image rendering when the display area and image pixel size do not match. But how significant is the effect? Let's look at the demo application.

## **Demo Application**

The purpose of the application is to help visualize the relationship between **initial file size x display area size (Container) x FilterQuality** in a tangible way through real devices or simulators. If viewed with the naked eye, it is recommended to use a real device rather than a simulator, as the computer screen resolution is another factor. The code for this demo can be copied from the end of the article.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733901979230/4298bc29-2773-4eb7-9802-e442438fbb81.gif align="center")

In the next section, we will summarize the main points related to using Image and FilterQuality in Flutter, along with images from the demo application to confirm the points discussed.

However, the images will be zoomed in for pixel peeping from screenshots via an iPad, as using normal size images in the article may not clearly show the differences. It is recommended to copy the application and try it on a real device if you have any doubts or want to see the actual impact.

## **Image Size Needed for the Sharpest Image**

Suppose we have a Container size of 50×50 pixels and an image size of 50×50 pixels. This is what we get:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733905504870/f5eec1ef-bc77-47c6-91fd-b1f6daa810da.png align="center")

You will notice that the image is not sharp regardless of the FilterQuality value. Adding FilterQuality only helps reduce pixelation.

Why is a 50×50 pixel image not sufficient for the Container, causing pixelation? Because we need to consider the DPR value. I have display this DPR value at the top of the application.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733905970156/2979d854-e156-4d25-b6a5-ce5f6611aa38.png align="center")

For an iPad 11” M3, the DPR is 2.0. This value is used to multiply with the logical pixel size of the container, meaning the image needed for this Container is (50×50)\*2.0 pixels or 100×100 pixels. From the demo app, it is clear that for a 50×50 Container, an initial image size of 100×100 is significantly sharper than a 50×50 image.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733906592887/2dc3c098-d01f-40cd-8582-fd7e184c9daa.png align="center")

Since our application must support multiple devices, we can use this conclusion to determine the image size needed for the project easily:

> ***Initial image size = Display area size \* Device Pixel Ratio (highest of the devices we want to support)***

## **Over-compressed Images can causes Artifacts**

From the image below, you can see that at a 50×50 container size, an initial image size of 200×200 shows compression artifacts (unlike pixelation, which spreads across the image).

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733907489280/cdac0cb1-7a05-44ec-bee1-580bcec7a6c1.png align="center")

We can easily fix this by using `FilterQuality.medium`, as referenced from the [Flutter API docs](https://api.flutter.dev/flutter/dart-ui/FilterQuality.html)

> When scaling down, [medium provi](https://api.flutter.dev/flutter/dart-ui/FilterQuality.html)des the best quality especially when scaling an image to less than half its size or for animating the scale factor between such reductions. Otherwise, [low an](https://api.flutter.dev/flutter/dart-ui/FilterQuality.html)d [high pro](https://api.flutter.dev/flutter/dart-ui/FilterQuality.html)vide similar effects for reductions of between 50% and 100% but the image may lose detail and have dropouts below 50%.

Flutter documentation recommends using FilterQuality.medium, which provides better clarity than low and high, which offer similar quality when used in smaller areas, as shown in the demo app image below.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733908821781/5ec3b52f-b5ee-446c-83f9-4290f0dc2b02.png align="center")

Personally, I find it hard to distinguish in this area because they are very similar, but let's use the value recommended by Flutter. In practical use, I would summarize it as follows:

> ***When using Image, set*** `FilterQuality.medium` ***as the default value.***

## **Conclusion**

After experimenting with the demo application, there are two key points to remember when developing applications: don't forget to consider the DPR of the devices you need to support, and set the default value of FilterQuality to FilterQuality.medium as a middle ground that helps display images well across various uses.

I hope this article helps you understand the purpose of FilterQuality and its usage principles better. If you want to try the code yourself, you can clone it from the GitHub link below.

## References

* Github: [https://github.com/Khanin-devmode/flutter-filter-quality-example.git](https://github.com/Khanin-devmode/flutter-filter-quality-example.git)
    
* [https://api.flutter.dev/flutter/widgets/Image/filterQuality.html](https://api.flutter.dev/flutter/widgets/Image/filterQuality.html)
    
* [https://api.flutter.dev/flutter/dart-ui/FilterQuality.html](https://api.flutter.dev/flutter/dart-ui/FilterQuality.html)