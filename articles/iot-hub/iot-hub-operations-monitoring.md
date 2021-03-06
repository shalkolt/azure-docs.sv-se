---
title: Azure IoT Hub åtgärdsövervakning | Microsoft Docs
description: Hur du använder Azure IoT Hub åtgärdsövervakning om du vill övervaka statusen för åtgärder på IoT hub i realtid.
author: nberdy
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 10/10/2017
ms.author: nberdy
ms.openlocfilehash: 3aa452cd178bd0d064726c5be7dbdf65c6ef8d92
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44160056"
---
# <a name="iot-hub-operations-monitoring"></a>Övervakning av IoT Hub-åtgärder

IoT Hub med åtgärdsövervakning kan du övervaka statusen för åtgärder på IoT hub i realtid. IoT Hub spårar händelser över flera åtgärdskategorier. Du kan välja skicka händelser från en eller flera kategorier till en slutpunkt för IoT-hubben för bearbetning. Du kan övervaka data för fel eller skapa mer komplexa bearbetning baserad på datamönster.

>[!NOTE]
>IoT Hub åtgärdsövervakning är inaktuell och kommer att tas bort från IoT Hub den 10 mars 2019. För att övervaka åtgärder och hälsotillstånd för IoT Hub, se [övervaka hälsotillståndet för Azure IoT Hub och diagnostisera problem snabbt][lnk-monitor]. Läs mer om utfasning tidslinjen [övervaka dina Azure-IoT-lösningar med Azure Monitor och Azure Resource Health][lnk-blog-announcement].

IoT Hub övervakar sex typer av händelser:

* Identitet åtgärder
* Enhetstelemetri
* Meddelanden från moln till enhet
* Anslutningar
* Filöverföringar
* Meddelanderedigering

> [!IMPORTANT]
> IoT Hub åtgärdsövervakning garanterar inte tillförlitligt och sorterad leverans av händelser. Beroende på IoT Hub underliggande infrastruktur, kan vissa händelser tappas bort eller levereras i ordning. Använd åtgärdsövervakning för att generera aviseringar baserat på signaler fel, till exempel misslyckade anslutningsförsök eller hög frekvens frånkopplingar för specifika enheter. Du bör inte förlita dig på åtgärdsövervakning händelser för att skapa en konsekvent lagring för enhetens tillstånd, t.ex. en butik spåra ansluten eller frånkopplad tillståndet för en enhet. 

## <a name="how-to-enable-operations-monitoring"></a>Så här aktiverar du åtgärdsövervakning

1. Skapa en IoT-hubb. Du hittar anvisningar om hur du skapar en IoT hub i den [börjar] [ lnk-get-started] guide.

1. Öppna bladet för din IoT hub. Därifrån klickar du på **Åtgärdsövervakning**.

    ![Åtgärder för dataåtkomst övervakning konfigurationen på portalen][1]

1. Välj de övervakning kategorier som du vill övervaka och klicka sedan på **spara**. Händelser är tillgängliga för läsning från den Event Hub-kompatibla slutpunkten som anges i **övervakningsinställningarna**. IoT Hub-slutpunkten kallas `messages/operationsmonitoringevents`.

    ![Konfigurera åtgärdsövervakning på IoT hub][2]

> [!NOTE]
> Att välja **utförlig** övervakning för den **anslutningar** kategorin gör IoT Hub för att generera ytterligare meddelanden. För alla kategorier, den **utförlig** inställningsändringar mängden information IoT Hub innehåller i varje felmeddelande.

## <a name="event-categories-and-how-to-use-them"></a>Händelsekategorier och hur du använder dem.

Varje åtgärdsövervakning kategorin spårar en annan typ av interaktion med IoT Hub, och varje övervakning kategori har ett schema som definierar hur händelser i den kategorin är strukturerade.

### <a name="device-identity-operations"></a>Identitet åtgärder

Enhetskategorin identitet operations spårar fel som uppstår vid försök att skapa, uppdatera eller ta bort en post i din IoT-hubbens identitetsregister. Spårning av den här kategorin är användbart för att etablera scenarier.

```json
{
    "time": "UTC timestamp",
    "operationName": "create",
    "category": "DeviceIdentityOperations",
    "level": "Error",
    "statusCode": 4XX,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID",
    "durationMs": 1234,
    "userAgent": "userAgent",
    "sharedAccessPolicy": "accessPolicy"
}
```

### <a name="device-telemetry"></a>Enhetstelemetri

Telemetri enhetskategorin spårar fel som inträffar på IoT-hubben och som är relaterade till pipelinen telemetri. Den här kategorin innehåller fel som uppstår när du skickar telemetrihändelser (t.ex begränsningar) och ta emot händelser (till exempel obehöriga läsare). Den här kategorin identifierar inte fel som orsakats av kod som körs på själva enheten.

```json
{
    "messageSizeInBytes": 1234,
    "batching": 0,
    "protocol": "Amqp",
    "authType": "{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
    "time": "UTC timestamp",
    "operationName": "ingress",
    "category": "DeviceTelemetry",
    "level": "Error",
    "statusCode": 4XX,
    "statusType": 4XX001,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID",
    "EventProcessedUtcTime": "UTC timestamp",
    "PartitionId": 1,
    "EventEnqueuedUtcTime": "UTC timestamp"
}
```

### <a name="cloud-to-device-commands"></a>Moln till enhet-kommandon

Moln till enhet kommandon kategorin spårar fel som inträffar på IoT-hubben och som är relaterade till moln till enhet meddelandepipeline. Den här kategorin innehåller fel som uppstår när du skickar meddelanden från molnet till enheten (till exempel obehöriga avsändare), ta emot meddelanden från molnet till enheten (till exempel leveransantalet har överskridits) och mottagande moln till enhet-meddelande (t ex feedback har gått ut). Den här kategorin fånga inte fel från en enhet som hanterar ett moln-till-enhet-meddelande felaktigt om moln till enhet meddelandet levererades har.

```json
{
    "messageSizeInBytes": 1234,
    "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
    "deliveryAcknowledgement": 0,
    "protocol": "Amqp",
    "time": " UTC timestamp",
    "operationName": "ingress",
    "category": "C2DCommands",
    "level": "Error",
    "statusCode": 4XX,
    "statusType": 4XX001,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID",
    "EventProcessedUtcTime": "UTC timestamp",
    "PartitionId": 1,
    "EventEnqueuedUtcTime": "UTC timestamp"
}
```

### <a name="connections"></a>Anslutningar

Anslutningar kategorin spårar fel som uppstår när enheter ansluta eller koppla från en IoT-hubb. Det är användbart för att identifiera obehöriga anslutningsförsök och för att spåra när anslutningen kopplas från för enheter i områden med dålig anslutning att spåra den här kategorin.

```json
{
    "durationMs": 1234,
    "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
    "protocol": "Amqp",
    "time": " UTC timestamp",
    "operationName": "deviceConnect",
    "category": "Connections",
    "level": "Error",
    "statusCode": 4XX,
    "statusType": 4XX001,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID"
}
```

### <a name="file-uploads"></a>Filöverföringar

Filen uppladdning kategorin spårar fel som inträffar på IoT-hubben och som är relaterade till filuppladdning. Den här kategorin omfattar:

* Fel som inträffar med SAS-URI, t.ex när den upphör att gälla innan en enhet meddelar hubb för en överförda.
* Det gick inte överföringar som rapporteras av enheten.
* Fel som uppstår när en fil inte hittas i storage när IoT Hub-meddelande meddelande skapas.

Den här kategorin identifierar inte fel som sker under tiden enheten laddar upp en fil till lagring.

```json
{
    "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
    "protocol": "HTTP",
    "time": " UTC timestamp",
    "operationName": "ingress",
    "category": "fileUpload",
    "level": "Error",
    "statusCode": 4XX,
    "statusType": 4XX001,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID",
    "blobUri": "http//bloburi.com",
    "durationMs": 1234
}
```

### <a name="message-routing"></a>Meddelanderedigering

Meddelandet routning kategorin spårar fel som inträffar när meddelandet vägen utvärdering och slutpunktshälsa anser vara av IoT Hub. Den här kategorin innehåller händelser, som när en regel som utvärderas till ”odefinierad”, när IoT Hub markerar en slutpunkt som dead och eventuella andra fel togs emot från en slutpunkt. Den här kategorin omfattar inte specifika fel om själva meddelandena (till exempel enhet begränsningsfel) som har rapporterats under kategorin ”enhetstelemetri”.

```json
{
    "messageSizeInBytes": 1234,
    "time": "UTC timestamp",
    "operationName": "ingress",
    "category": "routes",
    "level": "Error",
    "deviceId": "device-ID",
    "messageId": "ID of message",
    "routeName": "myroute",
    "endpointName": "myendpoint",
    "details": "ExternalEndpointDisabled"
}
```

## <a name="connect-to-the-monitoring-endpoint"></a>Ansluta till slutpunkten för övervakning

Övervakningsslutpunkt på IoT hub är en Event Hub-kompatibla slutpunkt. Du kan använda valfri metod som fungerar med Event Hubs kan läsa meddelanden från övervakning från den här slutpunkten. Följande exempel skapar en grundläggande läsare som inte passar för distributioner med hög genomströmning. Mer information om hur du bearbetar meddelanden från Event Hubs finns i självstudiekursen [Komma igång med Event Hubs][lnk-eventhubs-tutorial].

För att ansluta till slutpunkten för övervakning, behöver du en anslutningssträng och namnet på slutpunkten. Följande steg visar hur du hittar de nödvändiga värdena i portalen:

1. Gå till din IoT Hub-resursbladet i portalen.

1. Välj **Åtgärdsövervakning**, och anteckna den **Event Hub-kompatibla namnet** och **Event Hub-kompatibla slutpunkten** värden:

    ![Event Hubs-kompatibla slutpunktsvärdena][img-endpoints]

1. Välj **principer för delad åtkomst**, välj sedan **service**. Anteckna den **primärnyckel** värde:

    ![Delade princip primär åtkomstnyckel][img-service-key]

I följande C#-kodexempel hämtas från en Visual Studio **Windows Classic Desktop** C#-konsolapp. Projektet har den **WindowsAzure.ServiceBus** NuGet-paketet installeras.

* Ersätt platshållaren för anslutningssträngen med en anslutningssträng som använder den **Event Hub-kompatibla slutpunkten** och tjänsten **primärnyckel** värden du antecknade tidigare som visas i följande exempel:

    ```cs
    "Endpoint={your Event Hub-compatible endpoint};SharedAccessKeyName=service;SharedAccessKey={your service primary key value}"
    ```

* Byt ut platshållaren namn övervakning slutpunkten för mot den **Event Hub-kompatibla namnet** värde som du antecknade tidigare.

```cs
class Program
{
    static string connectionString = "{your monitoring endpoint connection string}";
    static string monitoringEndpointName = "{your monitoring endpoint name}";
    static EventHubClient eventHubClient;

    static void Main(string[] args)
    {
        Console.WriteLine("Monitoring. Press Enter key to exit.\n");

        eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, monitoringEndpointName);
        var d2cPartitions = eventHubClient.GetRuntimeInformation().PartitionIds;
        CancellationTokenSource cts = new CancellationTokenSource();
        var tasks = new List<Task>();

        foreach (string partition in d2cPartitions)
        {
            tasks.Add(ReceiveMessagesFromDeviceAsync(partition, cts.Token));
        }

        Console.ReadLine();
        Console.WriteLine("Exiting...");
        cts.Cancel();
        Task.WaitAll(tasks.ToArray());
    }

    private static async Task ReceiveMessagesFromDeviceAsync(string partition, CancellationToken ct)
    {
        var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.UtcNow);
        while (true)
        {
            if (ct.IsCancellationRequested)
            {
                await eventHubReceiver.CloseAsync();
                break;
            }

            EventData eventData = await eventHubReceiver.ReceiveAsync(new TimeSpan(0,0,10));

            if (eventData != null)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                Console.WriteLine("Message received. Partition: {0} Data: '{1}'", partition, data);
            }
        }
    }
}
```

## <a name="next-steps"></a>Nästa steg
Om du vill fortsätta för att utforska funktionerna för IoT Hub, se:

* [Utvecklarhandboken för IoT Hub][lnk-devguide]
* [Distribuera AI till gränsenheter med Azure IoT Edge][lnk-iotedge]

<!-- Links and images -->
[1]: media/iot-hub-operations-monitoring/enable-OM-1.png
[2]: media/iot-hub-operations-monitoring/enable-OM-2.png
[img-endpoints]: media/iot-hub-operations-monitoring/monitoring-endpoint.png
[img-service-key]: media/iot-hub-operations-monitoring/service-key.png

[lnk-blog-announcement]: https://azure.microsoft.com/blog/monitor-your-azure-iot-solutions-with-azure-monitor-and-azure-resource-health
[lnk-monitor]: iot-hub-monitor-resource-health.md
[lnk-get-started]: quickstart-send-telemetry-dotnet.md
[lnk-diagnostic-metrics]: iot-hub-metrics.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
