---
title: Hur du frågar loggar från Azure Monitor för virtuella datorer (förhandsversion) | Microsoft Docs
description: Azure Monitor för virtuella datorer lösningen vidarebefordrar mätvärden och loggdata till Log Analytics och den här artikeln beskriver posterna och innehåller exempelfrågor.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/25/2018
ms.author: magoedte
ms.openlocfilehash: 90816061766a423f7dbc7d277433a95c5bcf6115
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50095430"
---
# <a name="how-to-query-logs-from-azure-monitor-for-vms-preview"></a>Hur du frågar loggar från Azure Monitor för virtuella datorer (förhandsversion)
Azure Monitor för virtuella datorer samlar in prestanda- och anslutningshanteringstjänsten mått, datorn och processen inventeringsdata och hälsotillståndsinformation och vidarebefordrar det till Log Analytics-datalager i Azure Monitor.  Informationen är tillgänglig för [search](../log-analytics/log-analytics-log-searches.md) i Log Analytics. Du kan använda dessa data för scenarier som omfattar planering av migreringsaktiviteter, kapacitetsanalys, identifiering och prestandafelsökning för på begäran.

## <a name="map-records"></a>Mappa poster
En post skapas per timme för varje unika datornamn och processen, förutom de poster som genereras när en process eller datorn startas inte eller är publicerat till Azure Monitor för virtuella datorer kartan funktion. Dessa poster har egenskaper i följande tabeller. Fält och värden i ServiceMapComputer_CL händelserna mappar till fält i resursen för datorer i Azure Resource Manager-API för ServiceMap. Fält och värden i ServiceMapProcess_CL händelserna mappar till fälten för Process-resurs i ServiceMap Azure Resource Manager API. Fältet ResourceName_s matchar namnfältet i motsvarande Resource Manager-resurs. 

Det finns internt genererade egenskaper som du kan använda för att identifiera unika processer och datorer:

- Dator: Använda *ResourceId* eller *ResourceName_s* att unikt identifiera en dator inom en Log Analytics-arbetsyta.
- Förlopp: Använda *ResourceId* att unikt identifiera en process i Log Analytics-arbetsytan. *ResourceName_s* är unikt inom kontexten för datorn där processen körs (MachineResourceName_s) 

Eftersom flera poster kan finnas för en angiven process och datorer i ett specifikt tidsintervall kan frågor returnera fler än en post för samma dator eller process. Om du vill inkludera endast den senaste posten, lägger du till ”| dedupliceringen ResourceId ”i frågan.

### <a name="connections"></a>Anslutningar
Anslutningsmått skrivs till en ny tabell i Log Analytics - VMConnection. Den här tabellen innehåller information om anslutningar för en virtuell dator (inkommande och utgående). Anslutningsmått blir också tillgängliga med API: er som tillhandahåller metoder för att hämta ett specifikt mått under ett tidsintervall.  TCP-anslutningar som härrör från ”*acceptera*- ing för en lyssnande socket är inkommande, samtidigt som de som används av *ansluta*- ing till en given IP och port är utgående. Riktning för en anslutning som representeras av egenskapen riktning, som kan vara inställd på antingen **inkommande** eller **utgående**. 

Poster i tabellerna genereras från data som rapporteras av beroendeagenten. Varje post representerar en områdes under ett tidsintervall för en minut. Egenskapen TimeGenerated anger början av tidsintervallet. Varje post innehåller information för att identifiera entiteten respektive, det vill säga, anslutning eller port samt mått som är associerade med denna entitet. För närvarande rapporteras endast nätverksaktivitet som sker med TCP över IPv4.

För att hantera kostnaden och komplexiteten, utgör anslutningen poster inte enskilda fysiska nätverksanslutningar. Flera fysiska nätverksanslutningar är grupperade i en logisk anslutning, som sedan visas i respektive tabell.  Betydelse, registrerar i *VMConnection* tabell representerar en logisk gruppering och inte de enskilda fysiska anslutningar som är som observeras. Fysiska nätverksanslutningen som delar samma värde för följande attribut under ett givet intervall för en minut, slås ihop till en enskild logisk post i *VMConnection*. 

| Egenskap  | Beskrivning |
|:--|:--|
|Riktning |Riktning för anslutningen värdet är *inkommande* eller *utgående* |
|Dator |Datorn FQDN |
|Process |Identiteten för processen eller grupper av processer, initierar/godkänna anslutningen |
|SourceIp |IP-adressen för källan |
|DestinationIp |IP-adressen för målet |
|DestinationPort |Portnumret för mål |
|Protokoll |Protokoll som används för anslutningen.  Värden är *tcp*. |

Information om antalet grupperade fysiska anslutningar finns i följande egenskaper för posten för att redovisa effekten av gruppering:

| Egenskap  | Beskrivning |
|:--|:--|
|LinksEstablished |Antal fysiska nätverksanslutningar som har upprättats under tidsperioden för rapportering |
|LinksTerminated |Antal fysiska nätverksanslutningar som har avslutats under tidsperioden för rapportering |
|LinksFailed |Antal fysiska nätverksanslutningar som har misslyckats under tidsperioden för rapportering. Den här informationen är endast tillgänglig för utgående anslutningar. |
|LinksLive |Antal fysiska nätverksanslutningar som var öppna längst ned i tidsfönstret|

#### <a name="metrics"></a>Mått

Förutom antalet anslutningsmått, information om mängden data som skickas och tas emot på en viss logisk anslutning eller nätverksport ingår även i följande egenskaper för posten:

| Egenskap  | Beskrivning |
|:--|:--|
|BytesSent |Sammanlagt antal byte som har skickats under tidsperioden för rapportering |
|BytesReceived |Sammanlagt antal byte som tagits emot under tidsperioden för rapportering |
|Svar |Antal svar som observerats under tidsperioden för rapportering. 
|ResponseTimeMax |Den största svarstid (millisekunder) observerats under tidsperioden för rapportering. Egenskapen är tomt om inget värde.|
|ResponseTimeMin |Den minsta svarstid (millisekunder) observerats under tidsperioden för rapportering. Egenskapen är tomt om inget värde.|
|ResponseTimeSum |Summan av alla svarstider (millisekunder) som observerats under tidsperioden för rapportering. Egenskapen är tomt om inget värde.|

Den tredje typ av data som rapporteras svarstid – hur länge en anropare ägna åt att vänta på en begäran som skickas via en anslutning som ska bearbetas och besvarats av fjärrslutpunkten. Svarstiden som rapporteras är en uppskattning av SANT svarstiden för det underliggande protokollet. Det beräknas med hjälp av heuristik baserat på observationer av flödet av data mellan käll- och slutet av en fysisk anslutning. Den övergripande är skillnaden mellan den tid som den sista byten av en begäran lämnar avsändaren och tid när den sista byten av svaret kommer tillbaka till den. Dessa två tidsstämplar används för att ge en bild av händelser som begäranden och svar på en viss fysisk anslutning. Skillnaden mellan dem representerar svarstiden för en enskild begäran. 

I den första versionen av den här funktionen är vår algoritmen ett approximativt värde som kan fungera med varierande framgång beroende på typ av App-protokollet som används för en viss nätverksanslutning. Exempelvis kan den aktuella metoden fungerar bra för begäranden och svar-baserade protokoll som HTTP (S), men inte fungerar med enkelriktad och message queue-baserade protokoll.

Här följer några viktiga saker att tänka på:

1. Om en process som tar emot anslutningar på samma IP-adress men över flera nätverksgränssnitt, rapporteras en separat post för varje gränssnitt. 
2. Poster med jokertecken IP innehåller ingen aktivitet. De ingår som representerar det faktum att en port på datorn är öppna för inkommande trafik.
3. För att minska detaljnivå och datavolym, utelämnas poster med jokertecken IP när det finns en matchande post (för samma process, port och protokoll) med en specifik IP-adress. När ett jokertecken IP-adressposten utelämnas, ställs egenskapen IsWildcardBind poster med specifika IP-adress ”true” som anger att porten som exponeras över varje gränssnitt för den reporting datorn.
4. Portar som är bundna endast på ett visst gränssnitt har IsWildcardBind inställd på ”False”.

#### <a name="naming-and-classification"></a>Namngivning och klassificering
För att underlätta för som IP-adressen för den fjärranslutna datorn för en anslutning ingår i egenskapen RemoteIp. För inkommande anslutningar RemoteIp är samma som SourceIp, och för utgående anslutningar är det samma som DestinationIp. Egenskapen RemoteDnsCanonicalNames representerar de kanoniska DNS-namn som rapporterats av den för RemoteIp. Egenskaperna RemoteDnsQuestions och RemoteClassification är reserverade för framtida användning. 

#### <a name="geolocation"></a>Geoplats
*VMConnection* innehåller även geoplats information för den fjärranslutna datorn för varje post för anslutning av följande egenskaper för posten: 

| Egenskap  | Beskrivning |
|:--|:--|
|RemoteCountry |Namnet på det land som är värd för RemoteIp.  Till exempel *USA* |
|RemoteLatitude |Geoplats latitud. Till exempel *47.68* |
|RemoteLongitude |Geoplats longitud. Till exempel *-122.12* |

#### <a name="malicious-ip"></a>Skadlig IP
Varje RemoteIp-egenskapen i *VMConnection* tabell kontrolleras mot en uppsättning IP-adresser med känd skadlig aktivitet. Om RemoteIp identifieras som skadlig följande egenskaper är ifyllda (de är tom, när den IP-Adressen inte anses vara skadlig) i följande egenskaper för posten:

| Egenskap  | Beskrivning |
|:--|:--|
|MaliciousIp |RemoteIp-adress |
|IndicatorThreadType |Threat indikatorn har identifierats är något av följande värden *Botnät*, *C2*, *CryptoMining*, *Darknet*, *DDos* , *MaliciousUrl*, *skadlig kod*, *nätfiske*, *Proxy*, *oönskade program*, *Visningslista*.   |
|Beskrivning |Beskrivning av observerade hotet. |
|TLPLevel |Trafikljus Protocol (TLP) är en av de definierade värdena *White*, *grönt*, *gul*, *Red*. |
|Konfidensbedömning |Värden är *0 – 100*. |
|Severity |Värden är *0 – 5*, där *5* är den mest allvarliga och *0* inte är allvarligt alls. Standardvärdet är *3*.  |
|FirstReportedDateTime |Första gången providern rapporterade indikatorn. |
|LastReportedDateTime |Senast indikatorn har setts av Interflow. |
|IsActive |Anger indikatorer inaktiveras med *SANT* eller *FALSKT* värde. |
|ReportReferenceLink |Länkar till rapporter som rör en viss övervakas. |
|AdditionalInformation |Tillhandahåller ytterligare information om det är tillämpligt, om observerade hotet. |

### <a name="servicemapcomputercl-records"></a>ServiceMapComputer_CL poster
Poster med en typ av *ServiceMapComputer_CL* har inventeringsdata för servrar med beroendeagenten. Dessa poster har egenskaper i följande tabell:

| Egenskap  | Beskrivning |
|:--|:--|
| Typ | *ServiceMapComputer_CL* |
| SourceSystem | *OpsManager* |
| ResourceId | Den unika identifieraren för en dator i arbetsytan |
| ResourceName_s | Den unika identifieraren för en dator i arbetsytan |
| ComputerName_s | Datorn FQDN |
| Ipv4Addresses_s | En lista över serverns IPv4-adresser |
| Ipv6Addresses_s | En lista över serverns IPv6-adresser |
| DnsNames_s | En matris med DNS-namn |
| OperatingSystemFamily_s | Windows- eller Linux |
| OperatingSystemFullName_s | Det fullständiga namnet på operativsystemet  |
| Bitness_s | Bitar som inte för datorn (32-bitars eller 64-bitars)  |
| PhysicalMemory_d | Fysiskt minne i MB |
| Cpus_d | Antal processorer |
| CpuSpeed_d | CPU-hastighet i MHz|
| VirtualizationState_s | *Okänd*, *fysiska*, *virtuella*, *hypervisor-programmet* |
| VirtualMachineType_s | *Hyper-v*, *vmware*och så vidare |
| VirtualMachineNativeMachineId_g | VM-ID som tilldelats av dess hypervisor-programmet |
| VirtualMachineName_s | Namnet på den virtuella datorn |
| BootTime_t | Starttiden |

### <a name="servicemapprocesscl-type-records"></a>ServiceMapProcess_CL poster
Poster med en typ av *ServiceMapProcess_CL* har inventeringsdata för TCP-anslutna processer på servrar med beroendeagenten. Dessa poster har egenskaper i följande tabell:

| Egenskap  | Beskrivning |
|:--|:--|
| Typ | *ServiceMapProcess_CL* |
| SourceSystem | *OpsManager* |
| ResourceId | Den unika identifieraren för en process i arbetsytan |
| ResourceName_s | Den unika identifieraren för en process på datorn där den körs|
| MachineResourceName_s | Resursnamnet för datorn |
| ExecutableName_s | Namnet på processprogramfil |
| StartTime_t | Starttid för process-pool |
| FirstPid_d | Första PID i programprocess-poolen |
| Description_s | Beskrivning av process |
| CompanyName_s | Namnet på företaget |
| InternalName_s | Det interna namnet |
| ProductName_s | Namnet på produkten |
| ProductVersion_s | Produktversion |
| FileVersion_s | Filversionen |
| CommandLine_s | Från kommandoraden |
| ExecutablePath _Vä | Sökvägen till den körbara filen |
| WorkingDirectory_s | Arbetskatalogen |
| Användarnamn | Det konto som processen körs |
| UserDomain | Den domän där processen körs |

## <a name="sample-log-searches"></a>Exempel på loggsökningar

### <a name="list-all-known-machines"></a>Lista över alla kända datorer
`ServiceMapComputer_CL | summarize arg_max(TimeGenerated, *) by ResourceId`

### <a name="list-the-physical-memory-capacity-of-all-managed-computers"></a>Lista över kapacitet för fysiskt minne för alla hanterade datorer.
`ServiceMapComputer_CL | summarize arg_max(TimeGenerated, *) by ResourceId | project PhysicalMemory_d, ComputerName_s`

### <a name="list-computer-name-dns-ip-and-os"></a>Lista datornamn, DNS, IP- och OS.
`ServiceMapComputer_CL | summarize arg_max(TimeGenerated, *) by ResourceId | project ComputerName_s, OperatingSystemFullName_s, DnsNames_s, Ipv4Addresses_s`

### <a name="find-all-processes-with-sql-in-the-command-line"></a>Hitta alla processer med ”sql” på kommandoraden
`ServiceMapProcess_CL | where CommandLine_s contains_cs "sql" | summarize arg_max(TimeGenerated, *) by ResourceId`

### <a name="find-a-machine-most-recent-record-by-resource-name"></a>Hitta en dator (senaste post) efter resursnamn
`search in (ServiceMapComputer_CL) "m-4b9c93f9-bc37-46df-b43c-899ba829e07b" | summarize arg_max(TimeGenerated, *) by ResourceId`

### <a name="find-a-machine-most-recent-record-by-ip-address"></a>Hitta en dator (senaste post) genom att IP-adress
`search in (ServiceMapComputer_CL) "10.229.243.232" | summarize arg_max(TimeGenerated, *) by ResourceId`

### <a name="list-all-known-processes-on-a-specified-machine"></a>Lista över alla kända processer på en angiven dator
`ServiceMapProcess_CL | where MachineResourceName_s == "m-559dbcd8-3130-454d-8d1d-f624e57961bc" | summarize arg_max(TimeGenerated, *) by ResourceId`

### <a name="list-all-computers-running-sql"></a>Lista över alla datorer som kör SQL
`ServiceMapComputer_CL | where ResourceName_s in ((search in (ServiceMapProcess_CL) "\*sql\*" | distinct MachineResourceName_s)) | distinct ComputerName_s`

### <a name="list-all-unique-product-versions-of-curl-in-my-datacenter"></a>Lista över alla unika produktversioner av curl i mitt datacenter
`ServiceMapProcess_CL | where ExecutableName_s == "curl" | distinct ProductVersion_s`

### <a name="create-a-computer-group-of-all-computers-running-centos"></a>Skapa en datorgrupp för alla datorer som kör CentOS
`ServiceMapComputer_CL | where OperatingSystemFullName_s contains_cs "CentOS" | distinct ComputerName_s`

### <a name="summarize-the-outbound-connections-from-a-group-of-machines"></a>Sammanfatta utgående anslutningar från en grupp datorer
```
// the machines of interest
let machines = datatable(m: string) ["m-82412a7a-6a32-45a9-a8d6-538354224a25"];
// map of ip to monitored machine in the environment
let ips=materialize(ServiceMapComputer_CL
| summarize ips=makeset(todynamic(Ipv4Addresses_s)) by MonitoredMachine=ResourceName_s
| mvexpand ips to typeof(string));
// all connections to/from the machines of interest
let out=materialize(VMConnection
| where Machine in (machines)
| summarize arg_max(TimeGenerated, *) by ConnectionId);
// connections to localhost augmented with RemoteMachine
let local=out
| where RemoteIp startswith "127."
| project ConnectionId, Direction, Machine, Process, ProcessName, SourceIp, DestinationIp, DestinationPort, Protocol, RemoteIp, RemoteMachine=Machine;
// connections not to localhost augmented with RemoteMachine
let remote=materialize(out
| where RemoteIp !startswith "127."
| join kind=leftouter (ips) on $left.RemoteIp == $right.ips
| summarize by ConnectionId, Direction, Machine, Process, ProcessName, SourceIp, DestinationIp, DestinationPort, Protocol, RemoteIp, RemoteMachine=MonitoredMachine);
// the remote machines to/from which we have connections
let remoteMachines = remote | summarize by RemoteMachine;
// all augmented connections
(local)
| union (remote)
//Take all outbound records but only inbound records that come from either //unmonitored machines or monitored machines not in the set for which we are computing dependencies.
| where Direction == 'outbound' or (Direction == 'inbound' and RemoteMachine !in (machines))
| summarize by ConnectionId, Direction, Machine, Process, ProcessName, SourceIp, DestinationIp, DestinationPort, Protocol, RemoteIp, RemoteMachine
// identify the remote port
| extend RemotePort=iff(Direction == 'outbound', DestinationPort, 0)
// construct the join key we'll use to find a matching port
| extend JoinKey=strcat_delim(':', RemoteMachine, RemoteIp, RemotePort, Protocol)
// find a matching port
| join kind=leftouter (VMBoundPort 
| where Machine in (remoteMachines) 
| summarize arg_max(TimeGenerated, *) by PortId 
| extend JoinKey=strcat_delim(':', Machine, Ip, Port, Protocol)) on JoinKey
// aggregate the remote information
| summarize Remote=makeset(iff(isempty(RemoteMachine), todynamic('{}'), pack('Machine', RemoteMachine, 'Process', Process1, 'ProcessName', ProcessName1))) by ConnectionId, Direction, Machine, Process, ProcessName, SourceIp, DestinationIp, DestinationPort, Protocol
```

## <a name="next-steps"></a>Nästa steg
* Om du är nybörjare på att skriva frågor i Log Analytics kan du granska [hur du använder Log Analytics-sidan](../log-analytics/query-language/get-started-analytics-portal.md) i Azure portal för att skriva Log Analytics-frågor.
* Lär dig mer om [skriva sökfrågor](../log-analytics/query-language/search-queries.md).