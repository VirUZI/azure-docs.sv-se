---
title: Azure CLI-skriptexempel – Begränsa webbtrafik | Microsoft Docs
description: Azure CLI-skriptexempel – Skapa en programgateway med en brandvägg för webbaserade program och en VM-skalningsuppsättning som använder OWASP-regler till att begränsa trafiken.
services: application-gateway
documentationcenter: networking
author: vhorne
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 01/29/2018
ms.author: victorh
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: 39750465008be586c58d9da309ffe435b2030c59
ms.sourcegitcommit: 0ce1ccdb34ad60321a647c691b0cff3b9d7a39c8
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/05/2020
ms.locfileid: "93397025"
---
# <a name="restrict-web-traffic-using-the-azure-cli"></a>Begränsa webbtrafik med hjälp av Azure CLI

Med det här skriptet skapar du en programgateway med en brandvägg för webbaserade program som använder en VM-skalningsuppsättning för serverdelen. Brandväggen för webbaserade program begränsar webbtrafiken baserat på OWASP-regler. När du har kört skriptet kan du testa programgatewayen med hjälp av dess offentliga IP-adress.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exempelskript

[!code-azurecli-interactive[main](../../../cli_scripts/application-gateway/create-vmss/create-vmss-waf.sh "Create application gateway")]

## <a name="clean-up-deployment"></a>Rensa distribution 

Kör följande kommando för att ta bort resursgruppen, programgatewayen och alla relaterade resurser.

```azurecli-interactive 
az group delete --name myResourceGroupAG --yes
```

## <a name="script-explanation"></a>Förklaring av skript

Det här skriptet använder följande kommandon för att skapa distributionen. Varje post i tabellen länkar till kommandospecifik dokumentation.

| Kommando | Kommentarer |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Skapar en resursgrupp där alla resurser lagras. |
| [az network vnet create](/cli/azure/network/vnet#az-network-vnet-create) | Skapar ett virtuellt nätverk. |
| [az network vnet subnet create](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-create) | Skapar ett undernät i ett virtuellt nätverk. |
| [az network public-ip create](/cli/azure/network/public-ip?view=azure-cli-latest) | Skapar den offentliga IP-adressen för programgatewayen. |
| [az network application-gateway create](/cli/azure/network/application-gateway?view=azure-cli-latest) | Skapar en programgateway. |
| [az vmss create](/cli/azure/vmss#az-vmss-create) | Skapar en VM-skalningsuppsättning. |
| [az storage account create](/cli/azure/storage/account#az-storage-account-create) | Skapar ett lagringskonto. |
| [az monitor diagnostic-settings create](/cli/azure/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create) | Skapar ett lagringskonto. |
| [az network public-ip show](/cli/azure/network/public-ip#az-network-public-ip-show) | Hämtar programgatewayens offentliga IP-adress. |

## <a name="next-steps"></a>Nästa steg

Mer information om Azure CLI finns i [Azure CLI-dokumentationen](/cli/azure/overview).

Du hittar fler CLI-skriptexempel för programgatewayer i [dokumentationen för Azure Application Gateway](../cli-samples.md).