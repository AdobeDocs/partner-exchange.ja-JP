---
title: AEP へのバッチデータの読み込み
description: バッチファイルをExperience Platformに読み込む方法
exl-id: 50576b67-b3ba-498e-86f6-7e1986b76985
source-git-commit: fe7519c35fb9155ce54cad85941c887f15881a38
workflow-type: tm+mt
source-wordcount: '461'
ht-degree: 10%

---

# AEP へのバッチデータの読み込み

AEP は、プロファイルデータを含むバッチファイルを、フラットファイル（parquet など）または [!UICONTROL エクスペリエンスデータモデル] (XDM) レジストリ。

AEP はバッチファイルを使用してデータを取り込むことができます。 JSON、Parquet および CSV 形式を使用できます。

この記事では、次の内容について説明します。

* バッチ取り込みの前提条件
* バッチ取得のベストプラクティスと制限
* バッチの作成方法
* バッチの完了方法
* バッチのステータスの確認方法

The [Postman Collection](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman) は、記事全体で、関連する呼び出し（番号別）を使用して参照されます。 Postmanコレクションのインストールと使用に関する詳細については、GitHub を参照してください。 [README](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/README.md) ページに貼り付けます。 また、 [loyalty](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20events.json) および [profile](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20profiles.json) データ。

このチュートリアルのすべての呼び出しに対して、Postman呼び出しフォルダー（4：バッチインポート、4a：プロファイルデータのバッチインポート、4b：イベントデータのバッチインポート）を使用します。

## バッチ取り込みの前提条件

* スキーマを定義し、データセットを作成します。
* データは JSON、Parquet、CSV 形式で書式設定する必要があります。
* [プラットフォームへの認証](https://docs.adobe.com/content/help/ja-JP/experience-platform/tutorials/home.html#!api-specification/markdown/narrative/tutorials/authenticate_to_acp_tutorial/authenticate_to_acp_tutorial.md).
* 上記のリンクにある認証に関するチュートリアルから、必要なヘッダーの値を収集します。

## バッチ取得のベストプラクティスと制限

* 最大バッチサイズ：100GB
* バッチあたりの最大ファイル数：1500
* ファイルのサイズが 512 MB を超える場合は、小さなチャンクに分割する必要があります。 詳しくは、 [開発者ガイド](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/ingest_architectural_overview/batch_data_ingestion_developer_guide.md)
* 1 行あたりのプロパティまたはフィールドの最大数：10,000
* 1 ユーザーあたりの 1 分あたりの最大バッチ数：138

## バッチの作成

このチュートリアルでは、JSON を形式として使用します。 その他の形式の例については、 [開発者ガイド](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/ingest_architectural_overview/batch_data_ingestion_developer_guide.md)
JSON を入力形式として使用してバッチを作成します（データセット ID を必ず含め、データがデータセットにリンクされた XDM スキーマに準拠していることを確認してください）。

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

これで、ファイルを新しく作成したバッチにアップロードできます（上記の応答の batch_id を使用）。

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
>API は、シングルパートのアップロードのみをサポートします。つまり、各ファイル/マイクロバッチは、個々の呼び出しでアップロードする必要があります。 content-type が application/octet-stream であることを確認します。

応答：

```
200 OK
```

## バッチの完了

すべてのファイルがアップロードされると、この呼び出しは、バッチがプロモーションの準備ができたことを示します。

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

バッチのステータスは、UI または API で確認できます（以下の呼び出しを参照）。 UI をチェックインするには、データセットに移動してステータスを確認します。

様々なバッチ取り込みステータスが見つかります [ここ](https://adobe.ly/2TMMCmj).


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

## 参考記事

* [データ取得 API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#/acpdr/swagger-specs)
* [バッチ取り込みの概要](https://docs.adobe.com/content/help/ja-JP/experience-platform/ingestion/home.html#!api-specification/markdown/narrative/technical_overview/ingest_architectural_overview/ingest_architectural_overview.md)
* [バッチ取得開発者ガイド](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/ingest_architectural_overview/batch_data_ingestion_developer_guide.md)
* [バッチ取得トラブルシューティングガイド](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/ingest_architectural_overview/batch_data_ingestion_troubleshooting_guide.md)
* [データ取り込みPostmanコレクション](https://github.com/adobe/experience-platform-postman-samples/blob/master/apis/experience-platform/Data%20Ingestion%20API.postman_collection.json)
* [認証に関するチュートリアル](https://docs.adobe.com/content/help/ja-JP/experience-platform/tutorials/home.html#!api-specification/markdown/narrative/tutorials/authenticate_to_acp_tutorial/authenticate_to_acp_tutorial.md)
