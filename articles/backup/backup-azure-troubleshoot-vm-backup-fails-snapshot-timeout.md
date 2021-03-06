---
title: 'Felsöka Azure Backup-fel: Gäst agenten Status otillgänglig'
description: Symptom, orsaker och lösningar i Azure Backup-fel relaterade till agenten, tillägg och diskar.
services: backup
author: genlin
manager: cshepard
keywords: Azure backup; VM-agenten; Nätverksanslutning;
ms.service: backup
ms.topic: troubleshooting
ms.date: 10/30/2018
ms.author: genli
ms.openlocfilehash: 5c37e2e3cabb81ed123146f283c7d568cc58816d
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50242636"
---
# <a name="troubleshoot-azure-backup-failure-issues-with-the-agent-or-extension"></a>Felsöka Azure Backup-fel: problem med agenten eller -tillägget

Den här artikeln innehåller åtgärder för felsökning som kan hjälpa dig att lösa Azure Backup-fel som rör kommunikation med VM-agenten och tillägg.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="vm-agent-unable-to-communicate-with-azure-backup"></a>VM-agenten kunde inte kommunicera med Azure Backup

Felmeddelande: ”VM-agenten kunde inte kommunicera med Azure Backup”<br>
Felkod: ”UserErrorGuestAgentStatusUnavailable”

När du har registrerat och schemalägga en virtuell dator för Backup-tjänsten Initierar säkerhetskopiering jobbet genom att kommunicera med VM-agenten att ta en ögonblicksbild för point-in-time. Något av följande villkor kan förhindra att ögonblicksbilden utlöses. När en ögonblicksbild inte utlöses misslyckas säkerhetskopieringen. Slutför följande felsökningssteg i angiven ordning och försök igen:

**Orsak 1: [den virtuella datorn inte har tillgång till internet](#the-vm-has-no-internet-access)**  
**Orsak 2: [agenten är installerad på den virtuella datorn, men det är inte svarar (för Windows virtuella datorer)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**    
**Orsak 3: [agenten installerad på den virtuella datorn är för gammal (för virtuella Linux-datorer)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**Orsak 4: [går inte att hämta den ögonblicksbild av statusen eller går inte att ta en ögonblicksbild](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**    
**Orsak 5: [säkerhetskopieringstillägget inte går att uppdatera eller läsa in](#the-backup-extension-fails-to-update-or-load)**  

## <a name="snapshot-operation-failed-due-to-no-network-connectivity-on-the-virtual-machine"></a>Ögonblicksbildsåtgärden misslyckas eftersom den virtuella datorn inte är ansluten till nätverket

Felmeddelande: ”ögonblicksbild-åtgärden misslyckades på grund av den virtuella datorn saknar nätverksanslutning”<br>
Felkod: ”ExtensionSnapshotFailedNoNetwork”

När du har registrerat och schemalägga en virtuell dator för Azure Backup-tjänsten Initierar säkerhetskopiering jobbet genom att kommunicera med VM-tillägg att ta en ögonblicksbild i tidpunkt för säkerhetskopiering. Något av följande villkor kan förhindra att ögonblicksbilden utlöses. Om ögonblicksbilden inte utlöses, kan det uppstå en säkerhetskopieringen har misslyckats. Slutför följande felsökningssteg i angiven ordning och försök igen:    
**Orsak 1: [den virtuella datorn inte har tillgång till internet](#the-vm-has-no-internet-access)**  
**Orsak 2: [går inte att hämta den ögonblicksbild av statusen eller går inte att ta en ögonblicksbild](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**Orsak 3: [säkerhetskopieringstillägget inte går att uppdatera eller läsa in](#the-backup-extension-fails-to-update-or-load)**  

## <a name="vmsnapshot-extension-operation-failed"></a>Vmsnapshot-tillägget misslyckas

Felmeddelande: ”vmsnapshot-tillägget misslyckades”<br>
Felkod: ”ExtentionOperationFailed”

När du har registrerat och schemalägga en virtuell dator för Azure Backup-tjänsten Initierar säkerhetskopiering jobbet genom att kommunicera med VM-tillägg att ta en ögonblicksbild i tidpunkt för säkerhetskopiering. Något av följande villkor kan förhindra att ögonblicksbilden utlöses. Om ögonblicksbilden inte utlöses, kan det uppstå en säkerhetskopieringen har misslyckats. Slutför följande felsökningssteg i angiven ordning och försök igen:  
**Orsak 1: [går inte att hämta den ögonblicksbild av statusen eller går inte att ta en ögonblicksbild](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**Orsak 2: [säkerhetskopieringstillägget inte går att uppdatera eller läsa in](#the-backup-extension-fails-to-update-or-load)**  
**Orsak 3: [agenten är installerad på den virtuella datorn, men det är inte svarar (för Windows virtuella datorer)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**Orsak 4: [agenten installerad på den virtuella datorn är för gammal (för virtuella Linux-datorer)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**

## <a name="backup-fails-because-the-vm-agent-is-unresponsive"></a>Det går inte att säkerhetskopiera eftersom VM-agenten inte svarar

Felmeddelande: ”Det gick inte att kommunicera med VM-agenten för ögonblicksbild av status” <br>
Felkod: ”GuestAgentSnapshotTaskStatusError”

När du har registrerat och schemalägga en virtuell dator för Azure Backup-tjänsten Initierar säkerhetskopiering jobbet genom att kommunicera med VM-tillägg att ta en ögonblicksbild i tidpunkt för säkerhetskopiering. Något av följande villkor kan förhindra att ögonblicksbilden utlöses. Om ögonblicksbilden inte utlöses, kan det uppstå en säkerhetskopieringen har misslyckats. Slutför följande felsökningssteg i angiven ordning och försök igen:  
**Orsak 1: [agenten är installerad på den virtuella datorn, men det är inte svarar (för Windows virtuella datorer)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**Orsak 2: [agenten installerad på den virtuella datorn är för gammal (för virtuella Linux-datorer)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**Orsak 3: [den virtuella datorn inte har tillgång till internet](#the-vm-has-no-internet-access)**  

## <a name="backup-fails-with-an-internal-error"></a>Säkerhetskopieringen misslyckas med ett internt fel

Felmeddelande: ”Det gick inte att säkerhetskopiera med ett internt fel – försök igen om några minuter” <br>
Felkod: ”BackUpOperationFailed” / ”BackUpOperationFailedV2”

När du har registrerat och schemalägga en virtuell dator för Azure Backup-tjänsten Initierar säkerhetskopiering jobbet genom att kommunicera med VM-tillägg att ta en ögonblicksbild i tidpunkt för säkerhetskopiering. Något av följande villkor kan förhindra att ögonblicksbilden utlöses. Om ögonblicksbilden inte utlöses, kan det uppstå en säkerhetskopieringen har misslyckats. Slutför följande felsökningssteg i angiven ordning och försök igen:  
**Orsak 1: [den virtuella datorn inte har tillgång till internet](#the-vm-has-no-internet-access)**  
**Orsak 2: [agenten installerad i den virtuella datorn, men den inte svarar (för Windows virtuella datorer)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**Orsak 3: [agenten installerad på den virtuella datorn är för gammal (för virtuella Linux-datorer)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**Orsak 4: [går inte att hämta den ögonblicksbild av statusen eller går inte att ta en ögonblicksbild](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**Orsak 5: [säkerhetskopieringstillägget inte går att uppdatera eller läsa in](#the-backup-extension-fails-to-update-or-load)**  
**Orsak 6: [Backup-tjänsten har inte behörighet att ta bort gamla återställningspunkter på grund av en grupp resurslås](#backup-service-does-not-have-permission-to-delete-the-old-restore-points-due-to-resource-group-lock)**

## <a name="causes-and-solutions"></a>Orsaker och lösningar

### <a name="the-vm-has-no-internet-access"></a>Den virtuella datorn har inte tillgång till internet
Per distributionskrav har inte den virtuella datorn Internetåtkomst. Eller så den kan ha begränsningar som förhindrar åtkomst till Azure-infrastrukturen.

För att fungera korrekt, kräver att säkerhetskopieringstillägget anslutning till Azure offentliga IP-adresser. Tillägget skickar kommandon till Azure storage-slutpunkt (HTTPs-URL) för hantering av ögonblicksbilder av den virtuella datorn. Om tillägget inte har åtkomst till det offentliga internet, misslyckas säkerhetskopieringen så småningom.

Det är möjligt att distribuera en proxyserver för att dirigera trafik för virtuella datorer.
##### <a name="create-a-path-for-https-traffic"></a>Skapa en sökväg för HTTPs-trafik

1. Om du har nätverksbegränsningar på plats (till exempel en nätverkssäkerhetsgrupp) kan du distribuera en proxyserver används HTTPs för att dirigera trafiken.
2. För att tillåta åtkomst till internet från proxyservern HTTPs, att lägga till regler för nätverkssäkerhetsgruppen, om du har en.

Läs hur du ställer in en HTTPs-proxy för VM-säkerhetskopieringar i [förbereda din miljö för att säkerhetskopiera virtuella Azure-datorer](backup-azure-arm-vms-prepare.md#establish-network-connectivity).

Den säkerhetskopierade virtuella datorn eller proxyservern som trafiken dirigeras kräver åtkomst till Azures offentliga IP-adresser

####  <a name="solution"></a>Lösning
Lös problemet genom att prova någon av följande metoder:

##### <a name="allow-access-to-azure-storage-that-corresponds-to-the-region"></a>Tillåt åtkomst till Azure storage som motsvarar regionen

Du kan använda [tjänsttaggar](../virtual-network/security-overview.md#service-tags) att tillåta anslutningar till lagring för den specifika regionen. Kontrollera att den regel som tillåter åtkomst till storage-kontot har högre prioritet än regeln som blockerar Internetåtkomst. 

![Nätverkssäkerhetsgruppen med storage-taggar för en region](./media/backup-azure-arm-vms-prepare/storage-tags-with-nsg.png)

För att förstå de steg för steg hur du konfigurerar tjänsttaggar, se [videon](https://youtu.be/1EjLQtbKm1M).

> [!WARNING]
> Tjänsttaggar för lagring finns i förhandsversion. De är tillgängliga i specifika regioner. En lista över regioner finns i [Tjänsttaggar för lagring](../virtual-network/security-overview.md#service-tags).

Om du använder Azure Managed Disks kan behöva du ett inledande ytterligare porten (port 8443) i brandväggar.

Om undernätet inte har en väg för utgående Internettrafik, måste du dessutom lägga till en slutpunkt med tjänsttagg ”Microsoft.Storage” i undernätet används. 

### <a name="the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>Agenten är installerad på den virtuella datorn, men det är inte svarar (för Windows virtuella datorer)

#### <a name="solution"></a>Lösning
VM-agenten kan vara skadad eller tjänsten har stoppats. Installera om den Virtuella datoragenten kan hämta den senaste versionen. Det hjälper också att starta om kommunikation med tjänsten.

1. Avgör om Windows-gästagenttjänsten körs i VM-tjänsterna (services.msc). Försök att starta om Windows-gästagenttjänsten och påbörja säkerhetskopieringen.    
2. Om Windows-gästagenttjänsten inte visas i tjänster på Kontrollpanelen, gå till **program och funktioner** att avgöra om tjänsten Windows-Gästagenten är installerad.
4. Om Windows-Gästagenten visas i **program och funktioner**, avinstallera Windows-Gästagenten.
5. Ladda ned och installera den [senaste versionen av agenten MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Du måste ha administratörsbehörighet för att slutföra installationen.
6. Kontrollera att Windows-Gästagenten tjänster visas i services.
7. Köra en säkerhetskopiering på begäran: 
    * I portalen, väljer **Säkerhetskopiera nu**.

Kontrollera också att [Microsoft .NET 4.5 har installerats](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) på den virtuella datorn. .NET 4.5 krävs för VM-agenten kan kommunicera med tjänsten.

### <a name="the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>Agenten installerad på den virtuella datorn är för gammal (för virtuella Linux-datorer)

#### <a name="solution"></a>Lösning
Mest agent-relaterade eller tillägget-relaterade fel för virtuella Linux-datorer orsakas av problem som påverkar en inaktuell VM-agent. Följ dessa allmänna riktlinjer för att felsöka problemet:

1. Följ anvisningarna för [uppdaterar Linux VM-agenten](../virtual-machines/linux/update-agent.md).

 > [!NOTE]
 > Vi *rekommenderar* att du uppdaterar agenten endast via en lagringsplats för distribution. Vi rekommenderar inte hämta agent-kod direkt från GitHub och uppdaterar den. Om den senaste agenten för din distribution inte är tillgänglig, kontakta distribution stöd för instruktioner om hur du installerar den. Om du vill söka efter den senaste agenten, går du till den [Windows Azure Linux-agent](https://github.com/Azure/WALinuxAgent/releases) sidan i GitHub-lagringsplatsen.

2. Se till att Azure-agenten körs på den virtuella datorn genom att köra följande kommando: `ps -e`

 Om processen inte körs måste du starta om den med hjälp av följande kommandon:

 * För Ubuntu: `service walinuxagent start`
 * För andra distributioner: `service waagent start`

3. [Konfigurera automatisk omstart agent](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).
4. Kör en ny test-säkerhetskopia. Om felet kvarstår, kan du samla in följande loggar från den virtuella datorn:

   * /var/lib/waagent/*.xml
   * /var/log/waagent.log
   * / var/logga/azure / *

Om vi behöver utförlig loggning för waagent gör du följande:

1. Leta upp följande rad i filen /etc/waagent.conf: **aktivera utförlig loggning (y | n)**
2. Ändra den **Logs.Verbose** värdet från *n* till *y*.
3. Spara ändringen och starta sedan om waagent genom att följa stegen som beskrivs i det här avsnittet.

###  <a name="the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>Det går inte att hämta den ögonblicksbild av statusen eller går inte att ta en ögonblicksbild
Säkerhetskopiering av virtuella datorer är beroende av utfärda ett kommando för ögonblicksbild till det underliggande lagringskontot. Backup kan misslyckas eftersom den har ingen åtkomst till lagringskontot eller eftersom körningen av uppgiften ögonblicksbild är försenad.

#### <a name="solution"></a>Lösning
Följande villkor kan orsaka uppgiften ögonblicksbild misslyckas:

| Orsak | Lösning |
| --- | --- |
| Virtuella datorns status rapporteras felaktigt eftersom Virtuellt datorn stängs av i Remote Desktop Protocol (RDP). | Om du stänger av den virtuella datorn i RDP Kontrollera portalen för att avgöra om virtuella datorns status är rätt. Om det inte är korrekt, stänger du den virtuella datorn i portalen med hjälp av den **avstängning** alternativet på VM-instrumentpanelen. |
| Den virtuella datorn kan inte hämta värden eller fabric-adress från DHCP. | DHCP måste vara aktiverat på gästen för IaaS VM-säkerhetskopiering för att fungera. Om den virtuella datorn inte kan hämta värden eller fabric-adress från DHCP-svar 245, kan inte den hämta eller köra några tillägg. Om du behöver en statisk privat IP-adress kan du konfigurera den med plattformen. DHCP-alternativ i den virtuella datorn måste vara aktiverat till vänster. Mer information finns i [ange en statisk IP för interna privata](../virtual-network/virtual-networks-reserved-private-ip.md). |

### <a name="the-backup-extension-fails-to-update-or-load"></a>Säkerhetskopieringstillägget inte går att uppdatera eller läsa in
Om tillägg inte kan läsas in, misslyckas säkerhetskopieringen eftersom det går inte att ta en ögonblicksbild.

#### <a name="solution"></a>Lösning

Avinstallera tillägget för att tvinga VMSnapshot-tillägget läses in på nytt. Nästa säkerhetskopiering försöket hämtar tillägget.

Avinstallera tillägget:

1. I den [Azure-portalen](https://portal.azure.com/)går du till den virtuella datorn som har drabbats av säkerhetskopieringen har misslyckats.
2. Välj **inställningar**.
3. Välj **Tillägg**.
4. Välj **Vmsnapshot-tillägget**.
5. Välj **avinstallera**.

För Linux VM, om VMSnapshot-tillägget inte visas i Azure-portalen [uppdatera Azure Linux Agent](../virtual-machines/linux/update-agent.md), och sedan köra säkerhetskopieringen. 

Gör så här gör tillägget installeras under nästa säkerhetskopiering.

### <a name="backup-service-does-not-have-permission-to-delete-the-old-restore-points-due-to-resource-group-lock"></a>Backup-tjänsten har inte behörighet att ta bort gamla återställningspunkter på grund av en grupp resurslås
Det här problemet är specifikt för hanterade virtuella datorer där användaren låser resursgruppen. I det här fallet säkerhetskopieringstjänsten kan inte ta bort äldre återställningspunkter. Eftersom det finns en gräns på 18 återställningspunkter, börjar nya säkerhetskopior misslyckas.

#### <a name="solution"></a>Lösning

Lös problemet genom att ta bort låset från resursgruppen och utför följande steg för att ta bort samlingen med återställningspunkter: 
 
1. Ta bort låset i resursgruppen där Virtuellt datorn finns. 
2. Installera ARMClient med Chocolatey: <br>
   https://github.com/projectkudu/ARMClient
3. Logga in på ARMClient: <br>
    `.\armclient.exe login`
4. Hämta den samling med återställningspunkter som motsvarar den virtuella datorn: <br>
    `.\armclient.exe get https://management.azure.com/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Compute/restorepointcollections/AzureBackup_<VM-Name>?api-version=2017-03-30`

    Exempel: `.\armclient.exe get https://management.azure.com/subscriptions/f2edfd5d-5496-4683-b94f-b3588c579006/resourceGroups/winvaultrg/providers/Microsoft.Compute/restorepointcollections/AzureBackup_winmanagedvm?api-version=2017-03-30`
5. Ta bort samlingen med återställningspunkter: <br>
    `.\armclient.exe delete https://management.azure.com/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Compute/restorepointcollections/AzureBackup_<VM-Name>?api-version=2017-03-30` 
6. Nästa schemalagda säkerhetskopiering skapar automatiskt en samling med återställningspunkter och nya återställningspunkter.

När du har gjort, kan du igen lägga tillbaka låset på resursgrupp för virtuell dator. 
