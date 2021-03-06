---
title: Remote Desktop Services inte startar på en virtuell Azure-dator | Microsoft Docs
description: Lär dig hur du felsöker problem med Remote Desktop Services när du ansluter till en virtuell dator | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: cshepard
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/23/2018
ms.author: genli
ms.openlocfilehash: a9967aec61aaab5bc6b4517407f36e2a6c7342c8
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50238870"
---
# <a name="remote-desktop-services-isnt-starting-on-an-azure-vm"></a>Fjärrskrivbordstjänster är inte startar på en Azure VM

Den här artikeln beskriver hur du felsöker problem med att ansluta till en Azure virtuell dator (VM) när Remote Desktop Services (TermService) är inte startar eller inte går att starta.

>[!NOTE]
>Azure har två olika distributionsmodeller som används för att skapa och arbeta med resurser: [Resource Manager och den klassiska distributionsmodellen](../../azure-resource-manager/resource-manager-deployment-model.md). Den här artikeln beskriver hur du använder Resource Manager-distributionsmodellen. Vi rekommenderar att du använder den här modellen för nya distributioner istället för att använda den klassiska distributionsmodellen.

## <a name="symptoms"></a>Symtom

När du försöker ansluta till en virtuell dator händer följande scenarier:

- VM-skärmbilden visar operativsystemet är fullständigt inläst och väntar på autentiseringsuppgifter.

    ![Skärmbild av virtuella datorns status](./media/troubleshoot-remote-desktop-services-issues/login-page.png)

- Du fjärrvisa händelseloggarna på den virtuella datorn med hjälp av Loggboken ser du att Remote Desktop Services (terminalserver) inte är startar eller misslyckas med att starta. Följande är en exempellogg:

    **Loggar namnet**: System </br>
    **Källan**: Service Control Manager </br>
    **Datum**: 2017-12-16 11:19:36 AM</br>
    **Händelse-ID**: 7022</br>
    **Uppgift kategori**: ingen</br>
    **Nivå**: fel</br>
    **Nyckelord**: klassisk</br>
    **Användaren**: ej tillämpligt</br>
    **Datorn**: vm.contoso.com</br>
    **Beskrivning av**: The Remote Desktop Services Avstannade vid start. 

    Du kan också använda funktionen åtkomst Seriekonsol för att leta efter felen med följande fråga: 

        wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Service Control Manager'] and EventID=7022 and TimeCreated[timediff(@SystemTime) <= 86400000]]]" | more 

## <a name="cause"></a>Orsak
 
Det här problemet uppstår eftersom Remote Desktop Services inte körs på den virtuella datorn. Orsaken kan vara beroende av följande scenarier: 

- TermService-tjänsten är inställd på **inaktiverad**. 
- TermService tjänsten kraschar eller hängande. 

## <a name="solution"></a>Lösning

Felsök problemet genom att använda Seriekonsolen eller [reparera den virtuella datorn offline](#repair-the-vm-offline) genom att koppla OS-disken för den virtuella datorn till en virtuell dator för återställning.

### <a name="use-serial-console"></a>Använda Seriekonsol

1. Åtkomst till den [Seriekonsolen](serial-console-windows.md) genom att välja **Support och felsökning** > **seriekonsolen**. Om funktionen är aktiverad på den virtuella datorn, kan du ansluta den virtuella datorn.

2. Skapa en ny kanal för en CMD-instans. Typ **CMD** att starta kanalen för att få dess namn.

3. Växla till kanalen som kör CMD-instans. I det här fallet bör den vara kanal 1.

   ```
   ch -si 1
   ```

4. Tryck på **RETUR** igen och ange ett giltigt användarnamn och lösenord (lokal eller domänåtkomst-ID) för den virtuella datorn.

5. Fråga status för tjänsten TermService.

   ```
   sc query TermService
   ```

6. Om status för tjänsten visar **stoppad**, försök att starta tjänsten.

    ```
    sc start TermService
     ``` 

7. Skicka frågor till tjänsten igen för att se till att tjänsten har startats.

   ```
   sc query TermService
   ```
    Om det inte går att starta tjänsten, följer du den lösning som baseras på felet som du fått:

    |  Fel |  Förslag |
    |---|---|
    |5 – ÅTKOMST NEKAD |Se [TermService tjänsten har stoppats på grund av nekad åtkomst fel](#termService-service-is-stopped-because-of-access-denied-error) |
    |1058 - ERROR_SERVICE_DISABLED  |Se [TermService tjänsten är inaktiverad.](#termService-service-is-disabled)  |
    |1059 - ERROR_CIRCULAR_DEPENDENCY |[Kontakta supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) att lösa problemet snabbt|
    |1068 - ERROR_SERVICE_DEPENDENCY_FAIL|[Kontakta supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) att lösa problemet snabbt|
    |1069 - ERROR_SERVICE_LOGON_FAILED  |[Kontakta supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) att lösa problemet snabbt    |
    |1070 - ERROR_SERVICE_START_HANG   | [Kontakta supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) att lösa problemet snabbt  |
    |1077 - ERROR_SERVICE_NEVER_STARTED   | Se [TermService tjänsten är inaktiverad](#termService-service-is-disabled)  |
    |1079 - ERROR_DIFERENCE_SERVICE_ACCOUNT   |[Kontakta supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) att lösa problemet snabbt |
    |1753   |[Kontakta supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) att lösa problemet snabbt   |

#### <a name="termservice-service-is-stopped-because-of-access-denied-error"></a>TermService tjänsten har stoppats på grund av nekad åtkomst fel

1. Ansluta till [Seriekonsolen](serial-console-windows.md#) och öppna en PowerShell-instans.
2. Hämta det Övervakare för processen genom att köra följande skript:

        remove-module psreadline  
        $source = "https://download.sysinternals.com/files/ProcessMonitor.zip" 
        $destination = "c:\temp\ProcessMonitor.zip" 
        $wc = New-Object System.Net.WebClient 
        $wc.DownloadFile($source,$destination) 
3. Nu starta en procmon spårning:

        procmon /Quiet /Minimized /BackingFile c:\temp\ProcMonTrace.PML 
4. Återskapa problemet problemet genom att starta den tjänst som ger åtkomst neka: 

        sc start TermService 
        
    När den misslyckades, gå vidare och avsluta spårningen Övervakare för processen:

        procmon /Terminate 
5. Samla in filen **c:\temp\ProcMonTrace.PML**, öppna den med hjälp av procmon och sedan filtrera efter **resultatet är NEKAD** som följande skärmbild visar:

    ![Filtrera efter resultat i Övervakare för processen](./media/troubleshoot-remote-desktop-services-issues/process-monitor-access-denined.png)

 
6. Åtgärda registernycklar, mappar eller filer som finns på utdata. Vanligtvis är det här problemet orsakas av loggen för kontot som används på tjänsten har inte ACL-behörighet att komma åt dessa objekt. Om du vill veta rätt ACL-behörighet för att logga in på kontot, kan du kontrollera en felfri virtuell dator. 

#### <a name="termservice-service-is-disabled"></a>TermService tjänsten är inaktiverad

1.  Återställa tjänsten till dess standardvärde för start:

        sc config TermService start= demand 
        
2.  Starta tjänsten:

        sc start TermService 
3.  Fråga efter dess status igen för att säkerställa att tjänsten körs: sc fråga TermService 
4.  Försök att conntet till virtuell dator med hjälp av fjärrskrivbord.


### <a name="repair-the-vm-offline"></a>Reparera den virtuella datorn offline

#### <a name="attach-the-os-disk-to-a-recovery-vm"></a>Koppla OS-disken till en virtuell dator för återställning

1. [Koppla OS-disk till virtuell återställningsdator](../windows/troubleshoot-recovery-disks-portal.md).
2. Starta en fjärrskrivbordsanslutning till den Virtuella återställningsdatorn. Se till att den anslutna disken flaggas som **Online** i konsolen Diskhantering. Observera den enhetsbeteckning som är tilldelad till den anslutna OS-disken.
3.  Öppna en upphöjd kommandotolk-instans (**kör som administratör**), och kör sedan följande skript. Vi förutsätter att den enhetsbeteckning som är tilldelad till den anslutna OS-disken är F. Ersätt den med lämpligt värde i den virtuella datorn. 

        reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv
        
        REM Set default values back on the broken service 
        reg add "HKLM\BROKENSYSTEM\ControlSet001\services\TermService" /v start /t REG_DWORD /d 3 /f
        reg add "HKLM\BROKENSYSTEM\ControlSet001\services\TermService" /v ObjectName /t REG_SZ /d "NT Authority\NetworkService“ /f
        reg add "HKLM\BROKENSYSTEM\ControlSet001\services\TermService" /v type /t REG_DWORD /d 16 /f
        reg add "HKLM\BROKENSYSTEM\ControlSet002\services\TermService" /v start /t REG_DWORD /d 3 /f
        reg add "HKLM\BROKENSYSTEM\ControlSet002\services\TermService" /v ObjectName /t REG_SZ /d "NT Authority\NetworkService" /f
        reg add "HKLM\BROKENSYSTEM\ControlSet002\services\TermService" /v type /t REG_DWORD /d 16 /f
4. [Koppla från den OS-disken och återskapa den virtuella datorn](../windows/troubleshoot-recovery-disks-portal.md), och sedan kontrollera om problemet är löst.

## <a name="need-help-contact-support"></a>Behöver du hjälp? Kontakta supporten

Om du fortfarande behöver hjälp, [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) att lösa problemet snabbt.
