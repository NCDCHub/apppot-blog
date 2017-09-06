---
layout: post
title: "AppPot Android SDK v3のサンプル"
date: 2017-08-19 19:43:23
description: 'AppPotのAndroid SDKをリリースしました。'
main-class: 'dev'
color: '#B31917'
tags:
- AppPot
- Android
categories:
twitter_text: 'AppPotのAndroid SDKをリリースしました。'
introduction: 'AppPotのAndroid SDKをリリースしました。'
---

1. AppPot SDKのインポート
AppPot SDKのjarをライブラリとして追加します。

![](sample-android-sdk-import-lib.png)

```
dependencies {
    compile fileTree(include: ['*.jar'], dir: 'libs')
    compile files('libs/AppPotSDK-3.2.0.jar')
    // その他略
}
```
