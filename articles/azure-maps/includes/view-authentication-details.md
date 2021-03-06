---
title: Visa information om autentisering för Azure Maps
description: Använd Azure Portal för att visa information om autentiseringen av Azure Maps.
author: anastasia-ms
ms.author: v-stharr
ms.date: 06/17/2020
ms.topic: include
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: a6ffcbf5a8c36958dd3ea74de4d826fe25a1139c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87126471"
---
Du kan visa information om Azure Maps-kontots autentisering i Azure Portal. I ditt konto går du till menyn **Inställningar** och väljer **autentisering**.

![Information om autentisering](../media/how-to-manage-authentication/how-to-view-auth.png)

När ett Azure Maps-konto har skapats finns Azure Maps- `x-ms-client-id` värdet på sidan Azure Portal information om autentisering. Det här värdet representerar kontot som ska användas för REST API begär Anden. Det här värdet ska lagras i program konfigurationen och hämtas innan du gör HTTP-förfrågningar när du använder Azure AD-autentisering med Azure Maps.
