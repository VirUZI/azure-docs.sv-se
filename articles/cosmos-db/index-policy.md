---
title: Azure Cosmos DB indexerings principer
description: Lär dig hur du konfigurerar och ändrar standard indexerings principen för automatisk indexering och bättre prestanda i Azure Cosmos DB.
author: timsander1
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 11/03/2020
ms.author: tisande
ms.openlocfilehash: 9e62d6c475a4aeb366d034af1c80fc728f1a9211
ms.sourcegitcommit: fa90cd55e341c8201e3789df4cd8bd6fe7c809a3
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/04/2020
ms.locfileid: "93335831"
---
# <a name="indexing-policies-in-azure-cosmos-db"></a>Indexeringsprinciper i Azure Cosmos DB
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

I Azure Cosmos DB har varje container en indexeringspolicy som avgör hur containerns objekt ska indexeras. Standardprincipen för indexering för nyligen skapade containrar indexerar alla egenskaper för alla objekt och tillämpar intervallindex för alla strängar och tal. På så sätt kan du få bättre prestanda för frågor utan att behöva tänka på indexering och index hantering.

I vissa fall kan det vara bra att åsidosätta det här automatiska beteendet så att det passar dina behov bättre. Du kan anpassa en behållares indexerings princip genom att ställa in dess *indexerings läge* och ta med eller undanta *egenskaps Sök vägar*.

> [!NOTE]
> Metoden för att uppdatera indexerings principer som beskrivs i den här artikeln gäller endast Azure Cosmos DB s SQL-API (Core). Läs mer om indexering i [Azure Cosmos DBS API för MongoDB](mongodb-indexing.md)

## <a name="indexing-mode"></a>Indexerings läge

Azure Cosmos DB stöder två indexerings lägen:

- **Konsekvent** : indexet uppdateras synkront när du skapar, uppdaterar eller tar bort objekt. Det innebär att konsekvensen för dina Läs frågor är den [konsekvens som kon figurer ATS för kontot](consistency-levels.md).
- **Ingen** : indexering har inaktiverats för behållaren. Detta används vanligt vis när en behållare används som ett rent nyckel värdes lager utan behov av sekundära index. Det kan också användas för att förbättra prestandan för Mass åtgärder. När Mass åtgärderna har slutförts kan index läget anges till konsekvent och övervakas med hjälp av [IndexTransformationProgress](how-to-manage-indexing-policy.md#dotnet-sdk) tills det är klart.

> [!NOTE]
> Azure Cosmos DB stöder också ett Lazy-indexerings läge. Lazy-indexering utför uppdateringar av indexet på en mycket lägre prioritetsnivå när motorn inte utför något annat arbete. Detta kan ge **inkonsekventa eller ofullständiga** frågeresultat. Om du planerar att fråga en Cosmos-container bör du inte välja Lazy-indexering. Nya behållare kan inte välja Lazy-indexering. Du kan begära ett undantag genom att kontakta [Azure-supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) (förutom om du använder ett Azure Cosmos-konto i ett [Server](serverless.md) fritt läge som inte stöder Lazy-indexering).

Indexerings principen är som standard inställd på `automatic` . Den uppnås genom att ställa in `automatic` egenskapen i indexerings principen på `true` . Genom att ange den här egenskapen kan `true` Azure-CosmosDB automatiskt indexera dokument när de skrivs.

## <a name="including-and-excluding-property-paths"></a><a id="include-exclude-paths"></a>Inklusive och exklusive egenskaps Sök vägar

En anpassad indexerings princip kan ange egenskaps Sök vägar som uttryckligen tas med eller undantas från indexering. Genom att optimera antalet sökvägar som indexeras kan du avsevärt minska svars tiden och RU-avgiften för Skriv åtgärder. Dessa sökvägar definieras enligt [metoden som beskrivs i avsnittet indexerings översikt](index-overview.md#from-trees-to-property-paths) med följande tillägg:

- en sökväg som leder till ett skalärt värde (sträng eller siffra) slutar med `/?`
- element från en matris behandlas tillsammans genom `/[]` notationen (i stället för `/0` `/1` osv.)
- `/*`jokertecknet kan användas för att matcha alla element under noden

Ta samma exempel igen:

```
    {
        "locations": [
            { "country": "Germany", "city": "Berlin" },
            { "country": "France", "city": "Paris" }
        ],
        "headquarters": { "country": "Belgium", "employees": 250 }
        "exports": [
            { "city": "Moscow" },
            { "city": "Athens" }
        ]
    }
```

- `headquarters` `employees` sökvägen är`/headquarters/employees/?`

- `locations` `country` sökvägen är`/locations/[]/country/?`

- sökvägen till något under `headquarters` är `/headquarters/*`

Vi kan till exempel inkludera `/headquarters/employees/?` sökvägen. Den här sökvägen ser till att vi indexerar egenskapen anställda men skulle inte indexera ytterligare kapslad JSON i den här egenskapen.

## <a name="includeexclude-strategy"></a>Inkludera/exkludera strategi

Alla indexerings principer måste innehålla rot Sök vägen `/*` antingen som en inkluderad eller undantagen sökväg.

- Inkludera rot Sök vägen för att selektivt exkludera sökvägar som inte behöver indexeras. Detta är den rekommenderade metoden eftersom Azure Cosmos DB indexera alla nya egenskaper som kan läggas till i din modell proaktivt.
- Undanta rot Sök vägen för att selektivt inkludera sökvägar som behöver indexeras.

- För sökvägar med vanliga tecken som innehåller alfanumeriska tecken och _ (under streck) behöver du inte undanta Sök vägs strängen runt dubbla citat tecken (till exempel "/Path/?"). För sökvägar med andra specialtecken måste du undvika Sök vägs strängen runt dubbla citat tecken (till exempel "/ \" Path-ABC \" /?"). Om du förväntar dig specialtecken i sökvägen kan du kringgå alla säkerhets vägar. Det innebär att det inte är någon skillnad om du avvisar alla sökvägar och bara de som innehåller specialtecken.

- System egenskapen `_etag` undantas från indexering som standard, om inte etag läggs till i den inkluderade sökvägen för indexering.

- Om indexerings läget är inställt på **konsekvent** `id` indexeras system egenskaperna och `_ts` indexeras automatiskt.

När du inkluderar och exkluderar sökvägar kan du stöta på följande attribut:

- `kind` kan vara antingen `range` eller `hash` . Stöd för hash-index är begränsat till likhets filter. Funktionen områdes index innehåller alla funktioner i hash-index samt effektiv sortering, intervall filter, system funktioner. Vi rekommenderar alltid att du använder ett intervall index.

- `precision` är ett tal definierat på index nivå för inkluderade sökvägar. Värdet `-1` anger maximal precision. Vi rekommenderar att alltid ange det här värdet till `-1` .

- `dataType` kan vara antingen `String` eller `Number` . Detta anger vilka typer av JSON-egenskaper som kommer att indexeras.

Om detta inte anges kommer dessa egenskaper att ha följande standardvärden:

| **Egenskaps namn**     | **Standardvärde** |
| ----------------------- | -------------------------------- |
| `kind`   | `range` |
| `precision`   | `-1`  |
| `dataType`    | `String` och `Number` |

Se [det här avsnittet](how-to-manage-indexing-policy.md#indexing-policy-examples) för indexerings princip exempel för att inkludera och exkludera sökvägar.

## <a name="includeexclude-precedence"></a>Inkludera/exkludera prioritet

Om dina inkluderade sökvägar och undantagna sökvägar har en konflikt, prioriteras den mer exakta sökvägen.

Här är ett exempel:

**Sökväg som ingår** : `/food/ingredients/nutrition/*`

**Undantagen sökväg** : `/food/ingredients/*`

I det här fallet har den medföljande sökvägen företräde framför den undantagna sökvägen eftersom den är mer exakt. Baserat på dessa sökvägar utesluts alla data i `food/ingredients` sökvägen eller kapslas in i indexet. Undantaget skulle vara data i den inkluderade sökvägen: `/food/ingredients/nutrition/*` , som skulle indexeras.

Här följer några regler för inkluderade och undantagna Sök vägs prioriteter i Azure Cosmos DB:

- Djupare sökvägar är mer exakta än smalare sökvägar. till exempel: `/a/b/?` är mer exakt än `/a/?` .

- `/?`Är mer exakt än `/*` . Till exempel `/a/?` är mer exakt än `/a/*` så prioriteras `/a/?` .

- Sökvägen `/*` måste antingen vara en inkluderad sökväg eller en undantagen sökväg.

## <a name="spatial-indexes"></a>Rums index

När du definierar en spatial sökväg i indexerings principen bör du definiera vilket index som ```type``` ska användas för den sökvägen. Möjliga typer för rums index är:

* Pekaren

* Polygonlasso

* MultiPolygon

* Lin Est ring

Azure Cosmos DB kommer som standard inte att skapa några rums index. Om du vill använda inbyggda, inbyggda SQL-funktioner, bör du skapa ett rums index för de egenskaper som krävs. Se [det här avsnittet](sql-query-geospatial-index.md) för indexerings princip exempel för att lägga till rums index.

## <a name="composite-indexes"></a>Sammansatta index

Frågor som har en `ORDER BY` sats med två eller flera egenskaper kräver ett sammansatt index. Du kan också definiera ett sammansatt index för att förbättra prestanda för många likhets-och intervall frågor. Som standard definieras inga sammansatta index så att du kan [lägga till sammansatta index](how-to-manage-indexing-policy.md#composite-indexing-policy-examples) vid behov.

Till skillnad från inkluderade eller exkluderade sökvägar kan du inte skapa en sökväg med `/*` jokertecken. Varje sammansatt sökväg har en implicit `/?` i slutet av den sökväg som du inte behöver ange. Sammansatta sökvägar leder till ett skalärt värde och det här är det enda värdet som ingår i det sammansatta indexet.

När du definierar ett sammansatt index anger du:

- Två eller flera egenskaps sökvägar. Den sekvens i vilken egenskaps Sök vägar definieras.

- Ordningen (stigande eller fallande).

> [!NOTE]
> När du lägger till ett sammansatt index använder frågan befintliga intervall index tills det nya sammansatta indexet har slutförts. När du lägger till ett sammansatt index kan du därför inte omedelbart Observera prestanda förbättringar. Det är möjligt att spåra förloppet för index omvandlingen [med hjälp av en av SDK: erna](how-to-manage-indexing-policy.md).

### <a name="order-by-queries-on-multiple-properties"></a>Sortera efter frågor på flera egenskaper:

Följande överväganden används när du använder sammansatta index för frågor med en `ORDER BY` sats med två eller fler egenskaper:

- Om de sammansatta index Sök vägarna inte matchar ordningen på egenskaperna i `ORDER BY` -satsen kan inte det sammansatta indexet stödja frågan.

- Ordningen för sammansatta index Sök vägar (stigande eller fallande) måste också matcha `order` i- `ORDER BY` satsen.

- Det sammansatta indexet stöder också en `ORDER BY` sats med motsatt ordning på alla sökvägar.

Tänk på följande exempel där ett sammansatt index definieras för egenskaper, ålder och _ts:

| **Sammansatt index**     | **Exempel `ORDER BY` fråga**      | **Stöds av sammansatt index?** |
| ----------------------- | -------------------------------- | -------------- |
| ```(name ASC, age ASC)```   | ```SELECT * FROM c ORDER BY c.name ASC, c.age asc``` | ```Yes```            |
| ```(name ASC, age ASC)```   | ```SELECT * FROM c ORDER BY c.age ASC, c.name asc```   | ```No```             |
| ```(name ASC, age ASC)```    | ```SELECT * FROM c ORDER BY c.name DESC, c.age DESC``` | ```Yes```            |
| ```(name ASC, age ASC)```     | ```SELECT * FROM c ORDER BY c.name ASC, c.age DESC``` | ```No```             |
| ```(name ASC, age ASC, timestamp ASC)``` | ```SELECT * FROM c ORDER BY c.name ASC, c.age ASC, timestamp ASC``` | ```Yes```            |
| ```(name ASC, age ASC, timestamp ASC)``` | ```SELECT * FROM c ORDER BY c.name ASC, c.age ASC``` | ```No```            |

Du bör anpassa indexerings principen så att du kan hantera alla nödvändiga `ORDER BY` frågor.

### <a name="queries-with-filters-on-multiple-properties"></a>Frågor med filter för flera egenskaper

Om en fråga har filter på två eller fler egenskaper kan det vara bra att skapa ett sammansatt index för dessa egenskaper.

Överväg till exempel följande fråga som har ett likhets filter för två egenskaper:

```sql
SELECT * FROM c WHERE c.name = "John" AND c.age = 18
```

Den här frågan är mer effektiv, tar mindre tid och använder färre RU-objekt, om det går att använda ett sammansatt index på (namn ASC, ålder ASC).

Frågor med intervall filter kan också optimeras med ett sammansatt index. Frågan kan dock bara ha ett enda intervall filter. Intervall filter är,,,, `>` `<` `<=` `>=` och `!=` . Range-filtret bör definieras sist i det sammansatta indexet.

Tänk på följande fråga med både likhets-och intervall filter:

```sql
SELECT * FROM c WHERE c.name = "John" AND c.age > 18
```

Den här frågan är mer effektiv med ett sammansatt index på (namn ASC, ålder ASC). Frågan skulle dock inte använda ett sammansatt index på (ålder ASC, Name ASC) eftersom likhets filtren måste definieras först i det sammansatta indexet.

Följande överväganden används när du skapar sammansatta index för frågor med filter för flera egenskaper

- Egenskaperna i frågans filter ska matcha dem i sammansatt index. Om en egenskap finns i det sammansatta indexet men inte ingår i frågan som ett filter, används inte det sammansatta indexet i frågan.
- Om en fråga har ytterligare egenskaper i filtret som inte har definierats i ett sammansatt index, kommer en kombination av sammansatta och intervall index att användas för att utvärdera frågan. Detta kräver färre RU: s än uteslutande med intervall index.
- Om en egenskap har ett intervall filter ( `>` ,,, `<` `<=` `>=` eller `!=` ), ska den här egenskapen definieras sist i det sammansatta indexet. Om en fråga har fler än ett intervall filter, används inte det sammansatta indexet.
- När du skapar ett sammansatt index för att optimera frågor med flera filter påverkas `ORDER` inte resultatet av det sammansatta indexet. Den här egenskapen är valfri.
- Om du inte definierar ett sammansatt index för en fråga med filter på flera egenskaper kommer frågan fortfarande att lyckas. RU-kostnaden för frågan kan dock minskas med ett sammansatt index.
- Frågor med båda agg regeringar (till exempel COUNT eller SUM) och filter drar också fördel av sammansatta index.

Tänk på följande exempel där ett sammansatt index definieras för egenskaper, ålder och tidsstämpel:

| **Sammansatt index**     | **Exempel fråga**      | **Stöds av sammansatt index?** |
| ----------------------- | -------------------------------- | -------------- |
| ```(name ASC, age ASC)```   | ```SELECT * FROM c WHERE c.name = "John" AND c.age = 18``` | ```Yes```            |
| ```(name ASC, age ASC)```   | ```SELECT * FROM c WHERE c.name = "John" AND c.age > 18```   | ```Yes```             |
| ```(name ASC, age ASC)```   | ```SELECT COUNT(1) FROM c WHERE c.name = "John" AND c.age > 18```   | ```Yes```             |
| ```(name DESC, age ASC)```    | ```SELECT * FROM c WHERE c.name = "John" AND c.age > 18``` | ```Yes```            |
| ```(name ASC, age ASC)```     | ```SELECT * FROM c WHERE c.name != "John" AND c.age > 18``` | ```No```             |
| ```(name ASC, age ASC, timestamp ASC)``` | ```SELECT * FROM c WHERE c.name = "John" AND c.age = 18 AND c.timestamp > 123049923``` | ```Yes```            |
| ```(name ASC, age ASC, timestamp ASC)``` | ```SELECT * FROM c WHERE c.name = "John" AND c.age < 18 AND c.timestamp = 123049923``` | ```No```            |

### <a name="queries-with-a-filter-as-well-as-an-order-by-clause"></a>Frågor med ett filter och en ORDER BY-sats

Om en fråga filtrerar på en eller flera egenskaper och har olika egenskaper i ORDER BY-satsen kan det vara bra att lägga till egenskaperna i filtret till- `ORDER BY` satsen.

Genom att till exempel lägga till egenskaperna i filtret till ORDER BY-satsen kan följande fråga skrivas om för att utnyttja ett sammansatt index:

Fråga med intervall index:

```sql
SELECT * FROM c WHERE c.name = "John" ORDER BY c.timestamp
```

Fråga med sammansatt index:

```sql
SELECT * FROM c WHERE c.name = "John" ORDER BY c.name, c.timestamp
```

Samma mönster och fråge optimeringar kan generaliseras för frågor med flera likhets filter:

Fråga med intervall index:

```sql
SELECT * FROM c WHERE c.name = "John", c.age = 18 ORDER BY c.timestamp
```

Fråga med sammansatt index:

```sql
SELECT * FROM c WHERE c.name = "John", c.age = 18 ORDER BY c.name, c.age, c.timestamp
```

Följande överväganden används när du skapar sammansatta index för att optimera en fråga med en filter-och- `ORDER BY` sats:

* Om frågan filtreras efter egenskaper bör dessa tas med först i- `ORDER BY` satsen.
* Om frågan filtrerar på flera egenskaper måste likhets filtren vara de första egenskaperna i `ORDER BY` satsen
* Om du inte definierar ett sammansatt index för en fråga med ett filter på en egenskap och en separat `ORDER BY` sats med en annan egenskap, kommer frågan fortfarande att lyckas. RU-kostnaden för frågan kan dock minskas med ett sammansatt index, särskilt om egenskapen i- `ORDER BY` satsen har en hög kardinalitet.
* Alla överväganden för att skapa sammansatta index för `ORDER BY` frågor med flera egenskaper och frågor med filter för flera egenskaper gäller fortfarande.


| **Sammansatt index**                      | **Exempel `ORDER BY` fråga**                                  | **Stöds av sammansatt index?** |
| ---------------------------------------- | ------------------------------------------------------------ | --------------------------------- |
| ```(name ASC, timestamp ASC)```          | ```SELECT * FROM c WHERE c.name = "John" ORDER BY c.name ASC, c.timestamp ASC``` | `Yes` |
| ```(name ASC, timestamp ASC)```          | ```SELECT * FROM c WHERE c.name = "John" AND c.timestamp > 1589840355 ORDER BY c.name ASC, c.timestamp ASC``` | `Yes` |
| ```(timestamp ASC, name ASC)```          | ```SELECT * FROM c WHERE c.timestamp > 1589840355 AND c.name = "John" ORDER BY c.timestamp ASC, c.name ASC``` | `No` |
| ```(name ASC, timestamp ASC)```          | ```SELECT * FROM c WHERE c.name = "John" ORDER BY c.timestamp ASC, c.name ASC``` | `No`  |
| ```(name ASC, timestamp ASC)```          | ```SELECT * FROM c WHERE c.name = "John" ORDER BY c.timestamp ASC``` | ```No```   |
| ```(age ASC, name ASC, timestamp ASC)``` | ```SELECT * FROM c WHERE c.age = 18 and c.name = "John" ORDER BY c.age ASC, c.name ASC,c.timestamp ASC``` | `Yes` |
| ```(age ASC, name ASC, timestamp ASC)``` | ```SELECT * FROM c WHERE c.age = 18 and c.name = "John" ORDER BY c.timestamp ASC``` | `No` |

## <a name="modifying-the-indexing-policy"></a>Ändra indexerings principen

En behållares indexerings princip kan uppdateras när [som helst genom att använda Azure Portal eller någon av de SDK](how-to-manage-indexing-policy.md): er som stöds. En uppdatering av indexerings principen utlöser en omvandling från det gamla indexet till den nya, som utförs online och på plats (så att ingen ytterligare lagrings utrymme förbrukas under driften). Den gamla indexerings principen omvandlas effektivt till den nya principen utan att det påverkar Skriv tillgänglighet, Läs tillgänglighet eller det data flöde som har allokerats på behållaren. Omvandling av index är en asynkron åtgärd och den tid det tar att slutföra beror på det etablerade data flödet, antalet objekt och deras storlek.

> [!IMPORTANT]
> Omvandling av index är en åtgärd som förbrukar [enheter för programbegäran](request-units.md). Enheter för programbegäran som används av en index omvandling faktureras inte för tillfället om du använder [Server](serverless.md) lös behållare. Dessa enheter för programbegäran debiteras när servern är allmänt tillgänglig.

> [!NOTE]
> Det är möjligt att spåra förloppet för index omvandlingen [med hjälp av en av SDK: erna](how-to-manage-indexing-policy.md).

Det påverkar inte Skriv tillgängligheten under index transformationer. Index omvandlingen använder din etablerade ru: er men har lägre prioritet än dina CRUD-åtgärder eller frågor.

Det finns ingen inverkan på Läs tillgänglighet när du lägger till ett nytt index. Frågor använder bara nya index när index omvandlingen har slutförts. Vid omvandlingen av index fortsätter Frågeredigeraren att använda befintliga index, så du ser liknande Läs prestanda under indexerings omvandlingen till det du hade observerat innan du påbörjar indexerings ändringen. När du lägger till nya index är det inte heller någon risk för ofullständiga eller inkonsekventa frågeresultat.

När du tar bort index och omedelbart kör frågor som filtrerar på de borttagna indexen, finns det ingen garanti för konsekventa eller fullständiga frågeresultat. Om du tar bort flera index och gör det i en enda princip ändring för index, ger Frågeredigeraren konsekventa och fullständiga resultat i index omvandlingen. Men om du tar bort index genom flera indexerings princip ändringar, kommer inte frågesyntaxen att tillhandahålla konsekventa eller fullständiga resultat förrän alla index omvandlingar har slutförts. De flesta utvecklare släpper inte indexen och försöker sedan omedelbart köra frågor som använder dessa index, så i praktiken är den här situationen osannolik.

> [!NOTE]
> Om möjligt bör du alltid försöka gruppera flera indexerings ändringar i en enda princip ändring för indexering

## <a name="indexing-policies-and-ttl"></a>Indexerings principer och TTL

Användning av [TTL-funktionen (Time-to-Live)](time-to-live.md) kräver indexering. Det innebär att:

- Det går inte att aktivera TTL på en behållare där indexerings läget är inställt på `none` ,
- Det går inte att ställa in indexerings läget på none i en behållare där TTL har Aktiver ATS.

För scenarier där ingen egenskaps Sök väg behöver indexeras, men TTL krävs, kan du använda en indexerings princip med ett indexerings läge inställt på `consistent` , inga inkluderade sökvägar och `/*` som den enda undantagna sökvägen.

## <a name="next-steps"></a>Nästa steg

Läs mer om indexering i följande artiklar:

- [Indexeringsöversikt](index-overview.md)
- [Hantera indexerings principen](how-to-manage-indexing-policy.md)
