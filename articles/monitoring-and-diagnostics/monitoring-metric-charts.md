---
title: Azure Monitor metrics explorer
description: Lär dig mer om nya funktioner i Azure Monitor Metrics Explorer
author: vgorbenko
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/17/2017
ms.author: vitaly.gorbenko
ms.component: metrics
ms.openlocfilehash: f82b4dff20e2b26e62889c41b3ff3c27bc86066a
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/10/2018
ms.locfileid: "48901423"
---
# <a name="azure-monitor-metrics-explorer"></a>Azure Monitor Metrics Explorer

Azure Monitor Metrics Explorer är en komponent i Microsoft Azure-portalen som tillåter ritning diagram, visuellt korrelera trender och undersöka toppar och dalar mått '. Metrics Explorer är ett viktigt startpunkt för att undersöka olika prestanda och tillgänglighetsproblem med dina program och infrastruktur i Azure eller övervakas av Azure Monitor-tjänster. 

## <a name="what-are-metrics-in-azure"></a>Vad är mått i Azure?

Mått i Microsoft Azure är serien med mätvärden och antal som samlas in och lagras över tid. Det finns mått för standard (eller ”plattformen”) och anpassade mått. Standardmått tillhandahålls till dig av själva Azure-plattformen. Standardmått visas statistik för hälsa och användning av dina Azure-resurser. Medan anpassade mått som ska skickas till Azure genom att dina program med den [Application Insights API för anpassade händelser](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics). Anpassade mått lagras i Application Insights-resurser tillsammans med andra program specifika mått.

## <a name="how-do-i-create-a-new-chart"></a>Hur gör jag för att skapa ett nytt diagram?

1. Öppna Azure portal
2. Gå till den nya **övervakaren** fliken och välj sedan **mått**.

   ![Bild av mått](./media/monitoring-metric-charts/0001.png)

3. Den **mått väljare** kommer automatiskt att vara öppen åt dig. Välj en resurs från listan för att visa dess tillhörande mått. Endast resurser med mått visas i listan.

   ![Bild av mått](./media/monitoring-metric-charts/0002.png)

   > [!NOTE]
   >Om du har mer än en Azure-prenumeration, Metrics Explorer-hämtningar i resurser i alla prenumerationer som har valts i Portal-inställningar -> Filter av prenumerationslista. Om du vill ändra den, klicka på kugghjulsikonen Portal inställningar på skärmen och välj vilka prenumerationer som du vill använda.

4. För vissa resurstyper (Storage-konton och virtuella datorer) innan du väljer ett mått måste du välja en **Namespace**. Varje namnområde har en egen uppsättning mått som är relevanta till endast det här namnområdet och inte till andra namnområden.

   Varje Azure Storage har till exempel mått för subservices ”BLOB”, ”filer”, ”Queues” och ”tabeller”, som är alla delar av storage-konto. Dock gäller mått ”Antal Kömeddelanden” naturligt till deltjänst ”kö” och inte till några andra subservices med lagring.

   ![Bild av mått](./media/monitoring-metric-charts/0003.png)

5. Välj ett mått i listan. Om du vet att en del av namnet på det mått som du vill kan du börja skriva den i att se en filtrerad lista över tillgängliga mått:

   ![Bild av mått](./media/monitoring-metric-charts/0004.png)

6. När du har valt ett mått återges i diagrammet med standardaggregeringen för det valda måttet. Klicka nu bara bort från den **mått väljare** att stänga den. Du kan även växla diagrammet till en annan aggregering. För vissa mått kan växlar aggregering du välja vilket värde som du vill visa i diagrammet. Exempelvis kan du växla mellan genomsnittlig, minsta och högsta värden. 

7. Genom att klicka på ikonen Lägg till mått ![ikon för mått](./media/monitoring-metric-charts/icon001.png) och upprepa steg 3 – 6 du kan lägga till fler mått på samma diagram.

   > [!NOTE]
   > Du normalt vill inte ha mått med olika enheter (d.v.s. ”millisekunder” och ”kilobyte”) eller med betydligt olika skala på ett diagram. Överväg istället att använda flera diagram. Klicka på knappen Lägg till diagram för att skapa flera diagram i Metrics Explorer.

## <a name="how-do-i-apply-filters-to-the-charts"></a>Hur jag för att använda filter i diagrammen?

Du kan använda filter på diagram som visar mått med dimensioner. Till exempel om måttet ”Transaktionsantal” har en dimension, ”svarstypen”, som anger om svaret från transaktioner har lyckats eller misslyckats sedan filtrering på den här dimensionen skulle rita en rad för diagram för endast lyckade (eller bara misslyckade) transaktioner. 

### <a name="to-add-a-filter"></a>Lägg till ett filter:

1. Klicka på ikonen Lägg till Filter ![filter-ikonen](./media/monitoring-metric-charts/icon002.png) ovanför diagrammet

2. Välj vilken dimension (egenskapen) som du vill filtrera

   ![mått-avbildning](./media/monitoring-metric-charts/0006.png)

3. Välj vilka dimensionsvärden som du vill inkludera när du gör i diagrammet (det här exemplet visar filtrerar ut lyckad storage-transaktioner):

   ![mått-avbildning](./media/monitoring-metric-charts/0007.png)

4. Klicka utanför Väljaren Filter att stänga den när du har valt filtervärdena. Diagrammet visar nu hur många lagringstransaktioner har misslyckats:

   ![mått-avbildning](./media/monitoring-metric-charts/0008.png)

5. Du kan upprepa steg 1 – 4 använda flera filter i samma diagram.

## <a name="how-do-i-segment-a-chart"></a>Hur jag för att segmentera ett diagram?

Du kan dela ett mått med dimensionen att visualisera hur olika segment av mått jämför mot varandra och identifiera öar segmenten i en dimension. 

### <a name="to-segment-a-chart"></a>Ett diagram-segmentet:

1. Klicka på ikonen Lägg till gruppering  ![mått-avbildning](./media/monitoring-metric-charts/icon003.png) ovanför diagrammet.
 
   > [!NOTE]
   > Du kan ha flera filter men bara en gruppering på ett enkelt diagram.

2. Välj en dimension som du vill segmentera diagrammet: 

   ![mått-avbildning](./media/monitoring-metric-charts/0010.png)

   Diagrammet visar nu nu flera rader, en för varje segment för dimension:

   ![mått-avbildning](./media/monitoring-metric-charts/0012.png)

3. Klicka på bort från den **gruppering väljare** att Stäng den.

   > [!NOTE]
   > Använd både filtrering och gruppering på samma dimension för att dölja segment som är inte relevant för ditt scenario och göra det lättare att läsa diagram.

## <a name="how-do-i-pin-charts-to-dashboards"></a>Hur jag för att fästa diagram till instrumentpaneler?

När du har konfigurerat diagrammen kan du lägga till den i instrumentpaneler så att du kan visa den igen, eventuellt i kontexten av andra övervakning telemetri, eller dela med ditt team. 

Att fästa ett diagram som är konfigurerade på en instrumentpanel:

När du har konfigurerat schemat klickar du på den **diagram åtgärder** menyn i högra viktigaste hörnet i diagrammet och klicka på **fäst på instrumentpanelen**.

   ![mått-avbildning](./media/monitoring-metric-charts/0013.png)

## <a name="next-steps"></a>Nästa steg

  Läs [skapa anpassade KPI-instrumentpaneler](https://docs.microsoft.com/azure/application-insights/app-insights-tutorial-dashboards) vill veta mer om bästa praxis för att skapa användbara instrumentpaneler med mått.