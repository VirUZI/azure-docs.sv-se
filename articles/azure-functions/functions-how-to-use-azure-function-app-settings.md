---
title: Konfigurera Function app-inställningar i Azure
description: Lär dig hur du konfigurerar inställningar för Azure Function-appar.
ms.assetid: 81eb04f8-9a27-45bb-bf24-9ab6c30d205c
ms.topic: conceptual
ms.date: 04/13/2020
ms.custom: cc996988-fb4f-47, devx-track-azurecli
ms.openlocfilehash: f597e58c70d6ac9daff753f5c0a54199c2383c42
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/25/2020
ms.locfileid: "96019521"
---
# <a name="manage-your-function-app"></a>Hantera din Function-app 

I Azure Functions tillhandahåller en Function-app körnings kontexten för dina enskilda funktioner. Function-appens beteenden gäller för alla funktioner som finns i en specifik Function-app. Alla funktioner i en Function-app måste vara av samma [språk](supported-languages.md). 

Enskilda funktioner i en Function-app distribueras tillsammans och skalas tillsammans. Alla funktioner i samma Function-app delar resurser, per instans, som Function-appen skalar. 

Anslutnings strängar, miljövariabler och andra program inställningar definieras separat för varje Function-app. Alla data som måste delas mellan Functions-appar ska lagras externt i ett sparat lager.

Den här artikeln beskriver hur du konfigurerar och hanterar dina Function-appar. 

> [!TIP]  
> Många konfigurations alternativ kan också hanteras med hjälp av [Azure CLI]. 

## <a name="get-started-in-the-azure-portal"></a>Kom igång i Azure Portal

1. Börja genom att gå till [Azure Portal] och logga in på ditt Azure-konto. I Sök fältet längst upp i portalen anger du namnet på din Function-app och väljer den i listan. 

2. Under **Inställningar** i den vänstra rutan väljer du **konfiguration**.

    :::image type="content" source="./media/functions-how-to-use-azure-function-app-settings/azure-function-app-main.png" alt-text="Översikt över Function-appen i Azure Portal":::

Du kan navigera till allt du behöver för att hantera din Function-app från översikts sidan, i synnerhet **[program inställningar](#settings)** och **[plattforms funktioner](#platform-features)**.

## <a name="application-settings"></a><a name="settings"></a>Programinställningar

Fliken **program inställningar** innehåller inställningar som används av din Function-app. De här inställningarna lagras krypterade och du måste välja **Visa värden** för att se värdena i portalen. Du kan också komma åt program inställningar med hjälp av Azure CLI.

### <a name="portal"></a>Portalen

Om du vill lägga till en inställning i portalen väljer du **ny program inställning** och lägger till det nya nyckel/värde-paret.

![Function app-inställningar i Azure Portal.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-settings-tab.png)

### <a name="azure-cli"></a>Azure CLI

[`az functionapp config appsettings list`](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-list)Kommandot returnerar de befintliga program inställningarna, som i följande exempel:

```azurecli-interactive
az functionapp config appsettings list --name <FUNCTION_APP_NAME> \
--resource-group <RESOURCE_GROUP_NAME>
```

[`az functionapp config appsettings set`](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set)Kommandot lägger till eller uppdaterar en program inställning. I följande exempel skapas en inställning med en nyckel med namnet `CUSTOM_FUNCTION_APP_SETTING` och värdet `12345` :


```azurecli-interactive
az functionapp config appsettings set --name <FUNCTION_APP_NAME> \
--resource-group <RESOURCE_GROUP_NAME> \
--settings CUSTOM_FUNCTION_APP_SETTING=12345
```

### <a name="use-application-settings"></a>Använd program inställningar

[!INCLUDE [functions-environment-variables](../../includes/functions-environment-variables.md)]

När du utvecklar en Function-app lokalt måste du upprätthålla lokala kopior av dessa värden i local.settings.jsi projekt filen. Mer information finns i [filen med lokala inställningar](functions-run-local.md#local-settings-file).

## <a name="platform-features"></a>Plattforms funktioner

Function-appar körs i, och underhålls av, Azure App Service plattformen. Därför har dina funktions appar till gång till de flesta av funktionerna i Azures centrala webb värd plattform. I den vänstra rutan får du till gång till många funktioner i den App Service-plattform som du kan använda i dina Function-appar. 

> [!NOTE]
> Alla App Service funktioner är inte tillgängliga när en Function-App körs på förbruknings värd planen.

Resten av den här artikeln fokuserar på följande App Service funktioner i Azure Portal som är användbara för funktioner:

+ [App Service redigerare](#editor)
+ [Konsol](#console)
+ [Avancerade verktyg (kudu)](#kudu)
+ [Distributionsalternativ](#deployment)
+ [CORS](#cors)
+ [Autentisering](#auth)

Mer information om hur du arbetar med App Service inställningar finns i [konfigurera Azure App Service inställningar](../app-service/configure-common.md).

### <a name="app-service-editor"></a><a name="editor"></a>App Service redigerare

![App Service redigeraren](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-appservice-editor.png)

App Services redigeraren är en avancerad i-Portal-redigerare som du kan använda för att ändra JSON-konfigurationsfiler och kommandofiler. Om du väljer det här alternativet startas en separat flik i webbläsaren med en grundläggande redigerare. På så sätt kan du integrera med git-lagringsplatsen, köra och felsöka kod och ändra programmets inställningar. Den här redigeraren ger en förbättrad utvecklings miljö för dina funktioner jämfört med den inbyggda funktions redigeraren.  

Vi rekommenderar att du tänker utveckla dina funktioner på den lokala datorn. När du utvecklar lokalt och publicerar till Azure är dina projektfiler skrivskyddade i portalen. Mer information finns i [kod-och test Azure Functions lokalt](functions-develop-local.md).

### <a name="console"></a><a name="console"></a>Konsol

![Function app-konsol](./media/functions-how-to-use-azure-function-app-settings/configure-function-console.png)

Konsolen i-portalen är ett idealiskt utvecklingsverktyg när du föredrar att interagera med din Function-app från kommando raden. Vanliga kommandon är att skapa och navigera mellan kataloger och filer, samt att köra kommandofiler och skript. 

När du utvecklar lokalt rekommenderar vi att du använder [Azure Functions Core tools](functions-run-local.md) och [Azure CLI].

### <a name="advanced-tools-kudu"></a><a name="kudu"></a>Avancerade verktyg (kudu)

![Konfigurera kudu](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-kudu.png)

De avancerade verktygen för App Service (kallas även kudu) ger till gång till avancerade administrativa funktioner i din Function-app. Från kudu kan du hantera system information, appinställningar, miljövariabler, plats tillägg, HTTP-rubriker och servervariabler. Du kan också starta **kudu** genom att bläddra till SCM-slutpunkten för din Function-app, t. ex. `https://<myfunctionapp>.scm.azurewebsites.net/` 


### <a name="deployment-center"></a><a name="deployment"></a>Distributionscenter

När du använder en käll kontroll lösning för att utveckla och underhålla din funktions kod kan du skapa och distribuera från käll kontroll i Deployment Center. Ditt projekt skapas och distribueras till Azure när du gör uppdateringar. Mer information finns [i distributions tekniker i Azure Functions](functions-deployment-technologies.md).

### <a name="cross-origin-resource-sharing"></a><a name="cors"></a>Cross-origin resource sharing (CORS)

För att förhindra körning av skadlig kod på klienten blockerar moderna webbläsare förfrågningar från webb program till resurser som körs i en separat domän. Med [resurs delning mellan ursprung (CORS)](https://developer.mozilla.org/docs/Web/HTTP/CORS) kan en `Access-Control-Allow-Origin` rubrik deklarera vilka ursprung som får anropa slut punkter i din Function-app.

#### <a name="portal"></a>Portalen

När du konfigurerar listan över **tillåtna ursprung** för din Function-app `Access-Control-Allow-Origin` läggs rubriken automatiskt till i alla svar från http-slutpunkter i din Function-app. 

![Konfigurera CORS-listan för Function-appen](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-cors.png)

När jokertecknet ( `*` ) används ignoreras alla andra domäner. 

Använd [`az functionapp cors add`](/cli/azure/functionapp/cors#az-functionapp-cors-add) kommandot för att lägga till en domän i listan över tillåtna ursprung. I följande exempel läggs contoso.com-domänen till:

```azurecli-interactive
az functionapp cors add --name <FUNCTION_APP_NAME> \
--resource-group <RESOURCE_GROUP_NAME> \
--allowed-origins https://contoso.com
```

Använd [`az functionapp cors show`](/cli/azure/functionapp/cors#az-functionapp-cors-show) kommandot för att visa en lista över aktuella tillåtna ursprung.

### <a name="authentication"></a><a name="auth"></a>Autentisering

![Konfigurera autentisering för en Function-app](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-authentication.png)

När funktioner använder en HTTP-utlösare kan du kräva att anrop först ska autentiseras. App Service stöder Azure Active Directory autentisering och inloggning med sociala leverantörer, till exempel Facebook, Microsoft och Twitter. Mer information om hur du konfigurerar vissa autentiseringsproviders finns i [Översikt över Azure App Service autentisering](../app-service/overview-authentication-authorization.md). 


## <a name="next-steps"></a>Nästa steg

+ [Konfigurera Azure App Service inställningar](../app-service/configure-common.md)
+ [Löpande distribution för Azure Functions](functions-continuous-deployment.md)

[Azure CLI]: /cli/azure/
[Azure-portalen]: https://portal.azure.com
