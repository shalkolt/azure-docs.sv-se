---
title: ta med fil
description: ta med fil
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 09/14/2018
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: a63937169097bc779dd285957420617f57ee76be
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47002239"
---
1. Logga in på [Azure Portal](https://portal.azure.com/) i ett nytt webbläsarfönster.

2. Välj **Skapa en resurs** > **Databaser** > **Azure Cosmos DB**.
   
   ![Azure Portal-databasfönstret](./media/cosmos-db-create-dbaccount-cassandra/create-nosql-db-databases-json-tutorial-1.png)

3. På sidan **Nytt konto** anger du inställningarna för det nya Azure Cosmos DB-kontot. 
 
    Inställning|Föreslaget värde|Beskrivning
    ---|---|---
    ID|*Ange ett unikt namn*|Ange ett unikt namn som identifierar Azure Cosmos DB-kontot. Eftersom*cassandra.cosmosdb.azure.com* läggs till det ID du anger för att skapa din kontaktpunkt bör du använda ett unikt men identifierbart ID.<br><br>Ditt id får bara innehålla gemener, siffror och bindestreck och måste innehålla mellan 3 och 50 tecken.
    API|Cassandra|API:n avgör vilken typ av konto som skapas. Azure Cosmos DB innehåller fem API:er för ditt program: SQL (dokumentdatabas), Gremlin (grafdatabas), MongoDB (dokumentdatabas), Azure-tabell och Cassandra, där var och en för närvarande kräver ett separat konto. <br><br>Välj **Cassandra** eftersom du i den här snabbstarten ska skapa en databas med breda kolumner som är frågningsbar med CQL-syntax.<br><br>[Läs mer om Cassandra-API:n](../articles/cosmos-db/cassandra-introduction.md).|
    Prenumeration|*Din prenumeration*|Välj den Azure-prenumeration som ska användas för det här Azure Cosmos DB-kontot. 
    Resursgrupp|Skapa ny<br><br>*Ange sedan samma unika namn som angavs ovan i ID:t*|Välj **Skapa ny** och ange sedan ett nytt resursgruppsnamn för ditt konto. För enkelhetens skull kan du använda samma namn som för ditt ID. 
    Plats|*Välj den region som är närmast dina användare*|Välj den geografiska plats som ska vara värd för ditt Azure Cosmos DB-konto. Använd den plats som är närmast dina användare så att de får så snabb åtkomst till data som möjligt.
    Fäst vid instrumentpanelen | Välj | Markera den här kryssrutan så att ditt nya databaskonto läggs till på portalens instrumentpanel för enkel åtkomst.

    Klicka sedan på **Skapa**.

    ![Den nya kontosidan för Azure Cosmos DB](./media/cosmos-db-create-dbaccount-cassandra/azure-cosmos-db-create-new-account.png)

4. Det tar några minuter att skapa kontot. Vänta tills portalen visar sidan **Grattis! Azure Cosmos DB-kontot har skapats**.

