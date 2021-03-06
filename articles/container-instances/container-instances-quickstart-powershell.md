---
title: クイック スタート - Azure Container Instances でアプリケーションを実行する - PowerShell
description: このクイック スタートでは、Azure PowerShell を使用して、Docker コンテナー アプリケーションを Azure Container Instances にデプロイします
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: quickstart
ms.date: 10/02/2018
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: b8cb84523288f45dfb719d69e4f7d227039598a9
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/25/2019
ms.locfileid: "56806914"
---
# <a name="quickstart-run-a-container-application-in-azure-container-instances-with-azure-powershell"></a>クイック スタート: Azure PowerShell を使って Azure Container Instances でコンテナー アプリケーションを実行する

Docker コンテナーを Azure で簡単にすばやく実行するには、Azure Container Instances を使用します。 仮想マシンをデプロイしたり、Kubernetes などの完全なコンテナー オーケストレーション プラットフォームを使用したりする必要はありません。 このクイック スタートでは、Azure portal を使用して Azure で Windows コンテナーを作成し、そのアプリケーションを完全修飾ドメイン名 (FQDN) を介して使用できるようにします。 1 つのデプロイ コマンドを実行して数秒後には、実行中のアプリケーションを閲覧できます。

![Azure Container Instances にデプロイされたアプリのブラウザーでの表示][qs-powershell-01]

Azure サブスクリプションをお持ちでない場合は、開始する前に [無料アカウント](https://azure.microsoft.com/free/) を作成してください。

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

PowerShell をローカル環境にインストールして使用する場合、このチュートリアルでは Azure PowerShell モジュールが必要です。 バージョンを確認するには、`Get-Module -ListAvailable Az` を実行します。 アップグレードする必要がある場合は、[Azure PowerShell モジュールのインストール](/powershell/azure/install-Az-ps)に関するページを参照してください。 PowerShell をローカルで実行している場合、`Connect-AzAccount` を実行して Azure との接続を作成することも必要です。

## <a name="create-a-resource-group"></a>リソース グループの作成

Azure のコンテナー インスタンスは、すべての Azure リソースと同様に、リソース グループにデプロイする必要があります。 リソース グループを使用すると、関連する Azure リソースを整理して管理できます。

まず、次の [New-AzResourceGroup][New-AzResourceGroup] コマンドを使用して、*myResourceGroup* という名前のリソース グループを *eastus* の場所に作成します。

 ```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-container"></a>コンテナーを作成する

リソース グループを作成すると、Azure でコンテナーを実行できます。 Azure PowerShell を使用してコンテナー インスタンスを作成するには、リソース グループ名、コンテナー インスタンス名、および Docker コンテナー イメージを [New-AzContainerGroup][New-AzContainerGroup] コマンドレットに渡します。 このクイック スタートでは、パブリック Docker Hub レジストリにある `microsoft/iis:nanoserver` Windows イメージを使用します。 このイメージには、Nano Server で動作するインターネット インフォメーション サービス (IIS) がパッケージされています。

1 つまたは複数の開くポート、DNS 名ラベル、またはその両方を指定することで、コンテナーをインターネットに公開することができます。 このクイック スタートでは、IIS にパブリックに到達できるよう、DNS 名ラベルを指定してコンテナーをデプロイします。

次のコマンドを実行して、コンテナー インスタンスを開始します。 `-DnsNameLabel` の値は、インスタンスを作成する Azure リージョン内で一意である必要があります。 エラー メッセージ "DNS 名ラベルは利用できません" が表示された場合は、別の DNS 名ラベルを試してください。

 ```azurepowershell-interactive
New-AzContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer -Image microsoft/iis:nanoserver -OsType Windows -DnsNameLabel aci-demo-win
```

数秒以内に、Azure から応答を受信します。 コンテナーの `ProvisioningState` は、最初は **Creating** と表示されますが、1、2 分で **Succeeded** に変わります。 [Get-AzContainerGroup][Get-AzContainerGroup] コマンドレットを使用して、デプロイの状態を確認します。

 ```azurepowershell-interactive
Get-AzContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

コンテナーのプロビジョニング状態、完全修飾ドメイン名 (FQDN)、IP アドレスがコマンドレットの出力に表示されます。

```console
PS Azure:\> Get-AzContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer


ResourceGroupName        : myResourceGroup
Id                       : /subscriptions/<Subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ContainerInstance/containerGroups/mycontainer
Name                     : mycontainer
Type                     : Microsoft.ContainerInstance/containerGroups
Location                 : eastus
Tags                     :
ProvisioningState        : Creating
Containers               : {mycontainer}
ImageRegistryCredentials :
RestartPolicy            : Always
IpAddress                : 52.226.19.87
DnsNameLabel             : aci-demo-win
Fqdn                     : aci-demo-win.eastus.azurecontainer.io
Ports                    : {80}
OsType                   : Windows
Volumes                  :
State                    : Pending
Events                   : {}
```

コンテナーの `ProvisioningState` が **Succeeded** になったら、ブラウザーでその `Fqdn` に移動します。 次のような Web ページが表示されたら成功です。 Docker コンテナーで実行されているアプリケーションが Azure に正常にデプロイされました。

![ブラウザーに表示された、Azure Container Instances を使用してデプロイされた IIS][qs-powershell-01]

## <a name="clean-up-resources"></a>リソースのクリーンアップ

コンテナーを使い終えたら、[Remove-AzContainerGroup][Remove-AzContainerGroup] コマンドレットを使用してそのコンテナーを削除します。

 ```azurepowershell-interactive
Remove-AzContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

## <a name="next-steps"></a>次の手順

このクイック スタートでは、パブリック Docker Hub レジストリ内のイメージから Azure コンテナー インスタンスを作成しました。 コンテナー イメージをビルドし、プライベート Azure コンテナー レジストリからデプロイする場合は、Azure Container Instances のチュートリアルに進んでください。

> [!div class="nextstepaction"]
> [Azure Container Instances のチュートリアル](./container-instances-tutorial-prepare-app.md)

<!-- IMAGES -->
[qs-powershell-01]: ./media/container-instances-quickstart-powershell/qs-powershell-01.png

<!-- LINKS -->
[New-AzResourceGroup]: /powershell/module/az.resources/new-Azresourcegroup
[New-AzContainerGroup]: /powershell/module/az.containerinstance/new-Azcontainergroup
[Get-AzContainerGroup]: /powershell/module/az.containerinstance/get-Azcontainergroup
[Remove-AzContainerGroup]: /powershell/module/az.containerinstance/remove-Azcontainergroup
