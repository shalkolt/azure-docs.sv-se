---
title: Felsökning begränsningsfel i Azure | Microsoft Docs
description: Begränsning fel, försök och backoff i Azure Compute.
services: virtual-machines
documentationcenter: ''
author: changov
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.service: virtual-machines
ms.devlang: na
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.date: 09/18/2018
ms.author: vashan, rajraj, changov
ms.openlocfilehash: b951d0b8d91729340cf382e70f72511fb009053e
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49386560"
---
# <a name="troubleshooting-api-throttling-errors"></a>Felsökning av API-begränsningsfel 

Azure Compute-begäranden kan att begränsas på en prenumeration och på basis av per region för att den övergripande prestanda för tjänsten. Vi garanterar att alla anrop till den Azure Compute-Resursprovidern (CRP), som hanterar resurser under Microsoft.Compute namnområdet inte överskrider högsta tillåtna API-begäran överföringshastighet. Det här dokumentet beskriver API begränsningar, information om hur du felsöker begränsning problem och bästa praxis för att undvika begränsas.  

## <a name="throttling-by-azure-resource-manager-vs-resource-providers"></a>Begränsning av Azure Resource Manager vs Resursprovidrar  

Azure Resource Manager sker valideringen av autentisering och första ordningen och begränsningar för all inkommande API-begäranden som åtkomsten till Azure. Hastighetsbegränsningar för anrop till Azure Resource Manager och relaterade diagnostiska HTTP-svarshuvuden beskrivs [här](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-request-limits).
 
När en Azure API-klient hämtar en begränsning fel, är HTTP-status 429 för många förfrågningar. Information om den begärandebegränsning utförs av Azure Resource Manager eller en underliggande resursprovider som CRP och inspektera de `x-ms-ratelimit-remaining-subscription-reads` för GET-begäranden och `x-ms-ratelimit-remaining-subscription-writes` svarshuvuden för icke-GET-begäranden. Om det återstående antalet anrop närmar sig 0, har prenumerationens Allmänt anrop gränsen som definierats av Azure Resource Manager nåtts. Aktiviteter med hjälp av alla prenumerationsklienter räknas samman. Begränsningen i annat fall kommer från resursprovidern mål (det som beskrivs i den `/providers/<RP>` begäran-URL-segmentet). 

## <a name="call-rate-informational-response-headers"></a>Anropa rate informationsmeddelande svarshuvuden 

| Huvud                            | Värdeformat                           | Exempel                               | Beskrivning                                                                                                                                                                                               |
|-----------------------------------|----------------------------------------|---------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| x-ms-ratelimit-återstående-resurs |```<source RP>/<policy or bucket>;<count>```| Microsoft.Compute/HighCostGet3Min;159 | Återstående antalet för API-anrop för begränsningsprincipen som täcker bucket eller åtgärden resursgruppen, inklusive mål för den här begäran                                                                   |
| x-ms-begäran-kostnad               | ```<count>   ```                             | 1                                     | Antalet antal ”debiteras” för den här HTTP-begäran mot principen gäller gränsen. Detta är normalt 1. Batch-begäranden, till exempel för att skala en skalningsuppsättning för virtuella datorer kan debitera flera antal. |


Observera att en API-begäran kan utsättas för flera principer för begränsning. Det blir en separat `x-ms-ratelimit-remaining-resource` rubrik för varje princip. 

Här är ett exempelsvar för att ta bort VM scale set-begäran.

```
x-ms-ratelimit-remaining-resource: Microsoft.Compute/DeleteVMScaleSet3Min;107 
x-ms-ratelimit-remaining-resource: Microsoft.Compute/DeleteVMScaleSet30Min;587 
x-ms-ratelimit-remaining-resource: Microsoft.Compute/VMScaleSetBatchedVMRequests5Min;3704 
x-ms-ratelimit-remaining-resource: Microsoft.Compute/VmssQueuedVMOperations;4720 
```

## <a name="throttling-error-details"></a>Begränsning felinformation

429 HTTP-status används ofta för att avvisa en begäran eftersom en anropsgränsen har nåtts. En typisk begränsning felsvar från Compute-Resursprovidern ser ut i exemplet nedan (endast relevant rubriker visas):

```
HTTP/1.1 429 Too Many Requests
x-ms-ratelimit-remaining-resource: Microsoft.Compute/HighCostGet3Min;46
x-ms-ratelimit-remaining-resource: Microsoft.Compute/HighCostGet30Min;0
Retry-After: 1200
Content-Type: application/json; charset=utf-8
{
  "code": "OperationNotAllowed",
  "message": "The server rejected the request because too many requests have been received for this subscription.",
  "details": [
    {
      "code": "TooManyRequests",
      "target": "HighCostGet30Min",
      "message": "{\"operationGroup\":\"HighCostGet30Min\",\"startTime\":\"2018-06-29T19:54:21.0914017+00:00\",\"endTime\":\"2018-06-29T20:14:21.0914017+00:00\",\"allowedRequestCount\":800,\"measuredRequestCount\":1238}"
    }
  ]
}

```

Principen med återstående antalet anrop 0 är den på grund av som begränsning felet returneras. I det här fallet är `HighCostGet30Min`. Övergripande formatet för svarstexten är allmänna Azure Resource Manager API fel format (kompatibel med OData). Den huvudsakliga felkoden `OperationNotAllowed`, är det Compute-Resursprovidern använder för att rapportera begränsningsfel (bland andra typer av klientfel). Den `message` egenskapen för den inre fel innehåller en serialiserade JSON-struktur med information om bandbreddsbegränsning överträdelsen.

Enligt beskrivningen ovan, varje begränsning fel innehåller den `Retry-After` rubriken, som innehåller det minsta antalet sekunder som klienten ska vänta innan en ny begäran. 

## <a name="best-practices"></a>Bästa praxis 

- Försök inte Azure-tjänst-API-fel ovillkorligt och/eller omedelbart. Vanligt förekommande avser klientkod att få in i en snabb omförsöksslinga när den påträffar ett fel som inte kan och försök igen. Återförsök kommer så småningom få slut tillåtna anrop gränsen för mål-åtgärden grupp och påverka andra klienter för prenumerationen. 
- Överväg att implementera proaktiv klientsidan automatisk begränsning när antalet tillgängliga anrop för en målgrupp för åtgärden sjunker under vissa lågtröskelövervakare i omfattande API automation fall. 
- När du kartlägger asynkrona åtgärder, respektera sidhuvudet Retry-After-tips. 
- Om klientkoden behöver information om en viss virtuell dator kan du fråga den virtuella datorn direkt i stället för att visa en lista över alla virtuella datorer i den aktuella resursgruppen eller hela prenumerationen och väljer sedan den nödvändiga virtuella datorn på klientsidan. 
- Om klientkoden måste virtuella datorer, diskar och ögonblicksbilder från en specifik Azure-plats, använder du platsbaserad form av frågan i stället för att fråga alla prenumeration virtuella datorer och sedan filtrera efter plats på klientsidan: `GET /subscriptions/<subId>/providers/Microsoft.Compute/locations/<location>/virtualMachines?api-version=2017-03-30` frågan till Compute-Resursprovidern nationella inställningar slutpunkter. 
-   När du skapar eller uppdaterar API-resurserna i synnerhet, virtuella datorer och VM-skalningsuppsättningar, är det mycket mer effektivt att spåra returnerade async-åtgärden för slutförande än avsökning på resurs-URL (baserat på den `provisioningState`).

## <a name="next-steps"></a>Nästa steg

Mer information om riktlinjer för återförsök för andra tjänster i Azure finns i [vägledningen om återförsök för specifika tjänster](https://docs.microsoft.com/azure/architecture/best-practices/retry-service-specific)
