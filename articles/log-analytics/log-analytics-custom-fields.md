---
title: Anpassade fält i Azure Log Analytics | Microsoft Docs
description: Funktionen för anpassade fält i Log Analytics kan du skapa dina egna sökbara fält från Log Analytics-poster som lägger till egenskaperna för en insamlad post.  Den här artikeln beskriver processen för att skapa ett anpassat fält och innehåller en detaljerad genomgång med en exempelhändelse.
services: log-analytics
documentationcenter: ''
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 31572b51-6b57-4945-8208-ecfc3b5304fc
ms.service: log-analytics
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2018
ms.author: bwren
ms.component: ''
ms.openlocfilehash: e86b79cb57b9d4a590d69a98eca591679ee10334
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/02/2018
ms.locfileid: "48042945"
---
# <a name="custom-fields-in-log-analytics"></a>Anpassade fält i Log Analytics
Den **anpassade fält** funktion i Log Analytics kan du utöka befintliga poster i Log Analytics genom att lägga till egna sökbara fält.  Anpassade fält fylls i automatiskt från data som extraheras från andra egenskaper i samma post.

![Översikt över anpassade fält](media/log-analytics-custom-fields/overview.png)

Posten exemplet nedan har till exempel användbara data i beskrivningen av händelsen.  Extrahera dessa data till olika egenskaper gör den tillgänglig för exempelvis sortering och filtrering.

![Loggsökningsknapp](media/log-analytics-custom-fields/sample-extract.png)

> [!NOTE]
> I förhandsversion är du begränsad till 100 anpassade fält i din arbetsyta.  Den här gränsen utökas när den här funktionen blir allmänt tillgänglig.
> 
> 

## <a name="creating-a-custom-field"></a>Skapa ett anpassat fält
När du skapar ett anpassat fält, måste Log Analytics förstå vilka data som ska använda för att fylla i dess värde.  En teknik från Microsoft Research kallas FlashExtract används för att snabbt identifiera dessa data.  I stället för att du behöver ange explicita instruktioner, Log Analytics lär sig om de data som du vill extrahera från de exempel som du anger.

I följande avsnitt finns proceduren för att skapa ett anpassat fält.  Längst ned i den här artikeln är en genomgång av en extrahering av exemplet.

> [!NOTE]
> Det anpassade fältet fylls när poster som matchar de angivna kriterierna läggs till Log Analytics, så visas endast på poster som samlas in när det anpassade fältet har skapats.  Anpassat fält kommer inte att lägga till poster som redan finns i datalagret när den har skapats.
> 

### <a name="step-1--identify-records-that-will-have-the-custom-field"></a>Steg 1 – identifiera poster som har anpassat fält
Det första steget är att identifiera de poster som får det anpassade fältet.  Du börjar med en [standard loggsökning](log-analytics-log-searches.md) och välj sedan en post för att fungera som den modell som Lär dig från Log Analytics.  När du anger att du kommer att extrahera data till ett anpassat fält i **fältet Extraheringsguiden** öppnas där du validerar och finjustera villkoret.

1. Gå till **Loggsökning** och använda en [frågan för att hämta poster](log-analytics-log-searches.md) som har anpassat fält.
2. Markera en post som Log Analytics använder för att fungera som en modell för att extrahera data för att fylla i det anpassade fältet.  Du kan identifiera de data som du vill extrahera från den här posten och Log Analytics använder den här informationen för att bestämma logik för att fylla i det anpassade fältet för alla liknande poster.
3. Klicka på knappen till vänster om alla textegenskap i posten och välj **extrahera fält från**.
4. Den **fältet Extraheringsguiden öppnas**, och den post som du har valt visas i den **Huvudexemplet** kolumn.  Anpassat fält definieras för posterna med samma värden i de egenskaper som är markerade.  
5. Om alternativet inte är exakt vad du kan välja ytterligare fält som avgränsa kriterier.  För att kunna ändra fältvärden för villkoret måste du avbryta och väljer en annan post som matchar de kriterier som du vill.

### <a name="step-2---perform-initial-extract"></a>Steg 2 – utför inledande extrahera.
När du har identifierat de poster som har anpassat fält kan identifiera du de data som du vill extrahera.  Log Analytics använder den här informationen för att identifiera liknande mönster i liknande poster.  I steg efter detta kommer du att kunna granska resultaten och ange ytterligare information för Log Analytics för att använda i sin analys.

1. Markera texten i exempelpost som du vill fylla i det anpassade fältet.  Sedan visas en dialogruta där att ange ett namn för fältet och att utföra inledande extrahera.  Tecknen  **\_CF** läggs automatiskt.
2. Klicka på **extrahera** att utföra en analys av insamlad poster.  
3. Den **sammanfattning** och **sökresultat** avsnitt visas resultatet av extrahera så att du kan granska dess noggrannhet.  **Sammanfattning** visar de kriterier som används för att identifiera poster och ett antal för var och en av de datavärden som identifieras.  **Sökresultat** innehåller en detaljerad lista med poster som matchar kriterierna.

### <a name="step-3--verify-accuracy-of-the-extract-and-create-custom-field"></a>Steg 3 – kontrollera korrektheten i extrahera och skapa anpassade fält
När du har utfört inledande extrahera, visar Log Analytics sina resultat baserat på data som redan har samlats in.  Om resultatet ser ut korrekt kan du skapa det anpassade fältet med inget ytterligare arbete.  Annars kan du sedan kan du förfina resultaten så att Log Analytics kan förbättra sin logik.

1. Om alla värden i den inledande extrahera inte stämmer klickar du på **redigera** ikonen bredvid ett felaktigt och välja **ändra den här markeringen** för att ändra valet.
2. Transaktionen har kopierats till den **ytterligare exempel** avsnittet under den **Huvudexemplet**.  Du kan justera Markera här för att hjälpa Log Analytics förstå bör har gjort valet.
3. Klicka på **extrahera** du använder den här nya informationen för att utvärdera alla befintliga poster.  Resultaten kan ändras efter poster än det som du just har ändrat baserat på den här nya insikter.
4. Fortsätt lägga till korrigeringar tills alla poster i extrahera identifiera data för att fylla i den nya anpassade fälten.
5. Klicka på **spara extrahera** när du är nöjd med resultatet.  Anpassat fält definieras nu, men den läggs inte till några poster ännu.
6. Vänta tills nya poster som matchar de angivna villkoren för att samlas in och kör sedan loggsökningen igen. Nya poster ska ha det anpassade fältet.
7. Använd det anpassade fältet som någon annan post egenskap.  Du kan använda den för att sammanställer och gruppera data och även använda den för att skapa nya insikter.

## <a name="viewing-custom-fields"></a>Visa anpassade fält
Du kan visa en lista över alla anpassade fält i hanteringsgruppen från den **avancerade inställningar** meny med Log Analytics-arbetsytan i Azure-portalen.  Välj **Data** och sedan **anpassade fält** en lista över alla anpassade fält i din arbetsyta.  

![Anpassade fält](media/log-analytics-custom-fields/list.png)

## <a name="removing-a-custom-field"></a>Ta bort ett anpassat fält
Det finns två sätt att ta bort ett anpassat fält.  Först är det **ta bort** alternativ för varje fält när du visar den fullständiga listan enligt beskrivningen ovan.  Det andra sättet är att hämta en post och klicka på knappen till vänster om fältet.  Menyn har ett alternativ för att ta bort det anpassade fältet.

## <a name="sample-walkthrough"></a>Exempelgenomgång
Följande avsnitt beskriver ett komplett exempel för att skapa ett anpassat fält.  Det här exemplet extraherar tjänstnamnet i Windows-händelser som indikerar att en tjänst som ändrar tillstånd.  Detta är beroende av händelser som skapats av Service Control Manager i systemloggen på Windows-datorer.  Om du vill följa det här exemplet måste du vara [samla in händelser med Information om systemloggen](log-analytics-data-sources-windows-events.md).

Vi anger du följande fråga för att returnera alla händelser från Service Control Manager som har händelse-ID 7036 som är den händelse som anger en tjänst som startar eller stoppar.

![Fråga](media/log-analytics-custom-fields/query.png)

Vi kan sedan välja en post med händelse-ID 7036.

![Posten i datakällan](media/log-analytics-custom-fields/source-record.png)

Vi vill tjänstnamnet som visas i den **RenderedDescription** egenskapen och välj knappen bredvid den här egenskapen.

![Extrahera fält](media/log-analytics-custom-fields/extract-fields.png)

Den **fältet Extraheringsguiden** öppnas, och **EventLog** och **EventID** fält har markerats i den **Huvudexemplet** kolumn.  Detta anger att det anpassade fältet definieras efter händelser från systemloggen med händelse-ID 7036.  Detta är tillräcklig så vi inte behöver välja andra fält.

![Huvudexempel](media/log-analytics-custom-fields/main-example.png)

Vi markerar du namnet på tjänsten i den **RenderedDescription** egenskap och Använd **Service** att identifiera namnet på tjänsten.  Anpassat fält kommer att anropas **Service_CF**.

![Fältrubrik](media/log-analytics-custom-fields/field-title.png)

Vi kan se att namnet på tjänsten identifieras korrekt för vissa poster men inte för andra.   Den **sökresultat** visa den del av namnet på den **WMI-prestanda adaptern** har inte valts.  Den **sammanfattning** visar att fyra poster med **DPRMA** service felaktigt ingår ett extra ord och två poster har identifierat **moduler Installer** i stället för **Installationsprogrammet för Windows-moduler**.  

![Sökresultat](media/log-analytics-custom-fields/search-results-01.png)

Vi börjar med den **WMI-prestanda adaptern** post.  Vi klickar du på redigeringsikonen och sedan **ändra den här markeringen**.  

![Ändra markering](media/log-analytics-custom-fields/modify-highlight.png)

Vi ökar Markera för att inkludera ordet **WMI** och kör sedan extrahera.  

![Ytterligare exempel](media/log-analytics-custom-fields/additional-example-01.png)

Vi kan se att poster för **WMI-prestanda adaptern** har korrigerats och Log Analytics även denna information används för att korrigera poster för **installationsprogrammet för Windows-modulen**.  Vi kan se i den **sammanfattning** avsnittet dock som **DPMRA** fortfarande inte har identifieras korrekt.

![Sökresultat](media/log-analytics-custom-fields/search-results-02.png)

Vi bläddrar du till en post med DPMRA-tjänsten och använder samma process för att korrigera posten.

![Ytterligare exempel](media/log-analytics-custom-fields/additional-example-02.png)

 När vi kör extraheringen, kan vi se alla våra resultat finns nu korrekt.

![Sökresultat](media/log-analytics-custom-fields/search-results-03.png)

Vi kan se att **Service_CF** skapas, men ännu inte har lagts till alla poster.

![Inledande antal](media/log-analytics-custom-fields/initial-count.png)

Efter en stund så nya händelser som samlas in, kan vi se att den **Service_CF** fält läggs nu till poster som matchar våra villkor.

![Slutliga resultaten](media/log-analytics-custom-fields/final-results.png)

Vi kan nu använda det anpassade fältet som någon annan post-egenskap.  För att visa det här kan vi skapa en fråga som grupperar efter den nya **Service_CF** fält som du vill kontrollera vilka tjänster som är mest aktiva.

![Gruppera efter fråga](media/log-analytics-custom-fields/query-group.png)

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om [loggsökningar](log-analytics-log-searches.md) att skapa frågor med hjälp av anpassade fält för kriterier.
* Övervaka [anpassade loggfiler](log-analytics-data-sources-custom-logs.md) som du tolkas med anpassade fält.

