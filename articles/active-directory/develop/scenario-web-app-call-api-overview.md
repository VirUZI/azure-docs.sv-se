---
title: 'Bygg en webbapp som anropar webb-API: er – Microsoft Identity Platform | Azure'
description: 'Lär dig hur du skapar en webbapp som anropar webb-API: er (översikt)'
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 07/14/2020
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: b4123b0424e52c6a2919de4b60bcc6158062b0f1
ms.sourcegitcommit: 6109f1d9f0acd8e5d1c1775bc9aa7c61ca076c45
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/10/2020
ms.locfileid: "94442641"
---
# <a name="scenario-a-web-app-that-calls-web-apis"></a>Scenario: en webbapp som anropar webb-API: er

Lär dig hur du skapar en webbapp som loggar användare i Microsoft Identity Platform och anropar webb-API: er åt den inloggade användaren.

## <a name="prerequisites"></a>Krav

Det här scenariot förutsätter att du redan har slutfört [scenariot: webbapp som loggar in användare](scenario-web-app-sign-user-overview.md).

## <a name="overview"></a>Översikt

Du lägger till autentisering till din webbapp så att den kan signera användare i och anropa ett webb-API för den inloggade användarens räkning.

![Webbapp som anropar webb-API:er](./media/scenario-webapp/web-app.svg)

Webb program som anropar webb-API: er är konfidentiella klient program.
Därför registrerar de en hemlighet (ett program lösen ord eller certifikat) med Azure Active Directory (Azure AD). Den här hemligheten skickas under anropet till Azure AD för att hämta en token.

## <a name="specifics"></a>Information

> [!NOTE]
> Att lägga till inloggning i en webbapp är att skydda själva webbappen. Skyddet uppnås med hjälp av *mellanprogram* bibliotek, inte Microsoft Authentication Library (MSAL). Föregående scenario, [webbapp som loggar in användare](scenario-web-app-sign-user-overview.md), och som omfattas av ämnet.
>
> Det här scenariot beskriver hur du anropar webb-API: er från en webbapp. Du måste hämta åtkomsttoken för dessa webb-API: er. Du kan använda MSAL-bibliotek för att hämta dessa tokens.

Utvecklingen av det här scenariot omfattar följande uppgifter:

- Under [program registrering](scenario-web-app-call-api-app-registration.md)måste du ange en svars-URI, en hemlighet eller ett certifikat som ska delas med Azure AD. Om du distribuerar din app till flera platser ger du en svars-URI för varje plats.
- [Program konfigurationen](scenario-web-app-call-api-app-configuration.md) måste ange de klientautentiseringsuppgifter som delats med Azure AD under program registreringen.

## <a name="recommended-reading"></a>Rekommenderad läsning

[!INCLUDE [recommended-topics](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="next-steps"></a>Nästa steg

Gå vidare till nästa artikel i det här scenariot, [Registrera appen](scenario-web-app-call-api-app-registration.md).