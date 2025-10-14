---
title: AEP へのバッチデータの読み込み
description: バッチファイルをExperience Platformに読み込む方法を説明します
exl-id: 50576b67-b3ba-498e-86f6-7e1986b76985
source-git-commit: fe7519c35fb9155ce54cad85941c887f15881a38
workflow-type: tm+mt
source-wordcount: '461'
ht-degree: 9%

---

# AEP へのバッチデータの読み込み

AEP は、フラットファイル（parquet など）からのプロファイルデータや、[!UICONTROL Experience Data Model] （XDM）レジストリの既知のスキーマに準拠するデータを含むバッチファイルを取り込むことができます。

AEP では、バッチファイルを使用してデータを取り込むことができます。 JSON、Parquet および CSV の形式を使用できます。

この記事では、次の内容について説明します。

* バッチ取り込みの前提条件
* バッチ取り込みのベストプラクティスと制限
* バッチの作成方法
* バッチの完了方法
* バッチステータスを確認する方法

[Postman コレクション &#x200B;](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman) は、関連する呼び出しを番号で使用して、記事全体で参照されます。 Postman コレクションのインストールと使用について詳しくは、Github [README](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/README.md) ページを参照してください。 また、[&#x200B; ロイヤルティ &#x200B;](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20events.json) データと [&#x200B; プロファイル &#x200B;](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20profiles.json) データのサンプルデータセットもあります。

このチュートリアルのすべての呼び出しには、Postman呼び出しフォルダー（4 : バッチインポート、4a : プロファイルデータのバッチインポートまたは 4b : イベントデータのバッチインポート）を使用します。

## バッチ取り込みの前提条件

* スキーマを定義し、データセットを作成します。
* データは、JSON、Parquet または CSV でフォーマットする必要があります。
* [&#x200B; プラットフォームへの認証 &#x200B;](https://docs.adobe.com/content/help/ja-JP/experience-platform/tutorials/home.html#!api-specification/markdown/narrative/tutorials/authenticate_to_acp_tutorial/authenticate_to_acp_tutorial.md)。
* 上記にリンクされている認証チュートリアルから、必要なヘッダーの値を収集します。

## バッチ取り込みのベストプラクティスと制限

* 最大バッチサイズ：100GB
* バッチあたりの最大ファイル数：1500
* ファイルが 512 MB を超える場合は、より小さなチャンクに分割する必要があります。 詳しくは、[&#x200B; 開発者ガイド &#x200B;](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/ingest_architectural_overview/batch_data_ingestion_developer_guide.md) を参照してください。
* 1 行あたりのプロパティまたはフィールドの最大数：10,000
* 1 ユーザーあたりの 1 分あたりの最大バッチ数：138

## バッチの作成

このチュートリアルでは、形式として JSON を使用します。 その他の形式の例については、[&#x200B; 開発者ガイド &#x200B;](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/ingest_architectural_overview/batch_data_ingestion_developer_guide.md) を参照してください。
JSON を入力形式として使用してバッチを作成します（データセット ID を含め、データがデータセットにリンクされた XDM スキーマに準拠していることを確認してください）。

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

新しく作成したバッチにファイルをアップロードできるようになりました（上記の応答の batch_id を使用）。

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
>API はシングルパートのアップロードのみをサポートしています。つまり、各ファイル/マイクロバッチは、個々の呼び出しでアップロードする必要があります。 content-type が application/octet-stream であることを確認します。

応答：

```
200 OK
```

## バッチの完了

すべてのファイルがアップロードされると、この呼び出しによってバッチの昇格準備が整ったことが通知されます。

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

バッチステータスは、UI または API 経由で確認できます（以下の呼び出しを参照）。 UI をチェックインするには、データセットに移動してステータスを確認します。

様々なバッチ取り込みステータスが [&#x200B; こちら &#x200B;](https://adobe.ly/2TMMCmj) で確認できます。


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
* [&#x200B; バッチ取得の概要 &#x200B;](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/ingest_architectural_overview/ingest_architectural_overview.md)
* [&#x200B; バッチ取得開発者ガイド &#x200B;](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/ingest_architectural_overview/batch_data_ingestion_developer_guide.md)
* [&#x200B; バッチ取得トラブルシューティングガイド &#x200B;](https://www.adobe.io/apis/experienceplatform/home/data-ingestion/data-ingestion-services.html#!api-specification/markdown/narrative/technical_overview/ingest_architectural_overview/batch_data_ingestion_troubleshooting_guide.md)
* [&#x200B; データ取り込みPostman コレクション &#x200B;](https://github.com/adobe/experience-platform-postman-samples/blob/master/apis/experience-platform/Data%20Ingestion%20API.postman_collection.json)
* [&#x200B; 認証のチュートリアル &#x200B;](https://docs.adobe.com/content/help/ja-JP/experience-platform/tutorials/home.html#!api-specification/markdown/narrative/tutorials/authenticate_to_acp_tutorial/authenticate_to_acp_tutorial.md)
