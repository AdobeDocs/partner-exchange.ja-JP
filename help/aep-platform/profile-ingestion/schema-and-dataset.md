---
title: AEPのスキーマとデータセットの作成
description: Experience Platformでスキーマとデータセットを作成します。
exl-id: a2773551-20a3-4a5b-ab53-60fa67e38ec0
TQID: https://experienceleague.adobe.com/uQtIQwCgsjOd5pR5w4LF634-Whvjl0jmF5WywVWlkZQ
product_v2:
  - id: d0a3eab4-7b10-4d96-a71e-6c0f8e7b7c87
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 6698ae880d1ad13a9387cb1ba66b9ba152d1d407
workflow-type: tm+mt
source-wordcount: 617
ht-degree: 25%

---

# スキーマとデータセットの作成

[Postman コレクション &#x200B;](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman)は、関連付けられた呼び出しを番号で使用して、記事全体で参照されます。 Postman コレクションのインストールと使用について詳しくは、Github [README](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/README.md) ページを参照してください。 [&#x200B; ロイヤルティ &#x200B;](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20events.json)および[&#x200B; プロファイル &#x200B;](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20profiles.json) データのサンプルデータセットもあります。

## スキーマ

スキーマは、データの構造と形式を表し、検証する一連のルールです。 スキーマは、高いレベルで、実世界のオブジェクト（人など）の抽象的な定義を提供し、そのオブジェクトの各インスタンスに含めるデータ（名、姓、誕生日など）の概要を示します。 スキーマは、UIまたは[!DNL Experience Platform] APIを使用して作成できます。

詳しくは、[このドキュメント &#x200B;](https://www.adobe.io/apis/experienceplatform/home/xdm/xdmservices.html#!api-specification/markdown/narrative/technical_overview/schema_registry/schema_composition/schema_composition.md)を参照してください。

### スキーマの作成

パートナーは、この[&#x200B; チュートリアル &#x200B;](https://docs.adobe.com/content/help/en/experience-platform/xdm/tutorials/create-schema-ui.html)に従って、UIを使用してスキーマを構築できます。 この例では、ロイヤルティプログラムのプロファイルスキーマを使用しています。 この例はプロファイルスキーマですが、イベントベースのスキーマも同様のプロセスを使用して使用できます。

APIを使用するには、パートナーは、[!DNL Experience Platform]権限が有効になっている既存のAdobe I/O統合を持っている必要があります。 このガイドでは、[I/O統合の作成](https://docs.adobe.com/content/help/ja-JP/experience-platform/tutorials/home.html#!api-specification/markdown/narrative/tutorials/authenticate_to_acp_tutorial/authenticate_to_acp_tutorial.md)について説明しています。

次に、[このリンク &#x200B;](https://docs.adobe.com/content/help/en/experience-platform/xdm/tutorials/create-schema-api.html)にアクセスして、APIを使用してスキーマを構築する方法を確認します。

Postmanを使用してスキーマを作成するには、フォルダー1:Create Schema、1a:Create Schema for PROFILE dataまたは1b:Create Schema for EVENT dataに含まれる呼び出しを使用します。

## データセット

Adobe [!DNL Experience Platform]に取り込まれるすべてのデータは、データセットに含まれています。 データセットは、スキーマ（列）とフィールド（行）で構成されるデータコレクション（通常はテーブル）を格納し管理するための構造です。 データセットには、保存するデータの様々な側面を記述したメタデータも含まれます。

カタログサービスは、[!DNL Experience Platform]内のデータの場所とリネージの記録システムであり、データセットの作成と管理に使用されます。 カタログは各データセットのメタデータを追跡します。このメタデータには、データセットが準拠するエクスペリエンスデータモデル（XDM）スキーマへの参照（次の節で説明）と、そのデータセットに取り込まれるレコードの数が含まれます。

詳細なデータセットの概要については、[こちら](https://docs.adobe.com/content/help/en/experience-platform/catalog/datasets/overview.html)を参照してください。

### データセットの作成

![&#x200B; データセットの作成アニメーション Gif](images/creating_a_dataset.gif)

<!-- 
We don't yet support hover text in images (and we render it poorly when included). I removed "Creating a Dataset" from the above image link. We can add it back when we support it (Summer 2020?) -Bob
-->

UIを使用してデータセットを作成します。

1. 「**[!UICONTROL データセットを作成]**」をクリックします。

1. 「**[!UICONTROL スキーマから作成]**」をクリックします。

1. 「**[!UICONTROL 完了]**」をクリックします。

データセットユーザーガイドについては、[こちら](https://docs.adobe.com/content/help/en/experience-platform/catalog/datasets/user-guide.html)を参照してください。

[APIを使用してデータセットを作成](https://docs.adobe.com/content/help/en/experience-platform/catalog/datasets/create.html)。

Postmanを使用してデータセットを作成するには、フォルダー2：データセットを作成、2a：プロファイルデータのデータセットを作成、または2b：イベントデータのデータセットを作成を使用します。

## パートナー向けのスキーマとデータセットのベストプラクティス

* パートナーデータでは、顧客の既存のプロファイルスキーマとエクスペリエンススキーマを組み合わせて作成するのではなく、別のプロファイルスキーマを使用する必要があります。
* パートナーは、可能な限りAdobe クラスとミックスインを使用する必要があります。
* パートナーは、既存のデータセットにデータを組み合わせるのではなく、別のデータセットを使用してデータをアップロードする必要があります。
* パートナーは今のところ、グローバルレジストリにスキーマをアップロードできません。
