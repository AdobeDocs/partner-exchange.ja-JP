---
title: AEP サンドボックスへのアクセスと参照
description: Experience Platformサンドボックスにアクセスして参照する方法を説明します。
exl-id: 62c21615-4b03-4900-a1ad-8f809c836491
source-git-commit: fe7519c35fb9155ce54cad85941c887f15881a38
workflow-type: tm+mt
source-wordcount: '737'
ht-degree: 1%

---

# AEP サンドボックスへのアクセスと参照

この記事では、次の内容について説明します。

* 既存の Exchange パートナーAdobe組織と共有 AEP サンドボックスの違い。
* AEP 共有サンドボックスへのアクセスをリクエストしています。
* AEP 共有サンドボックスへの招待メールを受け取る。
* での新しいユーザーの招待 [!DNL Admin Console].
* AEP UI を移動します。

AEP のサンドボックステクノロジーの一般的な概要については、次を参照してください [記事](https://docs.adobe.com/content/help/ja-JP/experience-platform/sandbox/home.html).

## 共有 AEP サンドボックス

Exchange パートナーは、様々なAdobeにアクセスできます [!DNL Experience Cloud] 製品 (AEP 以外の製品： [!DNL Analytics], [!DNL Target]、プラットフォームタグなど ) を、独自のAdobeで使用 [!DNL Experience Cloud] 組織（非共有）。 パートナーには、ユーザーやその他の権限を管理するための、自分の組織に対するシステム管理者のアクセス権が付与されます。 Adobe [!DNL Experience Platform] (AEP) の処理は、他のAdobeサンドボックスとは異なります。 主な違いを次に示します。

* AEP へのアクセスは、パートナーのメインAdobeを通じては実行されません [!DNL Experience Cloud] サンドボックス組織
* AEP へのアクセスは、共有の Exchange 組織を通じてAdobeされます。
* 他の多くのAdobeExchange のパートナー企業が、同じ組織を使用して AEP にアクセスしています
   * AEP サンドボックス機能を使用すると、他のパートナーがこの共有組織内のデータやアクティビティを表示したり変更したりすることはできません。各パートナーは、共有組織内の別のサンドボックスにアクセスできます。
* この共有組織内の管理権限は非常に制限されています。
* AEP の Sandbox へのアクセス権を付与されると、Admin ConsoleまたはメインExperience Cloudのホームページにいる間、UI の右上に組織切り替えボタン上の 2 つの組織が表示されます。 ただし、AEP にサインインした場合は、共有組織のみが表示されます。

## 共有 AEP サンドボックスへのアクセスをリクエスト

を送信 [サポートリクエスト](https://adobeexchangeec.zendesk.com/hc/ja-jp/requests/new) を次の情報と共に使用します。

* メールアドレス
* 件名： AEP サンドボックスリクエスト
* 製品：一般的なプロビジョニング/サンドボックス
* チケットの種類：プログラムサポート — Exchange プログラム/プロビジョニングリクエストの質問
* 説明： AEP サンドボックスを使用する必要がある統合の使用例について、簡単に説明します。
* AEP サンドボックスに追加する必要のあるすべてのユーザー名と電子メールも必ず指定してください。 リクエストの後に追加のAdobeを追加することもできますが、追加のチケットを介して追加のユーザーを追加する必要があります（以下を参照）。

## 電子メールの招待状を受け取る

AEP サンドボックスをリクエストした主要連絡先には、Adobeを使用して「開始」するよう促す自動電子メールが届きます [!DNL Experience Platform]. 主要連絡先には、次の節で説明する管理権限も付与されます。

電子メールで「はじめに」ボタンを選択する代わりに、に直接移動します。 `https://platform.adobe.com.` 招待内の電子メールアドレスに関連付けられているAdobe IDを使用してログインします。Adobe IDに関連付けられていない場合は、電子メールアドレスを作成します。

## 追加のユーザーの招待

を送信 [サポートリクエスト](https://adobeexchangeec.zendesk.com/hc/ja-jp/requests/new) を次の情報と共に使用します。

* 要求者のメールアドレス
* 件名：AEP Sandbox — 管理者/ユーザーの追加
* 製品：一般的なプロビジョニング/サンドボックス
* チケットの種類：プログラムサポート — Exchange プログラム/プロビジョニングリクエストの質問
* 説明：追加するユーザーのリスト（名前と E メール）

## AEP UI の操作

AEP UI を見る [紹介ビデオ](https://docs.adobe.com/content/help/en/platform-learn/tutorials/intro-to-platform/interface-tour.html)

AEP UI 内には、左側のパネルから移動できる 12 の主な領域があります。 ただし、このタイプの統合で最も重要な節は、スキーマ、データセット、プロファイルです。

* ホーム — ランディング画面

   * 使い始める前にアクティビティを紹介します
   * 学習コンテンツへのリンクを提供します。
   * スキーマ、データセット、プロファイルなど、一部の主要な AEP オブジェクトのダッシュボードビューを提供します。

* ワークフロー — データを AEP に取り込むための共通のワークフローが開始されます。
* 接続/ソース — AEP に取り込まれるデータのソースを管理します。
* 接続/宛先 — 外部システムにデータを送信するための接続を管理します。
* プロファイル：個々の顧客プロファイルの表示と管理
* セグメント — 顧客セグメントを参照、作成および変更します
* ID - ID 名前空間を参照、作成および変更します。これらは、顧客を一意に識別するために使用されるプライマリ ID のタイプです。
* モデル（データサイエンス） — 埋め込み Jupyter Notebooks 環境の使用など、データサイエンスアクティビティに取り組む
* サービス（データサイエンス） — データサイエンスレシピをサービスとして公開する
* スキーマ — スキーマを参照、作成および変更します。これらは、データを整理しておくために使用される詳細なデータ定義です。
* データセット — データセットの参照、作成、管理。データセットは、スキーマによって定義され、AEP 内のデータが存在する場所です
* クエリ — クエリのリポジトリを参照、作成、変更および使用して、データセット内のデータから洞察を得る
* 監視 — バッチとストリーミングの両方で AEP に出入りするすべてのデータのステータスを表示
