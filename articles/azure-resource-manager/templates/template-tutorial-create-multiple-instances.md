---
title: Skapa flera resursinstanser
description: Lär dig hur du skapar en Azure Resource Manager-mall för att distribuera flera resurser.
author: mumian
ms.date: 04/23/2020
ms.topic: tutorial
ms.author: jgao
ms.custom: ''
ms.openlocfilehash: dac1e9429965607aea1490f0ec3b540d7b441d09
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91614401"
---
# <a name="tutorial-create-multiple-resource-instances-with-arm-templates"></a>Självstudie: skapa flera resurs instanser med ARM-mallar

Lär dig hur du itererar i din Azure Resource Manager-mall (ARM) för att skapa flera instanser av en Azure-resurs. I den här självstudien ändrar du en mall för att skapa tre lagringskontoinstanser.

![Skapa diagram med Azure Resource Manager flera instanser](./media/template-tutorial-create-multiple-instances/resource-manager-template-create-multiple-instances-diagram.png)

Den här självstudien omfattar följande uppgifter:

> [!div class="checklist"]
> * Öppna en snabbstartsmall
> * Redigera mallen
> * Distribuera mallen

Om du inte har en Azure-prenumeration kan du [skapa ett kostnadsfritt konto ](https://azure.microsoft.com/free/) innan du börjar.

## <a name="prerequisites"></a>Krav

För att kunna följa stegen i den här artikeln behöver du:

* Visual Studio Code med Resource Manager Tools-tillägg. Se [snabb start: skapa Azure Resource Manager mallar med Visual Studio Code](quickstart-create-templates-use-visual-studio-code.md).

## <a name="open-a-quickstart-template"></a>Öppna en snabbstartsmall

[Azure snabb starts mallar](https://azure.microsoft.com/resources/templates/) är en lagrings plats för ARM-mallar. I stället för att skapa en mall från början får du en exempelmall som du anpassar. Den mall som används i den här snabbstarten kallas [Create a standard storage account](https://azure.microsoft.com/resources/templates/101-storage-account-create/) (Skapa ett standardlagringskonto). Mallen definierar en Azure Storage-kontoresurs.

1. Från Visual Studio **Code väljer du** > **Öppna fil**.
2. I **Filnamn** klistrar du in följande URL:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```
3. Välj **Öppna** för att öppna filen.
4. Det finns en resurs med namnet ”Microsoft.Storage/storageAccounts” definierad i mallen. Jämför mallen med [mallreferensen](/azure/templates/Microsoft.Storage/storageAccounts). Det är bra att få viss grundläggande förståelse av mallen innan den anpassas.
5. Välj **Arkiv** > **Spara som** för att spara filen som **azuredeploy.jspå** den lokala datorn.

## <a name="edit-the-template"></a>Redigera mallen

Den befintliga mallen skapar ett lagringskonto. Du kan anpassa mallen för att skapa tre lagringskonton.

Från Visual Studio Code gör du följande fyra ändringar:

![Azure Resource Manager skapar flera instanser](./media/template-tutorial-create-multiple-instances/resource-manager-template-create-multiple-instances.png)

1. Lägg till elementet `copy` i resursdefinitionen för lagringskontot. I kopieringselementet anger du antalet iterationer och en variabel för den här loopen. Värdet för antal måste vara ett positivt heltal och får inte överskrida 800.
2. Funktionen `copyIndex()` returnerar den aktuella iterationen i loopen. Du kan använda indexet som namnprefix. `copyIndex()` är nollbaserat. Om du vill åsidosätta indexvärdet kan du skicka ett värde i funktionen copyIndex(). Till exempel *copyIndex(1)*.
3. Ta bort elementet **variabler** eftersom det inte används längre.
4. Ta bort element **Utdata**. Det behövs inte längre.

Den färdiga mallen ser ut så här:

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
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2019-04-01",
      "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
      "location": "[parameters('location')]",
      "sku": {
        "name": "[parameters('storageAccountType')]"
      },
      "kind": "StorageV2",
      "copy": {
        "name": "storagecopy",
        "count": 3
      },
      "properties": {}
    }
  ]
}
```

Mer information om hur du skapar flera instanser finns i [distribuera flera instanser av en resurs eller egenskap i arm-mallar](./copy-resources.md)

## <a name="deploy-the-template"></a>Distribuera mallen

1. Logga in på [Azure Cloud Shell](https://shell.azure.com)

1. Välj önskad miljö genom att välja antingen **PowerShell** eller **bash** (för CLI) i det övre vänstra hörnet.  Du måste starta om gränssnittet när du byter.

    ![Azure Portal Cloud Shell Ladda upp fil](./media/template-tutorial-use-template-reference/azure-portal-cloud-shell-upload-file.png)

1. Välj **Ladda upp/ned filer** och välj sedan **Ladda upp**. Se föregående skärmbild. Välj den fil som du sparade i föregående avsnitt. När du har överfört filen kan du använda kommandot **ls** och kommandot **Cat** för att kontrol lera att filen har laddats upp.

1. Kör följande kommandon från Cloud Shell. Välj fliken så att du ser PowerShell-koden eller CLI-koden.

    # <a name="cli"></a>[CLI](#tab/CLI)

    ```azurecli
    echo "Enter a project name that is used to generate resource group name:" &&
    read projectName &&
    echo "Enter the location (i.e. centralus):" &&
    read location &&
    resourceGroupName="${projectName}rg" &&
    az group create --name $resourceGroupName --location "$location" &&
    az deployment group create --resource-group $resourceGroupName --template-file "$HOME/azuredeploy.json"
    ```

    # <a name="powershell"></a>[PowerShell](#tab/PowerShell)

    ```azurepowershell
    $projectName = Read-Host -Prompt "Enter a project name that is used to generate resource group name"
    $location = Read-Host -Prompt "Enter the location (i.e. centralus)"
    $resourceGroupName = "${projectName}rg"

    New-AzResourceGroup -Name $resourceGroupName -Location "$location"
    New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile "$HOME/azuredeploy.json"
    ```

    ---

Om du vill visa alla tre lagringskonton utelämnar du parametern --name:

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
echo "Enter a project name that is used to generate resource group name:" &&
read projectName &&
resourceGroupName="${projectName}rg" &&
az storage account list --resource-group $resourceGroupName &&
echo "Press [ENTER] to continue ..."
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
$projectName = Read-Host -Prompt "Enter a project name that is used to generate resource group name"
$resourceGroupName = "${projectName}rg"

Get-AzStorageAccount -ResourceGroupName $resourceGroupName
Write-Host "Press [ENTER] to continue ..."
```

---

Jämför lagringskontonamn med namndefinitionen i mallen.

## <a name="clean-up-resources"></a>Rensa resurser

När Azure-resurserna inte längre behövs rensar du de resurser som du har distribuerat genom att ta bort resursgruppen.

1. Från Azure Portal väljer du **resurs grupp** på den vänstra menyn.
2. Ange resursgruppens namn i fältet **Filtrera efter namn**.
3. Välj resursgruppens namn.  Du får se totalt tre resurser i resurs gruppen.
4. Välj **ta bort resurs grupp** på den översta menyn.

## <a name="next-steps"></a>Nästa steg

I de här självstudierna lärde du dig att skapa flera instanser av ett lagringskonto.  I nästa självstudie utvecklar du en mall med flera resurser och flera resurstyper. Några av resurserna har beroende resurser.

> [!div class="nextstepaction"]
> [Skapa beroende resurser](./template-tutorial-create-templates-with-dependent-resources.md)
