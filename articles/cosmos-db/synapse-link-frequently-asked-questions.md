---
title: Vanliga frågor och svar om Azure Synapse Link för Azure Cosmos DB
description: Få svar på vanliga frågor om Synapse-länken för Azure Cosmos DB i områden som fakturering, analys lager, säkerhet, tid att leva i analys lager.
author: Rodrigossz
ms.author: rosouz
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/09/2020
ms.openlocfilehash: 0791ed6882feedeab47b75eff6a69bf0a49ab7ee
ms.sourcegitcommit: fa90cd55e341c8201e3789df4cd8bd6fe7c809a3
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/04/2020
ms.locfileid: "93341302"
---
# <a name="frequently-asked-questions-about-azure-synapse-link-for-azure-cosmos-db"></a>Vanliga frågor och svar om Azure Synapse Link för Azure Cosmos DB
[!INCLUDE[appliesto-sql-mongodb-api](includes/appliesto-sql-mongodb-api.md)]

Azure Synapse-länken för Azure Cosmos DB skapar en tätt integrerad integrering mellan Azure Cosmos DB och Azure Synapse Analytics. Det gör det möjligt för kunder att köra nära real tids analys över sina drift data med fullständig prestanda isolering från sina transaktions arbets belastningar och utan ETL-pipeline. Den här artikeln besvarar vanliga frågor om Synapse Link för Azure Cosmos DB.

## <a name="general-faq"></a>Allmänna vanliga frågor och svar

### <a name="is-azure-synapse-link-supported-for-all-azure-cosmos-db-apis"></a>Stöds Azure Synapse-länken för alla Azure Cosmos DB API: er?

I den offentliga för hands versionen stöds Azure Synapse-länken för API: et för Azure Cosmos DB SQL (Core) och för Azure Cosmos DB-API för MongoDB. 

### <a name="is-azure-synapse-link-supported-for-multi-region-azure-cosmos-db-accounts"></a>Stöds Azure Synapse-länken för Azure Cosmos DB konton i flera regioner?

Ja, för Azure Cosmos-konton med flera regioner distribueras även de data som lagras i analys lagret globalt. Oavsett region för endast en skrivning eller regioner för flera skrivningar kan analysfrågor som utförs via Azure Synapse Analytics tjänas från den närmaste lokala regionen.

När du planerar att konfigurera ett Azure Cosmos DBs konto för flera regioner med stöd för analys av till gång, rekommenderar vi att du har alla nödvändiga regioner som lagts till när kontot skapas.

### <a name="can-i-choose-to-enable-azure-synapse-link-for-only-certain-region-and-not-all-regions-in-a-multi-region-account-set-up"></a>Kan jag välja att bara aktivera Azure Synapse-länken för vissa regioner och inte alla regioner i ett konto med flera regioner?

I för hands versionen när Azure Synapse-länken är aktive rad för ett konto med flera regioner skapas analys lagret i alla regioner. Underliggande data är optimerade för data flöde och transaktions konsekvens i transaktions arkivet.

### <a name="is-backup-and-restore-supported-for-azure-synapse-link-enabled-accounts"></a>Stöds säkerhets kopiering och återställning av Azure Synapse Link-aktiverade konton?

Automatisk säkerhets kopiering och återställning av dina data i det analytiska arkivet stöds inte för närvarande för behållarna med analytisk lagring aktiverat. 

När Synapse-länken är aktive rad på ett databas konto kommer Azure Cosmos DB fortsätta att automatiskt [ta säkerhets kopior](./online-backup-and-restore.md) av dina data i transaktions arkivet (endast) i behållare vid schemalagd säkerhets kopierings intervall, som alltid. Det är viktigt att Observera att när en behållare med ett analytiskt arkiv som är aktive rad återställs till ett nytt konto, kommer behållaren att återställas med enbart transaktions lagring och inget analytiskt Arkiv har Aktiver ATS. 

### <a name="can-i-disable-the-azure-synapse-link-feature-for-my-azure-cosmos-db-account"></a>Kan jag inaktivera funktionen Azure Synapse-länk för mitt Azure Cosmos DB konto?

När Synapse Link-funktionen har aktiverats på kontonivå kan du för närvarande inte inaktivera den. Du kommer inte att debiteras om Synapse Link-funktionen aktiveras på kontonivå och det inte finns några analysarkivaktiverade containrar. 

Om du behöver inaktivera funktionen har du två alternativ. Det första är att ta bort och Azure Cosmos DB-kontot, skapa ett nytt och sedan migrera data om det behövs. Det andra alternativet är att öppna en supportbegäran för att få hjälp med att migrera data till ett annat konto.

## <a name="azure-cosmos-db-analytical-store"></a>Azure Cosmos DB analys lager

### <a name="can-i-enable-analytical-store-on-existing-containers"></a>Kan jag aktivera analytisk lagring på befintliga behållare?

För närvarande kan analys lagret bara aktive ras för nya behållare (både i nya och befintliga konton).

### <a name="can-i-disable-analytical-store-on-my-azure-cosmos-db-containers-after-enabling-it-during-container-creation"></a>Kan jag inaktivera analytisk lagring på mina Azure Cosmos DB behållare när den har Aktiver ATS?

För närvarande kan inte analysarkivet inaktiveras på en Azure Cosmos DB-container när den har aktiverats medan containern skapas.

### <a name="is-analytical-store-supported-for-azure-cosmos-db-containers-with-autoscale-provisioned-throughput"></a>Stöds ett analys lager för Azure Cosmos DB behållare med autoskalning av allokerat data flöde?

Ja, analys lagret kan aktive ras på behållare med autoskalning av allokerat data flöde.

### <a name="is-there-any-effect-on-azure-cosmos-db-transactional-store-provisioned-rus"></a>Finns det någon inverkan på Azure Cosmos DB transaktions lager etablerade ru: er?

Azure Cosmos DB garanterar prestanda isolering mellan transaktions-och analys arbets belastningar. Aktivering av analys lagret på en behållare påverkar inte RU/s-etableringen i Azure Cosmos DB transaktions lager. Transaktionerna (Läs & skrivning) och lagrings kostnader för analys lagret debiteras separat. Mer information finns i [prissättningen för Azure Cosmos DB Analytical Store](analytical-store-introduction.md#analytical-store-pricing) .

### <a name="are-delete-and-update-operations-on-the-transactional-store-reflected-in-the-analytical-store"></a>Avspeglas borttagnings-och uppdaterings åtgärder i transaktions arkivet i analys lagret?

Ja, rader och uppdateringar av data i transaktions arkivet visas i analys lagret. Du kan konfigurera Time to Live (TTL) på behållaren för att inkludera historiska data så att analys arkivet behåller alla versioner av objekt som uppfyller kriterierna för analytiskt TTL. Mer information finns i [Översikt över analys-TTL](analytical-store-introduction.md#analytical-ttl) .

### <a name="can-i-connect-to-analytical-store-from-analytics-engines-other-than-azure-synapse-analytics"></a>Kan jag ansluta till ett analys lager från andra analys motorer än Azure Synapse Analytics?

Du kan bara komma åt och köra frågor mot analysarkivet med hjälp av de olika körningar som tillhandahålls av Azure Synapse Analytics. Du kan bara köra frågor mot och analysera analysarkivet med:

* Synapse Spark med fullständigt stöd för Scala, python, SparkSQL och C#. Synapse Spark är centralt för datateknik och vetenskapsscenarier
* SQL serverlös med T-SQL-språk och stöd för välbekanta BI-verktyg (till exempel Power BI Premium)

### <a name="can-i-connect-to-analytical-store-from-synapse-sql-provisioned"></a>Kan jag ansluta till ett analys lager från Synapse SQL etablerad?

För närvarande går det inte att komma åt analys lagret från Synapse SQL-etableringen.

### <a name="can-i-write-back-the-query-aggregation-results-from-synapse-back-to-the-analytical-store"></a>Kan jag skriva tillbaka frågan agg regerings resultat från Synapse tillbaka till analys lagret?

Analytiskt lagring är en skrivskyddad lagrings plats i en Azure Cosmos DB-behållare. Därför kan du inte direkt skriva tillbaka agg regerings resultaten till analys lagret, men kan skriva dem till Azure Cosmos DB transaktions Arkiv för en annan behållare, som senare kan användas som ett behållar lager.

### <a name="is-the-autosync-replication-from-transactional-store-to-the-analytical-store-asynchronous-or-synchronous-and-what-are-the-latencies"></a>Är den synkroniserade replikeringen från transaktions lagringen till det asynkrona arkivet asynkront eller synkront och vad är fördröjningen?

Svars tiden för automatisk synkronisering är vanligt vis inom 2 minuter. I händelse av en delad data flödes databas med ett stort antal behållare kan svars tiden för automatisk synkronisering av enskilda behållare vara högre och ta upp till 5 minuter. Vi vill veta mer om hur svars tiden passar dina scenarier. För det kan du kontakta [Azure Cosmos DB-teamet](mailto:cosmosdbsynapselink@microsoft.com).

### <a name="are-there-any-scenarios-where-the-items-from-the-transactional-store-are-not-automatically-propagated-to-the-analytical-store"></a>Finns det några scenarier där objekten från transaktions arkivet inte automatiskt sprids till analys lagret?

Om vissa objekt i din behållare strider mot det [väldefinierade schemat för analys](analytical-store-introduction.md#analytical-schema)tas de inte med i analys lagret. Om du har scenarier som har blockerats av väldefinierat schema för analys, e-posta [Azure Cosmos DBS teamet](mailto:cosmosdbsynapselink@microsoft.com) för hjälp.

### <a name="can-i-partition-the-data-in-analytical-store-differently-from-transactional-store"></a>Kan jag partitionera data i ett analytiskt lager på olika sätt från transaktions lagring?

Data i analysarkiv partitioneras baserat på den vågräta partitioneringen i shards i transaktionslagret. För närvarande kan du inte välja en annan partitioneringsstrategi för analysarkivet.

### <a name="can-i-customize-or-override-the-way-transactional-data-is-transformed-into-columnar-format-in-the-analytical-store"></a>Kan jag anpassa eller åsidosätta hur transaktions data omvandlas till kolumn format i analys lagret?

För närvarande kan du inte transformera data objekt när de automatiskt sprids från transaktions arkivet till analytisk lagring. Om du har scenarier som blockeras av den här begränsningen, e-posta [Azure Cosmos DB-teamet](mailto:cosmosdbsynapselink@microsoft.com).

### <a name="is-analytical-store-supported-by-terraform"></a>Har Terraform stöd för analytisk lagring?

Terraform har för närvarande inte stöd för analytiska lagringscontainrar. Läs mer i [GitHub-problem för Terraform](https://github.com/hashicorp/terraform/issues).

## <a name="analytical-time-to-live-ttl"></a>TTL-värde (Analytical Time to Live)

### <a name="is-ttl-for-analytical-data-supported-at-both-container-and-item-level"></a>Stöds ett TTl-värde för analys data på både container-och objekt nivå?

För närvarande kan TTl för analys data bara konfigureras på container nivå och det finns inget stöd för att ange analytiskt TTL på objekt nivå.

### <a name="after-setting-the-container-level--analytical-ttl-on-an-azure-cosmos-db-container-can-i-change-to-a-different-value-later"></a>Kan jag ändra till ett annat värde senare när du har angett ett analytiskt TTL-värde för container nivå på en Azure Cosmos DB-behållare?

Ja, analytiskt TTL-värde kan uppdateras till ett giltigt värde. Se artikeln om [analytiskt TTL](analytical-store-introduction.md#analytical-ttl) för mer information om analys-TTL.

### <a name="can-i-update-or-delete-an-item-from-the-analytical-store-after-it-has-been-ttld-out-from-the-transactional-store"></a>Kan jag uppdatera eller ta bort ett objekt från analys arkivet efter att det har varit TTL från transaktions lagringen?

Alla transaktionella uppdateringar och borttagningar kopieras till analys lagret, men om objektet har rensats från transaktions lagret kan det inte uppdateras i analys lagret. Mer information finns i artikeln om [analys-TTL](analytical-store-introduction.md#analytical-ttl) .

## <a name="billing"></a>Fakturering

### <a name="what-is-the-billing-model-of-azure-synapse-link-for-azure-cosmos-db"></a>Vad är fakturerings modellen för Azure Synapse-länken för Azure Cosmos DB?

[Azure Cosmos DB Analytical Store](analytical-store-introduction.md) är tillgängligt i den offentliga för hands versionen utan några avgifter för analys butiken fram till den 30 augusti 2020. Synapse Spark och Synapse SQL debiteras via [Synapse service-förbrukning](https://azure.microsoft.com/pricing/details/synapse-analytics/).

## <a name="security"></a>Säkerhet

### <a name="what-are-the-ways-to-authenticate-with-the-analytical-store"></a>Vilka är de olika sätten att autentisera med analys lagret?

Autentisering med analys lagret är detsamma som i ett transaktions lager. För en specifik databas kan du autentisera med den primära eller skrivskyddade nyckeln. Du kan utnyttja den länkade tjänsten i Azure Synapse Studio för att förhindra att Azure Cosmos DB nycklar klistras in i Spark-anteckningsbokarna. Åtkomst till den här länkade tjänsten är tillgänglig för alla som har åtkomst till arbets ytan.

## <a name="synapse-run-times"></a>Synapse körnings tider

### <a name="what-are-the-currently-supported-synapse-run-times-to-access-azure-cosmos-db-analytical-store"></a>Vilka Synapse-körningar som stöds just nu för att komma åt Azure Cosmos DB Analytical Store?

|Azure Synapse runtime |Aktuellt stöd |
|---------|---------|
|Azure Synapse Spark-pooler | Läsa, skriva (via transaktions lager), tabell, tillfällig vy |
|Azure Synapse SQL Server-pooler    | Läsa, Visa |
|Azure Synapse SQL etablerad   |  Inte tillgängligt |

### <a name="do-my-azure-synapse-spark-tables-sync-with-my-azure-synapse-sql-serverless-tables-the-same-way-they-do-with-azure-data-lake"></a>Synkroniserar mina Azure Synapse Spark-tabeller med mina Azure Synapse SQL Server-tabeller på samma sätt som med Azure Data Lake?

Den här funktionen är för närvarande inte tillgänglig.

### <a name="can-i-do-spark-structured-streaming-from-analytical-store"></a>Kan jag använda Spark-strukturerad strömning från analys lager?

För närvarande används Spark-stöd för direkt uppspelning för Azure Cosmos DB implementeras med hjälp av funktionen ändra feed i transaktions arkivet och det stöds ännu inte från analytisk lagring.

## <a name="azure-synapse-studio"></a>Azure Synapse Studio

### <a name="in-the-azure-synapse-studio-how-do-i-recognize-if-im-connected-to-an-azure-cosmos-db-container-with-the-analytics-store-enabled"></a>Hur känner jag igen om jag är ansluten till en Azure Cosmos DB behållare med Analytics Store aktiverat i Azure Synapse Studio?

En Azure Cosmos DB behållare som är aktive rad med analys lager har följande ikon:

:::image type="content" source="./media/synapse-link-frequently-asked-questions/analytical-store-icon.png" alt-text="Azure Cosmos DB behållare aktive rad med analys lager – ikon":::

En transaktions Arkiv behållare visas med följande ikon:

:::image type="content" source="./media/synapse-link-frequently-asked-questions/transactional-store-icon.png" alt-text="Azure Cosmos DB behållare aktive rad med transaktions lager – ikon":::
 
### <a name="how-do-you-pass-azure-cosmos-db-credentials-from-azure-synapse-studio"></a>Hur skickar du Azure Cosmos DB autentiseringsuppgifter från Azure Synapse Studio?

Azure Cosmos DB autentiseringsuppgifter skickas för närvarande när den länkade tjänsten skapas av användaren som har åtkomst till Azure Cosmos DB databaserna. Åtkomst till det arkivet är tillgänglig för andra användare som har åtkomst till arbets ytan.

## <a name="next-steps"></a>Nästa steg

* Lär dig mer om [fördelarna med Azure Synapse-länk](synapse-link.md#synapse-link-benefits)

* Lär dig mer om [integreringen mellan Azure Synapse-länken och Azure Cosmos DB](synapse-link.md#synapse-link-integration).