---
title: Översikt över Microsoft Azure Stack Edge Pro | Microsoft Docs
description: Beskriver Azure Stack Edge Pro, en lagrings lösning som använder en fysisk enhet för nätverks baserad överföring till Azure.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: overview
ms.date: 09/09/2020
ms.author: alkohli
ms.openlocfilehash: 51469c23574f55c102f0c6fb98e5aa5178b59a4a
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/01/2020
ms.locfileid: "96455748"
---
# <a name="what-is-azure-stack-edge-pro-with-fpga"></a>Vad är Azure Stack Edge Pro med FPGA?

[!INCLUDE [data-box-edge-gateway-rename-note](../../includes/data-box-edge-gateway-rename-note.md)]

Azure Stack Edge Pro med FPGA är en AI-aktiverad Edge-baserad data behandlings enhet med funktioner för nätverks data överföring. Den här artikeln ger en översikt över Azure Stack Edge Pro med FPGA-lösning, fördelar, viktiga funktioner och scenarier där du kan distribuera den här enheten.

Azure Stack Edge Pro med FPGA är en maskinvaru-som-tjänst-lösning. Microsoft levererar en molnbaserad enhet med en inbyggd fält programmerbar grind mat ris (FPGA) som aktiverar accelererad AI-inferencing och har alla funktioner i en nätverks lagrings-Gateway. 

## <a name="use-cases"></a>Användningsfall

Här följer de olika scenarier där Azure Stack Edge Pro kan användas för snabb Machine Learning (ML) inferencing vid kant-och förbearbetning av data innan de skickas till Azure.

- **Härledning med Azure Machine Learning** – med Azure Stack Edge Pro kan du köra ml-modeller för att få snabba resultat som kan åtgärdas innan data skickas till molnet. Den fullständiga data uppsättningen kan alternativt överföras för att fortsätta att träna och förbättra dina ML-modeller. Mer information om hur du använder Azure ML-accelererade maskin varu modeller på den Azure Stack Edge Pro-enheten finns i [Distribuera Azure ml-maskin accelererade modeller på Azure Stack Edge Pro](../machine-learning/how-to-deploy-fpga-web-service.md#deploy-to-a-local-edge-server).

- **Förbearbeta data** för datatransformering innan du skickar dem till Azure för att skapa en mer åtgärds bara data uppsättning. Förbearbeta kan användas för att: 

    - Aggregera data.
    - Ändra data, till exempel för att ta bort personliga data.
    - Delmängd data för att optimera lagring och bandbredd, eller för ytterligare analys.
    - Analysera och reagera på IoT-händelser. 

- **Överför data över nätverk till Azure** – Använd Azure Stack Edge Pro för att enkelt och snabbt överföra data till Azure för att möjliggöra ytterligare beräkning och analys eller för arkivering. 

## <a name="key-capabilities"></a>De viktigaste funktionerna

Azure Stack Edge Pro har följande funktioner:

|Funktion |Beskrivning  |
|---------|---------|
|Accelererad AI-inferencing| Aktive rad av de inbyggda FPGA.|
|Databehandling       |Möjliggör analys, bearbetning och filtrering av data.|
|Höga prestanda | Beräkning och data överföringar med höga prestanda.|
|Dataåtkomst     | Direktåtkomst till data från Azure Storage-blobar och Azure Files med moln-API:er för ytterligare databehandling i molnet. Lokal cache på enheten används för snabb åtkomst till de senast använda filerna.|
|Molnbaserad hantering     |Enhet och tjänst hanteras via Azure Portal.  |
|Offlineuppladdning     | Frånkopplat läge stöder scenarier med offlineuppladdning.|
|Protokoll som stöds     | Stöd för SMB- och NFS-standardprotokoll för datainmatning. <br> Mer information om versioner som stöds finns i [Azure Stack Edge Pro system-krav](azure-stack-edge-system-requirements.md).|
|Datauppdatering     | Möjlighet att uppdatera lokala filer med det senaste från molnet.|
|Kryptering    | BitLocker-stöd för lokal kryptering av data och skydda dataöverföring till molnet via *https*.|
|Bandbredds begränsning| Begränsa bandbredds användningen under hög belastnings tider.|
|ExpressRoute | Säkerhet har lagts till via ExpressRoute. Använd peering-konfiguration där trafik från lokala enheter till moln lagrings slut punkter överförs över ExpressRoute. Mer information finns i [Översikt över ExpressRoute](../expressroute/expressroute-introduction.md).

## <a name="components"></a>Komponenter

Azure Stack Edge Pro-lösningen består av Azure Stack Edge-resurs, Azure Stack Edge Pro-fysisk enhet och ett lokalt webb gränssnitt.

* **Azure Stack den fysiska enheten för Edge Pro** – en rackoptimerad 1U-server som tillhandahålls av Microsoft och som kan konfigureras för att skicka data till Azure.
    
* **Azure Stack Edge-resurs** – en resurs i Azure Portal som låter dig hantera en Azure Stack Edge Pro-enhet från ett webb gränssnitt som du kan komma åt från olika geografiska platser. Använd Azure Stack Edge-resursen för att skapa och hantera resurser, Visa och hantera enheter och aviseringar och hantera resurser.  

    <!--![The Azure Stack Edge service in Azure portal](media/data-box-overview/data-box-Edge-service1.png)-->

    Mer information finns i [skapa en order för din Azure Stack Edge Pro-enhet](azure-stack-edge-deploy-prep.md#create-a-new-resource).

* **Azure Stack Edge Pro-lokalt webb gränssnitt** – Använd det lokala webb gränssnittet för att köra diagnostik, stänga av och starta om Azure Stack Edge Pro-enheten, Visa kopierings loggar och kontakta Microsoft Support för att skicka en tjänstbegäran.

    <!--![The Azure Stack Edge Pro local web UI](media/data-box-Edge-overview/data-box-Edge-local-web-ui.png)-->

    Information om hur du använder det webbaserade användar gränssnittet finns i [använda det webbaserade gränssnittet för att administrera Azure Stack Edge Pro](azure-stack-edge-manage-access-power-connectivity-mode.md).

## <a name="region-availability"></a>Regional tillgänglighet

Azure Stack fysisk enhet i Edge Pro, Azure-resursen och mål lagrings kontot som du överför data till, behöver inte allt vara i samma region.

- **Resurs tillgänglighet** – en lista över alla regioner där Azure Stack Edge-resursen är tillgänglig finns i Azure- [produkter tillgängliga per region](https://azure.microsoft.com/global-infrastructure/services/?products=databox&regions=all). Azure Stack Edge Pro kan också distribueras i Azure Government molnet. Mer information finns i [Vad är Azure Government?](../azure-government/documentation-government-welcome.md).
    
- **Destinationslagringskonton** – de lagringskonton som lagrar data som är tillgängliga i alla Azure-regioner. De regioner där lagrings konton lagrar Azure Stack Edge Pro-data bör ligga nära den plats där enheten befinner sig för optimala prestanda. Ett lagringskonto som är långt från enheten leder till långa svarstider och långsammare prestanda.

## <a name="next-steps"></a>Nästa steg

- Granska [system kraven för Azure Stack Edge Pro](azure-stack-edge-system-requirements.md).
- Förstå [Azure Stack Edge Pro-gränser](azure-stack-edge-limits.md).
- Distribuera [Azure Stack Edge Pro](azure-stack-edge-deploy-prep.md) i Azure Portal.