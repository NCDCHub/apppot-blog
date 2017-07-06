---
layout: post
title: "AppPot JavaScript SDKのGateway APIのサポート"
date: 2017-07-05 22:43:23
description: 'AppPot Javascript SDKで他システムを簡単に呼び出せるGateway APIのサポートを開始しました。'
main-class: 'dev'
color: '#B31917'
tags:
- Dev
- JavaScript
- 他システム連携
categories:
twitter_text: 'AppPot Javascript SDKで他システムを簡単に呼び出せるGateway APIのサポートを開始しました。'
introduction: 'AppPot Javascript SDKで他システムを簡単に呼び出せるGateway APIのサポートを開始しました。'
---

AppPotには他のデータベースや、WebサービスをAppPotを経由して簡単に呼び出せるGateway APIという機能があります。APIの詳細はこちらをご覧ください。
[Gateway API](http://docs.apppot.jp/apppot/09.GatewayAPI.html)

今回、AppPotのJavaScript SDKで、Gateway APIのサポートを開始しましたので、使い方をお知らせします。
iOS、Android SDKでも順次提供を開始予定です。

## SDKの更新
Gateway APIはJavaScript SDKのバージョン`2.3.27`からサポートされていますので、`npm update`を実行して、SDKを最新にしてください。

## SDKのインターフェイス仕様
Gateway APIはGET、POST、PUT、REMOVEのメソッドを持っています。

```
const AppPot = AppPotSDK.getService(config);

// TODO 認証などの処理

AppPot.Gateway.get(serviceName, url, queryparam, body, option)
  .then((response) => {
     ...
  });
AppPot.Gateway.post(serviceName, url, queryparam, body, option)
  .then((response) => {
     ...
  });
AppPot.Gateway.put(serviceName, url, queryparam, body, option)
  .then((response) => {
     ...
  });
AppPot.Gateway.remove(serviceName, url, queryparam, body, option)
  .then((response) => {
     ...
  });
```

## パラメタ
各パラメタの説明は以下の通りです。

- serviceName: AppPot管理コンソールで設定したサービスの名前
- url: 連携先に渡すURLパス
- queryparam: URL末尾に付加する、クエリパラメータ `{key: value}` の形式のオブジェクトか、文字列を指定できる。使用しない場合はnullを指定する。
- body: 連携先に渡すBody部。使用しない場合はnullを指定する。
- option: 以下のオプションを指定可能
	- original: Boolean  
	  連携先サービスのレスポンスをそのまま受け取るかどうか。
	  `true` を指定すると、`{error: {error時の返答}, response: {連携先のHTTPレスポンス}}` という形式でレスポンスを受け取れる
	  `false` の場合、レスポンスのbodyをjsonとして解釈したオブジェクトを受け取る

以下は、DBコネクタを使って、データベースに接続する際の、クライアントの実装のイメージです。

```
// AppPotに登録されているサービス名
const serviceName = 'mysql-service';

// テーブル名
const tableName = 'Building';

// データの登録
AppPot.Gateway.post(serviceName, tableName, null, {
	"BUILDINGID":"00001",
	"BUILDINGNAME":"NCDC00001",
	"CREATEDDATE":"2017/01/01 09:10",
	"UPDATEDATE":"2017/01/01 09:10"
})
.then(function(json){
	// 成功した時の処理
});

// データの取得 jsonでデータが受け取れる(option未指定)
AppPot.Gateway.get(serviceName, tableName, {
	"BUILDINGID":"NCDC00001",
})
.then(function(json){
	// 成功した時の処理
});

// データの取得 (option指定：JSON形式でデータを受信)
AppPot.Gateway.get(serviceName, tableName, {
	"BUILDINGID":"NCDC00001",
	}, null, { original: false })
.then(function(json){
	// 成功した時の処理
});

// データの取得 (option指定：連携先の形式でデータを受信)
AppPot.Gateway.get(serviceName, tableName, {
	"BUILDINGID":"NCDC00001",
	}, null, { original: true })
.then(function(result){
	// 成功した時の処理
});

// データの取得 select文を発行し、データを取得できる
AppPot.Gateway.get(serviceName, 'query', {
	query: "select BUILDINGID, BUILDINGNAME from Building where BUILDINGNAME like 'NCDC%'"
})
.then(function(json){
	// 成功した時の処理
});

// putでデータを更新
AppPot.Gateway.put(serviceName, tableName, null, {
	"BUILDINGID":"00002",
	"BUILDINGNAME":"NCDC00001",
	"CREATEDDATE":"2017/01/01 09:10",
	"UPDATEDATE":"2017/01/01 09:10"
})
.then(function(json){
	// 成功した時の処理
});

// removeで削除
AppPot.Gateway.remove(serviceName, tableName, {
	"BUILDINGID":"NCDC00001",
}).then(function(json){
	// 成功した時の処理
});
```
