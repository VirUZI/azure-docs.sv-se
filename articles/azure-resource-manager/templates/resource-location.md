---
title: Mallens resurs plats
description: Beskriver hur du anger resurs plats i en Azure Resource Manager mall.
ms.topic: conceptual
ms.date: 09/04/2019
ms.custom: ''
ms.openlocfilehash: e1310c81d7b726e9be2fe9f38a0bb8d701035ba4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91613704"
---
# <a name="set-resource-location-in-arm-template"></a>Ange resurs plats i ARM-mallen

När du distribuerar en Azure Resource Manager-mall (ARM) måste du ange en plats för varje resurs. Platsen behöver inte vara samma plats som resurs gruppens plats.

## <a name="get-available-locations"></a>Hämta tillgängliga platser

Olika resurs typer stöds på olika platser. Använd Azure PowerShell eller Azure CLI för att hämta de platser som stöds för en resurs typ.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
((Get-AzResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes `
  | Where-Object ResourceTypeName -eq batchAccounts).Locations
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az provider show \
  --namespace Microsoft.Batch \
  --query "resourceTypes[?resourceType=='batchAccounts'].locations | [0]" \
  --out table
```

---

## <a name="use-location-parameter"></a>Använd plats parameter

För att ge flexibilitet när du distribuerar din mall använder du en parameter för att ange plats för resurser. Ange standardvärdet för parametern till `resourceGroup().location` .

I följande exempel visas ett lagrings konto som har distribuerats till en plats som anges som en parameter:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "Storage Account type"
      }
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]",
      "metadata": {
        "description": "Location for all resources."
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat('storage', uniquestring(resourceGroup().id))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2018-07-01",
      "name": "[variables('storageAccountName')]",
      "location": "[parameters('location')]",
      "sku": {
        "name": "[parameters('storageAccountType')]"
      },
      "kind": "StorageV2",
      "properties": {}
    }
  ],
  "outputs": {
    "storageAccountName": {
      "type": "string",
      "value": "[variables('storageAccountName')]"
    }
  }
}
```

## <a name="next-steps"></a>Nästa steg

* En fullständig lista över mall funktioner finns i [Azure Resource Manager Template Functions](template-functions.md).
* Mer information om mallfiler finns i [förstå strukturen och syntaxen för ARM-mallar](template-syntax.md).
