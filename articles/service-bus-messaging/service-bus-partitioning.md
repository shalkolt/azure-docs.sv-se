---
title: Skapa partitionerade Azure Service Bus-köer och ämnen | Microsoft Docs
description: Beskriver hur du ska partitionera Service Bus-köer och ämnen med hjälp av flera koordinatorerna.
services: service-bus-messaging
author: spelluru
manager: timlt
ms.service: service-bus-messaging
ms.topic: article
ms.date: 09/06/2018
ms.author: spelluru
ms.openlocfilehash: 2ca2063158634dfa42da094b77c70a2730e82176
ms.sourcegitcommit: 42405ab963df3101ee2a9b26e54240ffa689f140
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/28/2018
ms.locfileid: "47423178"
---
# <a name="partitioned-queues-and-topics"></a>Partitionerade köer och ämnen

Azure Service Bus använder flera koordinatorerna att bearbeta meddelanden och flera meddelandearkiv för att lagra meddelanden. En konventionell kö eller ämne hanteras av en enda asynkron meddelandekö och lagras i ett meddelandearkiv. Service Bus *partitioner* aktivera köer och ämnen, eller *meddelandeentiteter*, som ska partitioneras över flera koordinatorerna och meddelandearkiv. Partitionering innebär att hela dataflödet för en partitionerad entitet begränsas inte längre av prestanda för en enda asynkron meddelandekö eller ett meddelandearkiv. Dessutom kan återges ett tillfälligt avbrott i ett meddelandearkiv inte en partitionerad kö eller ett ämne inte tillgänglig. Partitionerade köer och ämnen kan innehålla alla avancerade funktioner för Service Bus, till exempel stöd för transaktioner och sessioner.

> [!NOTE]
> Partitionering är tillgängliga när en entitet skapas för alla köer och ämnen i Basic eller Standard-SKU: er. Det är inte tillgänglig för Premium SKU-meddelanden, men alla befintliga partitionerade enheter i Premium-namnområden som fortsätter att fungera som förväntat.
 
Det går inte att ändra alternativet partitionering på en befintlig kö eller ämne; Du kan bara ställa in alternativet när du skapar entiteten.

## <a name="how-it-works"></a>Hur det fungerar

Varje partitionerad kö eller ämne består av flera fragment. Varje fragment lagras i en annan meddelandearkiv och hanteras av en annan asynkron meddelandekö. När ett meddelande skickas till en partitionerad kö eller ett ämne tilldelar Service Bus meddelandet till en av fragment. Valet görs slumpmässigt av Service Bus eller genom att använda en partitionsnyckel som avsändaren kan ange.

När en klient vill få ett meddelande från en partitionerad kö eller från en prenumeration på en partitionerad ämne, Service Bus frågor alla fragment för meddelanden, returnerar du det första meddelandet som erhålls från någon av meddelandearkiven på mottagaren. Den andra meddelanden och returnerar dem när den får ytterligare Service Bus-cacheminnen ta emot begäranden. En mottagande klienten är inte medveten om partitionering; klientinriktade beteendet för en partitionerad kö eller ett ämne (till exempel läsa, Slutför, skjuta upp systemkön, förhämtar,) är identiska i beteendet hos en vanlig entitet.

Det finns ingen extra kostnad när de skickar ett meddelande till eller ta emot ett meddelande från en partitionerad kö eller ett ämne.

## <a name="enable-partitioning"></a>Aktivera partionering

Använd Azure SDK 2.2 eller senare för att använda partitionerade köer och ämnen med Azure Service Bus, eller ange `api-version=2013-10` eller senare i HTTP-förfrågningar.

### <a name="standard"></a>Standard

I Standard messaging-nivån kan skapa du Service Bus-köer och ämnen i 1, 2, 3, 4 eller 5 GB storlekar (standardvärdet är 1 GB). Med partitionering aktiverat, skapar Service Bus 16 kopior (16 partitioner) för entiteten för varje GB som du anger. Därmed, om du skapar en kö som är 5 GB i storlek med 16 partitioner största köstorlek blir (5 \* 16) = 80 GB. Du kan se den maximala storleken för din partitionerad kö eller ämne genom att titta på posten den [Azure-portalen][Azure portal]i den **översikt** bladet för denna entitet.

### <a name="premium"></a>Premium

I ett namnområde för Premium-nivån stöds partitionering entiteter inte. Du kan dock fortfarande skapa Service Bus-köer och ämnen i 1, 2, 3, 4, 5, 10, 20, 40 eller 80 GB storlekar (standardvärdet är 1 GB). Du kan se storleken på din kö eller ett ämne genom att titta på posten den [Azure-portalen][Azure portal]i den **översikt** bladet för denna entitet.

### <a name="create-a-partitioned-entity"></a>Skapa en partitionerad entitet

Det finns flera sätt att skapa en partitionerad kö eller ett ämne. När du skapar kön eller ämnet från ditt program kan du aktivera partitionering för kön eller ämnet genom att ange respektive den [QueueDescription.EnablePartitioning] [ QueueDescription.EnablePartitioning] eller [ TopicDescription.EnablePartitioning] [ TopicDescription.EnablePartitioning] egenskap **SANT**. De här egenskaperna måste anges vid tiden kön eller ämnet har skapats och är endast tillgängliga i den äldre [WindowsAzure.ServiceBus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) biblioteket. Som tidigare nämnts bör går det inte att ändra de här egenskaperna på en befintlig kö eller ämne. Exempel:

```csharp
// Create partitioned topic
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

Du kan också skapa en partitionerad kö eller ett ämne i den [Azure-portalen][Azure portal]. När du skapar en kö eller ämne i portalen på **aktivera partitionering** alternativ i kön eller ämnet **skapa** dialogrutan är markerad som standard. Du kan bara inaktivera det här alternativet i en Standard-nivån entity; på Premium-nivån partitionering stöds inte, och kryssrutan har ingen effekt. 

## <a name="use-of-partition-keys"></a>Användning av partitionsnycklar

När ett meddelande i kön till en partitionerad kö eller ett ämne, kontrollerar Service Bus förekomsten av en partitionsnyckel. Om den hittar en väljs fragment som baseras på nyckeln. Om den inte hittar en partitionsnyckel, väljs fragment baserat på en intern algoritm.

### <a name="using-a-partition-key"></a>Med hjälp av en partitionsnyckel

Vissa scenarier, till exempel sessioner eller transaktioner, behöver meddelanden som ska lagras i en specifik fragment. De här scenarierna kräver användning av en partitionsnyckel. Alla meddelanden som använder samma partitionsnyckel tilldelas till samma fragment. Om fragmentet är tillfälligt otillgänglig kan returneras Service Bus ett fel.

Beroende på scenario används olika meddelandeegenskaper som en partitionsnyckel:

**Sessions-ID**: om ett meddelande har den [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid) egenskapsuppsättning och Service Bus använder sedan **SessionID** som partitionsnyckel. På så sätt kan hanteras alla meddelanden som hör till samma session av samma meddelandekö. Sessioner gör att Service Bus att garantera meddelandeordningsföljd samt konsekvenskontroll av sessionstillstånd.

**PartitionKey**: om ett meddelande har den [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) egenskapen men inte den [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid) egenskapsuppsättning och Service Bus använder sedan den [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) egenskap värdet som partitionsnyckel. Om meddelandet har både den [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid) och [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) egenskaper definierade båda egenskaperna måste vara identiska. Om den [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) egenskapen till ett annat värde än den [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid) egenskapen, Service Bus returnerar ett ogiltigt åtgärdsundantag. Den [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) egenskapen ska användas om en avsändare skickar-session medveten transaktionella meddelanden. Partitionsnyckeln säkerställer att alla meddelanden som skickas i en transaktion som hanteras av samma asynkrona meddelandekön.

**MessageId**: om kön eller ämnet har den [RequiresDuplicateDetection](/dotnet/api/microsoft.azure.management.servicebus.models.sbqueue.requiresduplicatedetection) egenskapen **SANT** och [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid) eller [ PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) egenskaper inte har angetts, kommer [MessageId](/dotnet/api/microsoft.azure.servicebus.message.messageid) egenskapsvärdet fungerar som partitionsnyckel. (Microsoft .NET och AMQP bibliotek automatiskt tilldela en meddelande-ID om det sändande programmet inte.) I det här fallet hanteras alla kopior av samma meddelande av samma meddelandekö. Detta ID kan Service Bus att identifiera och eliminera dubbletter av meddelanden. Om den [RequiresDuplicateDetection](/dotnet/api/microsoft.azure.management.servicebus.models.sbqueue.requiresduplicatedetection) egenskapen inte är inställt på **SANT**, Service Bus beaktar inte den [MessageId](/dotnet/api/microsoft.azure.servicebus.message.messageid) egenskapen som en partitionsnyckel.

### <a name="not-using-a-partition-key"></a>Inte använder en partitionsnyckel

Om en partitionsnyckel distribuerar Service Bus meddelanden i en resursallokering till alla delar av partitionerad kö eller ämne. Om det valda fragmentet inte är tillgänglig, tilldelar Service Bus meddelandet till en annan fragment. På så sätt kan skicka åtgärden lyckas trots meddelandelagring är tillfälligt otillgänglig. Du kommer dock inte uppnå garanterad sorteringen som tillhandahåller en partitionsnyckel.

En mer detaljerad beskrivning av förhållandet mellan tillgänglighet (inga partitionsnyckel) och konsekvens (med en partitionsnyckel) finns i [i den här artikeln](../event-hubs/event-hubs-availability-and-consistency.md). Den här informationen gäller lika partitionerade Service Bus-enheter.

För att ge Service Bus finns tillräckligt med tid för att sätta meddelandet i en annan fragment i [OperationTimeout](/dotnet/api/microsoft.azure.servicebus.queueclient.operationtimeout) värdet som angetts för den klient som skickar meddelandet måste vara större än 15 sekunder. Vi rekommenderar att du ställer in den [OperationTimeout](/dotnet/api/microsoft.azure.servicebus.queueclient.operationtimeout) egenskapen till standardvärdet på 60 sekunder.

En partitionsnyckel ”PIN-koder”, ett meddelande till en specifik fragment. Om meddelandearkivet som innehåller det här fragmentet som är tillgänglig, returneras ett fel i Service Bus. Om en partitionsnyckel, Service Bus kan välja ett annat fragment och åtgärden lyckas. Vi rekommenderar därför att du inte anger en partitionsnyckel om det inte krävs.

## <a name="advanced-topics-use-transactions-with-partitioned-entities"></a>Avancerade ämnen: använda transaktioner med partitionerade enheter

Meddelanden som skickas som del i en transaktion måste ange en partitionsnyckel. Nyckeln kan vara något av följande egenskaper: [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid), [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey), eller [MessageId](/dotnet/api/microsoft.azure.servicebus.message.messageid). Alla meddelanden som skickas som en del av samma transaktion måste ange samma partitionsnyckel. Om du försöker skicka ett meddelande utan någon partitionsnyckel inom en transaktion, returnerar ett ogiltigt åtgärdsundantag i Service Bus. Om du försöker skicka flera meddelanden inom samma transaktion som har olika partitionsnycklar, returnerar ett ogiltigt åtgärdsundantag i Service Bus. Exempel:

```csharp
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    Message msg = new Message("This is a message");
    msg.PartitionKey = "myPartitionKey";
    messageSender.SendAsync(msg); 
    ts.CompleteAsync();
}
committableTransaction.Commit();
```

Om någon av de egenskaper som fungerar som en partitionsnyckel ställs in, låser Service Bus meddelandet till en specifik fragment. Detta beror på om huruvida en transaktion används. Vi rekommenderar att du inte anger en partitionsnyckel om det inte är nödvändigt.

## <a name="using-sessions-with-partitioned-entities"></a>Med sessioner med partitionerade enheter

Om du vill skicka ett transaktionsmeddelande till en session-medvetna ämne eller en kö, meddelandet måste ha den [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid) egenskapsuppsättning. Om den [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) egenskap har angetts, det måste vara identisk med den [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid) egenskapen. Om de skiljer sig returnerar ett ogiltigt åtgärdsundantag i Service Bus.

Till skillnad från vanlig (icke-partitionerad) köer eller ämnen går det inte att använda en enda transaktion för att skicka flera meddelanden till olika sessioner. Om du försöker returnerar ett ogiltigt åtgärdsundantag i Service Bus. Exempel:

```csharp
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    Message msg = new Message("This is a message");
    msg.SessionId = "mySession";
    messageSender.SendAsync(msg); 
    ts.CompleteAsync();
}
committableTransaction.Commit();
```

## <a name="automatic-message-forwarding-with-partitioned-entities"></a>Automatisk vidarebefordring med partitionerade enheter

Service Bus stöder automatisk meddelande vidarebefordran från, till eller mellan partitionerade enheter. Om du vill aktivera automatisk vidarebefordring, ange den [QueueDescription.ForwardTo] [ QueueDescription.ForwardTo] egenskapen källkön eller prenumeration. Om meddelandet anger en partitionsnyckel ([SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid), [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey), eller [MessageId](/dotnet/api/microsoft.azure.servicebus.message.messageid)), som partitionsnyckel används för målentitet.

## <a name="considerations-and-guidelines"></a>Överväganden och riktlinjer
* **Hög konsekvens funktioner**: om en entitet använder funktioner som sessioner, identifiering av dubbletter eller explicit kontroll över Partitioneringsnyckel kommer de asynkrona åtgärderna alltid dirigeras till specifika fragment. Om någon av fragment har hög belastning eller det underliggande arkivet är i feltillstånd, dessa åtgärder misslyckas och tillgänglighet minskar. Övergripande är konsekvens fortfarande mycket högre än partitionerade enheter. endast en delmängd av trafiken upplever problem, till skillnad från all trafik. Mer information finns i den här [beskrivning av tillgänglighet och konsekvens](../event-hubs/event-hubs-availability-and-consistency.md).
* **Hantering av**: åtgärder som att skapa, uppdatera och ta bort måste utföras på alla fragment för entiteten. Om alla fragment är i feltillstånd, kan det resultera i fel för dessa åtgärder. För Get-åtgärden måste information som meddelandet som aggregeras från alla fragment. Om alla fragment är i feltillstånd, har Tillgänglighetsstatus för entiteten rapporterats som begränsad.
* **Låg volym meddelande scenarier**: sådana scenarier, särskilt när de använder HTTP-protokollet, kan du behöva utföra flera ta emot för att hämta alla meddelanden. Ta emot förfrågningar klientdelen utför mottagning på alla fragment och cachelagrar alla svar som tagits emot. En efterföljande receive-begäran på samma anslutning kan dra nytta av den här cachelagring och ta emot svarstiderna blir lägre. Men om du har flera anslutningar eller använda HTTP upprättar som en ny anslutning för varje begäran. Det finns därför ingen garanti för att den skulle hamna på samma nod. Om alla befintliga meddelanden är låst och cachelagras i en annan klientdel, Mottagningsåtgärden returnerar **null**. Meddelanden förfaller så småningom och du kan få dem igen. HTTP keep-alive rekommenderas.
* **Bläddra/Peek meddelanden**: endast tillgängliga i den äldre [WindowsAzure.ServiceBus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) biblioteket. [PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatch) inte alltid returnerar antalet meddelanden som anges i den [MessageCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription.messagecount) egenskapen. Det finns två vanliga orsaker till problemet. En orsak är att mängden meddelanden aggregerade storlek överskrider den maximala storleken på 256 KB. En annan orsak är att om kön eller ämnet har den [EnablePartitioning egenskapen](/dotnet/api/microsoft.servicebus.messaging.queuedescription.enablepartitioning) inställd **SANT**, en partition kanske inte har tillräckligt med meddelanden för att slutföra det begärda antalet meddelanden. I allmänhet om ett program vill få ett visst antal meddelanden, det bör anropa [PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatch) upprepade gånger tills det blir det antalet meddelanden eller så finns det inga fler meddelanden att granska. Mer information, inklusive exempel, finns det [QueueClient.PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatch) eller [SubscriptionClient.PeekBatch](/dotnet/api/microsoft.servicebus.messaging.subscriptionclient.peekbatch) API-dokumentationen.

## <a name="latest-added-features"></a>Senaste tillagda funktioner

* Lägg till eller ta bort regeln stöds nu med partitionerade enheter. Skiljer sig från icke-partitionerad entiteter dessa åtgärder stöds inte under transaktioner. 
* AMQP har nu stöd för skicka och ta emot meddelanden till och från en partitionerad entitet.
* AMQP har nu stöd för följande åtgärder: [Batch skicka](/dotnet/api/microsoft.servicebus.messaging.queueclient.sendbatch), [Batch ta emot](/dotnet/api/microsoft.servicebus.messaging.queueclient.receivebatch), [Receive av ordningsnummer](/dotnet/api/microsoft.servicebus.messaging.queueclient.receive), [granska](/dotnet/api/microsoft.servicebus.messaging.queueclient.peek), [ Förnya låset](/dotnet/api/microsoft.servicebus.messaging.queueclient.renewmessagelock), [schemalägga meddelande](/dotnet/api/microsoft.azure.servicebus.queueclient.schedulemessageasync), [avbryta schemalagda meddelande](/dotnet/api/microsoft.azure.servicebus.queueclient.cancelscheduledmessageasync), [Lägg till regel](/dotnet/api/microsoft.azure.servicebus.ruledescription), [ta bort regeln](/dotnet/api/microsoft.azure.servicebus.ruledescription), [Förnya låset för sessionen](/dotnet/api/microsoft.servicebus.messaging.messagesession.renewlock), [Set sessionstillstånd](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate), [Get sessionstillstånd](/dotnet/api/microsoft.servicebus.messaging.messagesession.getstate), och [räkna upp sessioner](/dotnet/api/microsoft.servicebus.messaging.queueclient.getmessagesessions).

## <a name="partitioned-entities-limitations"></a>Partitionerade enheter begränsningar

Service Bus tillämpar för närvarande följande begränsningar på partitionerade köer och ämnen:

* Partitionerade köer och ämnen stöds inte på nivån Premium och meddelanden. Sessioner stöds på den högsta nivån med hjälp av sessions-ID. 
* Partitionerade köer och ämnen stöder inte skicka meddelanden som hör till olika sessioner i en enda transaktion.
* Service Bus tillåter för närvarande upp till 100 partitionerade köer och ämnen per namnområde. Varje partitionerad kö eller ämne räknas mot kvoten på 10 000 enheter per namnområde (inte gäller för Premium-nivån).

## <a name="next-steps"></a>Nästa steg

Läs mer om grundläggande begrepp för den AMQP 1.0 messaging specifikationen i den [AMQP 1.0-protokollguide](service-bus-amqp-protocol-guide.md).

[Azure portal]: https://portal.azure.com
[QueueDescription.EnablePartitioning]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.enablepartitioning
[TopicDescription.EnablePartitioning]: /dotnet/api/microsoft.servicebus.messaging.topicdescription.enablepartitioning
[QueueDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.forwardto
[AMQP 1.0 support for Service Bus partitioned queues and topics]: service-bus-partitioned-queues-and-topics-amqp-overview.md
