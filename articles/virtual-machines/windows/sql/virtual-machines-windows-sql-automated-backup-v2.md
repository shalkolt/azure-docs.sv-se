---
title: Automatisk säkerhetskopiering v2 för SQL Server 2016/2017 virtuella Azure-datorer | Microsoft Docs
description: Beskriver funktionen automatisk säkerhetskopiering för SQL Server 2016/2017 virtuella datorer som körs i Azure. Den här artikeln är specifik för virtuella datorer med Resource Manager.
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: craigg
tags: azure-resource-manager
ms.assetid: ebd23868-821c-475b-b867-06d4a2e310c7
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/03/2018
ms.author: jroth
ms.openlocfilehash: 4619c26e34c90f58702ad286f76a999f83f49cc4
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/08/2018
ms.locfileid: "33894517"
---
# <a name="automated-backup-v2-for-azure-virtual-machines-resource-manager"></a>Automatisk säkerhetskopiering v2 för virtuella Azure-datorer (Resource Manager)

> [!div class="op_single_selector"]
> * [SQL Server 2014](virtual-machines-windows-sql-automated-backup.md)
> * [SQL Server 2016/2017](virtual-machines-windows-sql-automated-backup-v2.md)

Automatisk säkerhetskopiering v2 konfigurerar automatiskt [hanterad säkerhetskopiering till Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) för alla befintliga och nya databaser på en Azure VM som kör SQL Server 2016/2017 Standard, Enterprise eller Developer-versioner. På så sätt kan du konfigurera regelbundna säkerhetskopieringar som använder beständiga Azure blob storage. Automatisk säkerhetskopiering v2 beror på den [tillägg för SQL Server IaaS Agent](virtual-machines-windows-sql-server-agent-extension.md).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a>Förutsättningar
Om du vill använda automatisk säkerhetskopiering v2, gå igenom följande krav:

**Operativsystemet**:

- Windows Server 2012 R2
- Windows Server 2016

**SQL Server-version/utgåva**:

- SQLServer 2016: Utvecklare, Standard eller Enterprise
- SQLServer 2017: Utvecklare, Standard eller Enterprise

> [!IMPORTANT]
> Automatisk säkerhetskopiering v2 fungerar med SQL Server 2016 eller högre. Om du använder SQL Server 2014 kan du använda automatisk säkerhetskopiering v1 för att säkerhetskopiera databaserna. Mer information finns i [automatisk säkerhetskopiering för SQL Server 2014 Azure Virtual Machines](virtual-machines-windows-sql-automated-backup.md).

**Databaskonfiguration**:

- Måldatabaserna måste använda den fullständiga återställningsmodellen. Mer information om inverkan av den fullständiga återställningsmodellen säkerhetskopior finns [säkerhetskopiering Under den fullständiga återställningsmodellen](https://technet.microsoft.com/library/ms190217.aspx).
- Systemdatabaser behöver inte använda fullständiga återställningsmodellen. Om du behöver säkerhetskopior som ska vidtas för modellen eller MSDB, måste du använda fullständiga återställningsmodellen.
- Måldatabaserna måste vara på standardinstansen för SQL Server. SQL Server IaaS-tillägg stöder inte namngivna instanser.

> [!NOTE]
> Automatisk säkerhetskopiering är beroende av **tillägg för SQL Server IaaS Agent**. Aktuell SQL virtuella galleriavbildningar lägga till tillägget som standard. Mer information finns i [tillägg för SQL Server IaaS Agent](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="settings"></a>Inställningar
I följande tabell beskrivs de alternativ som kan konfigureras för automatisk säkerhetskopiering v2. De faktiska konfigurationsstegen varierar beroende på om du använder Azure-portalen eller Azure Windows PowerShell-kommandon.

### <a name="basic-settings"></a>Grundinställningar

| Inställning | Intervall (standard) | Beskrivning |
| --- | --- | --- |
| **Automatisk säkerhetskopiering** | Aktivera/inaktivera (inaktiverat) | Aktiverar eller inaktiverar automatisk säkerhetskopiering för en Azure-dator som kör SQL Server 2016/2017 utvecklare, Standard eller Enterprise. |
| **Kvarhållningsperioden** | 1 – 30 dagar (30 dagar) | Antal dagar säkerhetskopiorna ska behållas. |
| **Lagringskonto** | Azure Storage-konto | Ett Azure storage-konto som ska användas för att lagra filer för automatisk säkerhetskopiering i blob storage. En behållare skapas på denna plats att lagra säkerhetskopior. Namngivningskonventionen för säkerhetskopian innehåller datum, tid och databas-GUID. |
| **Kryptering** |Aktivera/inaktivera (inaktiverat) | Aktiverar eller inaktiverar kryptering. När kryptering är aktiverat finns certifikat som används för att återställa säkerhetskopian i det angivna lagringskontot. Den använder samma **automaticbackup** behållare med samma namngivningskonvention. Om lösenordet ändras, skapas ett nytt certifikat med lösenordet, men det gamla certifikatet finns kvar för att återställa tidigare säkerhetskopior. |
| **Lösenord** |Text för lösenord | Ett lösenord för krypteringsnycklar. Lösenordet är bara obligatoriskt om kryptering är aktiverat. Du måste ha rätt lösenord och relaterade certifikatet som användes när säkerhetskopian skapades för att återställa en krypterad säkerhetskopiering. |

### <a name="advanced-settings"></a>Avancerade inställningar

| Inställning | Intervall (standard) | Beskrivning |
| --- | --- | --- |
| **System databassäkerhetskopieringar** | Aktivera/inaktivera (inaktiverat) | När aktiverat funktionen också säkerhetskopierar systemdatabaserna: Master, MSDB och modell. Kontrollera att de är i fullständigt återställningsläge om du vill att säkerhetskopior som ska vidtas för databaserna MSDB och modell. Säkerhetskopior av tas aldrig för Master. Och inga säkerhetskopior har tagit för TempDB. |
| **Schemat för säkerhetskopiering** | Manuell/automatisk (automatisk) | Som standard bestäms automatiskt schemat för säkerhetskopiering baserat på loggen tillväxt. Manuell Säkerhetskopieringsschemat tillåter användaren att ange tidsfönstret för säkerhetskopior. I det här fallet ske säkerhetskopieringar endast vid den angivna frekvensen och under det angivna tidsfönstret för en viss dag. |
| **Frekvens för fullständig säkerhetskopiering** | Varje dag/vecka | Frekvens för fullständiga säkerhetskopieringar. I båda fallen måste börja och fullständiga säkerhetskopieringar under nästa schemalagda tidpunkt fönster. När varje vecka väljs säkerhetskopieringar kan sträcka sig över flera dagar tills alla databaser har säkerhetskopierats. |
| **Starttid för fullständig säkerhetskopiering** | 00:00 – 23:00 (01:00) | Starttid för en viss dag då fullständiga säkerhetskopieringar kan göras. |
| **Fullständig säkerhetskopiering tidsfönstret** | 1 – 23 timmar (1 timme) | Varaktighet för tidsfönstret för en viss dag då fullständiga säkerhetskopieringar kan göras. |
| **Frekvens för säkerhetskopiering av loggen** | 5 – 60 minuter (60 minuter) | Frekvensen för säkerhetskopior. |

## <a name="understanding-full-backup-frequency"></a>Förstå frekvensen för fullständig säkerhetskopiering
Det är viktigt att förstå skillnaden mellan dagliga och veckovisa fullständiga säkerhetskopieringar. Tänk på följande två exempel.

### <a name="scenario-1-weekly-backups"></a>Scenario 1: Veckovisa säkerhetskopior
Du har en SQL Server-VM som innehåller ett antal stora databaser.

Måndag aktivera automatisk säkerhetskopiering v2 med följande inställningar:

- Schemat för säkerhetskopiering: **manuell**
- Fullständig säkerhetskopiering synkroniseringsfrekvensen: **varje vecka**
- Fullständig säkerhetskopiering starttiden: **01:00**
- Fullständig säkerhetskopiering tidsfönstret: **1 timme**

Detta innebär att nästa tillgängliga fönster för säkerhetskopiering finns tisdag kl 1 timma. Då börjar automatisk säkerhetskopiering säkerhetskopiera databaserna en i taget. I det här scenariot är databaserna tillräckligt stor för att utföra fullständiga säkerhetskopieringar för de första några databaserna. Men efter en timme har inte alla databaserna som säkerhetskopierats.

När detta händer börjar automatisk säkerhetskopiering säkerhetskopiera återstående databaser nästa dag, onsdag kl 1 under en timme. Om inte alla databaser har säkerhetskopierats i den tid, försök igen nästa dag på samma gång. Detta fortsätter tills alla databaser har säkerhetskopierats.

När den når tisdag igen börjar automatisk säkerhetskopiering säkerhetskopierar alla databaser igen.

Det här scenariot visar att automatisk säkerhetskopiering fungerar endast inom det angivna tidsfönstret, och varje databas säkerhetskopieras en gång per vecka. Det visar att det är möjligt för säkerhetskopiering ska sträcka sig över flera dagar i de fall där det inte går att slutföra alla säkerhetskopior i en dag.

### <a name="scenario-2-daily-backups"></a>Scenario 2: Daglig säkerhetskopiering
Du har en SQL Server-VM som innehåller ett antal stora databaser.

Måndag aktivera automatisk säkerhetskopiering v2 med följande inställningar:

- Schemat för säkerhetskopiering: manuell
- Fullständig säkerhetskopiering synkroniseringsfrekvensen: varje dag
- Fullständig säkerhetskopiering starttiden: 22:00
- Fullständig säkerhetskopiering tidsfönstret: 6 timmar

Detta innebär att nästa tillgängliga fönster för säkerhetskopiering är måndag klockan 10 i 6 timmar. Då börjar automatisk säkerhetskopiering säkerhetskopiera databaserna en i taget.

Tisdagen 10 i 6 timmar och fullständiga säkerhetskopieringar för alla databaser starta sedan igen.

> [!IMPORTANT]
> När du schemalägger en daglig säkerhetskopiering, rekommenderas att du schemalägger ett brett tidsfönster så alla databaser kan säkerhetskopieras i den här gången. Detta är särskilt viktigt i de fall där du har en stor mängd data att säkerhetskopiera.

## <a name="configure-in-the-portal"></a>Konfigurera i portalen

Du kan använda Azure-portalen för att konfigurera automatisk säkerhetskopiering v2 under etableringen eller för befintliga SQL Server 2016/2017 virtuella datorer.

## <a name="configure-for-new-vms"></a>Konfigurera för nya virtuella datorer

Använda Azure portal för att konfigurera automatisk säkerhetskopiering v2 när du skapar en ny SQL Server 2016 eller 2017 virtuell dator i Resource Manager-distributionsmodellen.

I den **SQL Server-inställningar** väljer **automatisk säkerhetskopiering**. I följande Skärmbild av Azure portal visas den **SQL automatisk säkerhetskopiering** inställningar.

![Automatisk säkerhetskopiering i SQL-konfigurationen i Azure-portalen](./media/virtual-machines-windows-sql-automated-backup-v2/automated-backup-blade.png)

> [!NOTE]
> Automatisk säkerhetskopiering v2 är inaktiverad som standard.

## <a name="configure-existing-vms"></a>Konfigurera befintliga virtuella datorer

Välj den virtuella datorn för SQL Server för befintliga SQL Server virtuella datorer. Välj sedan den **SQL Server-konfigurationsfilen** avsnitt i den virtuella datorn **inställningar**.

![SQL automatisk säkerhetskopiering av befintliga virtuella datorer](./media/virtual-machines-windows-sql-automated-backup-v2/sql-server-configuration.png)

I den **SQL Server-konfigurationsfilen** klickar du på den **redigera** i avsnittet automatisk säkerhetskopiering.

![Konfigurera automatisk säkerhetskopiering i SQL för befintliga virtuella datorer](./media/virtual-machines-windows-sql-automated-backup-v2/sql-server-configuration-edit.png)

När du är klar klickar du på den **OK** knappen längst ned på den **SQL Server-konfigurationsfilen** inställningar för att spara dina ändringar.

Om du aktiverar automatisk säkerhetskopiering för första gången, konfigurerar Azure SQL Server IaaS-Agent i bakgrunden. Under denna tid kanske Azure-portalen inte visar att automatisk säkerhetskopiering har konfigurerats. Vänta några minuter för att agenten ska installeras, konfigureras. Efter det visar Azure-portalen de nya inställningarna.

## <a name="configure-with-powershell"></a>Konfigurera med PowerShell

Du kan använda PowerShell för att konfigurera automatisk säkerhetskopiering v2. Innan du börjar måste du:

- [Hämta och installera den senaste Azure PowerShell](http://aka.ms/webpi-azps).
- Öppna Windows PowerShell och koppla den till ditt konto med den **Connect-AzureRmAccount** kommando.

### <a name="install-the-sql-iaas-extension"></a>Installera SQL IaaS-tillägg
Om du har etablerat en SQL Server-datorn från Azure portal bör SQL Server IaaS-tillägget vara installerad. Du kan fastställa om det har installerats för den virtuella datorn genom att anropa **Get-AzureRmVM** kommandot och undersöka den **tillägg** egenskapen.

```powershell
$vmname = "vmname"
$resourcegroupname = "resourcegroupname"

(Get-AzureRmVM -Name $vmname -ResourceGroupName $resourcegroupname).Extensions 
```

Du bör se den visas som ”SqlIaaSAgent” eller ”SQLIaaSExtension” om tillägget SQL Server IaaS-Agent är installerad. **ProvisioningState** för tillägget bör också visa ”lyckades”. 

Om det inte är installerat eller inte fungerar som ska etableras, kan du installera det med följande kommando. Utöver VM namn och resursen gruppen, måste du också ange regionen (**$region**) som den virtuella datorn finns i.

```powershell
$region = “EASTUS2”
Set-AzureRmVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region 
```

### <a id="verifysettings"></a> Kontrollera aktuella inställningar
Om du har aktiverat automatisk säkerhetskopiering under etableringen kan du använda PowerShell för att kontrollera den aktuella konfigurationen. Kör den **Get-AzureRmVMSqlServerExtension** kommando och undersöka den **AutoBackupSettings** egenskapen:

```powershell
(Get-AzureRmVMSqlServerExtension -VMName $vmname -ResourceGroupName $resourcegroupname).AutoBackupSettings
```

Du bör få utdata liknar följande:

```
Enable                      : True
EnableEncryption            : False
RetentionPeriod             : 30
StorageUrl                  : https://test.blob.core.windows.net/
StorageAccessKey            :  
Password                    : 
BackupSystemDbs             : False
BackupScheduleType          : Manual
FullBackupFrequency         : WEEKLY
FullBackupStartTime         : 2
FullBackupWindowHours       : 2
LogBackupFrequency          : 60
```

Om din utdata visar att **aktivera** är inställd på **FALSKT**, och sedan att aktivera automatisk säkerhetskopiering. Goda nyheter är att aktivera och konfigurera automatisk säkerhetskopiering på samma sätt. Finns i nästa avsnitt för den här informationen.

> [!NOTE] 
> Om du kontaktar inställningarna innan du gör en ändring, är det möjligt att du kommer tillbaka gamla konfigurationsvärden. Vänta en stund och kontrollera inställningarna igen för att säkerställa att ändringarna har tillämpats.

### <a name="configure-automated-backup-v2"></a>Konfigurera automatisk säkerhetskopiering v2
Du kan använda PowerShell för att aktivera automatisk säkerhetskopiering samt för att ändra dess konfiguration och funktionalitet när som helst. 

Först, Välj eller skapa ett lagringskonto för de säkerhetskopiera filerna. Följande skript väljer ett lagringskonto eller skapar den om den inte finns.

```powershell
$storage_accountname = “yourstorageaccount”
$storage_resourcegroupname = $resourcegroupname

$storage = Get-AzureRmStorageAccount -ResourceGroupName $resourcegroupname `
    -Name $storage_accountname -ErrorAction SilentlyContinue
If (-Not $storage)
    { $storage = New-AzureRmStorageAccount -ResourceGroupName $storage_resourcegroupname `
    -Name $storage_accountname -SkuName Standard_GRS -Location $region } 
```

> [!NOTE]
> Automatisk säkerhetskopiering stöder inte lagra säkerhetskopior i premium-lagring, men det kan göra säkerhetskopior från Virtuella diskar som använder Premium-lagring.

Använd sedan den **ny AzureRmVMSqlServerAutoBackupConfig** kommando för att aktivera och konfigurera automatisk säkerhetskopiering v2-inställningar för att lagra säkerhetskopior i Azure storage-konto. I det här exemplet ställs säkerhetskopiorna ska behållas i 10 dagar. System databassäkerhetskopiorna är aktiverade. Fullständiga säkerhetskopieringar är schemalagda för varje vecka med ett tidsfönster som börjar vid 20:00 i två timmar. Säkerhetskopieringar är schemalagda för var 30: e minut. Det andra kommandot **Set AzureRmVMSqlServerExtension**, uppdaterar den angivna virtuella Azure-datorn med de här inställningarna.

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname -BackupSystemDbs `
    -BackupScheduleType Manual -FullBackupFrequency Weekly `
    -FullBackupStartHour 20 -FullBackupWindowInHours 2 `
    -LogBackupFrequencyInMinutes 30 

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname 
```

Det kan ta flera minuter att installera och konfigurera SQL Server IaaS-Agent. 

Om du vill aktivera kryptering, ändra tidigare skriptet skickar den **EnableEncryption** parametern tillsammans med ett lösenord (säker sträng) för den **CertificatePassword** parametern. Följande skript gör inställningar för automatisk säkerhetskopiering i föregående exempel och lägger till kryptering.

```powershell
$password = "P@ssw0rd"
$encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  

$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -EnableEncryption -CertificatePassword $encryptionpassword `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname -BackupSystemDbs `
    -BackupScheduleType Manual -FullBackupFrequency Weekly `
    -FullBackupStartHour 20 -FullBackupWindowInHours 2 `
    -LogBackupFrequencyInMinutes 30 

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

Bekräfta dina inställningar tillämpas [verifiera konfigurationen av automatisk säkerhetskopiering](#verifysettings).

### <a name="disable-automated-backup"></a>Inaktivera automatisk säkerhetskopiering
Om du vill inaktivera automatisk säkerhetskopiering, kör du skriptet samma utan den **-aktivera** parametern till den **ny AzureRmVMSqlServerAutoBackupConfig** kommando. Avsaknad av den **-aktivera** parametern signalerar kommandot för att inaktivera funktionen. Precis som med installationen, kan det ta flera minuter att inaktivera automatisk säkerhetskopiering.

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -ResourceGroupName $storage_resourcegroupname

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

### <a name="example-script"></a>Exempelskript
Följande skript innehåller en uppsättning variabler som du kan anpassa om du vill aktivera och konfigurera automatisk säkerhetskopiering för den virtuella datorn. I ditt fall kan du behöva anpassa skriptet baserat på dina krav. Du skulle behöva ändra om du vill aktivera kryptering eller inaktivera säkerhetskopiering för systemdatabaser.

```powershell
$vmname = "yourvmname"
$resourcegroupname = "vmresourcegroupname"
$region = “Azure region name such as EASTUS2”
$storage_accountname = “storageaccountname”
$storage_resourcegroupname = $resourcegroupname
$retentionperiod = 10
$backupscheduletype = "Manual"
$fullbackupfrequency = "Weekly"
$fullbackupstarthour = "20"
$fullbackupwindow = "2"
$logbackupfrequency = "30"

# ResourceGroupName is the resource group which is hosting the VM where you are deploying the SQL IaaS Extension 

Set-AzureRmVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region

# Creates/use a storage account to store the backups

$storage = Get-AzureRmStorageAccount -ResourceGroupName $resourcegroupname `
    -Name $storage_accountname -ErrorAction SilentlyContinue
If (-Not $storage)
    { $storage = New-AzureRmStorageAccount -ResourceGroupName $storage_resourcegroupname `
    -Name $storage_accountname -SkuName Standard_GRS -Location $region }

# Configure Automated Backup settings

$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays $retentionperiod -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname -BackupSystemDbs `
    -BackupScheduleType $backupscheduletype -FullBackupFrequency $fullbackupfrequency `
    -FullBackupStartHour $fullbackupstarthour -FullBackupWindowInHours $fullbackupwindow `
    -LogBackupFrequencyInMinutes $logbackupfrequency

# Apply the Automated Backup settings to the VM

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

## <a name="monitoring"></a>Övervakning

Du har två huvudsakliga alternativ för att övervaka automatisk säkerhetskopiering på SQL Server 2016/2017. Eftersom automatisk säkerhetskopiering använder funktionen SQL Server-hanterad säkerhetskopiering kan tillämpas på samma sätt som övervakning för både.

Du kan börja avsöka status genom att anropa [msdb.managed_backup.sp_get_backup_diagnostics](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/managed-backup-sp-get-backup-diagnostics-transact-sql). Eller fråga den [msdb.managed_backup.fn_get_health_status](https://docs.microsoft.com/sql/relational-databases/system-functions/managed-backup-fn-get-health-status-transact-sql) tabellvärdesfunktion funktion.

Ett annat alternativ är att dra nytta av den inbyggda funktionen Database Mail för meddelanden.

1. Anropa den [msdb.managed_backup.sp_set_parameter](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/managed-backup-sp-set-parameter-transact-sql) lagrade proceduren för att tilldela en e-postadress till den **SSMBackup2WANotificationEmailIds** parameter. 
1. Aktivera [SendGrid](../../../sendgrid-dotnet-how-to-send-email.md) att skicka e-postmeddelanden från den virtuella Azure-datorn.
1. Använd SMTP-servern och användaren namn om du vill konfigurera Database Mail. Du kan konfigurera Database Mail i SQL Server Management Studio eller med Transact-SQL-kommandon. Mer information finns i [Database Mail](https://docs.microsoft.com/sql/relational-databases/database-mail/database-mail).
1. [Konfigurera SQL Server Agent om du vill använda Database Mail](https://docs.microsoft.com/sql/relational-databases/database-mail/configure-sql-server-agent-mail-to-use-database-mail).
1. Kontrollera att SMTP-porten får både via den lokala VM-brandväggen och nätverkssäkerhetsgruppen för den virtuella datorn.

## <a name="next-steps"></a>Nästa steg
Automatisk säkerhetskopiering v2 konfigurerar hanterad säkerhetskopiering på virtuella Azure-datorer. Därför är det viktigt att [Läs dokumentationen om hanterad säkerhetskopiering](https://msdn.microsoft.com/library/dn449496.aspx) att förstå beteendet och konsekvenser.

Du kan hitta ytterligare en säkerhetskopia och återställa riktlinjerna för SQL Server på virtuella Azure-datorer i följande artikel: [säkerhetskopiering och återställning av SQL Server i Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).

Information om andra tillgängliga automation-aktiviteter finns [tillägg för SQL Server IaaS Agent](virtual-machines-windows-sql-server-agent-extension.md).

Mer information om hur du kör SQL Server på Azure Virtual Machines finns [SQL Server på Azure virtuella datorer – översikt](virtual-machines-windows-sql-server-iaas-overview.md).

