---
title: AEP スキーマとデータセットの作成
description: Experience Platformでスキーマとデータセットを作成する。
exl-id: a2773551-20a3-4a5b-ab53-60fa67e38ec0
source-git-commit: fe7519c35fb9155ce54cad85941c887f15881a38
workflow-type: tm+mt
source-wordcount: '487'
ht-degree: 20%

---

# スキーマとデータセットの作成

The [Postman Collection](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman) は、記事全体で、関連する呼び出し（番号別）を使用して参照されます。 Postmanコレクションのインストールと使用に関する詳細については、GitHub を参照してください。 [README](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/README.md) ページに貼り付けます。 また、 [loyalty](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20events.json) および [profile](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20profiles.json) データ。

## スキーマ

スキーマは、データの構造と形式を表現し検証する一連のルールです。スキーマは、高いレベルで、実世界のオブジェクト（人など）の抽象的な定義を提供し、そのオブジェクトの各インスタンスに含めるデータ（名、姓、誕生日など）の概要を示します。 スキーマは、UI または [!DNL Experience Platform] API

詳しくは、 [このドキュメント](https://www.adobe.io/apis/experienceplatform/home/xdm/xdmservices.html#!api-specification/markdown/narrative/technical_overview/schema_registry/schema_composition/schema_composition.md) を参照してください。

### スキーマの作成

パートナーは、次の手順に従うことで、UI を使用してスキーマを構築できます [チュートリアル](https://docs.adobe.com/content/help/ja-JP/experience-platform/xdm/tutorials/create-schema-ui.html). この例では、ロイヤルティプログラムのプロファイルスキーマを使用します。 この例はプロファイルスキーマですが、イベントベースのスキーマも同様のプロセスを使用して使用できます。

API を使用するには、パートナーは、 [!DNL Experience Platform] 権限が有効です。 このガイドを参照してください。 [I/O 統合の作成](https://docs.adobe.com/content/help/ja-JP/experience-platform/tutorials/home.html#!api-specification/markdown/narrative/tutorials/authenticate_to_acp_tutorial/authenticate_to_acp_tutorial.md).

その後、訪問 [このリンク](https://docs.adobe.com/content/help/en/experience-platform/xdm/tutorials/create-schema-api.html) を参照してください。

Postman経由でスキーマを作成するには、フォルダー 1：スキーマの作成、1a：プロファイルデータのスキーマの作成、1b：イベントデータのスキーマの作成に含まれる呼び出しを使用します。

## データセット

Adobeに取り込まれるすべてのデータ [!DNL Experience Platform] は、データセットに含まれています。 データセットは、通常、スキーマ（列）とフィールド（行）を含むテーブルであるデータコレクションのストレージと管理をおこなう構成体です。データセットには、保存するデータの様々な側面を記述したメタデータも含まれます。

カタログサービスは、内のデータの場所とリネージのレコードシステムです [!DNL Experience Platform]、およびは、データセットの作成と管理に使用されます。 カタログは各データセットのメタデータを追跡します。このメタデータには、データセットが準拠するエクスペリエンスデータモデル（XDM）スキーマへの参照（次の節で説明）と、そのデータセットに取り込まれるレコードの数が含まれます。

移動 [ここ](https://docs.adobe.com/content/help/en/experience-platform/catalog/datasets/overview.html) 詳細なデータセットの概要を参照してください。

### データセットの作成

![データセットアニメーション GIF の作成](images/creating_a_dataset.gif)

<!-- 
We don't yet support hover text in images (and we render it poorly when included). I removed "Creating a Dataset" from the above image link. We can add it back when we support it (Summer 2020?) -Bob
-->

UI を使用してデータセットを作成します。

1. クリック **[!UICONTROL データセットを作成]**.

1. クリック **[!UICONTROL スキーマから作成]**.

1. 「**[!UICONTROL 完了]**」をクリックします。

移動 [ここ](https://docs.adobe.com/content/help/en/experience-platform/catalog/datasets/user-guide.html) 」を参照してください。

[API を使用したデータセットの作成](https://docs.adobe.com/content/help/en/experience-platform/catalog/datasets/create.html).

Postmanでデータセットを作成するには、フォルダー 2：データセットを作成、2a：プロファイルデータのデータセットを作成、2b：イベントデータのデータセットを作成を使用します。

## パートナー向けのスキーマとデータセットのベストプラクティス

* パートナーデータは、個別のプロファイルスキーマを使用する必要があり、顧客の既存のプロファイルスキーマとエクスペリエンススキーマの組み合わせを作成する必要があります。
* パートナーは、可能な限りAdobeクラスとミックスインを使用する必要があります。
* パートナーは、データを既存のデータセットに組み合わせる代わりに、別のデータセットを使用してデータをアップロードする必要があります。
* パートナーは、現時点では、スキーマをグローバルレジストリにアップロードできません。
