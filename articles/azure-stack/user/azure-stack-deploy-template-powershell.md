---
title: Azure Stack で PowerShell を使用してテンプレートをデプロイする | Microsoft Docs
description: PowerShell を使用して Azure Stack にテンプレートをデプロイします。
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 12fe32d7-0a1a-4c02-835d-7b97f151ed0f
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/04/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: 7af06347dd1ca5a3e7e27e5db4849b5540b106b9
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2019
ms.locfileid: "55250012"
---
# <a name="deploy-a-template-to-azure-stack-using-powershell"></a>PowerShell を使用して Azure Stack にテンプレートをデプロイする

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

PowerShell を使用して、Azure Resource Manager テンプレートを Azure Stack にデプロイできます。 この記事では、PowerShell を使用してテンプレートをデプロイする方法について説明します。

## <a name="run-azurerm-powershell-cmdlets"></a>Azure PowerShell コマンドレットの実行

この例では、**AzureRM** PowerShell コマンドレットと GitHub に格納されているテンプレートを使用します。 テンプレートにより、Windows Server 2012 R2 Datacenter の仮想マシンが作成されます。

>[!NOTE]
>この例を試す前に、Azure Stack ユーザーに [PowerShell が構成されている](azure-stack-powershell-configure-user.md)ことを確認します。

1. [https://aka.ms/AzureStackGitHub](https://aka.ms/AzureStackGitHub) に移動し、**101-simple-windows-vm** テンプレートを見つけます。 テンプレートを `C:\templates\azuredeploy-101-simple-windows-vm.json` に保存します。
2. 管理者特権の PowerShell コマンド プロンプトを開きます。
3. 次のスクリプト内の `username` と `password` を自分のユーザー名とパスワードに置き換えてから、スクリプトを実行します。

    ```PowerShell
    # Set deployment variables
    $myNum = "001" # Modify this per deployment
    $RGName = "myRG$myNum"
    $myLocation = "local"
   
    # Create resource group for template deployment
    New-AzureRmResourceGroup -Name $RGName -Location $myLocation
   
    # Deploy simple IaaS template
    New-AzureRmResourceGroupDeployment `
        -Name myDeployment$myNum `
        -ResourceGroupName $RGName `
        -TemplateFile c:\templates\azuredeploy-101-simple-windows-vm.json `
        -NewStorageAccountName mystorage$myNum `
        -DnsNameForPublicIP mydns$myNum `
        -AdminUsername <username> `
        -AdminPassword ("<password>" | ConvertTo-SecureString -AsPlainText -Force) `
        -VmName myVM$myNum `
        -WindowsOSVersion 2012-R2-Datacenter
    ```

    >[!IMPORTANT]
    >このスクリプトを実行するたびに、`$myNum` パラメーターの値をインクリメントし、デプロイの上書きを防ぎます。

4. Azure Stack ポータルを開き、**[参照]**、**[仮想マシン]** の順に選択して、新しい仮想マシン (**myDeployment001**) を探します。

## <a name="next-steps"></a>次の手順

- [Visual Studio を使用したテンプレートのデプロイ](azure-stack-deploy-template-visual-studio.md)
