---
title: Datalagring i LUIS - Språkförståelse
titleSuffix: Azure Cognitive Services
description: Lär dig hur data lagras i Språkförståelse (LUIS). LUIS lagrar krypterade i ett Azure datalager som motsvarar den region som anges av nyckeln.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: f876c4e279e723120794c550392512f5672ef91e
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/22/2018
ms.locfileid: "49637653"
---
# <a name="data-storage-and-removal-in-language-understanding-luis-cognitive-services"></a>Datalagring och tas bort i Cognitive Services för Språkförståelse (LUIS)
LUIS lagrar krypterade i ett Azure datalager som motsvarar den region som anges av nyckeln. Dessa data lagras i 30 dagar. 

## <a name="export-and-delete-app"></a>Exportera och ta bort appen
Användarna har full kontroll över [exportera](luis-how-to-start-new-app.md#export-app) och [tar bort](luis-how-to-start-new-app.md#delete-app) appen. 

## <a name="utterances-in-an-intent"></a>Yttranden i ett intent
Ta bort exempel yttranden som används för träning [LUIS](luis-reference-regions.md). Om du tar bort en exempel-uttryck från LUIS-appen tas bort från LUIS-webbtjänsten och är inte tillgänglig för export.

## <a name="utterances-in-review"></a>Yttranden i granskning
Du kan ta bort yttranden från listan över användare yttranden som LUIS föreslår i den  **[granskningssidan endpoint yttranden](luis-how-to-review-endoint-utt.md)**. Tar bort yttranden i den här listan förhindrar du att de ska visas som förslag, men ta bort inte dem från loggar.

## <a name="accounts"></a>Konton
Om du tar bort ett konto raderas alla appar, och deras exempel yttranden och loggar. Dessa data hålls kvar i 60 dagar innan kontot och data tas bort permanent.

Tar bort kontot är tillgänglig från den **inställningar** sidan. Välj namnet på ditt konto i det övre högra navigeringsfältet för att komma till den **inställningar** sidan.

## <a name="data-inactivity-as-an-expired-subscription"></a>Data inaktivitet som en prenumeration som har upphört att gälla
För datakvarhållning och borttagning, kan en inaktiv LUIS-app på _Microsofts gottfinnande_ behandlas som en prenumeration som har upphört att gälla. En app betraktas som inaktiv om den uppfyller följande kriterier för de senaste 90 dagarna: 

* Har haft **inga** anrop som görs till den.
* Har inte ändrats.
* Inte har en aktuell nyckel tilldelat till den.
* Inte har haft en användarinloggning till den.

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [Lär dig mer om att exportera och ta bort en app](luis-how-to-start-new-app.md)