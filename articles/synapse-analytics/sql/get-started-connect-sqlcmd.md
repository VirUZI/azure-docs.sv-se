---
title: Ansluta till Synapse SQL med SQLCMD
description: Använd kommando rads verktyget sqlcmd för att ansluta till och fråga Server lös SQL-pool och dedikerad SQL-pool.
services: synapse analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: stefanazaric
ms.reviewer: jrasnick
ms.openlocfilehash: 88ee95789bdc1c6ee9884a021067318caab203d4
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/01/2020
ms.locfileid: "96451639"
---
# <a name="connect-to-synapse-sql-with-sqlcmd"></a>Ansluta till Synapse SQL med SQLCMD

> [!div class="op_single_selector"]
> * [Azure Data Studio)](get-started-azure-data-studio.md)
> * [Power BI](get-started-power-bi-professional.md)
> * [Visual Studio](../sql-data-warehouse/sql-data-warehouse-query-visual-studio.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
> * [sqlcmd](../sql/get-started-connect-sqlcmd.md)
> * [SSMS](get-started-ssms.md)

Du kan använda kommando rads verktyget [SQLCMD](/sql/tools/sqlcmd-utility?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) för att ansluta till och fråga Server lös SQL-pool och dedikerad SQL-pool i Synapse SQL.  

## <a name="1-connect"></a>1. Anslut
Kom igång med [SQLCMD](/sql/tools/sqlcmd-utility?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)genom att öppna kommando tolken och ange **SQLCMD** följt av anslutnings STRÄNGEN för din Synapse SQL-databas. Anslutningssträngen kräver följande parametrar:

* **Server (-S):** Server i formatet `<`servernamn`>`. database.windows.net
* **Databas (-d):** Databas namn
* **Aktivera citerade identifierare (-I):** Identifierare med citat tecken måste vara aktiverade för att ansluta till en Synapse SQL-instans

Om du vill använda SQL Server autentisering måste du lägga till parametrarna för användar namn och lösen ord:

* **Användare (-U):** Serveranvändare i formatet `<`Användare`>`
* **Lösen ord (-P):** Lösen ord kopplat till användaren

Anslutnings strängen kan se ut som i följande exempel:

**Serverlös SQL-pool**

```sql
C:\>sqlcmd -S partyeunrt.database.windows.net -d demo -U Enter_Your_Username_Here -P Enter_Your_Password_Here -I
```

**Dedikerad SQL-pool**

```
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
```

Om du vill använda Azure Active Directory-integrerad autentisering måste du lägga till Azure Active Directory-parametrar:

* **Azure Active Directory-autentisering (-G):** använder Azure Active Directory för autentisering

Anslutnings strängen kan se ut som i följande exempel:

**Serverlös SQL-pool**

```
C:\>sqlcmd -S partyeunrt.database.windows.net -d demo -G -I
```

**Dedikerad SQL-pool**

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -G -I
```

> [!NOTE]
> Du måste [aktivera Azure Active Directory-autentisering](../sql-data-warehouse/sql-data-warehouse-authentication.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) för att kunna autentisera med Active Directory.

## <a name="2-query"></a>2. fråga

### <a name="use-dedicated-sql-pool"></a>Använd dedikerad SQL-pool

Efter anslutningen kan du skicka alla [Transact-SQL](/sql/t-sql/language-reference?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) -uttryck (T-SQL) som stöds mot instansen. I det här exemplet skickas frågor i interaktivt läge:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

För dedikerad SQL-pool visar följande exempel hur du kör frågor i batch-läge med alternativet-Q eller skickar din SQL till SQLCMD:

```sql
sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I -Q "SELECT name FROM sys.tables;"
```

```sql
"SELECT name FROM sys.tables;" | sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I > .\tables.out
```

### <a name="use-serverless-sql-pool"></a>Använda en serverlös SQL-pool

När du har anslutit kan du skicka alla [Transact-SQL](/sql/t-sql/language-reference?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) -uttryck (T-SQL) som stöds mot instansen.  I följande exempel skickas frågor i interaktivt läge:

```sql
C:\>sqlcmd -S partyeunrt.database.windows.net -d demo -U Enter_Your_Username_Here -P Enter_Your_Password_Here -I
1> SELECT COUNT(*) FROM  OPENROWSET(BULK 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer/release/us_population_county/year=20*/*.parquet', FORMAT='PARQUET')
2> GO
3> QUIT
```

I SQL-pool utan Server visar exemplen som följer dig hur du kör frågor i batch-läge med alternativet-Q eller skickar din SQL till SQLCMD:

```sql
sqlcmd -S partyeunrt.database.windows.net -d demo -U Enter_Your_Username_Here -P 'Enter_Your_Password_Here' -I -Q "SELECT COUNT(*) FROM  OPENROWSET(BULK 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer/release/us_population_county/year=20*/*.parquet', FORMAT='PARQUET')"
```

```sql
"SELECT COUNT(*) FROM  OPENROWSET(BULK 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer/release/us_population_county/year=20*/*.parquet', FORMAT='PARQUET')" | sqlcmd -S partyeunrt.database.windows.net -d demo -U Enter_Your_Username_Here -P 'Enter_Your_Password_Here' -I > ./tables.out
```

## <a name="next-steps"></a>Nästa steg

Mer information om alternativ för SQLCMD finns i [SQLCMD-dokumentationen](/sql/tools/sqlcmd-utility?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest).
