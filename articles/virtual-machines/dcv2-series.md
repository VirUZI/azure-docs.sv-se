---
title: DC-serien – Azure Virtual Machines
description: Specifikationer för virtuella datorer i DC-serien.
author: susaxen
ms.service: virtual-machines
ms.subservice: sizes
ms.topic: conceptual
ms.date: 02/20/2020
ms.author: jushiman
ms.openlocfilehash: 1809194ce551fc828389cc31f5c7d6f28a2cddd6
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91972381"
---
# <a name="dcsv2-series"></a>DCsv2-serien


DCsv2-serien kan hjälpa till att skydda konfidentialiteten och integriteten hos dina data och kod när de bearbetas i det offentliga molnet. De här datorerna backas upp av den senaste generationen Intel XEON E-2288G-processor med SGX-teknik. Med Intel Turbo Boost-tekniken kan de här datorerna gå upp till 5,0 GHz. DCsv2 Series-instanser gör det möjligt för kunder att skapa säkra enklaven-baserade program för att skydda sin kod och sina data medan den används.

Exempel på användnings områden är: konfidentiell data delning för multidelar, bedrägeri identifiering, tvättning av pengar, blockchain, konfidentiell användnings analys, informations analys och konfidentiell maskin inlärning.

[Premium Storage](premium-storage-performance.md): stöds *<br>
[Premium Storage caching](premium-storage-performance.md): stöds<br>
[Direktmigrering](maintenance-and-updates.md): stöds inte<br>
[Minnes bebetjänings uppdateringar](maintenance-and-updates.md): stöds inte<br>
[Stöd för VM-generering](generation-2.md): generation 1 och 2<br>

* Förutom Standard_DC8_v2

| Storlek             | Virtuell processor | Minne: GiB | Temporär lagring (SSD) GiB | Maximalt antal datadiskar | Maximalt genomflöde för cachelagring och temporär lagring: IOPS / Mbit/s (cachestorlek i GiB) | Högsta antal nätverkskort/förväntad nätverks bandbredd (Mbit/s) | EPC-minne (MiB) |
|------------------|------|-------------|------------------------|----------------|-------------------------------------------------------------------------|----------------------------------------------|---------------------|
| Standard_DC1s_v2 | 1    | 4           | 50                     | 1              | 2000/16                                                                                               | 2   | 28                                      |
| Standard_DC2s_v2 | 2    | 8           | 100                    | 2              | 4000/32                                                                                               | 2  | 56                                          |
| Standard_DC4s_v2 | 4    | 16          | 200                    | 4              | 8000/64                                                                                               | 2  | 112                                          |
| Standard_DC8_v2  | 8   | 32          | 400                    | 8              | 16000/128                                                                                         | 2   | 168                                         |

- Virtuella datorer i DCsv2-serien är virtuella [datorer i generation 2](./generation-2.md#creating-a-generation-2-vm) och stöder bara `Gen2` avbildningar.
- För närvarande tillgängligt i de regioner som anges [här](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines&regions=all).
- Tidigare generation av konfidentiella beräknings datorer: [DC-serien](sizes-previous-gen.md#preview-dc-series)
- Skapa virtuella DCsv2-datorer med hjälp av [Azure Portal](./linux/quick-create-portal.md) eller [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-azure-compute.acc-virtual-machine-v2?tab=overview)



## <a name="other-sizes-and-information"></a>Andra storlekar och information

- [Generell användning](sizes-general.md)
- [Minnesoptimerad](sizes-memory.md)
- [Lagringsoptimerad](sizes-storage.md)
- [GPU-optimerad](sizes-gpu.md)
- [Databehandling med höga prestanda](sizes-hpc.md)
- [Tidigare generationer](sizes-previous-gen.md)

Pris kalkylator: [pris kalkylator](https://azure.microsoft.com/pricing/calculator/)

Mer information om disk typer: [disk typer](./disks-types.md#ultra-disk)

## <a name="next-steps"></a>Nästa steg

Lär dig mer om hur [Azure Compute Units (ACU)](acu.md) kan hjälpa dig att jämföra beräknings prestanda i Azure SKU: er.