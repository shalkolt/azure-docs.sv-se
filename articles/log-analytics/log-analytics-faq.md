---
title: Log Analytics vanliga frågor och svar | Microsoft Docs
description: Svar på vanliga frågor om Azure Log Analytics-tjänsten.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ad536ff7-2c60-4850-a46d-230bc9e1ab45
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: 08a85cea75d692573f9e9e6c4bcd8bb61e03867f
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/02/2018
ms.locfileid: "48041977"
---
# <a name="log-analytics-faq"></a>Vanliga frågor och svar om Log Analytics
Den här Microsoft-FAQ är en lista över vanliga frågor om Log Analytics i Microsoft Azure. Om du har ytterligare frågor om Log Analytics kan du gå till den [diskussionsforum](https://social.msdn.microsoft.com/Forums/azure/home?forum=opinsights) och ställa frågor. När en fråga är vanliga vi lägga till det i den här artikeln så att den finns snabbt och enkelt.


## <a name="new-logs-experience"></a>Nya upplevelsen för loggar

### <a name="q-whats-the-difference-between-the-new-logs-experience-and-log-analytics"></a>F: Vad är skillnaden mellan den nya upplevelsen för loggar och Log Analytics?

S: de är samma sak. [Log Analytics ingår som en funktion i Azure Monitor](../azure-monitor/azure-monitor-rebrand.md) att tillhandahålla en mer enhetlig övervakning miljö. Den nya upplevelsen för loggar i Azure Monitor är exakt samma som Log Analytics-frågor som många kunder redan har använt.

### <a name="q-can-i-still-use-log-search"></a>F: kan jag fortfarande använda Log Search? 

S: log Search finns för närvarande är fortfarande tillgängliga i OMS-portalen och i Azure-portalen under namnet **loggar (klassisk)**. OMS-portalen kommer officiellt dras tillbaka den 15 januari 2019. Den klassiska upplevelsen för loggar i Azure-portalen kommer att dras tillbaka gradvis och ersätts den nya upplevelsen för loggar. 

### <a name="q-can-i-still-use-advanced-analytics-portal"></a>F. Kan jag fortfarande använda Advanced Analytics-portalen? 
Den nya upplevelsen för loggar i Azure-portalen är baserad på den [Advanced Analytics-portalen](https://portal.loganalytics.io/), men den fortfarande kan användas utanför Azure-portalen. En översikt över för att ta bort den här externa portalen tillkännages snart.

### <a name="q-why-cant-i-see-query-explorer-and-save-buttons-in-the-new-logs-experience"></a>F. Varför kan inte se Query Explorer och spara knappar i den nya upplevelsen för loggar?

**Fråga Explorer**, **spara** och **ange avisering** knappar är inte tillgängliga när du utforskar loggar i kontexten för en viss resurs. För att skapa aviseringar, spara eller läsa in en fråga, begränsas loggar till en arbetsyta. För att öppna loggar i kontexten för arbetsytan, Välj **alla tjänster** > **övervakaren** > **loggar**. Senaste använda arbetsytan har valts, men du kan välja en annan arbetsyta. Se [visa och analysera data i Log Analytics](../log-analytics/log-analytics-log-search-portals.md) för mer information.

### <a name="q-how-do-i-extract-custom-fields-in-the-new-logs-experience"></a>F. Hur extraherar anpassade fält i den nya upplevelsen för loggar? 

S: anpassade fält extrahering stöds för närvarande i den klassiska loggar upplevelse. 

### <a name="q-where-do-i-find-list-view-in-the-new-logs"></a>F. Var hittar jag listvy i de nya loggarna? 

S: listvyn är inte tillgänglig i de nya loggarna. Det finns en pil till vänster om varje post i resultattabellen. Klicka på pilen för att öppna informationen för en viss post. 

### <a name="q-after-running-a-query-a-list-of-suggested-filters-shows-up-but-it-doesnt-include-all-filters-how-can-i-see-the-rest"></a>F. En lista över föreslagna filter som visas när du har kört en fråga, men de omfattar inte alla filter. Hur kan jag se resten? 

S: det som för närvarande visas är en förhandsgranskning av hur filter. Detta är nu baserat på hela resultatuppsättningen i stället för att vara begränsad till 10 000 postgränsen av Användargränssnittet. Det här är en lista över de mest populära filter och 10 vanligaste värden för varje filter. 

### <a name="q-why-am-i-getting-the-error-register-resource-provider-microsoftinsights-for-this-subscription-to-enable-this-query-in-logs-after-drilling-in-from-vm"></a>F. Varför får jag felet: ”registrera resursprovidern” Microsoft.Insights ”för den här prenumerationen att aktivera den här frågan” i loggar efter Detaljgranskning i från virtuell dator? 

S: som standard många resursproviders registreras automatiskt, men du kan behöva registrera några resursproviders manuellt. Den här koden konfigurerar din prenumeration för att arbeta med resursprovidern. Omfattningen för registrering är alltid prenumerationen. Mer information finns i [Resursproviders och resurstyper](../azure-resource-manager/resource-manager-supported-services.md#portal).

### <a name="q-why-am-i-am-getting-no-access-error-message-when-accessing-logs-from-a-vm-page"></a>F. Varför kan jag får ingen åtkomst felmeddelande vid åtkomst till loggar från en sida för virtuell dator? 

S: Om du vill visa VM-loggar måste beviljas läsbehörighet till arbetsytor som lagrar VM-loggarna. I dessa fall kan måste administratören bevilja dig behörigheter i Azure.

### <a name="q-why-can-i-can-access-my-workspace-in-oms-portal-but-i-get-the-error-you-have-no-access-in-the-azure-portal"></a>F. Varför får jag åtkomst till Min arbetsyta i OMS-portalen, men jag får felet ”du har inte behörighet” i Azure-portalen?  

S: Om du vill öppna en arbetsyta i Azure måste du ha Azure behörigheter. Det finns tillfällen där du inte kanske har rätt behörighet. I dessa fall kan administratören måste ge dig med behörigheter i Azure.See [OMS-portalen som flyttar till Azure](../log-analytics/log-analytics-oms-portal-transition.md) för mer information.

### <a name="q-why-cant-i-cant-see-view-designer-entry-in-logs"></a>F. Varför ser jag inte kan inte Vydesigner posten i loggarna? 
S: view Designer finns bara i loggarna för användare som har tilldelats med deltagarbehörighet eller högre.

### <a name="q-can-i-still-use-the-analytics-portal-outside-of-azure"></a>F. Kan jag fortfarande använda analysportalen utanför Azure?
A. Ja, loggarna sidan i Azure och [Advanced Analytics-portalen](https://portal.loganalytics.io) baseras på samma kod. Log Analytics ingår som en funktion i Azure Monitor för att ge en mer enhetlig upplevelse för övervakning. Du kan fortfarande komma åt Analytics-portalen med hjälp av URL: https://portal.loganalytics.io/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/workspaces/{workspaceName}.



## <a name="general"></a>Allmänt

### <a name="q-how-can-i-see-my-views-and-solutions-in-azure-portal"></a>F. Hur hittar jag Mina vyer och lösningar i Azure-portalen? 

S: en lista över vyer och installerade lösningar är tillgängliga i Azure-portalen. Klicka på **Alla tjänster**. Välj i listan över resurser, **övervakaren**, klicka sedan på **... Mer**. Senaste använda arbetsytan har valts, men du kan välja en annan arbetsyta. 

### <a name="q-why-i-cant-create-workspaces-in-west-central-us-region"></a>F. Varför kan jag skapa arbetsytor i regionen USA, västra region? 

S: det här området är i tillfälligt kapacitetsgränsen. Gränsen är planerad att åtgärdas i den första halvan av 2019.


### <a name="q-does-log-analytics-use-the-same-agent-as-azure-security-center"></a>F. Använder Log Analytics samma agent som Azure Security Center?

S: med tidig juni 2017 började Azure Security Center använder Microsoft Monitoring Agent för att samla in och lagra data. Mer information finns i [Azure Security Center-plattformen migrering vanliga frågor och svar](../security-center/security-center-platform-migration-faq.md).

### <a name="q-what-checks-are-performed-by-the-ad-and-sql-assessment-solutions"></a>F. Vilka kontroller som utförs av AD och SQL-bedömning lösningar?

S: följande fråga visar en beskrivning av alla kontroller som utförs för närvarande:

```
(Type=SQLAssessmentRecommendation OR Type=ADAssessmentRecommendation) | dedup RecommendationId | select FocusArea, ActionArea, Recommendation, Description | sort Type, FocusArea,ActionArea, Recommendation
```

Resultaten kan sedan exporteras till Excel för vidare undersökning.

### <a name="q-why-do-i-see-something-different-than-oms-in-the-system-center-operations-manager-console"></a>F. Varför ser jag något annat än OMS i System Center Operations Manager-konsolen?

S: beroende på vilka Update Rollup av Operations Manager du har, kan du se en nod för *System Center Advisor*, *Operational Insights*, eller *Log Analytics*.

Text sträng uppdateringen *OMS* ingår i ett hanteringspaket som importeras manuellt. Om du vill se aktuell text och funktioner, följer du anvisningarna i den senaste System Center Operations Manager Update Rollup KB-artikeln och uppdatera konsolen.

### <a name="q-is-there-an-on-premises-version-of-log-analytics"></a>F: finns det en lokal version av Log Analytics?

S: Nej. Log Analytics är en skalbar molnbaserad tjänst som bearbetar och lagrar stora mängder data. 

### <a name="q-how-do-i-troubleshoot-if-log-analytics-is-no-longer-collecting-data"></a>F. Hur felsöker jag om Log Analytics inte längre att samla in data?

S: för en prenumeration och en arbetsyta som skapats innan den 2 April 2018 som finns på den *kostnadsfri* prisnivå, om mer än 500 MB data skickas under en dag, stoppar insamling av data under resten av dagen. Når den dagliga gränsen är en vanlig orsak som Log Analytics slutar att samla in data eller data verkar sakna.  

Log Analytics skapar en händelse av typen *pulsslag* och kan användas för att avgöra om datainsamling stoppas. 

Kör följande fråga i sökningen för att kontrollera om du når den dagliga gränsen och saknade data: `Heartbeat | summarize max(TimeGenerated)`

Om du vill kontrollera en viss dator, kör du följande fråga: `Heartbeat | where Computer=="contosovm" | summarize max(TimeGenerated)`

När datainsamlingen slutar, beroende på det tidsintervall som valts, visas inte några poster som returneras.   

I följande tabell beskrivs skäl som stoppar insamling av data och en rekommenderad åtgärd för att återuppta insamling av data:

| Insamling av orsak stoppar                       | Att återuppta insamling av data |
| -------------------------------------------------- | ----------------  |
| Kostnadsfria data gränsen<sup>1</sup>       | Vänta tills nästföljande månad för samlingen att starta om automatiskt, eller<br> Ändra till en betald prisnivå |
| Azure-prenumerationen är i ett pausat tillstånd på grund av: <br> Kostnadsfri utvärderingsversion avslutades <br> Azure-pass har upphört att gälla <br> Varje månad utgiftsgränsen har nåtts (till exempel på en MSDN eller Visual Studio-prenumeration)                          | Konvertera till en betald prenumeration <br> Konvertera till en betald prenumeration <br> Ta bort gränsen, eller vänta tills begränsningen återställs |

<sup>1</sup> om din arbetsyta på den *kostnadsfri* prisnivå, är du begränsad till att skicka 500 MB data per dag till tjänsten. När du når den dagliga gränsen stoppas insamling av data till nästa dag. Data som skickas när datainsamling har stoppats har indexerats inte och är inte tillgängliga för sökning. När insamling av data återupptar sker bearbetningen endast för nya data som skickas. 

Log Analytics använder UTC-tid och varje dag startas normalt vid midnatt UTC. Om arbetsytan når den dagliga gränsen, återupptar bearbetningen under den första timmen av nästa dag för UTC.

### <a name="q-how-can-i-be-notified-when-data-collection-stops"></a>F. Hur kan jag bli meddelad när datainsamlingen slutar?

S: med stegen som beskrivs i [skapa en ny log-avisering](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md) meddelas när datainsamlingen slutar.

Ange när du skapar aviseringen för när datainsamlingen slutar den:

- **Definiera aviseringsvillkor** ange Log Analytics-arbetsytan som mål för resursen.
- **Aviseringskriterier** ange följande:
   - **Signalnamn** Välj **anpassade loggsökning**.
   - **Sökfråga** till`Heartbeat | summarize LastCall = max(TimeGenerated) by Computer | where LastCall < ago(15m)`
   - **Aviseringslogik** är **Baserad på** *antal resultat* och **Villkor** som är *Större än* ett **Tröskelvärde** på *0*
   - **Tidsperiod** av *30* minuter och **Varningsfrekvens** till varje *10* minuter
- **Definiera aviseringsinformation** ange följande:
   - **Namnet** till *insamling av Data stoppades*
   - **Allvarlighetsgrad** till *varning*

Ange en befintlig eller skapa en ny [åtgärdsgrupp](../monitoring-and-diagnostics/monitoring-action-groups.md) så att när aviseringen log matchar villkoren, du får ett meddelande om du har ett pulsslag saknas för mer än 15 minuter.

## <a name="configuration"></a>Konfiguration
### <a name="q-can-i-change-the-name-of-the-tableblob-container-used-to-read-from-azure-diagnostics-wad"></a>F. Kan jag ändra namnet på tabell-/ blob-behållaren som används för att läsa från Azure Diagnostics SÄKERHETSSPECIFIKA?

A. Nej, det går inte för närvarande att läsa från valfri tabeller eller behållare i Azure storage.

### <a name="q-what-ip-addresses-does-the-log-analytics-service-use-how-do-i-ensure-that-my-firewall-only-allows-traffic-to-the-log-analytics-service"></a>F. IP-adresser använder den Log Analytics-tjänsten? Hur undviker jag att min brandvägg endast tillåter trafik till Log Analytics-tjänsten?

A. Log Analytics-tjänsten bygger på Azure. Log Analytics-IP-adresser som tillhör den [IP-intervall i Microsoft Azure Datacenter](http://www.microsoft.com/download/details.aspx?id=41653).

Eftersom tjänstdistributioner görs, ändra faktiska IP-adresserna för Log Analytics-tjänsten. DNS-namn ska tillåtas via brandväggen finns dokumenterade i [krav på](log-analytics-concept-hybrid.md#network-firewall-requirements).

### <a name="q-i-use-expressroute-for-connecting-to-azure-does-my-log-analytics-traffic-use-my-expressroute-connection"></a>F. Jag bara använder ExpressRoute för att ansluta till Azure. Använder Mina Log Analytics-trafik min ExpressRoute-anslutning?

A. De olika typerna av ExpressRoute trafik beskrivs i den [dokumentation om ExpressRoute](../expressroute/expressroute-faqs.md#supported-services).

Trafik till Log Analytics använder offentliga peering ExpressRoute-kretsen.

### <a name="q-is-there-a-simple-and-easy-way-to-move-an-existing-log-analytics-workspace-to-another-log-analytics-workspaceazure-subscription"></a>F. Finns det ett enkelt och smidigt sätt att flytta en befintlig Log Analytics-arbetsyta till en annan Log Analytics-arbetsyta/Azure-prenumeration?

A. Den `Move-AzureRmResource` cmdlet kan du flytta en Log Analytics-arbetsyta och ett Automation-konto från en Azure-prenumeration till en annan. Mer information finns i [Move-AzureRmResource](http://msdn.microsoft.com/library/mt652516.aspx).

Den här ändringen kan också göras i Azure-portalen.

Du kan inte flytta data från en Log Analytics-arbetsyta till en annan eller ändra den region som Log Analytics-data lagras i.

### <a name="q-how-do-i-add-log-analytics-to-system-center-operations-manager"></a>F: hur gör jag för att lägga till Log Analytics till System Center Operations Manager?

S: uppdatera till den senaste samlade uppdateringen och importera hanteringspaket kan du ansluta Operations Manager till Log Analytics.

>[!NOTE]
>Operations Manager-anslutning till Log Analytics är endast tillgänglig för System Center Operations Manager 2012 SP1 och senare.

### <a name="q-how-can-i-confirm-that-an-agent-is-able-to-communicate-with-log-analytics"></a>F: hur kan jag bekräfta att en agent ska kunna kommunicera med Log Analytics?

S: för att säkerställa att agenten kan kommunicera med OMS, gå till: Kontrollpanelen-, säkerhets- och inställningar, **Microsoft Monitoring Agent**.

Under den **Azure Log Analytics (OMS)** fliken ska du leta efter en grön bockmarkering. En grön bockmarkering som bekräftar att agenten ska kunna kommunicera med Azure-tjänsten.

En gul varningsikon innebär att agenten har problem med kommunikationen med Log Analytics. En vanlig orsak är att Microsoft Monitoring Agent-tjänsten har stoppats. Använda tjänsthanteraren för att starta om tjänsten.

### <a name="q-how-do-i-stop-an-agent-from-communicating-with-log-analytics"></a>F: hur stoppar jag en agent från att kommunicera med Log Analytics?

S: i System Center Operations Manager, ta bort datorn från listan över hanterade datorer OMS. Operations Manager uppdaterar konfigurationen av agenten inte längre rapporten till Log Analytics. Agenter som är direkt anslutna till Log Analytics, kan du hindra dem från kommunicerar via: Kontrollpanelen-, säkerhets- och inställningar, **Microsoft Monitoring Agent**.
Under **Azure Log Analytics (OMS)**, ta bort alla arbetsytor som visas.

### <a name="q-why-am-i-getting-an-error-when-i-try-to-move-my-workspace-from-one-azure-subscription-to-another"></a>F: Varför får jag ett fel när jag försöker flytta Min arbetsyta från en Azure-prenumeration till en annan?

S: Om du använder Azure-portalen kan du kontrollera att endast arbetsytan har valts för flytten. Markera inte lösningarna – flyttas automatiskt när arbetsytan har flyttats. 

Se till att du har behörighet i både Azure-prenumerationer.

## <a name="agent-data"></a>Agentinformationen
### <a name="q-how-much-data-can-i-send-through-the-agent-to-log-analytics-is-there-a-maximum-amount-of-data-per-customer"></a>F. Hur mycket data kan jag skicka via agenten till Log Analytics? Finns det en maximal mängd data per kund?
A. Det kostnadsfria abonnemanget anger en daglig högsta gräns på 500 MB per arbetsyta. Standard och premium-planer har ingen gräns för mängden data som överförs. Som en tjänst i molnet, Log Analytics har utformats för att automatiskt skala upp till referensen volymen kommer från en kund – även om det är terabyte per dag.

Log Analytics-agenten har utformats för att se till att den har ett litet utrymme. Datavolymen varierar beroende på de lösningar som du aktiverar. Du kan hitta detaljerad information på datavolymen och se en analys på detaljnivå av lösningen i den [användning](log-analytics-usage.md) sidan.

Mer information kan du läsa en [kunden blogg](http://thoughtsonopsmgr.blogspot.com/2015/09/one-small-footprint-for-server-one.html) visar sina resultat när du har utvärderat Resursanvändning (fotavtryck) OMS-agenten.

### <a name="q-how-much-network-bandwidth-is-used-by-the-microsoft-management-agent-mma-when-sending-data-to-log-analytics"></a>F. Hur mycket nätverksbandbredd används av Microsoft Management Agent (MMA) när du skickar data till Log Analytics?

A. Bandbredd är en funktion för mängden data som skickas. Data komprimeras som skickas över nätverket.

### <a name="q-how-much-data-is-sent-per-agent"></a>F. Hur mycket data skickas per agent?

A. Mängden data som skickats per agent beror på:

* Lösningar som du har aktiverat
* Antalet loggar och prestandaräknare som samlas in
* Mängden data i loggarna

Den kostnadsfria prisnivån är ett bra sätt att integrera flera servrar och mätare vanliga datavolym. Den totala användningen visas på den [användning](log-analytics-usage.md) sidan.

Använd följande fråga för datorer som kommer köras WireData-agenten kan se hur mycket data som skickas:

```
Type=WireData (ProcessName="C:\\Program Files\\Microsoft Monitoring Agent\\Agent\\MonitoringHost.exe") (Direction=Outbound) | measure Sum(TotalBytes) by Computer
```

## <a name="next-steps"></a>Nästa steg
* [Kom igång med Log Analytics](log-analytics-get-started.md) vill veta mer om Log Analytics och kom igång på bara några minuter.
