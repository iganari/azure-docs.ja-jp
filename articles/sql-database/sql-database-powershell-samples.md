---
title: SQL Database の Azure PowerShell スクリプトの例 | Microsoft Docs
description: Azure SQL Database サーバー、エラスティック プール、データベース、ファイアウォールの作成と管理で役立つ Azure PowerShell スクリプトの例。
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 02/01/2019
ms.openlocfilehash: 07e530a30898e57916b91632c4bf49d43d69471a
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/01/2019
ms.locfileid: "55564852"
---
# <a name="azure-powershell-samples-for-azure-sql-database"></a>Azure SQL Database 用の Azure PowerShell サンプル

Azure SQL Database では、Azure PowerShell を使用してデータベース、インスタンス、プールを構成することができます。

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

PowerShell をインストールしてローカルで使用する場合、このチュートリアルでは Azure PowerShell モジュール バージョン 5.7.0 以降が必要になります。 バージョンを確認するには、`Get-Module -ListAvailable AzureRM` を実行します。 アップグレードする必要がある場合は、[Azure PowerShell モジュールのインストール](/powershell/azure/install-az-ps)に関するページを参照してください。 PowerShell をローカルで実行している場合、`Connect-AzureRmAccount` を実行して Azure との接続を作成することも必要です。

## <a name="single-database-and-elastic-pools"></a>単一データベースとエラスティック プール

次の表には、Azure SQL Database の Azure PowerShell スクリプトのサンプルへのリンクが含まれています。

| |  |
|---|---|
|**単一データベースとエラスティック プールを作成して構成する**||
| [単一のデータベースを作成し、データベース サーバーのファイアウォール規則を構成する](scripts/sql-database-create-and-configure-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | この PowerShell スクリプトでは、単一の Azure SQL Database を作成し、サーバー レベルのファイアウォール規則を構成します。 |
| [エラスティック プールを作成し、プールされているデータベースを移動する](scripts/sql-database-move-database-between-pools-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | この PowerShell スクリプトでは、Azure SQL Database のエラスティック プールを作成し、プールされているデータベースを移動して、コンピューティング サイズを変更します。|
|**geo レプリケーションを構成しフェールオーバーする**||
| [アクティブ geo レプリケーションを使用して、単一のデータベースを構成およびフェールオーバーする](scripts/sql-database-setup-geodr-and-failover-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| この PowerShell スクリプトは、単一の Azure SQL Database のアクティブ geo レプリケーションを構成し、セカンダリ レプリカにフェールオーバーします。 |
| [アクティブ geo レプリケーションを使用して、プールされているデータベースを構成およびフェールオーバーする](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| この PowerShell スクリプトは、SQL エラスティック プールにある Azure SQL Database のアクティブ geo レプリケーションを構成し、セカンダリ レプリカにフェールオーバーします。 |
| [単一データベースのフェールオーバー グループを構成およびフェールオーバーする](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | この PowerShell スクリプトは、Azure SQL Database サーバー インスタンスのフェールオーバー グループを構成し、このフェールオーバー グループにデータベースを追加し、セカンダリ サーバーにフェールオーバーします |
|**単一のデータベースとエラスティック プールをスケーリングする**||
| [単一のデータベースをスケーリングする](scripts/sql-database-monitor-and-scale-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | この PowerShell スクリプトは、Azure SQL Database のパフォーマンス メトリックを監視し、そのデータベースを上位のコンピューティング サイズにスケーリングして、パフォーマンス メトリックの 1 つにアラート ルールを作成します。 |
| [エラスティック プールをスケーリングする](scripts/sql-database-monitor-and-scale-pool-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | この PowerShell スクリプトは、Azure SQL Database エラスティック プールのパフォーマンス メトリックを監視し、そのデータベースを上位のコンピューティング サイズにスケーリングして、パフォーマンス メトリックの 1 つにアラート ルールを作成します。  |
| **監査と脅威の検出** |
| [監査と脅威検出を構成する](scripts/sql-database-auditing-and-threat-detection-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| この PowerShell スクリプトは、Azure SQL Database の監査と脅威検出ポリシーを構成します。 |
| **データベースを復元、コピー、インポートする**||
| [データベースを復元する](scripts/sql-database-restore-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| この PowerShell スクリプトは、geo 冗長バックアップから Azure SQL Database を復元し、削除された Azure SQL Database を最新のバックアップに復元します。 |
| [新しいサーバーにデータベースをコピーする](scripts/sql-database-copy-database-to-new-server-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| この PowerShell スクリプトは、新しい Azure SQL サーバーに既存の Azure SQL Database のコピーを作成します。 |
| [データベースを bacpac ファイルからインポートする](scripts/sql-database-import-from-bacpac-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| この PowerShell スクリプトは、bacpac ファイルから Azure SQL サーバーにデータベースをインポートします。 |
| **データベース間でデータを同期する**||
| [SQL Database 間でデータを同期する](scripts/sql-database-sync-data-between-sql-databases.md?toc=%2fpowershell%2fmodule%2ftoc.json) | この PowerShell スクリプトは、複数の Azure SQL Database 間で同期するデータ同期を構成します。 |
| [SQL Database とオンプレミスの SQL Server のデータを同期する](scripts/sql-database-sync-data-between-azure-onprem.md?toc=%2fpowershell%2fmodule%2ftoc.json) | この PowerShell スクリプトは、Azure SQL Database とオンプレミスの SQL Server データベースのデータ同期を構成します。 |
| [SQL データ同期の同期スキーマを更新する](scripts/sql-database-sync-update-schema.md?toc=%2fpowershell%2fmodule%2ftoc.json) | この PowerShell スクリプトは、データ同期の同期スキーマから項目を追加または削除します。 |
|||

[単一データベースの Azure PowerShell API](sql-database-single-databases-manage.md#powershell-manage-sql-database-servers-and-single-databases) に関するセクションを参照してください。

## <a name="managed-instance"></a>マネージド インスタンス

次の表には、Azure SQL Database Managed Instance の Azure PowerShell スクリプトのサンプルへのリンクが含まれています。

| |  |
|---|---|
|**マネージド インスタンスを作成して構成する**||
| [Managed Instance を作成し、管理する](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/27/quick-start-script-create-azure-sql-managed-instance-using-powershell/) | この PowerShell スクリプトでは、Azure PowerShell を使用してマネージド インスタンスを作成および管理する方法が示されます |
| [Azure Resource Manager テンプレート を使用してマネージド インスタンスを作成および管理する](scripts/sql-managed-instance-create-powershell-azure-resource-manager-template.md?toc=%2fpowershell%2fmodule%2ftoc.json) | この PowerShell スクリプトは、Azure PowerShell Azure Resource Manager テンプレートを使用してマネージド インスタンスを作成および管理する方法を示しています。|
| **透過的なデータ暗号化 (TDE) の構成**||
| [Azure Key Vault の独自のキーを使用してマネージド インスタンスで Transparent Data Encryption を管理する](scripts/transparent-data-encryption-byok-sql-managed-instance-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| この PowerShell スクリプトによって、Azure Key Vault のキーを使用し、Azure SQL マネージド インスタンスの Bring Your Own Key シナリオで Transparent Data Encryption (TDE) が構成されます。|
|||

[マネージド インスタンスの Azure PowerShell API](sql-database-managed-instance-create-manage.md#powershell-create-and-manage-managed-instances) に関するセクションを参照してください。
