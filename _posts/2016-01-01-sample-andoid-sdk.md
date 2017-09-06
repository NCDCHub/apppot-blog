---
layout: post
title: "AppPot Android SDK v3のサンプル"
date: 2017-01-19 19:43:23
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

![](./images/sample-android-sdk-import-lib.png)

```
dependencies {
    compile fileTree(include: ['*.jar'], dir: 'libs')
    compile files('libs/AppPotSDK-3.2.0.jar')
    // その他略
}
```

2. AppPotの初期化とログイン



3. モデルの定義

`jp.co.ncdc.apppot.stew.dto.APObject`を継承したモデルクラスを定義します。

```
package jp.apppot.android.sample;

import jp.co.ncdc.apppot.stew.dto.APObject;

public class Company extends APObject {

    public int companyCode;
    public String companyName;
}
```

