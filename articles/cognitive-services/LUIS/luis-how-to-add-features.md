---
title: Fras listor för att förbättra identifieringen av entiteten
titleSuffix: Azure Cognitive Services
description: Använd Språkförståelse (LUIS) att lägga till appfunktioner som kan förbättra identifiering eller förutsägelser av avsikter och entiteter som kategorier och mönster
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/06/2018
ms.author: diberry
ms.openlocfilehash: 32ef8ba2f6416e1b59fc98595f1b204e94bd2ead
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2018
ms.locfileid: "48830998"
---
# <a name="use-phrase-lists-to-boost-signal-of-word-list"></a>Använd frasen visar att boost signaler med ordlistan

Du kan lägga till funktioner till din LUIS-app för att förbättra dess noggrannhet. Funktionerna bidrar LUIS genom att tillhandahålla tips att vissa ord och fraser är en del av en app domän ordförråd. 

## <a name="add-phrase-list"></a>Lägg till frasen lista

1. Öppna din app genom att klicka på namnet på **Mina appar** , och klicka sedan på **skapa**, klicka sedan på **fras listor** i vänster panel i din app. 

2. På den **fras listor** klickar du på **Skapa ny frasen lista**. 
 
3. I den **lägga till frasen lista** dialogrutan skriver du ”städer” som namn på listan med fraser. I den **värdet** skriver värdena i listan med fraser. Du kan ange ett värde i taget eller en uppsättning värden, avgränsade med kommatecken och tryck sedan på **RETUR**.

    ![Lägg till frasen lista städer](./media/luis-add-features/add-phrase-list-cities.png)

4. LUIS kan föreslå relaterade värden för att lägga till i listan fras. Klicka på **rekommenderar** att hämta en grupp med föreslagna värden från relaterade till added value(s). Du kan klicka på någon av de föreslagna värdena eller klicka på **Lägg till alla** att lägga till dem alla.

    ![Fras listan över föreslagna värden](./media/luis-add-features/related-values.png)

5. Klicka på **dessa värden är utbytbara** om listvärden har lagts till frasen finns alternativ som är utbytbara.

    ![Fras listan över föreslagna värden](./media/luis-add-features/interchangeable.png)

6. Klicka på **Spara**. Listan med frasen ”städer” har lagts till i den **fras listor** sidan.

<a name="edit-phrase-list"></a>
<a name="delete-phrase-list"></a>
<a name="deactivate-phrase-list"></a>

> [!Note]
> Du kan ta bort eller inaktivera en fras lista från verktygsfältet sammanhangsberoende på den **fras listor** sidan.

## <a name="pattern-regular-expression-feature"></a>Funktionen för mönster (återkommande uttryck) 
**Den här funktionen är inaktuell**. Nya funktioner i mönstret kan inte läggas till LUIS. Alla befintliga mönstret-funktioner som stöds till maj 2018. Bidra till standard LUIS-reguljärt uttryck matchar med en pull-begäran till den [identifierare fulltext Github-lagringsplatsen](https://github.com/Microsoft/Recognizers-Text). 

## <a name="next-steps"></a>Nästa steg

Efter att lägga till, redigera, ta bort eller inaktivera en fras lista [träna och testa appen](luis-interactive-test.md) igen för att se om prestanda förbättras.
