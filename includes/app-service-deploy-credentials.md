---
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 04/20/2020
ms.author: cephalin
ms.openlocfilehash: c3fa57dd162fbbfbf0d46f73bffc78f279ef2968
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/09/2020
ms.locfileid: "83649122"
---
* **Autentiseringsuppgifter för användar nivå**: en uppsättning autentiseringsuppgifter för hela Azure-kontot. Den kan användas för att distribuera till App Service för alla appar, i alla prenumerationer, som Azure-kontot har behörighet att komma åt. Det är den standard uppsättning som finns i portalens GUI (till exempel **Översikt** och **Egenskaper** för appens [resurs sida](../articles/azure-resource-manager/management/manage-resources-portal.md#manage-resources)). När en användare beviljas åtkomst till appen via Role-Based Access Control (RBAC) eller behörigheten administratör kan användaren använda sina egna autentiseringsuppgifter på användar nivå tills åtkomsten har återkallats. Dela inte dessa autentiseringsuppgifter med andra Azure-användare.

* **Autentiseringsuppgifter för app-nivå**: en uppsättning autentiseringsuppgifter för varje app. Den kan endast användas för att distribuera till appen. Autentiseringsuppgifterna för varje app genereras automatiskt när appen skapas. De kan inte konfigureras manuellt, men de kan återställas när som helst. För att en användare ska beviljas åtkomst till autentiseringsuppgifter för app-nivå via (RBAC) måste användaren vara deltagare eller högre i appen (inklusive den inbyggda rollen som webbplats deltagare). Läsarna får inte publicera och har inte åtkomst till dessa autentiseringsuppgifter.