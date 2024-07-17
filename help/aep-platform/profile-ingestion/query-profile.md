---
title: 統合プロファイルへのアクセス
description: API を使用して統合プロファイルにアクセスします。
exl-id: c9d2fa2d-9ffe-4e66-996f-ad930bee22c6
source-git-commit: fe7519c35fb9155ce54cad85941c887f15881a38
workflow-type: tm+mt
source-wordcount: '675'
ht-degree: 8%

---

# プロファイル API を使用した統合プロファイルへのアクセス

Adobe [!DNL Experience Platform] はリアルタイムで顧客プロファイルにアクセスできます。[[!DNL Experience Platform]  リアルタイム顧客プロファイル API](https://adobe.ly/2TtDHWr) は、これを操作するように設計されています。 Profile API を使用してリアルタイム顧客プロファイルデータにアクセスする方法については、この [ チュートリアル ](https://docs.adobe.com/content/help/en/experience-platform/profile/api/getting-started.html) を参照してください。

この記事では、上記にリンクされているチュートリアルを実質的に参照します。

[Postman コレクション ](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman) は、関連する呼び出しを番号で使用して、記事全体で参照されます。 Postman コレクションのインストールと使用について詳しくは、Github [README](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/README.md) ページを参照してください。 また、[ ロイヤルティ ](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20events.json) データと [ プロファイル ](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20profiles.json) データのサンプルデータセットもあります。

このセクションでは、Postman フォルダー 5：プロファイルルックアップ、5a：リアルタイムルックアップ PROFILE data または 5b：リアルタイムルックアップ EVENT data を使用します。

## API の使用

次の節では、Experience Platformへの認証について説明します。 API パス、ヘッダー情報などについて説明します。

### [!DNL Platform] に対する認証

以下のいずれかの呼び出しを行う前に、[this](https://docs.adobe.com/content/help/ja-JP/experience-platform/tutorials/authentication.html) 認証に関するチュートリアルを参照してください。

### API パス

リアルタイム顧客プロファイル API に必要なプラットフォームゲートウェイ URL は `https://platform.adobe.io/` です。

API のベースパスは `/data/core/ups/access/entities` です。

完全パスの例：`https://platform.adobe.io/data/core/ups/access/entities`

### ヘッダー情報

ヘッダーには以下を含める必要があります。

* 認証
* x-gw-ims-org-id - console.adobe.io から取得
* x-api-key - console.adobe.io を通じて取得
* x-sandbox-name - Adobe統合マネージャーから取得
* Content-Type：application/json

ヘッダーについて説明する詳細は、[ チュートリアル ](https://adobe.ly/2PTHuKv) を参照してください。

## ID を使用したリアルタイム顧客プロファイルへのアクセス

プロファイル API を使用すると、GETリクエスト経由で ID を使用してプロファイルにアクセスできます。 以下の節では、この [ ガイド ](https://docs.adobe.com/content/help/en/experience-platform/profile/api/entities.html) に従います。

### ID を使用したプロファイルデータへのアクセス

API は、ID を使用してプロファイル情報にアクセスできるようにします。 これは、パラメーターおよびエンティティ ID 名前空間の 1 つとして、エンティティ ID を使用して/access/entities にGETリクエストを行うことで行います。 メモ：50 件のレコードを返すリクエストは、422 件の HTTP ステータスと「関連 ID が多すぎます」というメッセージのみを配信するので、より多くのパラメーターを使用して検索を絞り込む必要があります。

リクエスト :

次のリクエストでは、顧客の電子メールと名前を ID を使用して取得します。

```
curl -X GET \
  'https://platform.adobe.io/data/core/ups/access/entities?schema.name=_xdm.context.profile&entityId=janedoe@example.com&entityIdNS=email&fields=identities,person.name,workEmail' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

応答：

```
{
    "BVrqzwVv7o2p3naHvnsWpqZXv3KJgA": {
        "entityId": "BVrqzwVv7o2p3naHvnsWpqZXv3KJgA",
        "sources": [
            "1000000000"
        ],
        "entity": {
            "identities": [
                {
                    "id": "89149270342662559642753730269986316601",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "janedoe@example.com",
                    "namespace": {
                        "code": "email"
                    }
                },
                {
                    "id": "janesmith@example.com",
                    "namespace": {
                        "code": "email"
                    }
                },
                {
                    "id": "89149270342662559642753730269986316604",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "58832431024964181144308914570411162539",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "89149270342662559642753730269986316602",
                    "namespace": {
                        "code": "ecid"
                    },
                    "primary": true
                }
            ],
            "person": {
                "name": {
                    "firstName": "Jane",
                    "middleName": "F",
                    "lastName": "Doe"
                }
            },
            "workEmail": {
                "primary": true,
                "address": "janedoe@example.com",
                "label": "Jane Doe",
                "type": "work",
                "status": "active"
            }
        },
        "lastModifiedAt": "2018-08-28T20:57:24Z"
    }
}
```

### ID のリストによるプロファイルへのアクセス

API は、/access/entities エンドポイントへのPOSTリクエストを使用し、ペイロードに ID を指定することで、ID のリストを使用してプロファイルへのアクセスを提供します。 これらの ID は、ID 値（entityId）と ID 名前空間（entityIdNS）で構成されます。

リクエスト :
次のリクエストは、ID のリストから複数の顧客の名前とメールアドレスを取得します。

```
curl -X POST \
  https://platform.adobe.io/data/core/ups/access/entities \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
    "schema":{
        "name":"_xdm.context.profile"
    },
    "fields":["identities","person.name","workEmail"],
    "identities":[
        {
            "entityId":"89149270342662559642753730269986316601",
            "entityIdNS":{
                "code":"ECID"
            }
        },
        {
            "entityId":"89149270342662559642753730269986316900",
            "entityIdNS":{
                "code":"ECID"
            }
        },
        {
            "entityId":"89149270342662559642753730269986316602",
            "entityIdNS":{
                "code":"ECID"
            }
        }
    ]
}'
```

応答：
応答が成功すると、リクエスト本文で指定されたエンティティのリクエストされたフィールドが返されます。

```
{
    "A29cgveD5y64ezlhxjUXNzcm": {
        "entityId": "A29cgveD5y64ezlhxjUXNzcm",
        "sources": [
            "1000000000"
        ],
        "entity": {
            "identities": [
                {
                    "id": "89149270342662559642753730269986316601",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "janedoe@example.com",
                    "namespace": {
                        "code": "email"
                    }
                },
                {
                    "id": "05DD23564EC4607F0A490D44",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "89149270342662559642753730269986316603",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "janesmith@example.com",
                    "namespace": {
                        "code": "email"
                    }
                },
                {
                    "id": "89149270342662559642753730269986316604",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "89149270342662559642753730269986316700",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "89149270342662559642753730269986316701",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "58832431024964181144308914570411162539",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "89149270342662559642753730269986316602",
                    "namespace": {
                        "code": "ecid"
                    },
                    "primary": true
                }
            ],
            "person": {
                "name": {
                    "firstName": "Jane",
                    "middleName": "F",
                    "lastName": "Doe"
                }
            },
            "workEmail": {
                "primary": true,
                "address": "janedoe@example.com",
                "label": "Jane Doe",
                "type": "work",
                "status": "active"
            }
        },
        "lastModifiedAt": "2018-08-28T20:57:24Z"
    },
    "A29cgveD5y64e2RixjUXNzcm": {
        "entityId": "A29cgveD5y64e2RixjUXNzcm",
        "sources": [
            ""
        ],
        "entity": {},
        "lastModifiedAt": "1970-01-01T00:00:00Z"
    },
    "A29cgveD5y64ezphxjUXNzcm": {
        "entityId": "A29cgveD5y64ezphxjUXNzcm",
        "sources": [
            "1000000000"
        ],
        "entity": {
            "identities": [
                {
                    "id": "89149270342662559642753730269986316602",
                    "namespace": {
                        "code": "ecid"
                    },
                    "primary": true
                },
                {
                    "id": "janedoe@example.com",
                    "namespace": {
                        "code": "email"
                    }
                }
            ],
            "person": {
                "name": {
                    "firstName": "Jane",
                    "middleName": "F",
                    "lastName": "Doe"
                }
            },
            "workEmail": {
                "primary": true,
                "address": "janedoe@example.com",
                "label": "Jane Doe",
                "type": "work",
                "status": "active"
            }
        },
        "lastModifiedAt": "2018-08-27T23:25:52Z"
    }
}
```

## 時系列イベント

パートナーは、/access/entities エンドポイントにGETリクエストを行うことで、関連するプロファイルエンティティの ID によって時系列イベントにアクセスできます。

### ID によるプロファイルの時系列イベントへのアクセス

時系列イベントには、/access/entities エンドポイントにGETリクエストを行い、関連するプロファイルエンティティの ID からアクセスします。 この ID は、ID 値（entityId）と ID 名前空間（entityIdNS）で構成されます。

リクエスト :
次のリクエストは、ID でプロファイルエンティティを検索し、そのエンティティに関連付けられた時系列イベントについて、endUserID、web および channel **すべて** のプロパティの値を取得します。

```
curl -X GET \
  'https://platform.adobe.io/data/core/ups/access/entities?schema.name=_xdm.context.experienceevent&relatedSchema.name=_xdm.context.profile&relatedEntityId=89149270342662559642753730269986316900&relatedEntityIdNS=ECID&fields=endUserIDs,web,channel&startTime=1531260476000&endTime=1531260480000&limit=1' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

応答：

応答が成功すると、リクエストパラメーターで指定された時系列イベントと関連フィールドのページ分割されたリストが返されます。

```
{
    "_page": {
        "orderby": "timestamp",
        "start": "c8d11988-6b56-4571-a123-b6ce74236036",
        "count": 1,
        "next": "c8d11988-6b56-4571-a123-b6ce74236037"
    },
    "children": [
        {
            "relatedEntityId": "A29cgveD5y64e2RixjUXNzcm",
            "entityId": "c8d11988-6b56-4571-a123-b6ce74236036",
            "timestamp": 1531260476000,
            "entity": {
                "endUserIDs": {
                    "_experience": {
                        "ecid": {
                            "id": "89149270342662559642753730269986316900",
                            "namespace": {
                                "code": "ecid"
                            }
                        }
                    }
                },
                "channel": {
                    "_type": "web"
                },
                "web": {
                    "webPageDetails": {
                        "name": "Fernie Snow",
                        "pageViews": {
                            "value": 1
                        }
                    }
                }
            },
            "lastModifiedAt": "2018-08-21T06:49:02Z"
        }
    ],
    "_links": {
        "next": {
            "href": "/entities?start=c8d11988-6b56-4571-a123-b6ce74236037&orderby=timestamp&schema.name=_xdm.context.experienceevent&relatedSchema.name=_xdm.context.profile&relatedEntityId=89149270342662559642753730269986316900&relatedEntityIdNS=ECID&fields=endUserIDs,web,channel&startTime=1531260476000&endTime=1531260480000&limit=1"
        }
    }
}
```

### プロファイルの時系列イベントのページネーション

時系列イベントを取得すると、結果はページ付けされます。結果の後続のページがある場合、応答の_page.next パラメーターには ID が含まれます。 さらに、応答の_links.next.href パラメーターは、後続のページを取得するためのリクエスト URI を提供します。

リクエスト :

次のリクエストは、_links.next.href URI をリクエストパスとして使用して、結果の次のページを取得します。

```
curl -X GET \

  'https://platform.adobe.io/data/core/ups/access/entities?start=c8d11988-6b56-4571-a123-b6ce74236037&orderby=timestamp&schema.name=_xdm.context.experienceevent&relatedSchema.name=_xdm.context.profile&relatedEntityId=89149270342662559642753730269986316900&relatedEntityIdNS=ECID&fields=endUserIDs,web,channel&startTime=1531260476000&endTime=1531260480000&limit=1' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

応答：

正常な応答は、結果の次のページを返します。この例では、_page.next および_links.next.href の空の文字列値で示されるように、結果の後続のページがない応答を示します。

```
{
    "_page": {
        "orderby": "timestamp",
        "start": "c8d11988-6b56-4571-a123-b6ce74236037",
        "count": 1,
        "next": ""
    },
    "children": [
        {
            "relatedEntityId": "A29cgveD5y64e2RixjUXNzcm",
            "entityId": "c8d11988-6b56-4571-a123-b6ce74236037",
            "timestamp": 1531260477000,
            "entity": {
                "endUserIDs": {
                    "_experience": {
                        "ecid": {
                            "id": "89149270342662559642753730269986316900",
                            "namespace": {
                                "code": "ecid"
                            }
                        }
                    }
                },
                "channel": {
                    "_type": "web"
                },
                "web": {
                    "webPageDetails": {
                        "name": "Fernie Snow",
                        "pageViews": {
                            "value": 1
                        }
                    }
                }
            },
            "lastModifiedAt": "2018-08-21T06:50:01Z"
        }
    ],
    "_links": {
        "next": {
            "href": ""
        }
    }
}
```

## 参考記事

* [リアルタイム顧客プロファイル API](https://adobe.ly/2TtDHWr)
* [ プロファイル API チュートリアルを使用してリアルタイム顧客プロファイルデータにアクセスする ](https://docs.adobe.com/content/help/en/experience-platform/profile/api/getting-started.html)
* [[!DNL Experience Platform]  認証ガイド ](https://docs.adobe.com/content/help/ja-JP/experience-platform/tutorials/authentication.html)
