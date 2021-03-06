---
title: Azure リソースの複数インスタンスのデプロイ | Microsoft Docs
description: Azure リソース マネージャー テンプレートで copy 操作と配列を使用して、リソースをデプロイする際に複数回反復処理する方法について説明します。
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
editor: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/15/2019
ms.author: tomfitz
ms.openlocfilehash: c343dfa3c0eac4aeabaa9244c6675b235fc95552
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/15/2019
ms.locfileid: "56311718"
---
# <a name="deploy-more-than-one-instance-of-a-resource-or-property-in-azure-resource-manager-templates"></a>Azure Resource Manager テンプレートでリソースまたはプロパティの複数のインスタンスをデプロイする

この記事では、リソースの複数のインスタンスを作成するために Azure Resource Manager テンプレートで反復処理する方法について説明します。 リソースをデプロイするかどうかを指定する必要がある場合は、[condition 要素](resource-manager-templates-resources.md#condition)に関する記述を参照してください。

チュートリアルについては、「[Resource Manager テンプレートを使用した複数のリソース インスタンスの作成](./resource-manager-tutorial-create-multiple-instances.md)」を参照してください。

## <a name="resource-iteration"></a>リソースの反復

デプロイ時に、リソースのインスタンスを 1 つまたは複数作成することを決定する必要がある場合は、リソースの種類に `copy` 要素を追加します。 copy 要素には、そのループの反復回数と名前を指定します。 count 値は正の整数である必要がありますが、800 を超えることはできません。 

複数回作成するリソースは、次の形式を取ります。

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": {},
      "copy": {
        "name": "storagecopy",
        "count": 3
      }
    }
  ],
  "outputs": {}
}
```

各リソースの名前には、現在のループの反復を返す `copyIndex()` 関数が含まれます。 `copyIndex()` は 0 から始まります。 次の例を見てください。

```json
"name": "[concat('storage', copyIndex())]",
```

この場合、以下の名前が作成されます。

* storage0
* storage1
* storage2

インデックス値をオフセットするには、copyIndex() 関数に値を渡します。 実行する反復処理の数は copy 要素で指定されたままですが、copyIndex の値が指定された値でオフセットされます。 次の例を見てください。

```json
"name": "[concat('storage', copyIndex(1))]",
```

この場合、以下の名前が作成されます。

* storage1
* storage2
* storage3

コピー操作は、配列内の各要素に対して反復処理するため、配列で作業するときに便利です。 配列で反復回数を指定するには `length` 関数を使います。また、配列における現在のインデックスを取得するには `copyIndex` を使います。 次の例を見てください。

```json
"parameters": { 
  "org": { 
    "type": "array", 
    "defaultValue": [ 
      "contoso", 
      "fabrikam", 
      "coho" 
    ] 
  }
}, 
"resources": [ 
  { 
    "name": "[concat('storage', parameters('org')[copyIndex()])]", 
    "copy": { 
      "name": "storagecopy", 
      "count": "[length(parameters('org'))]" 
    }, 
    ...
  } 
]
```

この場合、以下の名前が作成されます。

* storagecontoso
* storagefabrikam
* storagecoho

既定では、リソース マネージャーは並列でリソースを作成します。 それらが作成される順序は保証されません。 しかし、リソースが順番にデプロイされるように指定したい場合もあります。 たとえば、運用環境を更新するとき、一度に特定の数だけ更新されるように更新時間をずらす必要がある場合があります。

リソースの複数のインスタンスを連続的にデプロイするには、`mode` を **serial** に、`batchSize` を一度にデプロイするインスタンスの数に設定します。 シリアル モードでは、Resource Manager はループ内で前のインスタンスへの依存関係を作成するので、前のバッチが完了するまで次のバッチは実行されません。

たとえば、ストレージ アカウントを一度に 2 つずつ、逐次的にデプロイするには、次のコマンドを使用します。

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": {},
      "copy": {
        "name": "storagecopy",
        "count": 4,
        "mode": "serial",
        "batchSize": 2
      }
    }
  ],
  "outputs": {}
}
```

mode プロパティでも **parallel** が既定値として使用されます。

## <a name="property-iteration"></a>プロパティの反復処理

リソース上のプロパティの複数の値を作成するには、プロパティ要素に `copy` 配列を追加します。 この配列にはオブジェクトが含まれていて、各オブジェクトには次のプロパティがあります。

* name - 複数の値を作成するプロパティの名前
* count - 作成する値の数。 count 値は正の整数である必要がありますが、800 を超えることはできません。
* input - プロパティに割り当てる値を格納するオブジェクト  

次の例は、仮想マシンで dataDisks プロパティに `copy` を適用する方法を示しています。

```json
{
  "name": "examplevm",
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2017-03-30",
  "properties": {
    "storageProfile": {
      "copy": [{
        "name": "dataDisks",
        "count": 3,
        "input": {
          "lun": "[copyIndex('dataDisks')]",
          "createOption": "Empty",
          "diskSizeGB": "1023"
        }
      }],
      ...
```

プロパティの反復処理内で `copyIndex` を使用する場合、反復処理の名前を指定する必要があります。 リソースの反復処理で使用する場合には名前を指定する必要はありません。

Resource Manager はデプロイ中に `copy` 配列を展開します。 配列の名前がプロパティの名前になります。 入力値がオブジェクトのプロパティになります。 デプロイされたテンプレートは次のようになります。

```json
{
  "name": "examplevm",
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2017-03-30",
  "properties": {
    "storageProfile": {
      "dataDisks": [
        {
          "lun": 0,
          "createOption": "Empty",
          "diskSizeGB": "1023"
        },
        {
          "lun": 1,
          "createOption": "Empty",
          "diskSizeGB": "1023"
        },
        {
          "lun": 2,
          "createOption": "Empty",
          "diskSizeGB": "1023"
        }
      ],
      ...
```

copy 要素は配列であるため、リソースの複数のプロパティを指定できます。 作成するプロパティごとにオブジェクトを追加します。

```json
{
  "name": "string",
  "type": "Microsoft.Network/loadBalancers",
  "apiVersion": "2017-10-01",
  "properties": {
    "copy": [
      {
        "name": "loadBalancingRules",
        "count": "[length(parameters('loadBalancingRules'))]",
        "input": {
          ...
        }
      },
      {
        "name": "probes",
        "count": "[length(parameters('loadBalancingRules'))]",
        "input": {
          ...
        }
      }
    ]
  }
}
```

リソースの反復処理とプロパティの反復処理は同時に使用できます。 プロパティの反復処理を名前で参照します。

```json
{
  "type": "Microsoft.Network/virtualNetworks",
  "name": "[concat(parameters('vnetname'), copyIndex())]",
  "apiVersion": "2018-04-01",
  "copy":{
    "count": 2,
    "name": "vnetloop"
  },
  "location": "[resourceGroup().location]",
  "properties": {
    "addressSpace": {
      "addressPrefixes": [
        "[parameters('addressPrefix')]"
      ]
    },
    "copy": [
      {
        "name": "subnets",
        "count": 2,
        "input": {
          "name": "[concat('subnet-', copyIndex('subnets'))]",
          "properties": {
            "addressPrefix": "[variables('subnetAddressPrefix')[copyIndex('subnets')]]"
          }
        }
      }
    ]
  }
}
```

## <a name="variable-iteration"></a>変数の反復処理

変数のインスタンスを複数作成するには、variables セクション内で `copy` プロパティを使用します。 `input` プロパティの値から構築された要素の配列を作成します。 `copy` プロパティは、変数内で使用することも、variables セクションの最上位レベルで使用することもできます。 変数の反復処理内で `copyIndex` を使用するときは、反復処理の名前を指定する必要があります。

次の例は、動的に構築された要素を持つ配列変数のさまざまな作成方法を示しています。 これを見ると、変数内で copy を使用して、オブジェクトと文字列の配列を作成する方法がわかります。 また、最上位レベルで copy を使用して、オブジェクト、文字列、および整数の配列を作成する方法もわかります。

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {
    "disk-array-on-object": {
      "copy": [
        {
          "name": "disks",
          "count": 5,
          "input": {
            "name": "[concat('myDataDisk', copyIndex('disks', 1))]",
            "diskSizeGB": "1",
            "diskIndex": "[copyIndex('disks')]"
          }
        },
        {
          "name": "diskNames",
          "count": 5,
          "input": "[concat('myDataDisk', copyIndex('diskNames', 1))]"
        }
      ]
    },
    "copy": [
      {
        "name": "top-level-object-array",
        "count": 5,
        "input": {
          "name": "[concat('myDataDisk', copyIndex('top-level-object-array', 1))]",
          "diskSizeGB": "1",
          "diskIndex": "[copyIndex('top-level-object-array')]"
        }
      },
      {
        "name": "top-level-string-array",
        "count": 5,
        "input": "[concat('myDataDisk', copyIndex('top-level-string-array', 1))]"
      },
      {
        "name": "top-level-integer-array",
        "count": 5,
        "input": "[copyIndex('top-level-integer-array')]"
      }
    ]
  },
  "resources": [],
  "outputs": {
    "exampleObject": {
      "value": "[variables('disk-array-on-object')]",
      "type": "object"
    },
    "exampleArrayOnObject": {
      "value": "[variables('disk-array-on-object').disks]",
      "type" : "array"
    },
    "exampleObjectArray": {
      "value": "[variables('top-level-object-array')]",
      "type" : "array"
    },
    "exampleStringArray": {
      "value": "[variables('top-level-string-array')]",
      "type" : "array"
    },
    "exampleIntegerArray": {
      "value": "[variables('top-level-integer-array')]",
      "type" : "array"
    }
  }
}
```

## <a name="depend-on-resources-in-a-loop"></a>ループ内のリソースへの依存

`dependsOn` 要素を使用することで、リソースを別のリソースの後にデプロイするよう指定することが可能です。 ループ内のリソースの集合に依存するリソースをデプロイするには、dependsOn 要素にコピー ループの名前を指定します。 次の例では、仮想マシンをデプロイする前に 3 つのストレージ アカウントをデプロイする方法を示します。 完全な仮想マシン定義は示されていません。 コピー要素の name が `storagecopy` に設定され、Virtual Machines の dependsOn 要素が `storagecopy` に設定されるよう注意してください。

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": {},
      "copy": {
        "name": "storagecopy",
        "count": 3
      }
    },
    {
      "apiVersion": "2015-06-15", 
      "type": "Microsoft.Compute/virtualMachines", 
      "name": "[concat('VM', uniqueString(resourceGroup().id))]",  
      "dependsOn": ["storagecopy"],
      ...
    }
  ],
  "outputs": {}
}
```

<a id="looping-on-a-nested-resource" />

## <a name="iteration-for-a-child-resource"></a>子リソースの反復処理
子リソースにコピー ループを使用することはできません。 通常は別のリソース内で入れ子になっているように定義するリソースの複数のインスタンスを作成するには、代わりに、そのリソースを最上位のリソースとして作成する必要があります。 type および name の各プロパティを使用して、親リソースとの関係を定義します。

たとえば、通常はデータ ファクトリ内の子リソースとしてデータセットを定義するとします。

```json
"resources": [
{
  "type": "Microsoft.DataFactory/datafactories",
  "name": "exampleDataFactory",
  ...
  "resources": [
    {
      "type": "datasets",
      "name": "exampleDataSet",
      "dependsOn": [
        "exampleDataFactory"
      ],
      ...
    }
  ]
```

複数のデータ セットを作成するには、それをデータ ファクトリの外部に移動します。 データセットは、データ ファクトリと同じレベルである必要がありますが、今までどおりデータ ファクトリの子リソースです。 type および name の各プロパティを使用して、データセットとデータ ファクトリの関係を保存します。 テンプレート内の位置から type を推論できなくなったため、`{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}` の形式で完全修飾型を指定する必要があります。

データ ファクトリのインスタンスを使用して親子関係を確立するには、親リソースの名前を含むデータ セットの名前を指定します。 `{parent-resource-name}/{child-resource-name}` の形式で入力します。  

次の例は、実装を示します。

```json
"resources": [
{
  "type": "Microsoft.DataFactory/datafactories",
  "name": "exampleDataFactory",
  ...
},
{
  "type": "Microsoft.DataFactory/datafactories/datasets",
  "name": "[concat('exampleDataFactory', '/', 'exampleDataSet', copyIndex())]",
  "dependsOn": [
    "exampleDataFactory"
  ],
  "copy": {
    "name": "datasetcopy",
    "count": "3"
  },
  ...
}]
```

## <a name="example-templates"></a>サンプル テンプレート

次の例は、リソースまたはプロパティの複数のインスタンスを作成するための一般的なシナリオを示しています。

|Template  |説明  |
|---------|---------|
|[Copy storage](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copystorage.json) |名前にインデックス番号を含む複数のストレージ アカウントをデプロイします。 |
|[Serial copy storage](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/serialcopystorage.json) |複数のストレージ アカウントを一度に 1 つずつデプロイします。 名前にはインデックス番号が含まれます。 |
|[Copy storage with array](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copystoragewitharray.json) |複数のストレージ アカウントをデプロイします。 名前には、配列からの値が含まれます。 |
|[VM deployment with a variable number of data disks](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-windows-copy-datadisks) |仮想マシンと共に複数のデータ ディスクをデプロイします。 |
|[Copy variables](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copyvariables.json) |変数を反復処理する各種の方法を示します。 |
|[Multiple security rules](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.json) |ネットワーク セキュリティ グループに複数のセキュリティ規則をデプロイします。 セキュリティ規則はパラメーターから構築されます。 パラメーターについては、[複数の NSG パラメーター ファイル](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.parameters.json)に関するページを参照してください。 |

## <a name="next-steps"></a>次の手順

* チュートリアルについては、「[チュートリアル: Resource Manager テンプレートを使用した複数のリソース インスタンスの作成](./resource-manager-tutorial-create-multiple-instances.md)」を参照してください。

* テンプレートのセクションについては、「[Azure Resource Manager のテンプレートの作成](resource-group-authoring-templates.md)」を参照してください。
* テンプレートをデプロイする方法については、「 [Azure リソース マネージャーのテンプレートを使用したアプリケーションのデプロイ](resource-group-template-deploy.md)」を参照してください。

