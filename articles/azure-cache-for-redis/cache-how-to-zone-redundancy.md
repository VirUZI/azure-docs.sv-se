---
title: Aktivera zon redundans för Azure cache för Redis (för hands version)
description: Lär dig hur du ställer in zon redundans för din Premium-och Enterprise-nivå Azure-cache för Redis-instanser
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.date: 08/11/2020
ms.openlocfilehash: 0cb7ee5b9fa02e726d03bf1ae9935c07ded6e4a6
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93088020"
---
# <a name="enable-zone-redundancy-for-azure-cache-for-redis-preview"></a>Aktivera zon redundans för Azure cache för Redis (för hands version)
I den här artikeln får du lära dig hur du konfigurerar en zon-redundant Azure-cache-instans med hjälp av Azure Portal.

Azure cache för Redis-nivåerna standard, Premium och Enterprise tillhandahåller inbyggd redundans genom att vara värd för varje cache på två dedikerade virtuella datorer (VM). Även om de här virtuella datorerna finns i separata [Azure-fel och uppdaterings domäner](../virtual-machines/manage-availability.md) och har hög tillgänglighet, är de utsatta för fel på data center nivå. Azure cache för Redis stöder också zon-redundans på dess Premium-och Enterprise-nivåer. En zon – redundant cache körs på virtuella datorer som sprids över flera [tillgänglighets zoner](../virtual-machines/manage-availability.md#use-availability-zones-to-protect-from-datacenter-level-failures). Den ger högre återhämtning och tillgänglighet.

## <a name="prerequisites"></a>Förutsättningar
* Azure-prenumeration – [skapa en kostnads fritt](https://azure.microsoft.com/free/)

> [!NOTE]
> Den här funktionen är för närvarande i för hands version – [kontakta oss](mailto:azurecache@microsoft.com) om du är intresse rad av.
>

## <a name="create-a-cache"></a>Skapa en cache
Följ dessa steg om du vill skapa en cache:

1. Logga in på [Azure Portal](https://portal.azure.com) och välj **Skapa en resurs** .
  
1. Välj **databaser** på sidan **nytt** och välj sedan **Azure cache för Redis** .

    :::image type="content" source="media/cache-create/new-cache-menu.png" alt-text="Välj Azure-cache för Redis.":::
   
1. På sidan **grundläggande** inställningar konfigurerar du inställningarna för det nya cacheminnet.
   
    | Inställning      | Föreslaget värde  | Beskrivning |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Prenumeration** | Välj din prenumeration. | Den prenumeration som du vill skapa den här nya Azure-cache för Redis-instansen för. | 
    | **Resursgrupp** | Välj en resurs grupp eller Välj **Skapa ny** och ange ett nytt resurs grupp namn. | Namnet på resurs gruppen där du vill skapa cachen och andra resurser. Genom att lägga till alla dina app-resurser i en resurs grupp kan du enkelt hantera eller ta bort dem tillsammans. | 
    | **DNS-namn** | Ange ett globalt unikt namn. | Cache-namnet måste vara en sträng mellan 1 och 63 tecken som bara innehåller siffror, bokstäver eller bindestreck. Namnet måste börja och sluta med en siffra eller en bokstav och får inte innehålla flera bindestreck i rad. Din cacheposts *värdnamn* är *\<DNS name> . Redis.cache.Windows.net* . | 
    | **Plats** | Välj en plats. | Välj en [region](https://azure.microsoft.com/regions/) nära andra tjänster som ska använda din cache. |
    | **Cachestorlek** | Välj en [Premium-eller Enterprise-nivå](https://azure.microsoft.com/pricing/details/cache/) -cache. |  Prisnivån avgör storlek, prestanda och funktioner som är tillgängliga för cacheminnet. Mer information finns i [Översikt över Azure Cache for Redis](cache-overview.md). |
   
1. På sidan **Avancerat** väljer du **antal repliker** för cache på Premium-nivå.
   
    :::image type="content" source="media/cache-how-to-multi-replicas/create-multi-replicas.png" alt-text="Välj Azure-cache för Redis.":::

1. Välj **tillgänglighets zoner** . 
   
    :::image type="content" source="media/cache-how-to-zone-redundancy/create-zones.png" alt-text="Välj Azure-cache för Redis.":::

1. Lämna de andra alternativen i standardinställningarna. 

    > [!NOTE]
    > Stöd för zon redundans fungerar endast med icke-klustrade och icke-geo-replikerade cacheminnen för närvarande. Dessutom stöder den inte privat länk, skalning, data persistens eller import/export.
    >

1. Klicka på **Skapa** . 
   
    Det tar en stund innan cacheminnet skulle skapas. Du kan övervaka förloppet på **översikts** sidan för Azure-cache för Redis. När **statusen** är **igång** är cacheminnet redo att användas.
   
    > [!NOTE]
    > Tillgänglighets zoner kan inte ändras efter att en cache har skapats.
    >

## <a name="next-steps"></a>Nästa steg
Läs mer om Azure cache för Redis-funktioner.

> [!div class="nextstepaction"]
> [Azure cache för Redis Premium service-nivåer](cache-overview.md#service-tiers)