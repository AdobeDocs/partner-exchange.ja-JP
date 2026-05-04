---
title: リアルタイムインポート
description: AEPにリアルタイムでデータを読み込む方法について説明します。
exl-id: 0b6215a9-1160-49ae-8aa5-302b47357200
TQID: https://experienceleague.adobe.com/GvWcwNPjQdmdKSUkwvJ2EpoCKJHGvf5c1Kn4dwWRVi8
product_v2:
  - id: d0a3eab4-7b10-4d96-a71e-6c0f8e7b7c87
source-git-commit: 6698ae880d1ad13a9387cb1ba66b9ba152d1d407
workflow-type: tm+mt
source-wordcount: 642
ht-degree: 7%

---

# データをAEPにストリーミングする

Adobe [!DNL Experience Platform]では、プロファイル イベントとエクスペリエンス イベントの両方をほぼリアルタイムでストリーミングして利用できます。 ストリーミングを介してAEPに送信されたすべてのデータは、データレイクに保持されます。 データは、APIまたはAdobe Launchを使用して、既存のデータセットや完全に新しいデータセットにストリーミングできます。

主な内容：

* XDM個人プロファイルへのストリーミング
* XDM ExperienceEventへのストリーミング
* AEPを使用したLaunch拡張機能によるストリーミング

[Postman コレクション &#x200B;](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman)は、関連付けられた呼び出しを番号で使用して、記事全体で参照されます。 Postman コレクションのインストールと使用について詳しくは、Github [README](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/README.md) ページを参照してください。 [&#x200B; ロイヤルティ &#x200B;](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20events.json)および[&#x200B; プロファイル &#x200B;](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20profiles.json) データのサンプルデータセットもあります。

## 前提条件

* [&#x200B; プラットフォームに認証](https://docs.adobe.com/content/help/ja-JP/experience-platform/tutorials/authentication.html)。
* 上記のリンクされた認証チュートリアルから、必要なヘッダーの値を収集します。

## ストリーミング接続の作成

AEPにストリーミングするには、まずストリーミング接続を作成する必要があります。 ストリーミング接続には、ストリーミングデータのソースや、[!DNL Experience Data Model] （XDM） スキーマに属するレコードを送信しているかどうかの属性が含まれます。 ストリーミング接続を作成すると、データをAEPにストリーミングするために使用する一意のURLが与えられます。

APIを介してストリーミング接続を作成する方法については[ここ](https://docs.adobe.com/content/help/ja-JP/experience-platform/ingestion/tutorials/create-streaming-connection.html)に、UIを介してストリーミング接続を作成する方法については[ここ](https://docs.adobe.com/content/help/ja-JP/experience-platform/ingestion/tutorials/create-streaming-connection-ui.html)にアクセスしてください。

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

今後のストリーミング取り込み呼び出しに対しては、上記の応答で指定されたIDを必ず保存してください（Postman コレクションは、CONNECTION_ID環境変数に保存されます）。

## プロファイルデータをAEPにストリーミング

この節では、Postman呼び出しフォルダーを使用します。3：リアルタイム読み込み、3a：プロファイルデータのリアルタイム読み込み。

ストリーミングプロファイルデータに対する応答を含む詳細なJSON リクエストについては、[こちら](https://docs.adobe.com/content/help/ja-JP/experience-platform/ingestion/tutorials/streaming-record-data.html)を参照してください。

手順：

1. XDM個人プロファイルスキーマの作成
1. XDM個人プロファイル（プライマリキー）のプライマリ ID記述子を設定する
1. XDM個人プロファイルレコードのデータセットの作成
1. ストリーミング取得APIを呼び出して、XDM個人プロファイルレコードを作成します
1. 新しく作成したプロファイルの取得

## エクスペリエンスイベントをAEPにストリーミングする

この節では、Postman呼び出しフォルダーを使用します。3：リアルタイム読み込み、3b：プロファイルデータのリアルタイム読み込み。

ストリーミングエクスペリエンスデータに対する応答を含む詳細なJSON リクエストについては、[こちら](https://docs.adobe.com/content/help/ja-JP/experience-platform/ingestion/tutorials/streaming-time-series-data.html)を参照してください。

手順：

1. XDM ExperienceEvent スキーマの作成
1. XDM ExperienceEvent （プライマリキー）のプライマリ ID記述子を設定する
1. XDM ExperienceEventsのデータセットの作成
1. ストリーミング取得APIを呼び出して、XDM ExperienceEventを作成します
1. 新しく作成したイベントの取得

## Experience Platform タグを使用したAEPへのストリーミング

Adobe [!DNL Experience Platform] Launch拡張機能は、Launch経由でAEPにストリーミングする方法を提供します。 詳しくは、[このガイド &#x200B;](https://docs.adobe.com/content/help/ja-JP/launch/using/extensions-ref/adobe-extension/aep-extension/overview.html)を参照してください。

## 関連記事

* [Data Ingestion API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#/acpdr/swagger-specs)
* [ストリーミング取り込みの概要](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/streaming_ingest/streaming_ingest_overview.md)
* [ストリーミング取り込み開発者ガイド](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/streaming_ingest/getting_started_with_platform_streaming_ingestion.md)
* [AEP Launch拡張機能の使用](https://docs.adobe.com/content/help/ja-JP/launch/using/extensions-ref/adobe-extension/aep-extension/overview.html)
