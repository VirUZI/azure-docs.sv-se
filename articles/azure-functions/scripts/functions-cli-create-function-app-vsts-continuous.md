---
title: Skapa en Function-app med DevOps-distribution – Azure CLI
description: Skapa en funktionsapp och distribuera funktionskod från Azure DevOps
ms.date: 07/03/2018
ms.topic: sample
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: 25e0f43a851bbf7658847aa52cc9ee778941f718
ms.sourcegitcommit: 04fb3a2b272d4bbc43de5b4dbceda9d4c9701310
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/12/2020
ms.locfileid: "94565219"
---
# <a name="create-a-function-in-azure-that-is-deployed-from-azure-devops"></a>Skapa en funktion i Azure som distribueras från Azure DevOps

Det här avsnittet visar hur du använder Azure Functions för att skapa en [Server](https://azure.microsoft.com/solutions/serverless/) lös Function-app med hjälp av [förbruknings planen](../functions-scale.md#consumption-plan). Function-appen, som är en behållare för dina funktioner, distribueras kontinuerligt från en Azure DevOps-lagringsplats. 

För att slutföra det här avsnittet måste du ha följande:

* En Azure DevOps-lagringsplats som innehåller ditt funktionsappsprojekt och där du har administratörsbehörighet.
* En [personlig åtkomsttoken (PAT)](/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate) till Azure DevOps-lagringsplatsen.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

 - I den här självstudien krävs version 2,0 eller senare av Azure CLI. Om du använder Azure Cloud Shell är den senaste versionen redan installerad. 

## <a name="sample-script"></a>Exempelskript

Det här exemplet skapar en funktionsapp och distribuerar funktionskod från Azure DevOps.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-vsts/deploy-function-app-with-function-vsts.sh?highlight=3-4 "Azure Service")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Förklaring av skript

I det här skriptet används följande kommandon för att skapa en resursgrupp, ett lagringskonto, en funktionsapp och alla relaterade resurser. Varje kommando i tabellen länkar till kommandospecifik dokumentation.

| Kommando | Kommentarer |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Skapar en resursgrupp där alla resurser lagras. |
| [az storage account create](/cli/azure/storage/account#az-storage-account-create) | Skapar lagringskontot för funktionsappen. |
| [az functionapp create](/cli/azure/functionapp#az-functionapp-create) | Skapar en Function-app i [förbruknings planen](../functions-scale.md#consumption-plan)utan server. |
| [az functionapp deployment source config](/cli/azure/functionapp/deployment/source#az-functionapp-deployment-source-config) | Kopplar en funktionsapp till en Git- eller Mercurial-lagringsplats. |

## <a name="next-steps"></a>Nästa steg

Mer information om Azure CLI finns i [Azure CLI-dokumentationen](/cli/azure).

Ytterligare CLI-skriptexempel för Azure Functions finns i [Azure Functions-dokumentationen](../functions-cli-samples.md).
