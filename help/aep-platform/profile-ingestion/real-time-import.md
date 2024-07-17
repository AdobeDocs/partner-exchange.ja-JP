---
title: リアルタイム読み込み
description: データを AEP にリアルタイムで読み込む方法を説明します。
exl-id: 0b6215a9-1160-49ae-8aa5-302b47357200
source-git-commit: fe7519c35fb9155ce54cad85941c887f15881a38
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 1%

---

# AEP へのデータのストリーミング

Adobe [!DNL Experience Platform] を使用すると、プロファイルイベントとエクスペリエンスイベントの両方をほぼリアルタイムでストリーミングして利用できます。 ストリーミングを介して AEP に送信されたデータはすべて、データレイクに保持されます。 データは、API またはAdobeの Launch を使用して、既存のデータセットや、まったく新しいデータセットにストリーミングできます。

この記事では、次の内容について説明します。

* XDM 個人プロファイルへのストリーミング
* XDM ExperienceEvent へのストリーミング
* AEP の Launch 拡張機能を使用したストリーミング

[Postman コレクション ](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman) は、関連する呼び出しを番号で使用して、記事全体で参照されます。 Postman コレクションのインストールと使用について詳しくは、Github [README](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/README.md) ページを参照してください。 また、[ ロイヤルティ ](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20events.json) データと [ プロファイル ](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20profiles.json) データのサンプルデータセットもあります。

## 前提条件

* [ プラットフォームへの認証 ](https://docs.adobe.com/content/help/ja-JP/experience-platform/tutorials/authentication.html)。
* 上記にリンクされている認証チュートリアルから、必要なヘッダーの値を収集します。

## ストリーミング接続の作成

AEP にストリーミングするには、まずストリーミング接続を作成する必要があります。 ストリーミング接続には、ストリーミングデータのソースや、[!DNL Experience Data Model] （XDM）スキーマに属するレコードで送信しているかどうかに関する属性が含まれています。 ストリーミング接続の作成後、AEP にデータをストリーミングするために使用する一意の URL が提供されます。

API を使用してストリーミング接続を作成する方法については [ こちら ](https://docs.adobe.com/content/help/en/experience-platform/ingestion/tutorials/create-streaming-connection.html) を、UI を使用してストリーミング接続を作成する方法については [ こちら ](https://docs.adobe.com/content/help/en/experience-platform/ingestion/tutorials/create-streaming-connection-ui.html) を参照してください。

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

今後のストリーミング取得呼び出しに対しては、上記の応答で提供された ID を必ず保存してください（Postman コレクションによって、CONNECTION_ID 環境変数に保存されます）。

## プロファイルデータの AEP へのストリーミング

このセクションでは、Postman呼び出しフォルダー（3：リアルタイムインポート、3a：プロファイルデータのリアルタイムインポート）を使用します。

ストリーミングプロファイルデータの応答を含む詳細な JSON リクエストについては、[ こちら ](https://docs.adobe.com/content/help/en/experience-platform/ingestion/tutorials/streaming-record-data.html) を参照してください。

手順：

1. XDM 個人プロファイルスキーマの作成
1. XDM 個人プロファイル（プライマリキー）のプライマリ ID 記述子の設定
1. XDM 個人プロファイルレコード用のデータセットの作成
1. ストリーミング取得 API を呼び出して、XDM 個人プロファイルレコードを作成します
1. 新しく作成されたプロファイルの取得

## エクスペリエンスイベントを AEP にストリーミング

この節では、Postman呼び出しフォルダー（3：リアルタイムインポート、3b：プロファイルデータのリアルタイムインポート）を使用します。

ストリーミングエクスペリエンスデータの応答を含む詳細な JSON リクエストについては、[ こちら ](https://docs.adobe.com/content/help/en/experience-platform/ingestion/tutorials/streaming-time-series-data.html) を参照してください。

手順：

1. XDM ExperienceEvent スキーマの作成
1. XDM ExperienceEvent （プライマリキー）のプライマリ ID 記述子を設定します
1. XDM ExperienceEvents のデータセットの作成
1. ストリーミング取得 API を呼び出して、XDM ExperienceEvent を作成します
1. 新しく作成されたイベントの取得

## Experience Platformタグを使用した AEP へのストリーミング

Adobe [!DNL Experience Platform] Launch 拡張機能は、Launch を介して AEP にストリーミングする方法を提供します。 詳しくは、[ このガイド ](https://docs.adobe.com/content/help/ja-JP/launch/using/extensions-ref/adobe-extension/aep-extension/overview.html) を参照してください。

## 参考記事

* [ データ取得 API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#/acpdr/swagger-specs)
* [ ストリーミング取得の概要 ](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/streaming_ingest/streaming_ingest_overview.md)
* [ ストリーミング取得開発者ガイド ](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/streaming_ingest/getting_started_with_platform_streaming_ingestion.md)
* [AEP Launch 拡張機能の使用 ](https://docs.adobe.com/content/help/ja-JP/launch/using/extensions-ref/adobe-extension/aep-extension/overview.html)
