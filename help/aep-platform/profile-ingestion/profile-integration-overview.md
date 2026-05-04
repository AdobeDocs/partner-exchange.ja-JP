---
title: '[!DNL Platform] プロファイル収集およびアクセス統合ガイドの概要'
description: ' [!DNL Experience Platform]  プロファイルの取り込みとアクセスの統合について説明します。'
exl-id: a593511c-dd4c-4437-af73-f44d795cacb8
TQID: https://experienceleague.adobe.com/whnqurJyM4QXl5ikRvez7hpKWRDuU4onzROsUk-WeSI
product_v2: id: d0a3eab4-7b10-4d96-a71e-6c0f8e7b7c87
topic_v2: id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377id: e1e0219c-f879-479f-8427-888ed2a6e9c2
source-git-commit: 6698ae880d1ad13a9387cb1ba66b9ba152d1d407
workflow-type: tm+mt
source-wordcount: 492
ht-degree: 3%

---

# 統合ガイド：[!DNL Experience Platform] プロファイルの取り込みとアクセス

Adobe [!DNL Experience Platform] （AEP）でイングレス機能とエグレス機能を構築する際には、この統合ガイドを使用する必要があります。 バッチ取り込み、ストリーミング取り込み、統合プロファイルアクセス（エグレス）用のAPIがあります。

開発を支援するために、Adobe Exchange チームによって[Postman コレクション ](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman)が作成されました。 このPostman コレクションは、統合ガイド全体で参照されています。

Postman コレクションのインストールと使用について詳しくは、Github [README](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/README.md) ページを参照してください。 [ ロイヤルティ ](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20events.json)および[ プロファイル ](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20profiles.json) データのサンプルデータセットもあります。

## 統合の使用例：インタラクティブ音声応答システム

インテグレーターの場合、[!DNL Experience Platform] APIは、ユーザーインターフェイス全体で提供されるプラットフォームのすべての機能を提供しますが、現在は顧客ワークフローと自動データフローを作成する機能を備えています。 インテグレーターは、データセットのステータスを定期的に確認し、新しいデータ取り込み手順を設定し、独自のオーディエンスアクティベーションソリューションを統合プロファイルに統合します。

インタラクティブ音声応答（IVR）システムとコールセンター管理ソフトウェアの世界を考えてみましょう。 サプライヤーは、[!DNL Experience Platform] APIを使用して、Experience Data Lakeに顧客のコールセンター活動の履歴情報を取り込むことができます。 データがXDM `ExperienceEvent` スキーマ （顧客インタラクションを表現するスキーマ）に取り込まれる場合、これらのインタラクションは摩擦なく統合プロファイルサービスに直接取り込むことができます。 この場合、`callerId`は顧客の識別子として使用されます。 ID サービスはID解決を処理し、統合プロファイルサービスがコールセンターとの最近のやり取りから顧客のプロファイルにデータポイントを追加するのを支援します。

顧客がコールセンターに次回呼び出すと、最初にIVRによって回答されます。 メッセージをパーソナライズし、発信者に合わせたオファーを提供するには、IVR システムで発信者についてより詳細に把握する必要があります。 そこで、統合プロファイルとのAPI統合の出番です。 IVR バックエンドは、ポイント ルックアップについてUnified Profile Serviceに問い合わせることができます。 次に、コールセンターのインタラクションにのみ適用されるプロファイル属性または他の顧客接点でのインタラクションの属性も含まれる顧客プロファイル全体を参照します。 複数のデータソースからのデータを組み合わせ、ID解決と統合プロファイルを使用することで、コールセンターとIVR プロバイダーは、Adobe [!DNL Experience Platform]でサポートされる、カスタマイズされた顧客体験を提供できます。」

## 一般的なリソース

* AEP [製品ドキュメント ](https://docs.adobe.com/content/help/ja-JP/experience-platform/landing/documentation/overview.html)。
* AEP [拡張性](https://www.adobe.com/insights/experience-platform-api-extensibility.html)。

## 質問またはフィードバックを送信しますか？

すべての質問とフィードバックは、[ サポートチャネル ](https://adobeexchangeec.zendesk.com/hc/ja-jp/requests/new)から送信してください
