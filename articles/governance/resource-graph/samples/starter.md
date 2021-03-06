---
title: Startfrågor för Azure Resource Graph
description: Använd Azure Resource Graph för att köra vissa startfrågor.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: quickstart
ms.service: resource-graph
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: ba3df8f0f7fa0443e64972647b6f146f756e62d6
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/23/2018
ms.locfileid: "49646636"
---
# <a name="starter-resource-graph-queries"></a>Startfrågor för Azure Resource Graph

Det första steget mot att förstå frågor med Azure Resource Graph är en grundläggande förståelse för [frågespråket](../concepts/query-language.md). Om du inte redan är bekant med [Azure Data Explorer](../../../data-explorer/data-explorer-overview.md) rekommenderar vi att du går igenom grunderna så att du förstår hur man skapar begäranden för de resurser som du letar efter.

Vi går igenom följande startfrågor:

> [!div class="checklist"]
> - [Antal Azure-resurser](#count-resources)
> - [Lista över resurser sorterade efter namn](#list-resources)
> - [Visning av alla virtuella datorer sorterade efter namn i fallande ordning](#show-vms)
> - [Visning av de första fem virtuella datorerna efter namn och OS-typ](#show-sorted)
> - [Antal virtuella datorer efter OS-typ](#count-os)
> - [Visning av resurser med lagring](#show-storage)
> - [Lista över alla offentliga IP-adresser](#list-publicip)
> - [Antal resurser som har IP-adresser konfigurerade efter prenumeration](#count-resources-by-ip)
> - [Lista över resurser med ett specifikt tagg-värde](#list-tag)
> - [Lista över alla lagringskonton med ett specifikt taggvärde](#list-specific-tag)

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free) innan du börjar.

## <a name="language-support"></a>Stöd för språk

Azure CLI (via ett tillägg) och Azure PowerShell (via en modul) har stöd för Azure Resource Graph. Kontrollera att din miljö är redo innan du kör någon av nedanstående frågor. Se [Azure CLI](../first-query-azurecli.md#add-the-resource-graph-extension) och [Azure PowerShell](../first-query-powershell.md#add-the-resource-graph-module) för anvisningar om hur du installerar och validerar din valda gränssnittsmiljö.

## <a name="count-resources"></a>Antal Azure-resurser

Den här frågan returnerar antalet Azure-resurser som finns i de prenumerationer som du har åtkomst till. Det är också en bra fråga för att verifiera att ditt gränssnittval har lämpliga Azure Resource Graph-komponenter installerade och fungerar korrekt.

```Query
summarize count()
```

```azurecli-interactive
az graph query -q "summarize count()"
```

```azurepowershell-interactive
Search-AzureRmGraph -Query "summarize count()"
```

## <a name="list-resources"></a>Lista över resurser sorterade efter namn

Utan begränsning till någon typ av resurs eller specifika matchningsegenskaper, returnerar den här frågan bara **namn**, **typ** och **plats** för Azure-resurserna men använder `order by` för att sortera dem efter **namn**-egenskapen i stigande (`asc`) ordning.

```Query
project name, type, location
| order by name asc
```

```azurecli-interactive
az graph query -q "project name, type, location | order by name asc"
```

```azurepowershell-interactive
Search-AzureRmGraph -Query "project name, type, location | order by name asc"
```

## <a name="show-vms"></a>Visning av alla virtuella datorer sorterade efter namn i fallande ordning

Istället för att få alla Azure-resurser, om vi bara vill ha en lista över virtuella datorer (som är av typ `Microsoft.Compute/virtualMachines`), kan vi matcha egenskapen **typ** i resultatet.
Som för den föregående frågan, ändrar `desc` `order by` till att vara fallande. Tecknet `=~` i typmatchningen talar om för Resource Graph att Resource Graph ska vara skiftlägesokänsligt.

```Query
project name, location, type
| where type =~ 'Microsoft.Compute/virtualMachines'
| order by name desc
```

```azurecli-interactive
az graph query -q "project name, location, type| where type =~ 'Microsoft.Compute/virtualMachines' | order by name desc"
```

```azurepowershell-interactive
Search-AzureRmGraph -Query "project name, location, type| where type =~ 'Microsoft.Compute/virtualMachines' | order by name desc"
```

## <a name="show-sorted"></a>Visning av de första fem virtuella datorerna efter namn och OS-typ

Den här frågan använder `limit` för att bara hämta fem matchande poster som sorteras efter namn. Typen av Azure-resurs är `Microsoft.Compute/virtualMachines`. `project` talar om Azure Resource Graph vilka egenskaper som ska inkluderas.

```Query
where type =~ 'Microsoft.Compute/virtualMachines'
| project name, properties.storageProfile.osDisk.osType
| top 5 by name desc
```

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Compute/virtualMachines' | project name, properties.storageProfile.osDisk.osType | top 5 by name desc"
```

```azurepowershell-interactive
Search-AzureRmGraph -Query "where type =~ 'Microsoft.Compute/virtualMachines' | project name, properties.storageProfile.osDisk.osType | top 5 by name desc"
```

## <a name="count-os"></a>Antal virtuella datorer efter OS-typ

Vi bygger vidare på den föregående frågan och begränsar fortfarande efter Azure-resurstyp `Microsoft.Compute/virtualMachines` men inte längre efter antalet returnerade poster.
I stället använder vi `summarize` och `count()` för att definiera hur värdena ska grupperas och aggregeras efter egenskap, som i det här exemplet är `properties.storageProfile.osDisk.osType`. Ett exempel på hur denna sträng ser ut i det fullständiga objektet visas i [Utforska resurser – Identifiering av virtuell maskin](../concepts/explore-resources.md#virtual-machine-discovery).

```Query
where type =~ 'Microsoft.Compute/virtualMachines'
| summarize count() by tostring(properties.storageProfile.osDisk.osType)
```

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Compute/virtualMachines' | summarize count() by tostring(properties.storageProfile.osDisk.osType)"
```

```azurepowershell-interactive
Search-AzureRmGraph -Query "where type =~ 'Microsoft.Compute/virtualMachines' | summarize count() by tostring(properties.storageProfile.osDisk.osType)"
```

Ett annat sätt att skriva samma fråga är att `extend` en egenskap och ge den ett tillfälligt namn för användning i frågan, i det här fallet **os**. **os** används då av `summarize` och `count()` som i föregående exempel.

```Query
where type =~ 'Microsoft.Compute/virtualMachines'
| extend os = properties.storageProfile.osDisk.osType
| summarize count() by tostring(os)
```

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Compute/virtualMachines' | extend os = properties.storageProfile.osDisk.osType | summarize count() by tostring(os)"
```

```azurepowershell-interactive
Search-AzureRmGraph -Query "where type =~ 'Microsoft.Compute/virtualMachines' | extend os = properties.storageProfile.osDisk.osType | summarize count() by tostring(os)"
```

> [!NOTE]
> Tänk på att medan `=~` tillåter skiftlägesokänslig matchning, kräver användning av egenskaper (som **properties.storageProfile.osDisk.osType**) i frågan att skiftläget är korrekt. Om egenskapen är i fel skiftläge kan den fortfarande returnera ett värde men grupperingen eller sammanfattningen blir felaktig.

## <a name="show-storage"></a>Visning av resurser med lagring

I stället för att explicit definiera den typ som ska matchas, hittar den här exempelfrågan alla Azure-resurser som `contains` ordet **storage** (lagring).

```Query
where type contains 'storage' | distinct type
```

```azurecli-interactive
az graph query -q "where type contains 'storage' | distinct type"
```

```azurepowershell-interactive
Search-AzureRmGraph -Query "where type contains 'storage' | distinct type"
```

## <a name="list-publicip"></a>Lista över alla offentliga IP-adresser

Hittar på ett liknande sätt som för den föregående frågan allt som har en typ som innehåller ordet **publicIPAddresses**. Den här frågan expanderar mönstret för att utesluta resultat där **properties.ipAddress** är null, för att endast returnera **properties.ipAddress** och för att `limit` resultaten till de 100 främsta. Du kan behöva hoppa över citattecknen beroende på valt gränssnitt.

```Query
where type contains 'publicIPAddresses' and properties.ipAddress != ''
| project properties.ipAddress
| limit 100
```

```azurecli-interactive
az graph query -q "where type contains 'publicIPAddresses' and properties.ipAddress != '' | project properties.ipAddress | limit 100"
```

```azurepowershell-interactive
Search-AzureRmGraph -Query "where type contains 'publicIPAddresses' and properties.ipAddress != '' | project properties.ipAddress | limit 100"
```

## <a name="count-resources-by-ip"></a>Antal resurser som har IP-adresser konfigurerade efter prenumeration

Om vi använder den föregående exempelfrågan och lägger till `summarize` och `count()`, kan vi få en lista efter prenumeration på resurser med konfigurerade IP-adresser.

```Query
where type contains 'publicIPAddresses' and properties.ipAddress != ''
| summarize count () by subscriptionId
```

```azurecli-interactive
az graph query -q "where type contains 'publicIPAddresses' and properties.ipAddress != '' | summarize count () by subscriptionId"
```

```azurepowershell-interactive
Search-AzureRmGraph -Query "where type contains 'publicIPAddresses' and properties.ipAddress != '' | summarize count () by subscriptionId"
```

## <a name="list-tag"></a>Lista över resurser med ett specifikt taggvärde

Vi kan begränsa resultaten med andra egenskaper än Azure-resurstyp, till exempel en tagg. I det här exemplet filtrerar vi för Azure-resurser med ett taggnamn innehållande **environment** som har värdet **internal**.

```Query
where tags.environment=~'internal'
| project name
```

```azurecli-interactive
az graph query -q "where tags.environment=~'internal' | project name"
```

```azurepowershell-interactive
Search-AzureRmGraph -Query "where tags.environment=~'internal' | project name"
```

Om den även finns krav på att ange vilka taggar som resursen har och deras värden, kan det här exemplet utökas genom att lägga till egenskapen **tags** till nyckelordet för `project`.

```Query
where tags.environment=~'internal'
| project name, tags
```

```azurecli-interactive
az graph query -q "where tags.environment=~'internal' | project name, tags"
```

```azurepowershell-interactive
Search-AzureRmGraph -Query "where tags.environment=~'internal' | project name, tags"
```

## <a name="list-specific-tag"></a>Lista över alla lagringskonton med specifikt taggvärde

Genom att kombinera det föregående exemplets filterkapacitet med filtrering efter Azure-resurstyp via egenskapen **type**, kan vi begränsa vår sökning till specifika typer av Azure-resurser med ett specifikt taggnamn och värde.

```Query
where type =~ 'Microsoft.Storage/storageAccounts'
| where tags['tag with a space']=='Custom value'
```

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Storage/storageAccounts' | where tags['tag with a space']=='Custom value'"
```

```azurepowershell-interactive
Search-AzureRmGraph -Query "where type =~ 'Microsoft.Storage/storageAccounts' | where tags['tag with a space']=='Custom value'"
```

> [!NOTE]
> I det här exemplet används `==` för matchning istället för villkorliga `=~`. `==` är en skiftlägeskänslig matchning.

## <a name="next-steps"></a>Nästa steg

- Läs mer om [frågespråket](../concepts/query-language.md)
- Lär dig att [utforska resurser](../concepts/explore-resources.md)
- Se exempel på [avancerade frågor](advanced.md)