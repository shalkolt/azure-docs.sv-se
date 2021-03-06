---
title: Vad är Academic Knowledge API?
titlesuffix: Azure Cognitive Services
description: Använd Academic Knowledge API för att tolka användarfrågor och hämta innehållsrik information från Academic Graph.
services: cognitive-services
author: mvorvoreanu
manager: cgronlun
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: overview
ms.date: 03/27/2017
ms.author: mivorvor
ms.openlocfilehash: d08cd7124b232e50365e72753eba97c6309f401c
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/10/2018
ms.locfileid: "48901218"
---
# <a name="academic-knowledge-api"></a>Academic Knowledge API

Välkommen till Academic Knowledge API. Med den här tjänsten kan du tolka användarfrågor för akademiska avsikter och hämta innehållsrik information från Microsoft Academic Graph (MAG). MAG-kunskapsbasen är en heterogen entitetsgraf i webbskala som består av entiteter som modellerar skolrelaterade aktiviteter: ämnesområde, författare, institution, artikel, plats och händelse. 

MAG-data hämtas från Bing-webbindexet samt en lokal kunskapsbas från Bing. På grund av pågående Bing-indexering innehåller detta API ny information från webben efter identifiering och indexering av Bing. Baserat på den här datamängden möjliggör Academic Knowledge API:er en kunskapsbaserad och interaktiv dialog som smidigt kombinerar reaktiv sökning med proaktiva förslagsfunktioner, omfattande sökresultat för forskningsartikeldiagram samt histogramdistributioner av attributvärdena för en uppsättning artiklar och relaterade entiteter.

Mer information om Microsoft Academic Graph finns på [http://aka.ms/academicgraph](http://aka.ms/academicgraph).

Academic Knowledge API har flyttats från förhandsversionen av Cognitive Services till Cognitive Services Labs. Den nya startsidan för projektet är: [https://labs.cognitive.microsoft.com/en-us/project-academic-knowledge](https://labs.cognitive.microsoft.com/en-us/project-academic-knowledge). Din befintliga API-nyckel fortsätter att fungera fram till den 24 maj 2018. Generera en ny API-nyckel efter detta datum. Observera att den betalade förhandsversionen inte längre är tillgänglig när din befintliga nyckel upphör att gälla. Kontakta vårt team om den kostnadsfria nivån av API:et inte är tillräcklig för dina behov. 

## <a name="features"></a>Funktioner
Academic Knowledge API består av fyra relaterade REST-slutpunkter:  
  1. **interpret** – tolkar en användarfrågesträng med naturligt språk. Returnerar kommenterade tolkningar för att aktivera funktioner för automatisk ifyllning i sökrutor som förutsäger vad användaren skriver.  
  2. **evaluate** – utvärderar ett frågeuttryck och returnerar Academic Knowledge-entitetsresultat.  
  3. **calchistogram** – beräknar ett histogram för fördelningen av attributvärden för de akademiska entiteter som returneras av ett frågeuttryck, till exempel fördelningen av källhänvisningar per år för en angiven författare.  
  4. **graph search** – söker efter ett givet grafmönster och returnerar matchade entitetsresultat.

När dessa API-metoder används tillsammans kan du skapa en omfattande semantisk sökfunktion. Med en användarfrågesträng ger metoden **interpret** dig en annoterad version av frågan och ett strukturerat frågeuttryck. Den kan även slutföra användarens fråga baserat på semantiken för underliggande akademiska data. Exempel: om en användare anger strängen *latent s* kan metoden **interpret** ge en uppsättning rankade tolkningar som indikerar att användaren kanske söker efter ämnesområdet *latent semantisk analys*, artikeln *latent strukturanalysis* eller andra entitetsuttryck som börjar med *latent s*. Den här informationen kan användas för att snabbt vägleda användaren till önskat sökresultat.

Metoden **evaluate** kan användas för att hämta en uppsättning matchande artikelentiteter från den akademiska kunskapsbasen, och **calchistogram**-metoden kan användas för att beräkna distributionen av attributvärden för en uppsättning artikelentiteter som kan användas för att ytterligare filtrera sökresultatet.        

Metoden **graph search** har två lägen: *json* och *lambda*. Läget *json* kan utföra matchning av diagrammönster enligt de diagrammönster som anges av ett JSON-objekt. Läget *lambda* kan utföra beräkningar på serversidan under diagramtraverseringar enligt de användardefinierade lambdauttrycken.

## <a name="getting-started"></a>Komma igång 
Detaljerad dokumentation finns i underavsnitten till vänster.  Observera att för att förbättra läsbarheten för exemplen innehåller REST API-anropen tecken (såsom blanksteg) som inte har URL-kodats.  Din kod behöver tillämpa lämpliga URL-kodningar.
