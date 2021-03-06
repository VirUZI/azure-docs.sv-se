---
title: Roll hantering för hanterad HSM-dataplan – Azure Key Vault | Microsoft Docs
description: Använd den här artikeln för att hantera roll tilldelningar för hanterad HSM
services: key-vault
author: amitbapat
ms.service: key-vault
ms.subservice: managed-hsm
ms.topic: tutorial
ms.date: 09/15/2020
ms.author: ambapat
ms.openlocfilehash: 78d4d9a8b5023731530c5e348f5c9ba72815d410
ms.sourcegitcommit: 6109f1d9f0acd8e5d1c1775bc9aa7c61ca076c45
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/10/2020
ms.locfileid: "94445650"
---
# <a name="managed-hsm-role-management"></a>Hantering av Managed HSM-roller

> [!NOTE]
> Key Vault stöder två typer av resurser: valv och hanterade HSM: er. Den här artikeln är en **hanterad HSM**. Om du vill lära dig hur du hanterar ett valv går du [till hantera Key Vault med hjälp av Azure CLI](../general/manage-with-cli2.md).

En översikt över hanterad HSM finns i [Vad är hanterad HSM?](overview.md). Om du inte har någon Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

Den här artikeln visar hur du hanterar roller för ett hanterat HSM-dataplan. Information om hanterad HSM-åtkomst kontroll modell finns i [hanterad HSM Access Control](access-control.md).

Om du vill tillåta ett säkerhets objekt (till exempel en användare, ett huvud namn för tjänsten, en grupp eller en hanterad identitet) för att utföra hanterade HSM-dataplans åtgärder, måste de tilldelas en roll som tillåter att dessa åtgärder utförs. Om du till exempel vill tillåta att ett program utför en signerings åtgärd med hjälp av en nyckel måste det tilldelas en roll som innehåller "Microsoft. Key Vault/managedHSM/Keys/Sign/Action" som en av data åtgärderna. En roll kan tilldelas i en speciell omfattning. Hanterad HSM lokalt RBAC stöder två scope, HSM-wide ( `/` eller `/keys` ) och per nyckel ( `/keys/<keyname>` ).

En lista över alla inbyggda roller för hanterad HSM och de åtgärder som de tillåter finns i [hanterade HSM-inbyggda roller](built-in-roles.md).

## <a name="prerequisites"></a>Krav

Om du vill använda Azure CLI-kommandona i den här artikeln måste du ha följande objekt:

* En prenumeration på Microsoft Azure. Om du inte har ett konto kan du registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial).
* Azure CLI-versionen 2.12.0 eller senare. Kör `az --version` för att hitta versionen. Om du behöver installera eller uppgradera kan du läsa informationen i [Installera Azure CLI]( /cli/azure/install-azure-cli).
* En hanterad HSM i din prenumeration. Se [snabb start: etablera och aktivera en hanterad HSM med Azure CLI](quick-create-cli.md) för att etablera och aktivera en hanterad HSM.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="sign-in-to-azure"></a>Logga in på Azure

Du kan logga in i Azure via CLI genom att skriva:

```azurecli
az login
```

Mer information om inloggnings alternativ via CLI finns i [Logga in med Azure CLI](/cli/azure/authenticate-azure-cli?view=azure-cli-latest&preserve-view=true)

## <a name="create-a-new-role-assignment"></a>Skapa en ny roll tilldelning

### <a name="assign-roles-for-all-keys"></a>Tilldela roller för alla nycklar

Använd `az keyvault role assignment create` kommandot för att tilldela en **hanterad HSM-kryptografiprovider** till användare som identifieras av User Principal Name **användare2- \@ contoso.com** för alla  **nycklar** (omfång `/keys` ) i ContosoHSM.

```azurecli-interactive
az keyvault role assignment create --hsm-name ContosoMHSM --role "Managed HSM Crypto Officer" --assignee user2@contoso.com  --scope /keys
```

### <a name="assign-role-for-a-specific-key"></a>Tilldela en roll för en enskild nyckel

Använd `az keyvault role assignment create` kommandot för att tilldela en **HANTERAd HSM-kryptografiprovider** rollen till användare som identifieras av User Principal Name **användare2 \@ contoso.com** för en speciell nyckel med namnet **myrsakey**.

```azurecli-interactive
az keyvault role assignment create --hsm-name ContosoMHSM --role "Managed HSM Crypto Officer" --assignee user2@contoso.com  --scope /keys/myrsakey
```

## <a name="list-existing-role-assignments"></a>Lista över befintliga roll tilldelningar

Används `az keyvault role assignment list` för att lista roll tilldelningar.

Alla roll tilldelningar i omfånget/(standard när inget--scope har angetts) för alla användare (standard när ingen tilldelas)

```azurecli-interactive
az keyvault role assignment list --hsm-name ContosoMHSM
```

Alla roll tilldelningar på HSM-nivå för en enskild användare **user1@contoso.com** .

```azurecli-interactive
az keyvault role assignment list --hsm-name ContosoMHSM --assignee user@contoso.com
```

Alla roll tilldelningar för en speciell användare **user2@contoso.com** för en speciell nyckel **myrsakey**.

```azurecli-interactive
az keyvault role assignment list --hsm-name ContosoMHSM --assignee user2@contoso.com --scope /keys/myrsakey
```

En speciell roll tilldelning för roll **hanterad HSM-kryptografisk tjänsteman** för en speciell användare **user2@contoso.com** för en speciell nyckel **myrsakey**


```azurecli-interactive
az keyvault role assignment list --hsm-name ContosoMHSM --assignee user2@contoso.com --scope /keys/myrsakey --role "Managed HSM Crypto Officer"
```

## <a name="delete-a-role-assignment"></a>Ta bort en roll tilldelning

Använd `az keyvault role assignment delete` kommandot för att ta bort en **HANTERAd HSM-krypto** -roll som tilldelats User **användare2 \@ contoso.com** for Key **myrsakey2**.

```azurecli-interactive
az keyvault role assignment delete --hsm-name ContosoMHSM --role "Managed HSM Crypto Officer" --assignee user2@contoso.com  --scope /keys/myrsakey2
```

## <a name="list-all-available-role-definitions"></a>Lista alla tillgängliga roll definitioner

Använd `az keyvault role definition list` kommandot för att lista alla roll definitioner.

```azurecli-interactive
az keyvault role definition list --hsm-name ContosoMHSM
```

## <a name="next-steps"></a>Nästa steg

- Se en översikt över [rollbaserad åtkomst kontroll i Azure (Azure RBAC)](../../role-based-access-control/overview.md).
- Se en självstudie om [hanterad HSM-roll hantering](role-management.md)
- Läs mer om [hanterad HSM-åtkomst kontroll modell](access-control.md)
- Se alla [inbyggda roller för hanterad HSM lokalt RBAC](built-in-roles.md)
