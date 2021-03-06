---
title: ta med fil
description: ta med fil
services: time-series-insights
author: ashannon7
ms.service: time-series-insights
ms.topic: include
ms.date: 08/20/2018
ms.author: anshan
ms.custom: include file
ms.openlocfilehash: fb45ea02f365cf4e7b394e249f9b91a784e5469f
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/19/2018
ms.locfileid: "46368863"
---
## <a name="grant-data-access"></a>Bevilja åtkomst till data

Följ dessa steg om du vill bevilja åtkomst till data för en användares huvudnamn:

1. Logga in på [Azure Portal](https://portal.azure.com).

2. Leta upp din Time Series Insights-miljö. Typ **tidsserier** i den **search** box. Välj **Time Series Environment** i sökresultatet. 

3. Välj Time Series Insights-miljön från listan.

4. Välj **Dataåtkomstprinciper**och välj sedan **+ Lägg till**.
    ![Hantera Time Series Insights-källan – miljö](media/iot-tsi-data-access/getstarted-grant-data-access1.png)

5. Välj **Välj användare**.  Sök efter användarnamn eller e-postadress att hitta användaren som du vill lägga till. Klicka på **Välj** att bekräfta valet. 

    ![Hantera Time Series Insights-källan – lägg till](media/iot-tsi-data-access/getstarted-grant-data-access2.png)

6. Välj **Välj roll**. Välj rätt roll för användaren:
    - Välj **deltagare** om du vill tillåta användare att ändra referensdata och dela sparade frågor och perspektiv med andra användare av miljön. 
    - Annars väljer **läsare** att tillåta användare söker data i miljön och sparar personliga (inte delade) frågor i miljön.

    Välj **Ok** att bekräfta valet rollen.

    ![Hantera Time Series Insights-källan – välj användare](media/iot-tsi-data-access/getstarted-grant-data-access3.png)

7. Välj **Ok** i den **Välj användarroll** sidan.

    ![Hantera Time Series Insights-källan – välj roll](media/iot-tsi-data-access/getstarted-grant-data-access4.png)

8. Den **Dataåtkomstprinciper** sidan visas användarna och roller för varje användare.

    ![Hantera Time Series Insights-källan – resultat](media/iot-tsi-data-access/getstarted-grant-data-access5.png)