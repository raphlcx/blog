---
title: "Tech note: Android LineageOS 15.1 without Google"
date: 2024-10-30T23:26:46+08:00
---

My experience from 2018 running LineageOS 15.1 on a Sony Xperia Z1 without Google services.

## Gesture Input

AOSP doesn’t include gesture input by default, but it can be added manually:

  1. Download the appropriate Google Apps package.
  1. Extract it, and from the Optional folder, open `swypelibs-lib-arm.tar.lz` to find the file `libjni_latinimegoogle.so`.
  1. Use `adb` to transfer this file to your phone: `adb push /path/to/libjni_latinimegoogle.so /system/lib`.
  1. Reboot the phone to complete setup.

## Apps

You can download apps from APKMirror. To install them, use:

```
adb install -r app-name.apk
```

## Google-less Setup Alternative

Alternatively, you can install Open GApps but skip signing in to your Google account on the device. Apps can still be installed manually from APKMirror.
