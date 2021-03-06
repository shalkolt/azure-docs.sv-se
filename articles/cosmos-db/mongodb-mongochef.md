---
title: Använda Studio 3T (MongoChef) med Azure Cosmos DB | Microsoft Docs
description: Lär dig att använda Studio 3T med ett Azure Cosmos DB MongoDB API-konto
keywords: mongochef studio 3T
services: cosmos-db
author: slyons
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: na
ms.topic: conceptual
ms.date: 01/18/2018
ms.author: sclyon
ms.openlocfilehash: af66f413055c01d9a4c3e94c7d3b9c5f1d5917b0
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/10/2018
ms.locfileid: "42054134"
---
# <a name="azure-cosmos-db-use-studio-3t-with-a-mongodb-api-account"></a>Azure Cosmos DB: Använda Studio 3T med ett MongoDB API-konto

Om du vill ansluta till ett Azure Cosmos DB MongoDB API-konto, måste du:

* Ladda ned och installera [Studio 3T](https://studio3t.com/) (kallades tidigare MongoChef)
* Har Azure Cosmos DB [anslutningssträngen](connect-mongodb-account.md) information för din MongoDB-kontot

## <a name="create-the-connection-in-studio-3t"></a>Skapa anslutningen i Studio 3T
Om du vill lägga till ditt Azure Cosmos DB-konto i Studio 3T connection manager utför du följande steg:

1. Hämta Azure Cosmos DB-anslutningsinformationen för din MongoDB API-konto med hjälp av anvisningarna i den [Anslut en MongoDB-program till Azure Cosmos DB](connect-mongodb-account.md) artikeln.

    ![Skärmbild av sidan med anslutningssträngen](./media/mongodb-mongochef/ConnectionStringBlade.png)
2. Klicka på **Connect** för att öppna hanteraren för anslutning, klicka sedan på **ny anslutning**

    ![Skärmbild av Anslutningshanteraren för Studio 3T](./media/mongodb-mongochef/ConnectionManager.png)
3. I den **ny anslutning** fönstret på den **Server** ange värden (FQDN) för Azure Cosmos DB-kontot och PORT.

    ![Skärmbild som visar fliken Studio 3T connection manager server](./media/mongodb-mongochef/ConnectionManagerServerTab.png)
4. I den **ny anslutning** fönstret på den **autentisering** fliken, välja autentiseringsläge **Basic (MONGODB-CR eller SCARM-SHA-1)** och ange användarnamn och lösenord.  Acceptera standardvärdet autentisering db (admin) eller ange ett eget värde.

    ![Skärmbild som visar fliken Studio 3T connection manager autentisering](./media/mongodb-mongochef/ConnectionManagerAuthenticationTab.png)
5. I den **ny anslutning** fönstret på den **SSL** fliken, kontrollera den **Använd SSL-protokollet för att ansluta** markerar du kryssrutan och **accepterar server självsignerat SSL-certifikat**  alternativknappen.

    ![Skärmbild som visar fliken Studio 3T anslutning manager SSL](./media/mongodb-mongochef/ConnectionManagerSSLTab.png)
6. Klicka på den **Testanslutningen** knappen för att verifiera anslutningsinformationen och klickar på **OK** tillbaka till fönstret Ny anslutning och klicka sedan på **spara**.

    ![Skärmbild av fönstret Studio 3T Testa anslutning](./media/mongodb-mongochef/TestConnectionResults.png)

## <a name="use-studio-3t-to-create-a-database-collection-and-documents"></a>Använda Studio 3T för att skapa en databas, samling och dokument
Om du vill skapa en databas, samling och dokument med hjälp av Studio 3T, utför du följande steg:

1. I **Anslutningshanteraren**, markera anslutningen och klicka på **Connect**.

    ![Skärmbild av Anslutningshanteraren för Studio 3T](./media/mongodb-mongochef/ConnectToAccount.png)
2. Högerklicka på värden och välj **lägga till databas**.  Ange ett databasnamn och klicka på **OK**.

    ![Skärmbild som visar alternativet Studio 3T Lägg till databas](./media/mongodb-mongochef/AddDatabase1.png)
3. Högerklicka på databasen och välj **Lägg till samling**.  Ange ett samlingsnamn och klicka på **skapa**.

    ![Skärmbild som visar alternativet Lägg till samling Studio 3T](./media/mongodb-mongochef/AddCollection.png)
4. Klicka på den **samling** menyn, klicka sedan på **Lägg till dokument**.

    ![Skärmbild som visar menyalternativet Studio 3T Lägg till dokument](./media/mongodb-mongochef/AddDocument1.png)
5. Klistra in följande i dialogrutan Lägg till dokument och klicka sedan på **Lägg till dokument**.

        {
        "_id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
               { "firstName": "Thomas" },
               { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "isRegistered": true
        }
6. Lägg till ett annat dokument, den här gången med följande innehåll:

        {
        "_id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam",
                 "givenName": "Jesse",
                "gender": "female", "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            {
                "familyName": "Miller",
                 "givenName": "Lisa",
                 "gender": "female",
                 "grade": 8 }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "isRegistered": false
        }
7. Kör en exempelfråga. Till exempel söka efter familjer med efternamn ”Andersen” och returnera föräldrar och statusfält.

    ![Skärmbild av Mongo-Chef frågeresultat](./media/mongodb-mongochef/QueryDocument1.png)

## <a name="next-steps"></a>Nästa steg
* Utforska Azure Cosmos DB MongoDB API [exempel](mongodb-samples.md).
