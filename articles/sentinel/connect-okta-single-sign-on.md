---
title: Anslut okta Single Sign-On data till Azure Sentinel | Microsoft Docs
description: Lär dig hur du ansluter okta-data för enskilda Sign-On till Azure Sentinel.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/17/2020
ms.author: yelevin
ms.openlocfilehash: 05a9b8009d896a2ee87df3e1c4493d249a887566
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87083930"
---
# <a name="connect-your-okta-single-sign-on-to-azure-sentinel-with-azure-function"></a>Anslut dina okta enkla Sign-On till Azure Sentinel med Azure Function

> [!IMPORTANT]
> Okta Single Sign-On data Connector i Azure Sentinel är för närvarande en offentlig för hands version.
> Den här funktionen tillhandahålls utan service nivå avtal och rekommenderas inte för produktions arbets belastningar. Vissa funktioner kanske inte stöds eller kan vara begränsade. Mer information finns i [Kompletterande villkor för användning av Microsoft Azure-förhandsversioner](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Med okta för enkel Sign-On (SSO) kan du enkelt ansluta alla dina [Sign-On okta-](https://www.okta.com/products/single-sign-on/) säkerhets lösnings loggar med Azure Sentinel, för att visa instrument paneler, skapa anpassade aviseringar och förbättra undersökningen. Integreringen mellan okta Single Sign-On och Azure Sentinel använder Azure Functions för att hämta loggdata med hjälp av REST API.

> [!NOTE]
> Data lagras på den geografiska platsen för den arbets yta där du kör Azure Sentinel.

## <a name="configure-and-connect-okta-single-sign-on"></a>Konfigurera och Anslut okta Single Sign-On

Azure Functions kan integrera och hämta händelser och loggar direkt från okta enda Sign-On och vidarebefordra dem till Azure Sentinel.

1. I Azure Sentinel-portalen klickar du på **data kopplingar** och väljer **okta-anslutning för enkel inloggning** .

1. Välj **Öppna kopplings sida**.

1. Följ anvisningarna på sidan **okta enkel inloggning** .

## <a name="find-your-data"></a>Hitta dina data

När en lyckad anslutning har upprättats visas data i Log Analytics under tabellen **Okta_CL** .

## <a name="validate-connectivity"></a>Verifiera anslutning

Det kan ta upp till 20 minuter innan loggarna börjar visas i Log Analytics.

## <a name="next-steps"></a>Nästa steg

I det här dokumentet har du lärt dig hur du ansluter okta-Sign-On till Azure-kontroll med Azure Function-appar. Mer information om Azure Sentinel finns i följande artiklar:

- Lär dig hur du [får insyn i dina data och potentiella hot](quickstart-get-visibility.md).
- Kom igång [med att identifiera hot med Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Använd arbets böcker](tutorial-monitor-your-data.md) för att övervaka dina data.

