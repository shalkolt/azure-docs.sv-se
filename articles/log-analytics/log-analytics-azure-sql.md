---
title: Azure SQL Analytics-lösningen i Log Analytics | Microsoft Docs
description: Azure SQL Analytics-lösningen hjälper dig att hantera dina Azure SQL-databaser
services: log-analytics
documentationcenter: ''
author: danimir
manager: carmonm
ms.reviewer: carlrab
ms.assetid: b2712749-1ded-40c4-b211-abc51cc65171
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/03/2018
ms.author: v-daljep
ms.component: ''
ms.openlocfilehash: 3c80007a8188fb239a13aaa0ccc9ef2237a2d8d1
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50025679"
---
# <a name="monitor-azure-sql-database-using-azure-sql-analytics-preview"></a>Övervaka Azure SQL Database med Azure SQL Analytics (förhandsversion)

![Azure SQL Analytics-symbol](./media/log-analytics-azure-sql/azure-sql-symbol.png)

Azure SQL Analytics är ett moln som övervakningslösning för övervakning av prestanda för Azure SQL-databaser, elastiska pooler och instanser som hanteras i stor skala och över flera prenumerationer via ett fönster för alla. Den samlar in och visar viktiga mått för Azure SQL Database-prestanda med inbyggd intelligens för prestandafelsökning av.

Genom att använda mått som du samlar in med lösningen kan skapa du anpassade regler för övervakning och aviseringar. Lösningen hjälper dig att identifiera problem på varje lager i din programstack. Azure diagnostikmått tillsammans med vyer för Log Analytics används för att presentera data om alla dina Azure SQL-databaser, elastiska pooler och databaser i hanterade instanser i en enda Log Analytics-arbetsyta. Log Analytics hjälper dig att samla in, korrelera och visualisera strukturerade och Ostrukturerade data.

Finns det inbäddade videoklippet för en praktisk översikt om hur du använder Azure SQL Analytics-lösningen och för vanliga Användningsscenarier:


> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Get-Intelligent-Insights-for-Improving-Azure-SQL-Database-Performance/player]
>

## <a name="connected-sources"></a>Anslutna källor

Azure SQL Analytics är ett moln som endast övervakning lösning stödjande strömning av diagnostiktelemetri för Azure SQL Database Managed Instance databaser och elastiska pooler.

Lösningen inte använder agenter för att ansluta till tjänsten Log Analytics, lösningen inte stöd för övervakning av SQL Server finns lokalt eller i virtuella datorer, se tabellen kompatibilitet.

| Ansluten källa | Stöds | Beskrivning |
| --- | --- | --- |
| **[Azure-diagnostik](log-analytics-azure-storage.md)** | **Ja** | Azure mått och loggfiler data skickas till Log Analytics direkt av Azure. |
| [Azure Storage-konto](log-analytics-azure-storage.md) | Nej | Log Analytics läsa inte data från ett lagringskonto. |
| [Windows-agenter](log-analytics-windows-agent.md) | Nej | Direkta Windows-agenter används inte av lösningen. |
| [Linux-agenter](log-analytics-linux-agents.md) | Nej | Direct Linux-agenter används inte av lösningen. |
| [SCOM-hanteringsgrupp](log-analytics-om-agents.md) | Nej | En direktanslutning till Log Analytics från SCOM-agenten används inte av lösningen. |

## <a name="configuration"></a>Konfiguration

Utför följande steg för att lägga till Azure SQL Analytics-lösningen till instrumentpanelen i Azure.

1. Lägga till Azure SQL Analytics-lösningen i din arbetsyta från [Azure marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureSQLAnalyticsOMS?tab=Overview).
2. I Azure-portalen klickar du på **+ skapa en resurs**, Sök sedan efter **Azure SQL Analytics**.  
    ![Övervakning och hantering](./media/log-analytics-azure-sql/monitoring-management.png)
3. Välj **Azure SQL Analytics (förhandsversion)** i listan
4. I den **Azure SQL Analytics (förhandsversion)** området klickar du på **skapa**.  
    ![Skapa](./media/log-analytics-azure-sql/portal-create.png)
5. I den **Skapa ny lösning** område, skapa en ny eller välj en befintlig arbetsyta som du vill lägga till lösningen och klicka sedan på **skapa**.

    ![Lägg till arbetsytan](./media/log-analytics-azure-sql/add-to-workspace.png)

### <a name="configure-azure-sql-databases-elastic-pools-and-managed-instances-to-stream-diagnostics-telemetry"></a>Konfigurera Azure SQL-databaser, elastiska pooler och hanterade instanser till stream-diagnostiktelemetri

När du har skapat Azure SQL Analytics-lösningen i din arbetsyta, för att övervaka prestanda för Azure SQL-databaser, databaser i Managed Instance databaser och elastiska pooler behöver du **konfigurera var och en** av dessa resurser som du vill övervaka för att strömma dess diagnostiktelemetri till lösningen. Följ instruktionerna på den här sidan:

- Aktivera Azure Diagnostics för din Azure SQL Database Managed Instance databaser och elastiska pooler till [strömma diagnostiktelemetri till Azure SQL Analytics](../sql-database/sql-database-metrics-diag-logging.md).

Sidan ovan innehåller också anvisningar om hur du aktiverar stöd för övervakning av flera Azure-prenumerationer från en enda Azure SQL Analytics-arbetsyta som en enda glasruta.

## <a name="using-the-solution"></a>Använda lösningen

När du lägger till lösningen till din arbetsyta, Azure SQL Analytics-panel har lagts till din arbetsyta och den visas i Översikt. Panelen visar antal Azure SQL-databaser, elastiska pooler, hanterade instanser och databaser i hanterade instanser som lösningen tar emot diagnostiktelemetri från.

![Azure SQL Analytics-ikonen](./media/log-analytics-azure-sql/azure-sql-sol-tile.png)

Lösningen innehåller två separata vyer – en för övervakning av Azure SQL-databaser och elastiska pooler och andra vyn för att övervaka Managed Instance och databaser i hanterade instanser.

Om du vill visa instrumentpanelen för Azure SQL Analytics för Azure SQL-databaser och elastiska pooler, klickar du på den övre delen av panelen. Klicka på den nedre delen av panelen om du vill visa Azure SQL Analytics övervakning instrumentpanel för hanterad instans och databaser i Managed Instance.

### <a name="viewing-azure-sql-analytics-data"></a>Visa Azure SQL Analytics-data

Instrumentpanelen innehåller en översikt över alla databaser som övervakas via olika perspektiv. För olika perspektiv ska fungera måste du aktivera rätt mått eller loggar på din SQL-resurser för att strömmas till Azure Log Analytics-arbetsyta.

Observera att om inte några mått eller loggar strömmas till Azure Log Analytics paneler i lösningen inte fylls i med information om övervakning.

### <a name="azure-sql-database-and-elastic-pool-view"></a>Visa Azure SQL-databas och elastisk pool

När Azure SQL Analytics panelen för Azure SQL Database och elastiska pooler har valts visas instrumentpanelen för övervakning.

![Översikt över Azure SQL Analytics](./media/log-analytics-azure-sql/azure-sql-sol-overview.png)

Att välja någon av panelerna, öppnar en rapport för nedåt i specifika perspektiv. När perspektiv är markerad öppnas rapporten nedåt.

![Timeout för Azure SQL Analytics](./media/log-analytics-azure-sql/azure-sql-sol-metrics.png)

Varje perspektiv i den här vyn innehåller sammanfattningar om prenumeration, server, elastisk pool och databasnivå. Dessutom kan visar varje perspektiv ett perspektiv specifika i rapporten till höger. Att välja prenumeration, server, poolen eller databasen i listan fortsätter att gå nedåt.

### <a name="managed-instance-and-databases-in-managed-instance-view"></a>Den hanterade instansen och databaser i vy för hanterad instans

När Azure SQL Analytics panelen för hanterade instanser och hanterade instanstjänsten databaser har valts visas instrumentpanelen för övervakning.

![Översikt över Azure SQL Analytics](./media/log-analytics-azure-sql/azure-sql-sol-overview-mi.png)

Att välja någon av panelerna, öppnar en rapport för nedåt i specifika perspektiv. När perspektiv är markerad öppnas rapporten nedåt.

Markerat Managed Instance vy innehåller uppgifter om hanterad instans-användning, databaser den innehåller och telemetri för frågor som körs för instansen.

![Timeout för Azure SQL Analytics](./media/log-analytics-azure-sql/azure-sql-sol-metrics-mi.png)

### <a name="perspectives"></a>Perspektiv

Den nedan tabellen beskriver perspektiv som stöds för två versioner av instrumentpanelen för Azure SQL-databaser och elastiska pooler och det andra för hanterad instans.

| Perspektiv | Beskrivning | Stöd för SQL-databaser och elastiska pooler | Hanterad instans-stöd |
| --- | ------- | ----- | ----- |
| Resursen efter typ | Perspektiv som räknar alla resurser som övervakas. | Ja | Ja | 
| Insikter | Tillhandahåller hierarkisk nedåt till smarta insikter i prestanda. | Ja | Ja |
| Fel | Tillhandahåller hierarkisk nedåt till SQL-fel som inträffat på databaserna. | Ja | Ja |
| Tidsgränser | Tillhandahåller hierarkisk nedåt till SQL-timeouter som inträffat på databaserna. | Ja | Nej |
| Blockings | Tillhandahåller hierarkisk nedåt till SQL-blockings som inträffat på databaserna. | Ja | Nej |
| Databasen väntar | Tillhandahåller hierarkisk nedåt till SQL vänta statistik på databasnivå. Omfattar sammanfattningar av total Väntetid och väntetiden per typ av vänta. |Ja | Ja |
| Frågevaraktighet | Tillhandahåller hierarkisk detaljnivå om frågestatistik för körning som frågevaraktigheten, CPU-användning, Data-i/o-användning, logg-i/o-användning. | Ja | Ja |
| Frågan väntar | Tillhandahåller hierarkisk detaljnivå om vänta frågestatistik efter vänta kategori. | Ja | Ja |

### <a name="intelligent-insights-report"></a>Intelligent Insights-rapport

Azure SQL Database [smarta insikter](../sql-database/sql-database-intelligent-insights.md) gör att du vet vad som händer med prestanda i Azure SQL-databaser och hanterad instans-databaser. Alla smarta insikter som samlas in kan visualiseras och nås via Insights perspektiv.

![Azure SQL Analytics insikter](./media/log-analytics-azure-sql/azure-sql-sol-insights.png)

### <a name="elastic-pool-and-database-reports"></a>Elastisk pool och databasrapporter

Både elastiska pooler och SQL-databaser har sina egna specifika rapporter som visar alla data som samlas in för resursen i den angivna tiden.

![Azure SQL Analytics-databas](./media/log-analytics-azure-sql/azure-sql-sol-database.png)

![Azure SQL-databaspool](./media/log-analytics-azure-sql/azure-sql-sol-pool.png)

### <a name="query-reports"></a>Fråga rapporter

Du kan jämföra prestanda för alla frågor via fråga rapporten via frågevaraktigheten och frågan väntar perspektiv. Den här rapporten jämför prestanda för frågor mellan olika databaser och gör det enkelt att identifiera databaser som utför den valda frågan bra jämfört med de som är långsamma.

![Azure SQL Analytics-frågor](./media/log-analytics-azure-sql/azure-sql-sol-queries.png)

## <a name="permissions"></a>Behörigheter

Om du vill använda Azure SQL Analytics kommer användare på minsta behöver tilldelas rollen läsare i Azure. Den här rollen kommer dock inte tillåta användare att se frågetexten eller utföra någon automatisk justering åtgärder. Fler fria roller i Azure som gör det möjligt med hjälp av lösningen i utsträckning är ägare, deltagare, SQL DB-deltagare eller SQL Server-deltagare. Du kanske också vill du skapa en anpassad roll i portalen med specifika behörigheter som krävs endast för att använda Azure SQL Analytics och ingen åtkomst till hantering av andra resurser.

### <a name="creating-a-custom-role-in-portal"></a>Skapa en anpassad roll i portalen

Upptäcker att vissa organisationer framtvinga strikt behörighetskontroller i Azure, hittar du följande PowerShell-skript som aktiverar skapandet av en anpassad roll ”SQL Analytics övervakning-operatör” i Azure portal med minst Läs- och skrivbehörighet krävs för att använda Azure SQL Analytics så stort som möjligt.

Ersätt ”{SubscriptionId}” i den med ditt Azure-prenumerations-ID-skriptet nedan och kör skriptet med inloggad som en ägare eller deltagare roll i Azure.

   ```powershell
    Connect-AzureRmAccount
    Select-AzureRmSubscription {SubscriptionId}
    $role = Get-AzureRmRoleDefinition -Name Reader
    $role.Name = "SQL Analytics Monitoring Operator"
    $role.Description = "Lets you monitor database performance with Azure SQL Analytics as a reader. Does not allow change of resources."
    $role.IsCustom = $true
    $role.Actions.Add("Microsoft.SQL/servers/databases/read");
    $role.Actions.Add("Microsoft.SQL/servers/databases/topQueries/queryText/*");
    $role.Actions.Add("Microsoft.Sql/servers/databases/advisors/read");
    $role.Actions.Add("Microsoft.Sql/servers/databases/advisors/write");
    $role.Actions.Add("Microsoft.Sql/servers/databases/advisors/recommendedActions/read");
    $role.Actions.Add("Microsoft.Sql/servers/databases/advisors/recommendedActions/write");
    $role.Actions.Add("Microsoft.Sql/servers/databases/automaticTuning/read");
    $role.Actions.Add("Microsoft.Sql/servers/databases/automaticTuning/write");
    $role.Actions.Add("Microsoft.Sql/servers/databases/*");
    $role.Actions.Add("Microsoft.Sql/servers/advisors/read");
    $role.Actions.Add("Microsoft.Sql/servers/advisors/write");
    $role.Actions.Add("Microsoft.Sql/servers/advisors/recommendedActions/read");
    $role.Actions.Add("Microsoft.Sql/servers/advisors/recommendedActions/write");
    $role.Actions.Add("Microsoft.Resources/deployments/write");
    $role.AssignableScopes = "/subscriptions/{SubscriptionId}"
    New-AzureRmRoleDefinition $role
   ```

När den nya rollen har skapats kan du tilldela den här rollen till varje användare som du måste tilldela anpassade behörigheter för att använda Azure SQL Analytics.

## <a name="analyze-data-and-create-alerts"></a>Analysera data och skapa aviseringar

Analysera data i Azure SQL Analytics är baserad på [Log Analytics-språket](./query-language/get-started-queries.md) för din anpassade frågor och rapporter. Hittar du beskrivningen av de tillgängliga data som samlas in från databasen resource för anpassade frågor i [mått och loggar tillgängliga](../sql-database/sql-database-metrics-diag-logging.md#metrics-and-logs-available).

Automatiserade aviseringar i lösningen baseras på skriva en Log Analytics-fråga som utlöser en avisering när ett villkor uppfylls. Du hitta nedan flera exempel på Log Analytics-frågor på vilka aviseringar kan konfigureras i lösningen.

### <a name="creating-alerts-for-azure-sql-database"></a>Skapa aviseringar för Azure SQL Database

Du kan enkelt [skapa aviseringar](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md) med data från Azure SQL Database-resurser. Här är några användbara [loggsökning](log-analytics-log-searches.md) frågor som du kan använda med en logg-avisering:

*Hög CPU i Azure SQL-databas*

```
AzureMetrics 
| where ResourceProvider=="MICROSOFT.SQL"
| where ResourceId contains "/DATABASES/"
| where MetricName=="cpu_percent" 
| summarize AggregatedValue = max(Maximum) by bin(TimeGenerated, 5m)
| render timechart
```

> [!NOTE]
> - Före kravet för att ställa in den här aviseringen är den övervakade databaser stream diagnostikmåtten (alternativet ”alla mått”) till lösningen.
> - Ersätt MetricName värdet cpu_percent med dtu_consumption_percent att få hög DTU-resultaten i stället.

*Hög CPU på Azure SQL Database elastiska pooler*

```
AzureMetrics 
| where ResourceProvider=="MICROSOFT.SQL"
| where ResourceId contains "/ELASTICPOOLS/"
| where MetricName=="cpu_percent" 
| summarize AggregatedValue = max(Maximum) by bin(TimeGenerated, 5m)
| render timechart
```

> [!NOTE]
> - Före kravet för att ställa in den här aviseringen är den övervakade databaser stream diagnostikmåtten (alternativet ”alla mått”) till lösningen.
> - Ersätt MetricName värdet cpu_percent med dtu_consumption_percent att få hög DTU-resultaten i stället.

*Azure SQL Database-lagring i genomsnitt över 95% i den senaste 1 tim*

```
let time_range = 1h;
let storage_threshold = 95;
AzureMetrics
| where ResourceId contains "/DATABASES/"
| where MetricName == "storage_percent"
| summarize max_storage = max(Average) by ResourceId, bin(TimeGenerated, time_range)
| where max_storage > storage_threshold
| distinct ResourceId
```

> [!NOTE]
> - Före kravet för att ställa in den här aviseringen är den övervakade databaser stream diagnostikmåtten (alternativet ”alla mått”) till lösningen.
> - Den här frågan kräver en aviseringsregel som ställas in för att ställa in en avisering när det finns resultat (> 0 resultat) från frågan anger att villkoret finns på vissa databaser. Utdata är en lista över databasresurser som är ovanför storage_threshold inom time_range som definierats.
> - Utdata är en lista över databasresurser som är ovanför storage_threshold inom time_range som definierats.

*Avisera om smarta insikter*

```
let alert_run_interval = 1h;
let insights_string = "hitting its CPU limits";
AzureDiagnostics
| where Category == "SQLInsights" and status_s == "Active" 
| where TimeGenerated > ago(alert_run_interval)
| where rootCauseAnalysis_s contains insights_string
| distinct ResourceId
```

> [!NOTE]
> - Före kravet för att ställa in den här aviseringen är övervakade databaser stream SQLInsights diagnostikloggen till lösningen.
> - Den här frågan kräver en varningsregel konfigureras att köras med samma frekvens som alert_run_interval för att undvika dubbla resultat. Regeln ska ställas in för att utlösa aviseringen när det finns resultat (> 0 resultat) från frågan.
> - Anpassa alert_run_interval för att ange tidsintervall för att kontrollera om villkoret har uppstått på databaser som har konfigurerats till stream SQLInsights loggen till lösningen.
> - Anpassa insights_string för att samla in utdata från insikter rot orsak analysis text. Det här är samma text som visas i Användargränssnittet för den lösning som du kan använda från de befintliga insikterna. Du kan också använda frågan nedan för att se texten i alla information som genererats för din prenumeration. Använda utdata från frågan för att inhämta distinkta strängar för att ställa in aviseringar på insikter.

```
AzureDiagnostics
| where Category == "SQLInsights" and status_s == "Active" 
| distinct rootCauseAnalysis_s
```

### <a name="creating-alerts-for-managed-instance"></a>Skapa aviseringar för hanterad instans

* Hanterad instans-lagring är över 90%

```
let storage_percentage_treshold = 90;
AzureDiagnostics
| where Category =="ResourceUsageStats"
| summarize (TimeGenerated, calculated_storage_percentage) = arg_max(TimeGenerated, todouble(storage_space_used_mb_s) *100 / todouble (reserved_storage_mb_s))
   by ResourceId
| where calculated_storage_percentage > storage_percentage_treshold
```

> [!NOTE]
> - Före krav för att ställa in den här aviseringen är att övervakade hanterad instans har strömning av ResourceUsageStats log aktiverat i lösningen.
> - Den här frågan kräver en aviseringsregel som ställas in för att utlösa en avisering när det finns resultat (> 0 resultat) från frågan anger att villkoret finns på den hanterade instansen. Utdata är procent lagringsanvändning på den hanterade instansen.

### <a name="pricing"></a>Prissättning

Medan lösningen kostar ingenting att använda gäller förbrukning av diagnostiktelemetri ovan de kostnadsfria enheterna för datainmatning allokerade varje månad, se [priserna för Log Analytics](https://azure.microsoft.com/en-us/pricing/details/monitor). De kostnadsfria enheterna för datainmatning tillhandahålls aktivera kostnadsfri övervakning av flera databaser varje månad. Observera att mer aktiva databaser med större arbetsbelastningar kommer mata in mer data jämfört med inaktiva databaser. Du kan enkelt övervaka din förbrukning för inmatning av data i lösningen genom att välja OMS-arbetsyta på navigeringsmenyn i Azure SQL Analytics och sedan välja användning och uppskattade kostnader.

## <a name="next-steps"></a>Nästa steg

- Använd [Loggsökningar](log-analytics-log-searches.md) i Log Analytics för att visa detaljerad Azure SQL-data.
- [Skapa dina egna instrumentpaneler](log-analytics-dashboards.md) visar Azure SQL-data.
- [Skapa aviseringar](log-analytics-alerts.md) när specifika Azure SQL-händelser inträffar.
