---
title: SQL-skript i Azure Synapse Studio (för hands version)
description: Introduktion till Azure Synapse Studio (för hands version) SQL-skript
services: synapse-analytics
author: pimorano
ms.service: synapse-analytics
ms.subservice: sql
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: pimorano
ms.reviewer: omafnan
ms.openlocfilehash: 3f3009799889bd6b118f586676e22338d821d37c
ms.sourcegitcommit: 295db318df10f20ae4aa71b5b03f7fb6cba15fc3
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/15/2020
ms.locfileid: "94635317"
---
# <a name="using-sql-scripts-in-azure-synapse-studio-preview"></a>Använda SQL-skript i Azure Synapse Studio (för hands version)

Azure Synapse Studio (för hands version) innehåller ett webb gränssnitt för SQL-skript där du kan redigera SQL-frågor. Du kan ansluta till SQL-poolen (förhands granskning). 

## <a name="begin-authoring-in-sql-script"></a>Börja redigera i SQL-skript 

Det finns flera sätt att starta redigerings funktionen i SQL-skript. Du kan skapa ett nytt SQL-skript via någon av följande metoder.

1. Från menyn utveckla väljer du ikonen **"+"** och sedan SQL- **skript**.

2. Från menyn **åtgärder** väljer du **nytt SQL-skript**.

3. Välj **Importera** från menyn **åtgärder** under utveckla SQL-skript. Välj ett befintligt SQL-skript från din lokala lagrings plats.
![nytt SQL-skript 3-åtgärder](media/author-sql-script/new-sql-script-3-actions.png)

## <a name="create-your-sql-script"></a>Skapa SQL-skript

1. Välj ett namn för ditt SQL-skript genom att välja **egenskaps** knappen och ersätta standard namnet som tilldelats SQL-skriptet. 
![nytt SQL-skript Byt namn](media/author-sql-script/new-sql-script-rename.png)

2. Välj den särskilda dedikerade SQL-poolen eller Server lös SQL-poolen från List menyn **Anslut till** . Eller om det behövs väljer du databasen från **Använd databas**. 
![ny SQL Välj pool](media/author-sql-script/new-sql-choose-pool.png)

3. Börja redigera SQL-skript med hjälp av IntelliSense-funktionen.

## <a name="run-your-sql-script"></a>Kör SQL-skriptet

Klicka på knappen **Kör** för att köra SQL-skriptet. Resultaten visas som standard i en tabell.

![ny tabell med SQL-skript resultat](media/author-sql-script/new-sql-script-results-table.png)

## <a name="export-your-results"></a>Exportera resultaten

Du kan exportera resultaten till din lokala lagring i olika format (inklusive CSV, Excel, JSON, XML) genom att välja "Exportera resultat" och välja tillägget.

Du kan också visualisera SQL-skript resultatet i ett diagram genom att välja **diagram** knappen. Välj kolumnen diagram typ och **kategori**. Du kan exportera diagrammet till en bild genom att välja **Spara som bild**. 

![nytt resultat diagram för SQL-skript](media/author-sql-script/new-sql-script-results-chart.png)

## <a name="explore-data-from-a-parquet-file"></a>Utforska data från en Parquet-fil

Du kan utforska Parquet-filer i ett lagrings konto med hjälp av SQL-skript för att förhandsgranska fil innehållet.

![nytt skript sqlod Parquet](media/author-sql-script/new-script-sqlod-parquet.png)

## <a name="sql-tables-external-tables-views"></a>SQL-tabeller, externa tabeller, vyer

Genom att välja menyn **åtgärder** under data kan du välja flera åtgärder, till exempel:

- Nytt SQL-skript
- Markera de 1000 översta raderna
- CREATE
- SLÄPP och skapa 
 
Utforska den tillgängliga gesten genom att högerklicka på noderna i SQL-databaser.
 
![ny skript databas](media/author-sql-script/new-script-database.png)

## <a name="create-folders-and-move-sql-scripts-into-a-folder"></a>Skapa mappar och flytta SQL-skript till en mapp

Från menyn åtgärder under utveckla SQL-skript väljer du "ny mapp" på menyn "åtgärder" under utveckla SQL-skript. Och skriv namnet på den nya mappen i popup-fönstret. 

> [!div class="mx-imgBorder"] 
> ![newsqlscript](./media/author-sql-script/new-sql-script-create-folder.png)

Om du vill flytta ett SQL-skript till en mapp kan du välja SQL-skriptet och välja "flytta till" på menyn åtgärder. Leta sedan rätt på målmappen i det nya fönstret och flytta SQL-skriptet till den valda mappen. Du kan också snabbt dra SQL-skriptet och släppa det i en mapp.  

> [!div class="mx-imgBorder"] 
> ![newsqlscript](./media/author-sql-script/new-sql-script-move-folder.png)

## <a name="next-steps"></a>Nästa steg

Mer information om hur du redigerar ett SQL-skript finns i [Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics).
