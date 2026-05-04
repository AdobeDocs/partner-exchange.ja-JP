---
title: AEPへのバッチデータのインポート
description: バッチファイルをExperience Platformに読み込む方法について説明します
exl-id: 50576b67-b3ba-498e-86f6-7e1986b76985
TQID: https://experienceleague.adobe.com/sJjuydUOIwlu4gv6qmokidQJVrYLN4--M8m3DTkcjf0
product_v2:
  - id: d0a3eab4-7b10-4d96-a71e-6c0f8e7b7c87
topic_v2:
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: 6698ae880d1ad13a9387cb1ba66b9ba152d1d407
workflow-type: tm+mt
source-wordcount: 646
ht-degree: 11%

---

# AEPへのバッチデータのインポート

AEPは、フラットファイル （parquetなど）または[!UICONTROL Experience Data Model] （XDM） レジストリの既知のスキーマに準拠するデータから、プロファイルデータを含むバッチファイルを取り込むことができます。

AEPは、バッチファイルを使用してデータを取り込むことができます。 JSON、Parquet、CSVの形式を使用できます。

主な内容：

* バッチ取り込みの前提条件
* バッチ取り込みのベストプラクティスと制限
* バッチの作成方法
* バッチを完了する方法
* バッチのステータスの確認方法

[Postman コレクション &#x200B;](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman)は、関連付けられた呼び出しを番号で使用して、記事全体で参照されます。 Postman コレクションのインストールと使用について詳しくは、Github [README](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/README.md) ページを参照してください。 [&#x200B; ロイヤルティ &#x200B;](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20events.json)および[&#x200B; プロファイル &#x200B;](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20profiles.json) データのサンプルデータセットもあります。

このチュートリアルのすべての呼び出しには、Postman呼び出しフォルダーを使用します。4：バッチインポート、4a：プロファイルデータのバッチインポート、または4b：イベントデータのバッチインポート。

## バッチ取り込みの前提条件

* スキーマを定義し、データセットを作成します。
* データは、JSON、Parquet、またはCSV形式にする必要があります。
* [&#x200B; プラットフォームに認証](https://docs.adobe.com/content/help/ja-JP/experience-platform/tutorials/home.html#!api-specification/markdown/narrative/tutorials/authenticate_to_acp_tutorial/authenticate_to_acp_tutorial.md)。
* 上記のリンクされた認証チュートリアルから、必要なヘッダーの値を収集します。

## バッチ取り込みのベストプラクティスと制限

* 最大バッチサイズ：100GB
* バッチあたりの最大ファイル数：1500
* ファイルが512 MBを超える場合は、小さなチャンクに分割する必要があります。 詳細については、[開発者ガイド &#x200B;](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/ingest_architectural_overview/batch_data_ingestion_developer_guide.md)を参照してください
* 1 行あたりのプロパティまたはフィールドの最大数：10,000
* 1 ユーザーあたりの 1 分あたりの最大バッチ数：138

## バッチの作成

このチュートリアルでは、JSONをフォーマットとして使用します。 その他の形式の例については、[開発者ガイドを参照してください](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/ingest_architectural_overview/batch_data_ingestion_developer_guide.md)
JSONを入力形式として使用してバッチを作成します（データセット IDを含め、データがデータセットにリンクされたXDM スキーマに準拠していることを確認してください）。

```json
curl -X POST "https://platform.adobe.io/data/foundation/import/batches" \
  -H "Accept: application/json" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key : {API_KEY}"
  -d '{
          "datasetId": "{DATASET_ID}",
           "inputFormat": {
                "format": "json"
           }
      }'
```

応答：

```json
{
    "id": "{BATCH_ID}",
    "imsOrg": "{IMS_ORG}",
    "updated": 0,
    "status": "loading",
    "created": 0,
    "relatedObjects": [
        {
            "type": "dataSet",
            "id": "{DATASET_ID}"
        }
    ],
    "version": "1.0.0",
    "tags": {},
    "createdUser": "{USER_ID}",
    "updatedUser": "{USER_ID}"
}
```

## ファイルのアップロード

新しく作成したバッチにファイルをアップロードできるようになりました（上記の応答のbatch_idを使用）。

```json
curl -X PUT "https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}/datasets/{DATASET_ID}/files/{FILE_NAME}.json" \
  -H "content-type: application/octet-stream" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key : {API_KEY}" \
  --data-binary "@{FILE_PATH_AND_NAME}.json"
```

>[!NOTE]
>
>APIは単一パートのアップロードのみをサポートしているため、各ファイル/マイクロバッチを個別の呼び出しでアップロードする必要があります。 content-type が application/octet-stream であることを確認します。

応答：

```
200 OK
```

## バッチを完了

すべてのファイルがアップロードされると、この呼び出しは、バッチを昇格する準備ができていることを示します。

```json
curl -X POST "https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}?action=COMPLETE" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key : {API_KEY}"
```

応答：

```
200 OK
```

## バッチのステータスの確認

バッチステータスは、UIまたはAPI経由で確認できます（以下の呼び出しを参照）。 UIを確認するには、DataSetに移動してステータスを確認します。

様々なバッチ取り込みステータスは[ここ](https://adobe.ly/2TMMCmj)にあります。


```json
curl GET "https://platform.adobe.io/data/foundation/catalog/batch/{BATCH_ID}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "x-api-key: {API_KEY}"
```

応答：

```json
{
    "{BATCH_ID}": {
        "imsOrg": "{IMS_ORG}",
        "created": 1494349962314,
        "createdClient": "MCDPCatalogService",
        "createdUser": "{USER_ID}",
        "updatedUser": "{USER_ID}",
        "updated": 1494349963467,
        "externalId": "{EXTERNAL_ID}",
        "status": "success",
        "errors": [
            {
                "code": "err-1494349963436"
            }
        ],
        "version": "1.0.3",
        "availableDates": {
            "startDate": 1337,
            "endDate": 4000
        },
        "relatedObjects": [
            {
                "type": "batch",
                "id": "foo_batch"
            },
            {
                "type": "connection",
                "id": "foo_connection"
            },
            {
                "type": "connector",
                "id": "foo_connector"
            },
            {
                "type": "dataSet",
                "id": "foo_dataSet"
            },
            {
                "type": "dataSetView",
                "id": "foo_dataSetView"
            },
            {
                "type": "dataSetFile",
                "id": "foo_dataSetFile"
            },
            {
                "type": "expressionBlock",
                "id": "foo_expressionBlock"
            },
            {
                "type": "service",
                "id": "foo_service"
            },
            {
                "type": "serviceDefinition",
                "id": "foo_serviceDefinition"
            }
        ],
        "metrics": {
            "foo": 1337
        },
        "tags": {
            "foo_bar": [
                "stuff"
            ],
            "bar_foo": [
                "woo",
                "baz"
            ],
            "foo/bar/foo-bar": [
                "weehaw",
                "wee:haw"
            ]
        },
        "inputFormat": {
            "format": "parquet",
            "delimiter": ".",
            "quote": "`",
            "escape": "\\",
            "nullMarker": "",
            "header": "true",
            "charset": "UTF-8"
        }
    }
}
```

## 関連記事

* [Data Ingestion API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#/acpdr/swagger-specs)
* [バッチ取り込みの概要](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/ingest_architectural_overview/ingest_architectural_overview.md)
* [バッチ取り込み開発者ガイド](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/ingest_architectural_overview/batch_data_ingestion_developer_guide.md)
* [バッチ取り込みトラブルシューティングガイド](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/ingest_architectural_overview/batch_data_ingestion_troubleshooting_guide.md)
* [Data Ingestion Postman Collection](https://github.com/adobe/experience-platform-postman-samples/blob/master/apis/experience-platform/Data%20Ingestion%20API.postman_collection.json)
* [認証チュートリアル](https://docs.adobe.com/content/help/ja-JP/experience-platform/tutorials/home.html#!api-specification/markdown/narrative/tutorials/authenticate_to_acp_tutorial/authenticate_to_acp_tutorial.md)
