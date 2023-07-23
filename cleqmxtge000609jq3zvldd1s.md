---
title: "Debug & hot reload Flutter on multiple devices"
datePublished: Thu Mar 02 2023 04:57:28 GMT+0000 (Coordinated Universal Time)
cuid: cleqmxtge000609jq3zvldd1s
slug: debug-hot-reload-flutter-on-multiple-devices
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1677732961984/c85df21a-c4cc-4499-b95f-eb8c51618007.png
tags: flutter, debugging

---

When developing/changing UI or even implementing features in Flutter for multiple devices, it is inefficient to only reflect changes on one device. However, running flutter on multiple devices is not built into the default configuration. In this article, I will provide an example and how to set up this configuration in VScode.

### Add new configuation in VScode

1. In debug tab, select Run and Debug dropdown &gt; Select Add Configuration
    
2. This will create `launch.json` file under .vscode folder.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677730228711/430d6fe8-0b39-4283-9fc1-ac75942cbca8.png align="center")

Default configuration file

```dart
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "coolors_inspired_flutter",
            "request": "launch",
            "type": "dart"
        },
        {
            "name": "coolors_inspired_flutter (profile mode)",
            "request": "launch",
            "type": "dart",
            "flutterMode": "profile"
        },
        {
            "name": "coolors_inspired_flutter (release mode)",
            "request": "launch",
            "type": "dart",
            "flutterMode": "release"
        },
    ]
}
```

### Check connected devices for device id

run `flutter devices` we need devices id for configuration.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677728463249/8e9a64b2-5a00-4373-bcd0-28aa92fc564c.png align="center")

### Add devices to the configuration file

We can put any name to a device but, the device id must match the value of the connected devices. If you push this file to the remote repository and other developers pull it, they won't be able to run as the device id is unique to each emulator and device. So better keep this configuration in the local repository.

To run multiple devices we put the device name in the array. The name put in this array must match the device name we put manually.

```dart
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "coolors_inspired_flutter",
            "request": "launch",
            "type": "dart"
        },
        {
            "name": "coolors_inspired_flutter (profile mode)",
            "request": "launch",
            "type": "dart",
            "flutterMode": "profile"
        },
        {
            "name": "coolors_inspired_flutter (release mode)",
            "request": "launch",
            "type": "dart",
            "flutterMode": "release"
        },
        {
            "name": "iPhone 14 Pro Max Emulator",
            "request": "launch",
            "type": "dart",
            "deviceId": "219457B3-D3F1-4285-8AC6-61E64DAAF132"
        },
        {
            "name": "Pixel 6A Emulator",
            "request": "launch",
            "type": "dart",
            "deviceId": "emulator-5554"
        }
    ],
    "compounds": [
        {
          "name": "All Mobile",
          "configurations": ["Pixel 6A Emulator", "iPhone 14 Pro Max Emulator"]
        }
     ]
}
```

### Select debug target

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677728674730/5afc788e-c70f-482b-9786-9dfc6f1f56ae.png align="center")

### Managing multiple debug sessions

We can select debug session from both the Run and Debug tab or select from the dropdown in the Flutter helper toolbar.

Run and Debug session.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677732911684/1308e2ba-1835-42da-aada-eff4616efb89.png align="center")

Flutter helper toolbar.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677729826935/5c02aa88-d9ef-443d-9d69-e83177f28708.png align="center")

### Conclusion

I have provided an example of the configuration file and how to get the device id to the configuration, I struggled to identify the device id to put it in the configuration and hope this helps other developer save some time. With this configuration, we can debug multiple devices at the same time and hot reload on all devices when changes are made. Tbh, this is satisfying and helps fix bugs and UI on each platform much faster.