---
title: 「[!DNL Platform] プロファイルの取得とアクセスの統合ガイドの概要」
description: プロファイルの取り込みとアクセス  [!DNL Experience Platform]  統合について説明します。
exl-id: a593511c-dd4c-4437-af73-f44d795cacb8
source-git-commit: fe7519c35fb9155ce54cad85941c887f15881a38
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 0%

---

# 統合ガイド：[!DNL Experience Platform] プロファイルの取り込みとアクセス

パートナーは、このAdobeガイドを使用して、統合 [!DNL Experience Platform] （AEP）での入力と出力の機能を構築するのを支援する必要があります。 バッチ取得、ストリーミング取得、統合プロファイルアクセス（エグレス）用の API が用意されています。

開発を支援するために、Adobe Exchangeチームによって [&#128279;](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman)0&rbrace;Postman コレクション &rbrace; が作成されました。 このPostman コレクションは、統合ガイド全体で参照されます。

Postman コレクションのインストールと使用について詳しくは、Github [README](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/README.md) ページを参照してください。 また、[&#x200B; ロイヤルティ &#x200B;](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20events.json) データと [&#x200B; プロファイル &#x200B;](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20loyalty%20profiles.json) データのサンプルデータセットもあります。

## 統合のユースケースの例：インタラクティブ音声応答システム

インテグレーターにとっては、[!DNL Experience Platform] API は、ユーザーインターフェイス全体で提供されるプラットフォームのすべての機能を提供しますが、現在は、顧客ワークフローと自動データフローを作成する機能を備えています。 インテグレーターは、データセットのステータスを定期的に確認し、新しいデータ取り込み手順を設定し、独自のオーディエンスアクティベーションソリューションを統合プロファイルと統合します。

インタラクティブ音声応答（IVR）システムとコールセンター管理ソフトウェアの世界を考えてみましょう。 サプライヤーは、[!DNL Experience Platform] の API を使用して、顧客がエクスペリエンスデータレイクで行ったコールセンターアクティビティの履歴情報を取り込むことができます。 データが XDM `ExperienceEvent` スキーマ（顧客インタラクションを表すスキーマ）に取り込まれている場合、これらのインタラクションは、統合プロファイルサービスに直接摩擦なしで取り込むことができます。 この場合、`callerId` は顧客の識別子として使用されます。 ID サービスは、ID 解決を行い、統合プロファイルサービスがコールセンターとの最近のインタラクションからのお客様のプロファイルへのデータポイントの追加を支援します。

お客様が次にコールセンターに電話をかけてきた際には、まず IVR から回答が得られます。 メッセージをパーソナライズし、呼び出し元に合わせてオファーを配信するには、IVR システムは呼び出し元についてさらに理解する必要があります。 統合プロファイルとの API 統合は、ここで行います。 IVR バックエンドは、ポイントルックアップのために統合プロファイルサービスに連絡できます。 次に、コールセンターインタラクションのみに適用されるプロファイル属性か、顧客プロファイル全体（他のタッチポイントでのインタラクションの属性も含む）のいずれかを調べます。 コールセンターと IVR プロバイダーは、複数のデータソースのデータを組み合わせ、ID 解決と統合プロファイルを使用することで、Adobe[!DNL Experience Platform] ールのサポートを受けながら、カスタマイズされたカスタマーエクスペリエンスを提供できます。」

## 一般的なリソース

* AEP [&#x200B; 製品ドキュメント &#x200B;](https://docs.adobe.com/content/help/ja-JP/experience-platform/landing/documentation/overview.html)。
* AEP [&#x200B; 拡張機能 &#x200B;](https://www.adobe.com/insights/experience-platform-api-extensibility.html)。

## 質問やフィードバックは？

すべての質問とフィードバックは、[&#x200B; サポートチャネル &#x200B;](https://adobeexchangeec.zendesk.com/hc/ja-jp/requests/new) を通じて送信してください
