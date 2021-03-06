---
title: Övervaka programmässigt ett Azure data factory | Microsoft Docs
description: Lär dig hur du övervakar en pipeline i en datafabrik med hjälp av olika software development Kit (SDK).
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/16/2018
ms.author: douglasl
ms.openlocfilehash: 343af57cc8f3e63965dc1fe1827b2945009ea8bf
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37045790"
---
# <a name="programmatically-monitor-an-azure-data-factory"></a>Övervaka ett Azure data factory programmässigt
Den här artikeln beskriver hur du övervakar en pipeline i en datafabrik med hjälp av olika software development Kit (SDK). 

## <a name="data-range"></a>Dataområdet

Data Factory lagrar bara pipeline kör data för 45 dagar. När du frågar programmässigt efter data om Data Factory-pipelinen körs - exempelvis med PowerShell-kommando `Get-AzureRmDataFactoryV2PipelineRun` -det finns inga maximala datum för den valfria `LastUpdatedAfter` och `LastUpdatedBefore` parametrar. Men om frågan för data för det senaste året, till exempel frågan returnerar inte ett fel, men returnerar bara pipeline kör data från de senaste 45 dagarna.

Om du vill bevara pipeline kör data mer än 45 dagar, ställa in din egen diagnostikloggning med [Azure-Monitor](monitor-using-azure-monitor.md).

## <a name="net"></a>.NET
En fullständig genomgång av hur du skapar och övervakning av en pipeline med .NET SDK finns [skapa en datafabrik och pipeline med hjälp av .NET](quickstart-create-data-factory-dot-net.md).

1. Lägg till följande kod för att alltid kontrollera status för pipeline köras förrän den är klar kopiering av data.

    ```csharp
    // Monitor the pipeline run
    Console.WriteLine("Checking pipeline run status...");
    PipelineRun pipelineRun;
    while (true)
    {
        pipelineRun = client.PipelineRuns.Get(resourceGroup, dataFactoryName, runResponse.RunId);
        Console.WriteLine("Status: " + pipelineRun.Status);
        if (pipelineRun.Status == "InProgress")
            System.Threading.Thread.Sleep(15000);
        else
            break;
    }
    ```

2. Lägg till följande kod som hämtar kopieringsaktiviteten kör information, till exempel storleken på data lästs/skrivits.

    ```csharp
    // Check the copy activity run details
    Console.WriteLine("Checking copy activity run details...");
   
    List<ActivityRun> activityRuns = client.ActivityRuns.ListByPipelineRun(
    resourceGroup, dataFactoryName, runResponse.RunId, DateTime.UtcNow.AddMinutes(-10), DateTime.UtcNow.AddMinutes(10)).ToList(); 
    if (pipelineRun.Status == "Succeeded")
        Console.WriteLine(activityRuns.First().Output);
    else
        Console.WriteLine(activityRuns.First().Error);
    Console.WriteLine("\nPress any key to exit...");
    Console.ReadKey();
    ```

Fullständig dokumentation för .NET SDK finns [Data Factory .NET SDK referens](/dotnet/api/microsoft.azure.management.datafactory?view=azure-dotnet).

## <a name="python"></a>Python
En fullständig genomgång av hur du skapar och övervakning av en pipeline med Python SDK finns [skapa en datafabrik och pipeline använder Python](quickstart-create-data-factory-python.md).

Lägg till följande kod för att övervaka pipeline kör:

```python
#Monitor the pipeline run
time.sleep(30)
pipeline_run = adf_client.pipeline_runs.get(rg_name, df_name, run_response.run_id)
print("\n\tPipeline run status: {}".format(pipeline_run.status))
activity_runs_paged = list(adf_client.activity_runs.list_by_pipeline_run(rg_name, df_name, pipeline_run.run_id, datetime.now() - timedelta(1),  datetime.now() + timedelta(1)))
print_activity_run_details(activity_runs_paged[0])
```

Fullständig dokumentation om Python SDK finns [Data Factory Python SDK referens](/python/api/overview/azure/datafactory?view=azure-python).

## <a name="rest-api"></a>REST-API
En fullständig genomgång av hur du skapar och övervakning av en pipeline med hjälp av REST-API finns [skapa en datafabrik och pipeline med hjälp av REST API](quickstart-create-data-factory-rest-api.md).
 
1. Kör följande skript för att kontinuerligt kontrollera pipelinekörningens status tills kopieringen av data är klar.

    ```powershell
    $request = "https://management.azure.com/subscriptions/${subsId}/resourceGroups/${resourceGroup}/providers/Microsoft.DataFactory/factories/${dataFactoryName}/pipelineruns/${runId}?api-version=${apiVersion}"
    while ($True) {
        $response = Invoke-RestMethod -Method GET -Uri $request -Header $authHeader
        Write-Host  "Pipeline run status: " $response.Status -foregroundcolor "Yellow"

        if ($response.Status -eq "InProgress") {
            Start-Sleep -Seconds 15
        }
        else {
            $response | ConvertTo-Json
            break
        }
    }
    ```
2. Kör följande skript för att hämta körningsinformation för kopieringsaktiviteten, till exempel storleken på data som lästs/skrivits.

    ```PowerShell
    $request = "https://management.azure.com/subscriptions/${subsId}/resourceGroups/${resourceGroup}/providers/Microsoft.DataFactory/factories/${dataFactoryName}/pipelineruns/${runId}/activityruns?api-version=${apiVersion}&startTime="+(Get-Date).ToString('yyyy-MM-dd')+"&endTime="+(Get-Date).AddDays(1).ToString('yyyy-MM-dd')+"&pipelineName=Adfv2QuickStartPipeline"
    $response = Invoke-RestMethod -Method GET -Uri $request -Header $authHeader
    $response | ConvertTo-Json
    ```

Fullständig dokumentation för REST-API finns [Data Factory REST API-referensen](/rest/api/datafactory/).

## <a name="powershell"></a>PowerShell
En fullständig genomgång av hur du skapar och övervakning av en pipeline som använder PowerShell Se [skapa en datafabrik och pipeline med hjälp av PowerShell](quickstart-create-data-factory-powershell.md).

1. Kör följande skript för att kontinuerligt kontrollera pipelinekörningens status tills kopieringen av data är klar.

    ```powershell
    while ($True) {
        $run = Get-AzureRmDataFactoryV2PipelineRun -ResourceGroupName $resourceGroupName -DataFactoryName $DataFactoryName -PipelineRunId $runId

        if ($run) {
            if ($run.Status -ne 'InProgress') {
                Write-Host "Pipeline run finished. The status is: " $run.Status -foregroundcolor "Yellow"
                $run
                break
            }
            Write-Host  "Pipeline is running...status: InProgress" -foregroundcolor "Yellow"
        }

        Start-Sleep -Seconds 30
    }
    ```
2. Kör följande skript för att hämta körningsinformation för kopieringsaktiviteten, till exempel storleken på data som lästs/skrivits.

    ```powershell
    Write-Host "Activity run details:" -foregroundcolor "Yellow"
    $result = Get-AzureRmDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $runId -RunStartedAfter (Get-Date).AddMinutes(-30) -RunStartedBefore (Get-Date).AddMinutes(30)
    $result
    
    Write-Host "Activity 'Output' section:" -foregroundcolor "Yellow"
    $result.Output -join "`r`n"
    
    Write-Host "\nActivity 'Error' section:" -foregroundcolor "Yellow"
    $result.Error -join "`r`n"
    ```

Fullständig dokumentation för PowerShell-cmdlets finns [Data Factory PowerShell cmdlet-referens](/powershell/module/azurerm.datafactoryv2/?view=azurermps-4.4.1).

## <a name="next-steps"></a>Nästa steg
Se [övervakaren rörledningar med hjälp av Azure-Monitor](monitor-using-azure-monitor.md) artikeln innehåller information om hur du använder Azure-Monitor för att övervaka pipelines som Data Factory. 

