---
title: Skapa ett Azure Event Hubs schema register
description: Den här artikeln visar hur du skapar ett schema register i ett Azure Event Hubs-namnområde.
ms.topic: how-to
ms.date: 09/22/2020
ms.custom: references_regions
ms.openlocfilehash: 90556e0843cda94dc79330321d027f8d28eb7d20
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91652201"
---
# <a name="create-an-azure-event-hubs-schema-registry-preview"></a>Skapa ett Azure Event Hubs schema Registry (för hands version)
Den här artikeln visar hur du skapar en schema grupp med scheman i ett schema register som finns i Azure Event Hubs. En översikt över funktionen schema register i Azure Event Hubs finns [i Azure schema Registry i Event Hubs](schema-registry-overview.md).

> [!NOTE]
> - **Schema register** funktionen är för närvarande en för **hands version**och rekommenderas inte för produktions arbets belastningar.
> - Funktionen är endast tillgänglig på **standard** -och **dedikerade** nivåer, inte i **Basic** -nivån.

## <a name="prerequisites"></a>Förutsättningar
[Skapa ett Event Hubs-namnområde](event-hubs-create.md#create-an-event-hubs-namespace). Du kan också använda ett befintligt namn område. 

## <a name="create-a-schema-group"></a>Skapa en schema grupp
1. Gå till sidan **Event Hubs namn område** . 
1. Välj **schema register** på den vänstra menyn. Om du vill skapa en schema grupp väljer du **+ schema grupp** i verktygsfältet. 

    :::image type="content" source="./media/create-schema-registry/namespace-page.png" alt-text="Schema register sida":::
1. Utför följande steg på sidan **skapa schema grupp** :
    1. Ange ett **namn** för schema gruppen.
    1. För **serialiserings typ**väljer du ett format för serialisering som gäller för alla scheman i schema gruppen. **Avro** är för närvarande den enda typ som stöds, så välj **Avro**. 
    1. Välj ett **kompatibilitetsläge** för alla scheman i gruppen. Det finns stöd för **Avro**, framåt och bakåt. 
    1. Välj sedan **skapa** för att skapa schema gruppen. 
1. Välj namnet på **schema gruppen** i listan över schema grupper.

    :::image type="content" source="./media/create-schema-registry/select-schema-group.png" alt-text="Schema register sida":::    
1. Du ser sidan **schema grupp** för gruppen.

    :::image type="content" source="./media/create-schema-registry/schema-group-page.png" alt-text="Schema register sida":::
    

## <a name="add-a-schema-to-the-schema-group"></a>Lägg till ett schema i schema gruppen
I det här avsnittet lägger du till ett schema i schema gruppen med hjälp av Azure Portal. 

1. På sidan **schema grupp** väljer du **+ schema** i verktygsfältet. 
1. Utför följande steg på sidan **skapa schema** :
    1. Ange ett **namn** för schemat.
    1. Ange följande **schema** i text rutan. Du kan också välja en fil med schemat.
    
        ```json
        {
            "type": "record",
            "name": "AvroUser",
            "namespace": "com.azure.schemaregistry.samples",
            "fields": [
                {
                    "name": "name",
                    "type": "string"
                },
                {
                    "name": "favoriteNumber",
                    "type": "int"
                }
            ]
        }
        ```
    1. Välj **Skapa**. 
1. Välj **schemat** i listan över scheman. 

    :::image type="content" source="./media/create-schema-registry/select-schema.png" alt-text="Schema register sida":::
1. Schema **översikts** sidan visas för schemat. 

    :::image type="content" source="./media/create-schema-registry/schema-overview-page.png" alt-text="Schema register sida":::    
1. Om det finns flera versioner av ett schema visas de i list rutan **versioner** . Välj en version att växla till det versions schemat. 

## <a name="create-a-new-version-of-schema"></a>Skapa en ny version av schema

1. Uppdatera schemat i text rutan och välj **Verifiera**. I följande exempel har ett nytt fält `id` lagts till i schemat. 

    :::image type="content" source="./media/create-schema-registry/update-schema.png" alt-text="Schema register sida":::    
    
1. Granska verifierings status och ändringar och välj **Spara**. 

    :::image type="content" source="./media/create-schema-registry/compare-save-schema.png" alt-text="Schema register sida":::     
1. Du ser att `2` har valts för **versionen** på sidan **schema översikt** . 

    :::image type="content" source="./media/create-schema-registry/new-version.png" alt-text="Schema register sida":::    
1. Välj `1` om du vill visa version 1 för schemat. 

    :::image type="content" source="./media/create-schema-registry/select-version.png" alt-text="Schema register sida":::    


## <a name="next-steps"></a>Nästa steg
Mer information om schema register finns [i Azure schema Registry i Event Hubs](schema-registry-overview.md).

