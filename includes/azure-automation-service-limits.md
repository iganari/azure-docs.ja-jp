---
title: インクルード ファイル
description: インクルード ファイル
services: automation
author: georgewallace
ms.service: automation
ms.topic: include
ms.date: 12/13/2018
ms.author: gwallace
ms.custom: include file
ms.openlocfilehash: f3ae2289112948dea7d2649c4fad6b1cafb3804b
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/15/2018
ms.locfileid: "53444226"
---
#### <a name="process-automation"></a>プロセスの自動化

| リソース | 上限 |メモ|
| --- | --- |---|
| Automation アカウントあたりの 30 秒ごとに送信できる新しいジョブの最大数 (スケジュールされていないジョブ) |100 |この制限に到達すると、次のジョブ作成要求は失敗します。 クライアントにエラー応答が届きます。|
| Automation アカウントあたりのインスタンスの同じ時刻に同時実行するジョブの最大数 (スケジュールされていないジョブ) |200 |この制限に到達すると、次のジョブ作成要求は失敗します。 クライアントにエラー応答が届きます。|
| 30 日のローリング期間におけるジョブ メタデータの最大ストレージ サイズ。 | 10 GB (約 400 万ジョブ)|この制限に到達すると、次のジョブ作成要求は失敗します。 |
| Automation アカウントあたりの 30 秒ごとにインポートできるモジュールの最大数 |5 ||
| モジュールの最大サイズ |100 MB ||
| ジョブ実行時間 - Free レベル |カレンダー月あたりサブスクリプションごとに 500 分 ||
| サンドボックスごとに許可される最大ディスク容量**<sup>1</sup>** |1 GB |Azure サンドボックスにのみ適用されます|
| サンドボックスに割り当てる最大メモリ量 **<sup>1</sup>** |400 MB |Azure サンドボックスにのみ適用されます|
| サンドボックスごとに許可されるネットワーク ソケットの最大数 **<sup>1</sup>** |1,000 |Azure サンドボックスにのみ適用されます|
| Runbook ごとに許可される最長実行時間 **<sup>1</sup>** |3 時間 |Azure サンドボックスにのみ適用されます|
| サブスクリプションでの Automation アカウントの最大数 |制限なし ||
|1 つの Hybrid Runbook Worker で実行される同時実行ジョブの最大数|50 ||
| Runbook ジョブの最大パラメーター サイズ   | 512 KB||
| Runbook の最大パラメーター   | 50|パラメーターに JSON または XML 文字列を渡し、パラメーターの 50 の上限に達したら、Runbook を使用して解析することができます。|
| Webhook のペイロードの最大サイズ |  512 KB|
| ジョブ データが保持される最大日数|30 日|
| PowerShell ワークフローの最大状態サイズ |5 MB| ワークフローのチェックポイント処理を行うときに PowerShell ワークフロー Runbook に適用されます。|

**<sup>1</sup>** サンドボックスは、複数のジョブで利用できる共有環境です。同じサンドボックスを利用するジョブには、そのサンドボックスのリソース制限が適用されます。

#### <a name="change-tracking-and-inventory"></a>変更履歴とインベントリ

次の表は、Change Tracking でのマシンごとの追跡項目制限を示します。

| **リソース** | **制限**| **メモ** |
|---|---|---|
|ファイル|500||
|レジストリ|250||
|Windows ソフトウェア|250|ソフトウェア更新プログラムは含まれません|
|Linux パッケージ|1250||
|サービス|250||
|デーモン|250||
