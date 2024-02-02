---
title: '"[!DNL Platform] プロファイル取り込みおよびアクセス統合ガイドの概要»'
description: の統合について説明します。 [!DNL Experience Platform] プロファイルの取り込みとアクセス。
exl-id: a593511c-dd4c-4437-af73-f44d795cacb8
source-git-commit: fe7519c35fb9155ce54cad85941c887f15881a38
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 0%

---

# 統合ガイド： [!DNL Experience Platform] プロファイルの取り込みとアクセス

パートナーは、この統合ガイドを使用して、Adobeでの入出力機能の構築を支援する必要があります。 [!DNL Experience Platform] (AEP) を参照してください。 バッチ取得、ストリーミング取得、統合プロファイルアクセス（エグレス）用の API があります。

開発を支援するには、 [Postman Collection](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman) は、Exchange チームによってAdobeされました。 このPostmanコレクションは、統合ガイド全体で参照されます。

Postmanコレクションのインストールと使用に関する詳細については、GitHub を参照してください。 [README](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/README.md) ページに貼り付けます。 また、 [loyalty](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20events.json) および [profile](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20profiles.json) データ。

## 統合の使用例：インタラクティブな音声応答システム

インテグレーターの場合、 [!DNL Experience Platform] API は、ユーザーインターフェイス全体で提供されるプラットフォームのすべての機能を提供しますが、顧客ワークフローと自動データフローを作成する機能を提供します。 インテグレーターは、データセットのステータスを定期的に確認し、新しいデータ取り込み手順を設定し、独自のオーディエンスアクティベーションソリューションを統合プロファイルに統合します。

Interactive Voice Response(IVR) システムとコールセンター管理ソフトウェアの世界を考えてみましょう。 仕入先は、 [!DNL Experience Platform] Experience Data Lake での顧客のコールセンターアクティビティの履歴情報を取り込むための API。 データが XDM に取り込まれている場合 `ExperienceEvent` スキーマ（顧客とのやり取りを表すスキーマ）を使用すると、これらのやり取りを、摩擦なしで統合プロファイルサービスに直接取り込むことができます。 この場合、 `callerId` は、顧客の識別子として使用されます。 ID サービスは、ID の解決を担当し、統合プロファイルサービスを支援して、コールセンターとの最近のやり取りから顧客のプロファイルにデータポイントを追加します。

次回、お客様がコールセンターに電話をかける際、最初に IVR によって回答されます。 メッセージをパーソナライズし、呼び出し元に合わせたオファーを提供するには、IVR システムが呼び出し元に関する詳細を理解する必要があります。 統合プロファイルとの API 統合は、ここで実現します。 IVR バックエンドは、ポイント参照について統合プロファイルサービスに問い合わせることができます。 次に、コールセンターのインタラクションのみに適用されるプロファイル属性か、完全な顧客プロファイルを参照します。このプロファイルには、他のタッチポイントでのインタラクションの属性も含まれます。 コールセンターと IVR プロバイダーは、複数のデータソースからのデータを組み合わせ、ID 解決と統合プロファイルを使用することで、Adobeがサポートする、カスタマイズされたカスタマーエクスペリエンスを提供できます [!DNL Experience Platform].&quot;

## 一般リソース

* AEP [製品ドキュメント](https://docs.adobe.com/content/help/ja-JP/experience-platform/landing/documentation/overview.html).
* AEP [拡張機能](https://www.adobe.com/insights/experience-platform-api-extensibility.html).

## ご質問やフィードバックは？

すべての質問とフィードバックを [サポートチャネル](https://adobeexchangeec.zendesk.com/hc/ja-jp/requests/new)
