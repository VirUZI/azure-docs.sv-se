---
title: Så här hittar du din Azure-prenumeration
description: Hitta din Azure-prenumeration så att du kan konfigurera din miljö.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 08/31/2020
ms.author: inhenkel
ms.custom: cli,portal, devx-track-azurecli
ms.openlocfilehash: 07088ebfa5fde4c3c790bd554c1eba8d6844b7d1
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/27/2020
ms.locfileid: "92736065"
---
# <a name="find-your-azure-subscription"></a>Hitta din Azure-prenumeration

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="portal"></a>[Portal](#tab/portal/)

## <a name="use-the-azure-portal"></a>Använda Azure-portalen

1. Logga in på [Azure-portalen](https://portal.azure.com).
1. Under rubriken Azure-tjänster väljer du prenumerationer. (Om inga prenumerationer visas kan du behöva byta Azure AD-klienter.) Dina prenumerations-ID: n visas i den andra kolumnen.
1. Kopiera prenumerations-ID och klistra in det i ett text dokument som du väljer att använda senare.

## <a name="cli"></a>[CLI](#tab/cli/)

## <a name="use-the-azure-cli"></a>Använda Azure CLI

<!-- NOTE: The following are in the includes file and are reused in other How To articles. All task based content should be in the includes folder with the "task-" prepended to the file name. -->

### <a name="list-your-azure-subscriptions-with-cli"></a>Lista dina Azure-prenumerationer med CLI

[!INCLUDE [List your Azure subscriptions with CLI](./includes/task-list-set-subscription-cli.md)]

### <a name="see-also"></a>Se även

* [Azure CLI](/cli/azure/ams?view=azure-cli-latest)

---

## <a name="next-steps"></a>Nästa steg

[Strömma en fil](stream-files-dotnet-quickstart.md)
