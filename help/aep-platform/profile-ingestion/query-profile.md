---
title: 統合プロファイルへのアクセス
description: APIを使用して統合プロファイルにアクセスします。
exl-id: c9d2fa2d-9ffe-4e66-996f-ad930bee22c6
TQID: https://experienceleague.adobe.com/ECndsmKpnN3No-PYL0kq0lktWuDK4Z6lFb99i82dK7k
product_v2:
  - id: d0a3eab4-7b10-4d96-a71e-6c0f8e7b7c87
topic_v2:
  - id: fd2e3797-f2ea-4b36-a9af-52acf5e90513
source-git-commit: 6698ae880d1ad13a9387cb1ba66b9ba152d1d407
workflow-type: tm+mt
source-wordcount: 797
ht-degree: 8%

---

# Profile APIを使用した統合プロファイルへのアクセス

Adobe [!DNL Experience Platform]は、リアルタイムで顧客プロファイルにアクセスできます。[[!DNL Experience Platform]  リアルタイム顧客プロファイル API](https://adobe.ly/2TtDHWr)は、そのやり取りを目的として設計されています。 Profile APIを使用してリアルタイムの顧客プロファイルデータにアクセスする方法については、[&#x200B; チュートリアル &#x200B;](https://docs.adobe.com/content/help/ja-JP/experience-platform/profile/api/getting-started.html)を参照してください。

この記事では、上記のチュートリアルを大幅に参照します。

[Postman コレクション &#x200B;](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman)は、関連付けられた呼び出しを番号で使用して、記事全体で参照されます。 Postman コレクションのインストールと使用について詳しくは、Github [README](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/README.md) ページを参照してください。 [&#x200B; ロイヤルティ &#x200B;](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20events.json)および[&#x200B; プロファイル &#x200B;](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20profiles.json) データのサンプルデータセットもあります。

この節では、Postman フォルダー5: Profile Lookup, 5a: Real-time lookup PROFILE dataまたは5b: Real-time lookup EVENT dataを使用します。

## API の使用

次の節は、Experience Platformへの認証に役立ちます。 API パスやヘッダー情報などについてご確認ください。

### [!DNL Platform]に認証

以下の呼び出しを行う前に、[this](https://docs.adobe.com/content/help/ja-JP/experience-platform/tutorials/authentication.html)認証チュートリアルを参照してください。

### API パス

リアルタイム顧客プロファイル APIに必要なプラットフォームゲートウェイ URLは次のとおりです：`https://platform.adobe.io/`

APIの基本パスは`/data/core/ups/access/entities`です

完全なパスの例は次のとおりです：`https://platform.adobe.io/data/core/ups/access/entities`

### ヘッダー情報

ヘッダーには以下を含める必要があります。

* 認証
* x-gw-ims-org-id - console.adobe.ioから取得
* x-api-key - console.adobe.ioから取得
* x-sandbox-name - Adobe Integration Managerから取得
* Content-Type：application/json

ヘッダーについて詳しくは、[&#x200B; チュートリアル &#x200B;](https://adobe.ly/2PTHuKv)を参照してください。

## IDを使用してリアルタイムの顧客プロファイルにアクセス

Profile APIは、GET リクエストを介してIDを使用してプロファイルにアクセスできるようにします。 以下のセクションは、この[&#x200B; ガイド &#x200B;](https://docs.adobe.com/content/help/ja-JP/experience-platform/profile/api/entities.html)に従います。

### IDを使用したプロファイルデータへのアクセス

APIは、IDを使用してプロファイル情報へのアクセスを許可します。 これは、エンティティ IDをパラメーターおよびエンティティ ID名前空間の1つとして持つ/access/entitiesにGET リクエストを行うことで実行されます。 メモ：50件のレコードを返すリクエストは、422のHTTP ステータスと「関連IDが多すぎる」というメッセージのみを配信し、検索をより多くのパラメーターで絞り込む必要があることに注意してください。

リクエスト：

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

### IDのリストによるプロファイルへのアクセス

APIは、/access/entities エンドポイントへのPOST リクエストを使用し、ペイロードでIDを指定することで、IDのリストを使用してプロファイルへのアクセスを許可します。 これらのIDは、ID値（entityId）とID名前空間（entityIdNS）で構成されます。

リクエスト：
次のリクエストは、IDのリストによって複数の顧客の名前とメールアドレスを取得します。

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

パートナーは、/access/entities エンドポイントにGET リクエストを行うことで、関連するプロファイルエンティティのIDによって時系列イベントにアクセスできます。

### ID によるプロファイルの時系列イベントへのアクセス

時系列イベントは、/access/entities エンドポイントにGET リクエストを行うことで、関連するプロファイルエンティティのIDによってアクセスされます。 このIDは、ID値（entityId）とID名前空間（entityIdNS）で構成されます。

リクエスト：
次のリクエストは、IDでプロファイルエンティティを検索し、エンティティに関連付けられたすべての&#x200B;**時系列イベントのプロパティ endUserID、web、およびチャネル**&#x200B;の値を取得します。

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

時系列イベントを取得すると、結果はページ付けされます。 結果の後続ページがある場合、応答の&lowbar;page.next パラメーターにはIDが含まれます。 さらに、応答の&lowbar;links.next.href パラメーターは、後続のページを取得するためのリクエスト URIを提供します。

リクエスト：

次のリクエストは、_links.next.href URIをリクエストパスとして使用して、結果の次のページを取得します。

```
curl -X GET \

  'https://platform.adobe.io/data/core/ups/access/entities?start=c8d11988-6b56-4571-a123-b6ce74236037&orderby=timestamp&schema.name=_xdm.context.experienceevent&relatedSchema.name=_xdm.context.profile&relatedEntityId=89149270342662559642753730269986316900&relatedEntityIdNS=ECID&fields=endUserIDs,web,channel&startTime=1531260476000&endTime=1531260480000&limit=1' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

応答：

正常な応答は、結果の次のページを返します。 この例では、&lowbar;page.nextおよび&lowbar;links.next.hrefの空の文字列値で示されるように、結果の後続ページがない応答を示します。

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

## 関連記事

* [Real-time Customer Profile API](https://adobe.ly/2TtDHWr)
* [Profile API チュートリアルを使用したリアルタイム顧客プロファイルデータへのアクセス](https://docs.adobe.com/content/help/ja-JP/experience-platform/profile/api/getting-started.html)
* [[!DNL Experience Platform]認証ガイド](https://docs.adobe.com/content/help/ja-JP/experience-platform/tutorials/authentication.html)
