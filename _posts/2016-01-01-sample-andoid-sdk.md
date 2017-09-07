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

## 1. AppPot SDKのインポート
AppPot SDKのjarをライブラリとして追加します。
また、AppPot SDKはgsonを使用しますので、合わせてライブラリとして追加します。

![](http://docs.apppot.jp/apppot/AndroidSDK_images/sample-android-sdk-import-lib.png)

アプリのディレクトリのlibs以下にjarを配置して、build.gradleファイルに以下の設定を追加します。
ライブラリのファイル名はバージョンによって異なりますので、使用する環境に合わせて変更してください。

```
dependencies {
    compile fileTree(include: ['*.jar'], dir: 'libs')
    compile files('libs/AppPotSDK-3.2.0.jar')
    compile files('libs/gson-2.2.4.jar')
    compile 'com.loopj.android:android-async-http:1.4.9'
    compile 'cz.msebera.android:httpclient:4.4.1.2'
    // その他のライブラリの設定は省略
}
```

## 2. AppPotの初期化とログイン

Login画面のActivityのonCreateなど起動時に実行される場所で、AppPotを使うための初期化を行います。
APService.setServiceInfoメソッドを利用して、companyIdやappId, appVersion, appKeyなどを指定します。
これらの値は実際には、設定ファイルに持つなどして下さい。

```

@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);

    // 略

    initAppInfo();
}

private void initAppInfo() {
    final APService service = APService.getInstance();
    service.setLogLevel(LogLevel.verbose);
    if (service.isInit()) {
        return;
    }

    Log.v(this.getLocalClassName(), "initAppInfo start");
    final Resources r = getResources();
    try {
        service.setServiceInfo(
                getApplicationContext(),
                AppPotConfig.companyId,
                AppPotConfig.appID,
                AppPotConfig.appKey,
                AppPotConfig.appVersion,
                AppPotConfig.hostName,
                AppPotConfig.contextRoot,
                AppPotConfig.portNumber,
                AppPotConfig.isUsePushNotification,
                AppPotConfig.isUseSSL);

    } catch (Exception e) {
        Toast.makeText(getApplicationContext(), e.getMessage(), Toast.LENGTH_SHORT).show();
    }

    Log.v(this.getLocalClassName(), "initAppInfo end");
}
```

## 3. モデルの定義

`jp.co.ncdc.apppot.stew.dto.APObject`を継承したモデルクラスを定義します。

```
package jp.apppot.android.sample;

import jp.co.ncdc.apppot.stew.dto.APObject;

public class Company extends APObject {

    public int companyCode;
    public String companyName;
}
```


