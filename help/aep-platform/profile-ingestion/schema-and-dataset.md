---
title: AEP のスキーマとデータセットの作成
description: Experience Platformでスキーマとデータセットを作成します。
exl-id: a2773551-20a3-4a5b-ab53-60fa67e38ec0
source-git-commit: fe7519c35fb9155ce54cad85941c887f15881a38
workflow-type: tm+mt
source-wordcount: '487'
ht-degree: 20%

---

# スキーマとデータセットの作成

[Postman コレクション &#x200B;](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman) は、関連する呼び出しを番号で使用して、記事全体で参照されます。 Postman コレクションのインストールと使用について詳しくは、Github [README](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/README.md) ページを参照してください。 また、[&#x200B; ロイヤルティ &#x200B;](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20events.json) データと [&#x200B; プロファイル &#x200B;](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20profiles.json) データのサンプルデータセットもあります。

## スキーマ

スキーマは、データの構造と形式を表現し検証する一連のルールです。スキーマは、概要レベルで実世界のオブジェクト（人など）の抽象的な定義を提供し、そのオブジェクトの各インスタンスに含めるデータ（名、姓、生年月日など）の概要を示します。 スキーマは、UI で作成することも、[!DNL Experience Platform] API を使用して作成することもできます。

詳しくは、[&#x200B; このドキュメント &#x200B;](https://www.adobe.io/apis/experienceplatform/home/xdm/xdmservices.html#!api-specification/markdown/narrative/technical_overview/schema_registry/schema_composition/schema_composition.md) を参照してください。

### スキーマの作成

パートナーは、この [&#x200B; チュートリアル &#x200B;](https://docs.adobe.com/content/help/ja-JP/experience-platform/xdm/tutorials/create-schema-ui.html) に従って、UI を使用してスキーマを作成できます。 この例では、ロイヤルティプログラムプロファイルスキーマを使用します。 例はプロファイルスキーマですが、同様のプロセスを使用してイベントベースのスキーマを使用できます。

この API を使用するには、パートナーは、[!DNL Experience Platform] の権限が有効になっている既存のAdobe I/O統合を持っている必要があります。 [I/O 統合の作成 &#x200B;](https://docs.adobe.com/content/help/ja-JP/experience-platform/tutorials/home.html#!api-specification/markdown/narrative/tutorials/authenticate_to_acp_tutorial/authenticate_to_acp_tutorial.md) については、このガイドを参照してください。

次に、[&#x200B; このリンク &#x200B;](https://docs.adobe.com/content/help/ja-JP/experience-platform/xdm/tutorials/create-schema-api.html) を参照して、API を使用してスキーマを作成する方法を確認してください。

Postmanを使用してスキーマを作成するには、フォルダー 1: Create Schema、1a: Create Schema for PROFILE data または 1b: Create Schema for EVENT data に含まれる呼び出しを使用します。

## データセット

Adobe[!DNL Experience Platform] に取り込まれるすべてのデータは、データセットに含まれます。 データセットは、通常、スキーマ（列）とフィールド（行）を含むテーブルであるデータコレクションのストレージと管理をおこなう構成体です。データセットには、保存するデータの様々な側面を記述したメタデータも含まれます。

カタログサービスは、[!DNL Experience Platform] 内のデータの場所と系列のレコードシステムであり、データセットの作成と管理に使用されます。 カタログは各データセットのメタデータを追跡します。このメタデータには、データセットが準拠するエクスペリエンスデータモデル（XDM）スキーマへの参照（次の節で説明）と、そのデータセットに取り込まれるレコードの数が含まれます。

データセットの概要について詳しくは [&#x200B; こちら &#x200B;](https://docs.adobe.com/content/help/ja-JP/experience-platform/catalog/datasets/overview.html) を参照してください。

### データセットの作成

![&#x200B; データセットのアニメーション Gif の作成 &#x200B;](images/creating_a_dataset.gif)

<!-- 
We don't yet support hover text in images (and we render it poorly when included). I removed "Creating a Dataset" from the above image link. We can add it back when we support it (Summer 2020?) -Bob
-->

UI を使用してデータセットを作成します。

1. **[!UICONTROL データセットを作成]** をクリックします。

1. **[!UICONTROL スキーマから作成]** をクリックします。

1. 「**[!UICONTROL 完了]**」をクリックします。

データセットユーザーガイドについては、[&#x200B; こちら &#x200B;](https://docs.adobe.com/content/help/ja-JP/experience-platform/catalog/datasets/user-guide.html) を参照してください。

[API を使用したデータセットの作成 &#x200B;](https://docs.adobe.com/content/help/ja-JP/experience-platform/catalog/datasets/create.html)。

Postmanを使用してデータセットを作成するには、フォルダー 2: データセットを作成、2a: プロファイルデータのデータセットを作成または 2b: EVENT データのデータセットを作成を使用します。

## パートナー向けのスキーマとデータセットのベストプラクティス

* パートナーデータは、顧客の既存のプロファイルスキーマとエクスペリエンススキーマを組み合わせて作成するのではなく、別のプロファイルスキーマを使用する必要があります。
* パートナーは、可能な限りAdobeクラスとミックスインを使用する必要があります。
* パートナーは、データを既存のデータセットに結合するのではなく、別のデータセットを使用してデータをアップロードする必要があります。
* 現時点では、パートナーはスキーマをグローバルレジストリにアップロードできません。
