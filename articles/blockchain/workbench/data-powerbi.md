---
title: Använd Azure Blockchain Workbench-data i Microsoft Power BI
description: Lär dig hur du läser in och visar SQL DB-data i Azure Blockchain Workbench i Microsoft Power BI.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 10/1/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: b1020389ef28c18c03536d686cd47ef0c65b9204
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/03/2018
ms.locfileid: "48242807"
---
# <a name="using-azure-blockchain-workbench-data-with-microsoft-power-bi"></a>Använda Azure Blockchain Workbench-data med Microsoft Power BI

Med Microsoft Power BI kan du enkelt generera kraftfulla rapporter från SQL DB-databaser med hjälp av Power BI Desktop och sedan publicera dem till [https://www.powerbi.com](http://www.powerbi.com).

Den här artikeln beskriver steg för steg hur du ansluter till SQL-databasen i Azure Blockchain Workbench från PowerBI Desktop, skapar en rapport och sedan distribuerar rapporten till powerbi.com.

## <a name="prerequisites"></a>Förutsättningar

* Ladda ned [PowerBI Desktop](https://aka.ms/pbidesktopstore).

## <a name="connecting-powerbi-to-data-in-azure-blockchain-workbench"></a>Ansluta PowerBI till data i Azure Blockchain Workbench

1.  Öppna Power BI Desktop.
2.  Välj **Hämta data**.

    ![Hämta data](./media/data-powerbi/get-data.png)
3.  Välj **SQL Server** i listan över typer av datakällor.

4.  Ange server- och databasnamnet i dialogrutan. Ange om du vill importera data eller köra en **DirectQuery**. Välj **OK**.

    ![Välj SQL Server](./media/data-powerbi/select-sql.png)

5.  Ange databasens autentiseringsuppgifter för åtkomst till Azure Blockchain Workbench. Välj **Databas** och ange dina autentiseringsuppgifter.

    Om du använder autentiseringsuppgifterna som skapades i samband med Azure Blockchain Workbench-distributionen är användarnamnet **dbadmin** och lösenordet är det lösenord som du angav under distributionen.

    ![SQL DB-inställningar](./media/data-powerbi/db-settings.png)

6.  När anslutningen till databasen har upprättats visas de tabeller och vyer som är tillgängliga i databasen i dialogrutan **Navigatör**. Vyerna är utformade för rapportering och har prefixet **vw**.

    ![Navigatör](./media/data-powerbi/navigator.png)

7.  Välj de vyer som du vill ta med. I demonstrationssyfte tar vi med **vwContractAction**, som innehåller information om alla åtgärder som har vidtagits i anslutning till ett kontrakt.

    ![Välja vyer](./media/data-powerbi/select-views.png)

Nu kan du skapa och publicera rapporter som vanligt med Power BI.

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [Databasvyer i Azure Blockchain Workbench](database-views.md)