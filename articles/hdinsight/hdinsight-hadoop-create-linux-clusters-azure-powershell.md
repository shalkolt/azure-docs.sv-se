---
title: Skapa Hadoop-kluster med hjälp av PowerShell - Azure HDInsight
description: Lär dig hur du skapar Hadoop, HBase, Storm eller Spark-kluster på Linux för HDInsight med hjälp av Azure PowerShell.
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: jasonh
ms.openlocfilehash: ed9c486dfa548e86e558c093113011cabfd3d63c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46997428"
---
# <a name="create-linux-based-clusters-in-hdinsight-using-azure-powershell"></a>Skapa Linux-baserade kluster i HDInsight med Azure PowerShell

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Azure PowerShell är en kraftfull skriptmiljö som du kan använda för att styra och automatisera distributionen och hanteringen av dina arbetsbelastningar i Microsoft Azure. Det här dokumentet innehåller information om hur du skapar ett Linux-baserade HDInsight-kluster med hjälp av Azure PowerShell. Den innehåller också ett exempelskript.

> [!NOTE]
> Azure PowerShell är endast tillgängligt på Windows-klienter. Om du använder en Linux-, Unix- eller Mac OS X-klient, se [skapar ett Linux-baserade HDInsight-kluster med klassiska Azure-CLI](hdinsight-hadoop-create-linux-clusters-azure-cli.md) för information om hur du använder den klassiska CLI för att skapa ett kluster.

## <a name="prerequisites"></a>Förutsättningar
Du måste ha följande innan du sätter igång:

* En Azure-prenumeration. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* [Azure PowerShell](/powershell/azure/install-azurerm-ps)

    > [!IMPORTANT]
    > Azure PowerShell-stöd för hantering av HDInsight-resurser med hjälp av Azure Service Manager **är föråldrat** och togs bort den 1 januari 2017. I stegen i det här dokumentet används de nya HDInsight-cmdletarna som fungerar med Azure Resource Manager.
    >
    > Följ stegen i [installera Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) att installera den senaste versionen av Azure PowerShell. Om du har skript som behöver ändras för att använda de nya cmdletarna som fungerar med Azure Resource Manager, hittar du mer information i [Migrera till Azure Resource Manager-baserade utvecklingsverktyg för HDInsight-kluster](hdinsight-hadoop-development-using-azure-resource-manager.md).

## <a name="create-cluster"></a>Skapa kluster

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Om du vill skapa ett HDInsight-kluster med hjälp av Azure PowerShell, måste du utföra följande procedurer:

* Skapa en Azure-resursgrupp
* Skapa ett Azure Storage-konto
* Skapa en Azure Blob-behållare
* Skapa ett HDInsight-kluster

Följande skript visar hur du skapar ett nytt kluster:

[!code-powershell[main](../../powershell_scripts/hdinsight/create-cluster/create-cluster.ps1?range=5-71)]

De värden som du anger för klusterinloggning används för att skapa Hadoop-användarkonto för klustret. Använd det här kontot för att ansluta till tjänster som finns i klustret, till exempel web UIs eller REST API: er.

De värden som du anger för SSH-användaren används för att skapa SSH-användare för klustret. Använd det här kontot för att starta en fjärrsession med SSH på klustret och köra jobb. Mer information finns i dokumentet [Använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

> [!IMPORTANT]
> Om du planerar att använda mer än 32 arbetsnoder (antingen när klustret skapas eller genom att skala klustret när du har skapat) måste du även ange en huvudnod storlek med minst 8 kärnor och 14 GB RAM-minne.
>
> Mer information om nodstorlekar och relaterade kostnader finns i [HDInsight-prissättning](https://azure.microsoft.com/pricing/details/hdinsight/).

Det kan ta upp till 20 minuter att skapa ett kluster.

## <a name="create-cluster-configuration-object"></a>Skapa kluster: konfigurationsobjekt

Du kan också skapa ett HDInsight konfiguration objekt med hjälp av `New-AzureRmHDInsightClusterConfig` cmdlet. Sedan kan du ändra det här konfigurationsobjektet för att aktivera ytterligare konfigurationsalternativ för klustret. Använd slutligen den `-Config` -parametern för den `New-AzureRmHDInsightCluster` cmdlet för att använda konfigurationen.

Följande skript skapar ett konfigurationsobjekt för att konfigurera en R Server på typ av HDInsight-kluster. Konfigurationen kan en kantnod, RStudio och ett annat lagringskonto.

[!code-powershell[main](../../powershell_scripts/hdinsight/create-cluster/create-cluster-with-config.ps1?range=59-98)]

> [!WARNING]
> Med ett storage-konto i en annan plats än HDInsight-kluster stöds inte. När du använder det här exemplet kan du skapa ytterligare storage-konto på samma plats som servern.

## <a name="customize-clusters"></a>Anpassa kluster

* Se [anpassa HDInsight-kluster med Bootstrap](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).
* Se [anpassa HDInsight-kluster med skriptåtgärd](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="delete-the-cluster"></a>Ta bort klustret

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Felsöka

Om du får problem med att skapa HDInsight-kluster läser du [åtkomstkontrollkrav](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Nästa steg

Nu när du har skapat ett HDInsight-kluster, kan du använda följande resurser för att lära dig hur du arbetar med ditt kluster.

### <a name="hadoop-clusters"></a>Hadoop-kluster

* [Använda Hive med HDInsight](hadoop/hdinsight-use-hive.md)
* [Använda Pig med HDInsight](hadoop/hdinsight-use-pig.md)
* [Använda MapReduce med HDInsight](hadoop/hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>HBase-kluster

* [Kom igång med HBase i HDInsight](hbase/apache-hbase-tutorial-get-started-linux.md)
* [Utveckla Java-program för HBase på HDInsight](hbase/apache-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Storm-kluster

* [Utveckla Java-topologier för Storm på HDInsight](storm/apache-storm-develop-java-topology.md)
* [Använda Python-komponenter i Storm på HDInsight](storm/apache-storm-develop-python-topology.md)
* [Distribuera och övervaka topologier med Storm på HDInsight](storm/apache-storm-deploy-monitor-topology-linux.md)

### <a name="spark-clusters"></a>Spark-kluster

* [Skapa ett fristående program med hjälp av Scala](spark/apache-spark-create-standalone-application.md)
* [Köra jobb via fjärranslutning på ett Spark-kluster med Livy](spark/apache-spark-livy-rest-interface.md)
* [Spark med BI: Utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg](spark/apache-spark-use-bi-tools.md)
* [Spark med Machine Learning: Använda Spark i HDInsight för att förutsäga resultatet av en livsmedelskontroll](spark/apache-spark-machine-learning-mllib-ipython.md)

