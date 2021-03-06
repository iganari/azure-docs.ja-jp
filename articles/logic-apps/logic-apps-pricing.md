---
title: 価格と課金 - Azure Logic Apps | Microsoft Docs
description: Azure Logic Apps の価格と課金のしくみについて説明します
services: logic-apps
ms.service: logic-apps
ms.suite: logic-apps
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, LADocs
ms.assetid: f8f528f5-51c5-4006-b571-54ef74532f32
ms.topic: article
ms.date: 09/24/2018
ms.openlocfilehash: 5f9147035c07bbe4fb3f38b74025015e70dd87b3
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/25/2018
ms.locfileid: "47159566"
---
# <a name="pricing-model-for-azure-logic-apps"></a>Azure Logic Apps の価格モデル

Azure Logic Apps を使用して、自動化されたスケーラブルな統合ワークフローをクラウドに作成して実行できます。 ここでは、Logic Apps の課金と価格のしくみの詳細を示します。 

<a name="consumption-pricing"></a>

## <a name="consumption-pricing-model"></a>従量課金モデル

パブリックまたは「グローバル」なLogic Apps サービスを使用して新しいロジック アプリを作成した場合は､使用分についてのみ課金されます｡ 新しいロジック アプリでは、従量課金プランと課金モデルが使用されます。これは、ロジック アプリ インスタンスによるアクションの実行がすべての課金対象になることを意味します。 ロジック アプリ定義内のすべてのステップがアクションになります。この中には、トリガー、制御フローの各ステップ、組み込まれているアクションの呼び出し、およびコネクタの呼び出しが含まれます。 詳細については、「[Logic Apps の価格](https://azure.microsoft.com/pricing/details/logic-apps)」を参照してください。

<a name="fixed-pricing"></a>

## <a name="fixed-pricing-model"></a>固定価格モデル

> [!NOTE]
> 統合サービス環境は*プライベート プレビュー*段階あります｡ アクセスを要求するには､[ここから参加要求を作成](https://aka.ms/iseprivatepreview)します。

専用のリソースを使用する分離されたプライベートな Logic Apps インスタンスである [*統合サービス環境*(ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) を使用して新しいロジック アプリを作成した場合は､内蔵のアクションと標準の ISE ラベルのコネクタに対する固定の月額料金を課金されます｡ ISE には無料の Enterprise コネクタ 1 つが付属するのに対し､追加の Enterprise コネクタは Enterprise コネクタの従量制価格に基づいて課金されます｡ 詳細については、「[Logic Apps の価格](https://azure.microsoft.com/pricing/details/logic-apps)」を参照してください。

<a name="triggers"></a>

## <a name="triggers"></a>トリガー

トリガーは、特定のイベントが発生したときに、ロジック アプリ インスタンスを作成する特別なアクションです。 トリガーにはさまざまな動作があり、それによってロジック アプリの課金方法が変わります。

* **ポーリング トリガー** – このトリガーは、ロジック アプリ インスタンスを作成しワークフローを開始するための基準を満たすメッセージの有無について、継続的にエンドポイントをチェックします。 ロジック アプリ インスタンスが作成されない場合でも、ポーリング要求は実行として数えられ、課金対象になります。 ポーリング間隔を指定するには、ロジック アプリ デザイナーを使用してトリガーを設定します。

  [!INCLUDE [logic-apps-polling-trigger-non-standard-metering](../../includes/logic-apps-polling-trigger-non-standard-metering.md)]

* **Webhook トリガー** – このトリガーは、クライアントが特定のエンドポイントに要求を送信するまで待機します。 Webhook のエンドポイントに送信された個々の要求がアクションの実行としてカウントされます。 たとえば、要求 Webhook トリガーと HTTP Webhook トリガーはどちらも Webhook トリガーです。

* **繰り返しトリガー** – このトリガーは、トリガー内に設定された繰り返し間隔に基づいてロジック アプリ インスタンスを作成します。 たとえば、3 日ごとに実行されるトリガーや、より複雑なスケジュールで実行されるトリガーを設定できます。

トリガーの実行は、ロジック アプリの [概要] ウィンドウの [トリガーの履歴] セクションで確認できます。

## <a name="actions"></a>Actions

組み込まれているアクション (HTTP、Azure Functions、API Management の呼び出しなどのアクションや制御フローのステップなど) は、ネイティブ アクションとして課金され、それぞれに該当する種類があります。 [コネクタ](https://docs.microsoft.com/connectors)を呼び出すアクションの種類は "ApiConnection" です。 これらのコネクタは、標準コネクタまたはエンタープライズ コネクタのいずれかに分類され、該当する[価格][pricing]に基づいて課金されます。 エンタープライズ コネクタは "*プレビュー*" 段階であり、標準コネクタとして課金されます。

成功した実行と失敗した実行のすべてがカウントされ、アクション実行として課金されます。 ただし、条件が満たされなかったためにスキップされたアクションや、ロジック アプリが完了前に強制終了されたために実行されなかったアクションは、アクション実行としてカウントされません。 無効なロジック アプリは新しいインスタンスをインスタンス化できないため、無効になっている間は課金されることはありません。

> [!NOTE]
> ロジック アプリを無効にした場合、現在実行中のインスタンスが完全に停止するまで少し時間がかかることがあります。

ループ内で実行されるアクションは、ループ内での各サイクルごとにカウントされます。 たとえば、10 項目で構成されるリストを処理する "for each" ループのアクションは、リストの項目数 (10) にループのアクション数 (1) を乗算し、ループを開始するアクション (1) を加算したものになります。 したがって、この例では、計算は (10 * 1) + 1 になり、結果は 11 のアクション実行になります。

## <a name="integration-account-usage"></a>統合アカウントの使用

従量制のプランには、Logic Apps の [B2B/EDI](logic-apps-enterprise-integration-b2b.md) 機能と [XML 処理](logic-apps-enterprise-integration-xml.md)機能を追加コストなしで調査、開発、およびテストできる[統合アカウント](logic-apps-enterprise-integration-create-integration-account.md)が含まれています。 リージョンごとに 1 つの統合アカウントを使用して、特定の[数まで成果物](../logic-apps/logic-apps-limits-and-config.md) (EDI 取引先と契約、マップ、スキーマ、アセンブリ、証明書、バッチ構成など) を格納できます。

Logic Apps は、Logic Apps SLA によってサポートされている基本統合アカウントと標準統合アカウントも提供します。 メッセージの処理のみを使用する場合、または大規模なビジネス エンティティと取引パートナー関係がある小規模なビジネス パートナーとして機能する場合は、基本統合アカウントを使用できます。 標準統合アカウントは、より複雑な B2B 関係をサポートし、管理できるエンティティの数が大きくなります。 詳しくは、「[Logic Apps の価格](https://azure.microsoft.com/pricing/details/logic-apps)」をご覧ください。

## <a name="next-steps"></a>次の手順

* [Logic Apps の詳細を見る][whatis]
* [初めてのロジック アプリの作成][create]

[pricing]: https://azure.microsoft.com/pricing/details/logic-apps/
[whatis]: logic-apps-overview.md
[create]: quickstart-create-first-logic-app-workflow.md

