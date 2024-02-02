---
title: リアルタイムインポート
description: リアルタイムで AEP にデータを読み込む方法を説明します。
exl-id: 0b6215a9-1160-49ae-8aa5-302b47357200
source-git-commit: fe7519c35fb9155ce54cad85941c887f15881a38
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 2%

---

# AEP へのデータのストリーミング

Adobe [!DNL Experience Platform] では、プロファイルイベントとエクスペリエンスイベントの両方をストリーミングし、ほぼリアルタイムで利用できます。 ストリーミング経由で AEP に送信されたすべてのデータは、データレイクで保持されます。 データは、既存のデータセットにストリーミングしたり、API を使用して、または Launch を使用して、まったく新しいデータセットにAdobeしたりできます。

この記事では、次の内容について説明します。

* XDM 個別プロファイルへのストリーミング
* XDM ExperienceEvent へのストリーミング
* AEP を使用した Launch 拡張機能のストリーミング

The [Postman Collection](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman) は、記事全体で、関連する呼び出し（番号別）を使用して参照されます。 Postmanコレクションのインストールと使用に関する詳細については、GitHub を参照してください。 [README](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/README.md) ページに貼り付けます。 また、 [loyalty](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20events.json) および [profile](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20profiles.json) データ。

## 前提条件

* [プラットフォームへの認証](https://docs.adobe.com/content/help/ja-JP/experience-platform/tutorials/authentication.html).
* 上記のリンクにある認証に関するチュートリアルから、必要なヘッダーの値を収集します。

## ストリーミング接続の作成

AEP にストリーミングするには、まずストリーミング接続を作成する必要があります。 ストリーミング接続には、ストリーミングデータのソースなどの属性と、 [!DNL Experience Data Model] (XDM) スキーマ。 ストリーミング接続を作成すると、データを AEP にストリーミングするために使用する一意の URL が提供されます。

移動 [ここ](https://docs.adobe.com/content/help/en/experience-platform/ingestion/tutorials/create-streaming-connection.html) API を使用したストリーミング接続の作成方法または [ここ](https://docs.adobe.com/content/help/en/experience-platform/ingestion/tutorials/create-streaming-connection-ui.html) を参照してください。

```json
curl -X POST https://platform.adobe.io/data/foundation/flowservice/connections \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '{
     "name": "Sample name",
     "providerId": "521eee4d-8cbe-4906-bb48-fb6bd4450033",
     "description": "Sample description",
     "connectionSpec": {
         "id": "bc7b00d6-623a-4dfc-9fdb-f1240aeadaeb",
         "version": "1.0"
     },
     "auth": {
         "specName": "Streaming Connection",
         "params": {
             "sourceId": "Sample connection",
             "dataType": "xdm",
             "name": "Sample connection"
         }
     }
 }
```

応答：

```json
 {
    "id": "77a05521-91d6-451c-a055-2191d6851c34",
    "etag": "\"a500e689-0000-0200-0000-5e31df730000\""
}
```

今後のストリーミング取り込み呼び出しに備えて、上記の応答で指定した ID を保存してください (Postmanコレクションはこの ID を CONNECTION_ID 環境変数に保存します )。

## プロファイルデータを AEP にストリーミング

この節では、Postman呼び出しフォルダー（3：リアルタイムインポート、3a：プロファイルデータのリアルタイムインポート）を使用します。

ストリーミングプロファイルデータの応答を含む詳細な JSON リクエストがドキュメントに記載されています [ここ](https://docs.adobe.com/content/help/en/experience-platform/ingestion/tutorials/streaming-record-data.html).

手順：

1. XDM 個別プロファイルスキーマの作成
1. XDM 個人プライマリ（プライマリキー）のプロファイル ID 記述子の設定
1. XDM 個別プロファイルレコードのデータセットの作成
1. ストリーミング取得 API を呼び出して、XDM 個別プロファイルレコードを作成する
1. 新しく作成されたプロファイルを取得

## エクスペリエンスイベントの AEP へのストリーミング

この節では、Postman呼び出しフォルダー（3：リアルタイムインポート、3b：プロファイルデータのリアルタイムインポート）を使用します。

ストリーミングエクスペリエンスデータの応答を含む詳細な JSON リクエストがドキュメントに記載されています [ここ](https://docs.adobe.com/content/help/en/experience-platform/ingestion/tutorials/streaming-time-series-data.html).

手順：

1. XDM ExperienceEvent スキーマの作成
1. XDM ExperienceEvent のプライマリID 記述子の設定（プライマリキー）
1. XDM ExperienceEvents のデータセットの作成
1. ストリーミング取得 API を呼び出して XDM ExperienceEvent を作成する
1. 新しく作成されたイベントを取得します

## Experience Platformタグを使用した AEP へのストリーミング

Adobe [!DNL Experience Platform] Launch 拡張機能を使用すると、Launch を介して AEP にストリーミングを送信できます。 詳しくは、 [このガイド](https://docs.adobe.com/content/help/ja-JP/launch/using/extensions-ref/adobe-extension/aep-extension/overview.html).

## 参考記事

* [データ取得 API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#/acpdr/swagger-specs)
* [ストリーミング取り込みの概要](https://docs.adobe.com/content/help/ja-JP/experience-platform/ingestion/home.html#!api-specification/markdown/narrative/technical_overview/streaming_ingest/streaming_ingest_overview.md)
* [ストリーミング取り込み開発者ガイド](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/streaming_ingest/getting_started_with_platform_streaming_ingestion.md)
* [AEP Launch 拡張機能の使用](https://docs.adobe.com/content/help/ja-JP/launch/using/extensions-ref/adobe-extension/aep-extension/overview.html)
