---
title: ta med fil
description: ta med fil
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 04/13/2018
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: 88751efdd5aaceddeed490c95d15d82a263fa81e
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38733836"
---
1. Logga in på [Azure Portal](https://portal.azure.com/) i ett nytt webbläsarfönster.

2. Klicka på **Skapa en resurs** > **Databaser** > **Azure Cosmos DB**.
   
   ![Panelen ”Databaser” i Azure-portalen](./media/cosmos-db-create-dbaccount-graph/create-nosql-db-databases-json-tutorial-1.png)

3. På sidan **Nytt konto** anger du inställningarna för det nya Azure Cosmos DB-kontot. 

    Inställning|Föreslaget värde|Beskrivning
    ---|---|---
    ID|*Ange ett unikt namn*|Ange ett unikt namn som identifierar Azure Cosmos DB-kontot. Eftersom*documents.azure.com* läggs till det ID du anger för att skapa din URI ska du använda ett unikt men identifierbart ID.<br><br>Ditt id får bara innehålla gemener, siffror och bindestreck och måste innehålla mellan 3 och 50 tecken.
    API|Gremlin (graf)|API:n avgör vilken typ av konto som skapas. Azure Cosmos DB innehåller fem API:er för ditt program: SQL (dokumentdatabas), Gremlin (grafdatabas), MongoDB (dokumentdatabas), Azure-tabell och Cassandra, där var och en för närvarande kräver ett separat konto. <br><br>Du väljer **Gremlin (graf)** eftersom du i den här snabbstarten ska skapa en graf som är frågningsbar med Gremlin-syntax.<br><br>[Läs mer om Graph API](../articles/cosmos-db/graph-introduction.md)
    Prenumeration|*Din prenumeration*|Välj den Azure-prenumeration som ska användas för det här Azure Cosmos DB-kontot. 
    Resursgrupp|Skapa ny<br><br>*Ange sedan samma unika namn som angavs ovan i ID:t*|Välj **Skapa ny** och ange sedan ett nytt resursgruppsnamn för ditt konto. För enkelhetens skull kan du använda samma namn som för ditt ID.
    Plats|*Välj den region som är närmast dina användare*|Välj den geografiska plats som ska vara värd för ditt Azure Cosmos DB-konto. Använd den plats som är närmast dina användare så att de får så snabb åtkomst till data som möjligt.
    Aktivera geo-redundans| Lämna tomt | Detta skapar en replikerad version av databasen i en andra (parad) region. Låt den vara tom.  
    Fäst vid instrumentpanelen | Välj | Markera den här kryssrutan så att ditt nya databaskonto läggs till på portalens instrumentpanel för enkel åtkomst.

    Klicka sedan på **Skapa**.

    ![Det nya kontobladet för Azure Cosmos DB](./media/cosmos-db-create-dbaccount-graph/azure-cosmos-db-create-new-account.png)

4. Det tar några minuter att skapa kontot. Vänta tills portalen visar sidan **Grattis! Azure Cosmos DB-kontot har skapats**.

    ![Meddelandefönstret i Azure-portalen](./media/cosmos-db-create-dbaccount-graph/azure-cosmos-db-graph-created.png)