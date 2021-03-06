---
title: Azure Stream Analytics-utdata till Cosmos DB
description: Den här artikeln beskriver hur du använder Azure Stream Analytics för att spara utdata till Azure Cosmos DB för JSON-utdata för dataarkivering och frågor med låg latens för Ostrukturerade JSON-data.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/28/2017
ms.openlocfilehash: 95cfc7e6d9515274aa7a3c5fde382244f3b33fab
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/20/2018
ms.locfileid: "42058259"
---
# <a name="azure-stream-analytics-output-to-azure-cosmos-db"></a>Azure Stream Analytics-utdata till Azure Cosmos DB  
Stream Analytics kan riktas mot [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) att aktivera arkivering och låg latens datafrågor för Ostrukturerade JSON-data för JSON-utdata. Det här dokumentet beskriver några av metodtipsen för att implementera den här konfigurationen.

För de som inte är bekant med Cosmos DB, ta en titt på [Utbildningsväg för Azure Cosmos DB](https://azure.microsoft.com/documentation/learning-paths/documentdb/) att komma igång. 

> [!Note]
> Just nu, Azure Stream Analytics endast stöd för anslutning till Azure Cosmos DB med hjälp av **SQL API**.
> Andra Azure Cosmos DB API: er stöds inte ännu. Om du punkt Azure Stream Analytics till Azure Cosmos DB-konton som skapats med andra API: er, kanske data inte korrekt lagras. 

## <a name="basics-of-cosmos-db-as-an-output-target"></a>Grunderna i Cosmos DB som en utdatamål
Azure Cosmos DB-utdata i Stream Analytics kan skriva dina stream bearbetning resultat som JSON-utdata till Cosmos DB-samling(ar). Stream Analytics skapa inte samlingar i databasen, i stället kräver att du skapar dem i förskott. Det här är så att faktureringen kostnaderna för Cosmos DB-samlingar styrs av dig och så att du kan finjustera prestanda, konsekvens och kapaciteten för dina samlingar med direkt med hjälp av den [Cosmos DB API: er](https://msdn.microsoft.com/library/azure/dn781481.aspx). 

Några av alternativen för Cosmos DB-samling beskrivs nedan.

## <a name="tune-consistency-availability-and-latency"></a>Finjustera konsekvens, tillgänglighet och svarstid
För att matcha dina programkrav, kan Azure Cosmos DB du justera databasen och samlingar och kompromissa mellan konsekvens, tillgänglighet och svarstid. Beroende på vilka nivåer av läsningskontinuitet behoven scenariot mot läser och skriver svarstid, du kan välja en konsekvensnivå på ditt databaskonto. Som standard kan Azure Cosmos DB också synkron indexering för varje CRUD-åtgärd i din samling. Det här är ett annat användbart alternativ för att styra skrivning/läsning prestanda i Azure Cosmos DB. Mer information finns i [ändra din databas och fråga konsekvensnivåer](../cosmos-db/consistency-levels.md) artikeln.

## <a name="upserts-from-stream-analytics"></a>Upsertar från Stream Analytics
Stream Analytics-integrering med Azure Cosmos DB kan du infoga eller uppdatera poster i din samling baserat på en viss dokument-ID-kolumn. Detta är kallas även en *Upsert*.

Stream Analytics använder en optimistisk upsert-metod, där uppdateringar utförs endast när insert misslyckas med ett dokument-ID-konflikt. Den här uppdateringen utförs som en uppdatering, så att den kan deluppdateringar till dokumentet, det vill säga, Lägg till nya egenskaper eller ersätta en befintlig egenskap utförs inkrementellt. Ändringar i värdena för matris egenskaper i JSON-dokument-resultat i hela matrisen komma över, det vill säga matrisen inte slagit samman.

## <a name="data-partitioning-in-cosmos-db"></a>Datapartitionering i Cosmos DB
Azure Cosmos DB [obegränsad](../cosmos-db/partition-data.md) är den rekommenderade metoden för att partitionera dina data, som Azure Cosmos DB automatiskt skalar partitioner baserat på din arbetsbelastning. Vid skrivning till obegränsade behållare, använder Stream Analytics så många parallella skrivare som tidigare frågesteg eller indata som partitioneringsschema.
> [!Note]
> För närvarande stöder Azure Stream Analytics endast obegränsade samlingar med partitionsnycklar på den översta nivån. Till exempel `/region` stöds. Kapslade partitionsnycklar (t.ex. `/region/name`) stöds inte. 

För fasta Azure Cosmos DB-samlingar kan Stream Analytics inget sätt att skala upp eller ut när de är full. De har en övre gräns på 10 GB och 10 000 RU/s genomströmning.  Om du vill migrera data från en fast behållare till en obegränsad behållare (till exempel en med minst 1 000 RU/s och en partitionsnyckel), måste du använda den [datamigreringsverktyget](../cosmos-db/import-data.md) eller [ändringsfeed biblioteket](../cosmos-db/change-feed.md).

Skrivning till flera fast behållare tas ur bruk och är inte den rekommenderade metoden för att skala ut ditt Stream Analytics-jobb. Artikeln [partitionering och skalning i Cosmos DB](../cosmos-db/sql-api-partition-data.md) innehåller ytterligare information.

## <a name="cosmos-db-settings-for-json-output"></a>Cosmos DB-inställningar för JSON-utdata
Skapar Cosmos DB som utdata i Stream Analytics genererar en uppmaning information som visas nedan. Det här avsnittet innehåller en förklaring av egenskaper-definition.


![documentdb stream analytics utdata skärmen](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-1.png)

Fält           | Beskrivning 
-------------   | -------------
Utdataalias    | Ett alias för att referera dessa utdata i frågan ASA   
Kontonamn    | Namn eller slutpunkt URI: N för Azure Cosmos DB-konto 
Kontonyckel     | Den delade åtkomstnyckeln för Azure Cosmos DB-konto
Databas        | Azure Cosmos DB-databasnamn
Samlingsnamn | Samlingsnamn för samlingen som ska användas. `MyCollection` är en giltig inmatning för exemplet – en samling som heter `MyCollection` måste finnas.  
Dokument-ID     | Valfri. Kolumnnamnet i utdatahändelserna används som den unika nyckeln åtgärder måste baseras på vilken insert eller update. Om fältet lämnas tomt kommer alla händelser att infogas, där du inte uppdateringen.
