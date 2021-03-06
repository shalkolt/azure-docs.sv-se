---
title: Använda Pig med Hadoop i HDInsight
description: Lär dig mer om att använda Pig med Hadoop i HDInsight.
services: hdinsight
author: jasonwhowell
ms.author: jasonh
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/23/2018
ms.openlocfilehash: e97763adbe7998ed93e3ba8b87d89ffe8d8de6aa
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/27/2018
ms.locfileid: "43045453"
---
# <a name="use-pig-with-hadoop-on-hdinsight"></a>Använda Pig med Hadoop i HDInsight

Lär dig hur du använder [Apache Pig](http://pig.apache.org/) med HDInsight.

Pig är en plattform för att skapa program för Hadoop med hjälp av en procedurmässig språk som kallas *Pig Latin*. Pig är ett alternativ till Java för att skapa *MapReduce* lösningar och ingår i Azure HDInsight. Använd följande tabell för att identifiera de olika sätt som Apache Pig-kan användas med HDInsight:

| **Använd den här** om du vill... | ...an **interaktiva** shell | ...**batch** bearbetning | ...med detta **klustret operativsystem** | ...from detta **klientoperativsystem** |
|:--- |:---:|:---:|:--- |:--- |
| [SSH](apache-hadoop-use-pig-ssh.md) |✔ |✔ |Linux |Linux, Unix, Mac OS X eller Windows |
| [REST API](apache-hadoop-use-pig-curl.md) |&nbsp; |✔ |Linux eller Windows |Linux, Unix, Mac OS X eller Windows |
| [.NET SDK för Hadoop](apache-hadoop-use-pig-dotnet-sdk.md) |&nbsp; |✔ |Linux eller Windows |Windows (för tillfället) |
| [Windows PowerShell](apache-hadoop-use-pig-powershell.md) |&nbsp; |✔ |Linux eller Windows |Windows |

> [!IMPORTANT]
> Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare. Mer information finns i [HDInsight-avveckling på Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a id="why"></a>Varför ska man använda Pig

En av utmaningarna med att bearbeta data med hjälp av MapReduce i Hadoop implementerar din standardbearbetningslogiken genom att använda endast en karta och en minska-funktion. För komplexa bearbetning du ofta behöva Bryt bearbetning i flera MapReduce-åtgärder som är sammankedjade tillsammans för att uppnå det önskade resultatet.

Pig kan du definiera bearbetning som en serie transformationer som data flödar genom för att skapa önskade utdata.

Pig Latin-språk kan du beskriva dataflödet från rådata indata via en eller flera omvandlingar till önskat resultat. Pig Latin program följer detta allmänna mönster:

* **Läs in**: läsa data hanteras från filsystemet

* **Omvandla**: ändra data

* **Dumpa eller lagra**: mata ut data till skärmen eller lagra den för bearbetning

### <a name="user-defined-functions"></a>Användardefinierade funktioner

Pig Latin även stöd för användardefinierade funktioner (UDF), där du kan anropa externa komponenter som implementerar logik som är svåra att modellen i Pig Latin.

Läs mer om Pig Latin [Pig Latin referens manuell 1](http://archive.cloudera.com/cdh/3/pig/piglatin_ref1.html) och [Pig Latin referens manuell 2](http://archive.cloudera.com/cdh/3/pig/piglatin_ref2.html).

Ett exempel på hur du använder UDF: er med Pig, finns i följande dokument:

* [Använda DataFu med Pig i HDInsight](apache-hadoop-use-pig-datafu-udf.md) -DataFu är en samling användbar UDF: er som underhålls av Apache
* [Använda Python med Pig och Hive i HDInsight](python-udf-hdinsight.md)
* [Använda C# med Hive och Pig i HDInsight](apache-hadoop-hive-pig-udf-dotnet-csharp.md)

## <a id="data"></a>Exempeldata

HDInsight innehåller olika exempel datauppsättningar, som lagras i den `/example/data` och `/HdiSamples` kataloger. Dessa kataloger finns i standardlagringen för klustret. Pig-exemplet i det här dokumentet används de *log4j* fil från `/example/data/sample.log`.

Varje logg i filen består av en rad med fält som innehåller en `[LOG LEVEL]` fält för att visa meddelandetyp och allvarlighetsgrad, till exempel:

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

I exemplet ovan är loggningsnivån fel.

> [!NOTE]
> Du kan också skapa en log4j-fil med hjälp av den [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) loggning verktyget och överför sedan filen till din blob. Se [ladda upp Data till HDInsight](../hdinsight-upload-data.md) anvisningar. Mer information om hur du använder blobar i Azure storage med HDInsight finns i [använda Azure Blob Storage med HDInsight](../hdinsight-hadoop-use-blob-storage.md).

## <a id="job"></a>Exempel-jobb

Följande Pig Latin-jobbet läser in den `sample.log` filen från standardlagringen för HDInsight-klustret. Sedan utförs en serie transformationer som resulterar i en uppräkning av hur många gånger varje Loggnivå inträffade i indata. Resultatet skrivs till STDOUT.

    LOGS = LOAD 'wasb:///example/data/sample.log';
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
    RESULT = order FREQUENCIES by COUNT desc;
    DUMP RESULT;

Följande bild visar en sammanfattning av vad varje transformering som gör att data.

![Grafisk representation av omvandlingarna][image-hdi-pig-data-transformation]

## <a id="run"></a>Köra Pig Latin-jobb

HDInsight kan köra Pig Latin-jobb med hjälp av olika metoder. Använd följande tabell för att bestämma vilken metod som passar dig och klicka sedan på länken för en genomgång.

| **Använd den här** om du vill... | ...an **interaktiva** shell | ...**batch** bearbetning | ...med detta **klustret operativsystem** | ...from detta **klienten** |
|:--- |:---:|:---:|:--- |:--- |
| [SSH](apache-hadoop-use-pig-ssh.md) |✔ |✔ |Linux |Linux, Unix, Mac OS X eller Windows |
| [CURL](apache-hadoop-use-pig-curl.md) |&nbsp; |✔ |Linux eller Windows |Linux, Unix, Mac OS X eller Windows |
| [.NET SDK för Hadoop](apache-hadoop-use-pig-dotnet-sdk.md) |&nbsp; |✔ |Linux eller Windows |Windows (för tillfället) |
| [Windows PowerShell](apache-hadoop-use-pig-powershell.md) |&nbsp; |✔ |Linux eller Windows |Windows |

> [!IMPORTANT]
> Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare. Mer information finns i [HDInsight-avveckling på Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="pig-and-sql-server-integration-services"></a>Pig- och SQL Server Integration Services

Du kan använda SQL Server Integration Services (SSIS) för att köra ett Pig-jobb. Azure Feature Pack för SSIS innehåller följande komponenter som fungerar med Pig-jobb på HDInsight.

* [Azure HDInsight Pig-aktivitet][pigtask]

* [Azure prenumeration Anslutningshanteraren][connectionmanager]

Läs mer om Azure Feature Pack för SSIS [här][ssispack].

## <a id="nextsteps"></a>Nästa steg
Nu när du har lärt dig hur du använder Pig med HDInsight kan använda följande länkar för att utforska andra sätt att arbeta med Azure HDInsight.

* [Ladda upp data till HDInsight](../hdinsight-upload-data.md)
* [Använda Hive med HDInsight][hdinsight-use-hive]
* [Använda Sqoop med HDInsight](hdinsight-use-sqoop.md)
* [Använda Oozie med HDInsight](../hdinsight-use-oozie.md)
* [Använda MapReduce-jobb med HDInsight][hdinsight-use-mapreduce]

[apachepig-home]: http://pig.apache.org/
[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html
[curl]: http://curl.haxx.se/
[pigtask]: http://msdn.microsoft.com/library/mt146781(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md

[hdinsight-use-hive]:../hdinsight-use-hive.md
[hdinsight-use-mapreduce]:../hdinsight-use-mapreduce.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md#mapreduce-sdk

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx


[image-hdi-pig-data-transformation]: ./media/hdinsight-use-pig/HDI.DataTransformation.gif
