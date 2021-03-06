---
title: Viktig information för Azure File Sync-agenten | Microsoft Docs
description: Viktig information om Azure File Sync-agenten.
services: storage
author: wmgries
ms.service: storage
ms.topic: article
ms.date: 10/10/2018
ms.author: wgries
ms.component: files
ms.openlocfilehash: 6f39088416f4b2c3d9b04a3d83c41c8738b2c595
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/10/2018
ms.locfileid: "49079505"
---
# <a name="release-notes-for-the-azure-file-sync-agent"></a>Viktig information för Azure File Sync-agenten
Med Azure File Sync kan du centralisera din organisations filresurser i Azure Files med samma flexibilitet, prestanda och kompatibilitet som du får om du använder en lokal filserver. Dina Windows Server-installationer omvandlas till ett snabbt cacheminne för Azure-filresursen. Du kan använda alla protokoll som är tillgängliga på Windows Server för att komma åt data lokalt (inklusive SMB, NFS och FTPS). Du kan ha så många cacheminnen som du behöver över hela världen.

Den här artikeln innehåller viktig information om versioner av Azure File Sync-agenten som stöds.

## <a name="supported-versions"></a>Versioner som stöds
Följande versioner av Azure File Sync-agenten stöds:

| Milstolpe | Agentversionsnummer | Utgivningsdatum | Status |
|----|----------------------|--------------|------------------|
| Samlad uppdatering september | 3.3.0.0 | 24 september 2018 | Stöds (rekommenderad version) |
| Samlad uppdatering augusti | 3.2.0.0 | 15 augusti 2018 | Stöds |
| Allmän tillgänglighet | 3.1.0.0 | 19 juli 2018 | Stöds |
| Samlad uppdatering juni | 3.0.13.0 | Den 29 juni 2018 | Inte stöd för – agentversion har upphört att gälla den 1 oktober 2018 |
| Uppdatera 2 | 3.0.12.0 | 22 maj 2018 | Inte stöd för – agentversion har upphört att gälla den 1 oktober 2018 |
| Samlad uppdatering april | 2.3.0.0 | 8 maj 2018 | Inte stöd för – agentversion har upphört att gälla den 1 oktober 2018 |
| Samlad uppdatering för mars | 2.2.0.0 | Den 12 mars 2018 | Inte stöd för – agentversion har upphört att gälla den 1 oktober 2018 |
| Samlad uppdatering februari | 2.1.0.0 | 28 februari 2018 | Inte stöd för – agentversion har upphört att gälla den 1 oktober 2018 |
| Uppdatera 1 | 2.0.11.0 | 8 februari 2018 | Inte stöd för – agentversion har upphört att gälla den 1 oktober 2018 |
| Samlad uppdatering januari | 1.4.0.0 | Den 8 januari 2018 | Inte stöd för – agentversion har upphört att gälla den 1 oktober 2018 |
| Samlad uppdatering november | 1.3.0.0 | Den 30 november 2017 | Inte stöd för – agentversion har upphört att gälla den 1 oktober 2018 |
| Samlad uppdatering för oktober | 1.2.0.0 | 31 oktober 2017 | Inte stöd för – agentversion har upphört att gälla den 1 oktober 2018 |
| Inledande förhandsversion | 1.1.0.0 | 26 september 2017 | Inte stöd för – agentversion har upphört att gälla den 1 oktober 2018 |

### <a name="azure-file-sync-agent-update-policy"></a>Uppdateringsprincip för Azure File Sync-agenten
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="agent-version-3300"></a>Agentversion 3.3.0.0
Följande viktiga information gäller 3.3.0.0 av Azure File Sync-agenten gavs ut den 24 September 2018. Detta är viktig för version 3.1.0.0.

Den här versionen innehåller följande korrigeringen:
- Registrerad server status är ”visas som offline” när du har i Azure File Sync-agenten har uppgraderats till version 3.1 eller 3.2.
- Storage Sync-agenten (FileSyncSvc)-tjänsten kraschar på grund av filer som har långa sökvägar.
- Registrera servern misslyckas med fel: Det gick inte att läsa in filen eller sammansättningen Kailani.Afs.StorageSyncProtocol.V3.

## <a name="agent-version-3200"></a>Agent-version 3.2.0.0
Följande viktiga information gäller 3.2.0.0 av Azure File Sync-agenten gavs ut den 15 augusti 2018. Detta är viktig för version 3.1.0.0.

Den här versionen innehåller följande korrigeringen:
- Synkronisering misslyckas med minnesfel-nummer (0x8007000e) på grund av minnesläcka

## <a name="agent-version-3100"></a>Agentversion 3.1.0.0
Följande viktiga information gäller 3.1.0.0 av Azure File Sync-agenten (gavs ut den 19 juli 2018).

### <a name="evaluation-tool"></a>Utvärderingsverktyg för
Innan du distribuerar Azure File Sync, bör du utvärdera om den är kompatibel med ditt system med hjälp av verktyget Azure File Sync-utvärdering. Det här verktyget är ett AzureRM PowerShell-cmdlet som söker efter potentiella problem med ditt filsystem och datauppsättningen, till exempel tecken som inte stöds eller en OS-version som inte stöds. Anvisningarna för installation och användning, finns i [Evaluation Tool](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-tool) avsnitt i Planeringsguiden. 

### <a name="agent-installation-and-server-configuration"></a>Agentinstallation och serverkonfiguration
Mer information om hur du installerar och konfigurerar Azure File Sync-agenten med Windows Server finns i [planera för distribution av Azure File Sync](storage-sync-files-planning.md) och [så här distribuerar du Azure File Sync](storage-sync-files-deployment-guide.md).

- Agentinstallationspaketet måste installeras med förhöjd behörighet (admin) behörigheter.
- Agenten stöds inte på distributionsalternativen för Windows Server Core eller Nano Server.
- Agenten stöds endast på Windows Server 2016 och Windows Server 2012 R2.
- Agenten kräver minst 2 GB fysiskt minne.
- Storage Sync-agenten (FileSyncSvc)-tjänsten stöder inte serverslutpunkter som finns på en volym med system volume information (SVI) directory komprimeras. Den här konfigurationen leder till oväntade resultat.

### <a name="interoperability"></a>Samverkan
- Program som antivirus, program för säkerhetskopiering och andra program som har åtkomst till nivåindelade filer kan orsaka oönskade återkallanden om de inte respekterar attributet offline och hoppar över att läsa innehållet i filerna. Mer information finns i [felsöka Azure File Sync](storage-sync-files-troubleshoot.md).
- Använd inte hanteraren för filserverresurser eller andra filgaller. Filgaller kan orsaka oändliga synkroniseringsfel om filer blockeras på grund av filgallret.
- Kör sysprep på en server som har Azure File Sync-agenten installerad stöds inte och kan leda till oväntade resultat. Agentregistreringen för installation och server ska inträffa efter distribution av server-avbildning och slutföra sysprep mini-installationen.
- Datadeduplicering och lagringsnivåer för moln stöds inte på samma volym.

### <a name="sync-limitations"></a>Synkroniseringsbegränsningar
Följande objekt synkroniseras inte, men resten av systemet fortsätter att fungera normalt:
- Sökvägar som är längre än 2 048 tecken.
- DACL-delen av en säkerhetsbeskrivning om den är större än 2 kB. (Det här är endast ett problem om du har mer än ca 40 åtkomstkontrollposter på ett enskilt objekt.)
- SACL-delen av en säkerhetsbeskrivning som används för granskning.
- Utökade attribut.
- Alternativa dataströmmar.
- Referenspunkter.
- Hårda länkar.
- Komprimering (om det angetts på en serverfil) bevaras inte när ändringar synkroniseras till filen från andra slutpunkter.
- Alla filer som krypterats med EFS (eller andra typer av kryptering från användarläget) som förhindrar att tjänsten läser data.

    > [!Note]  
    > Azure File Sync krypterar alltid data under överföring. Vilande data är alltid krypterade i Azure.
 
### <a name="server-endpoint"></a>Server-slutpunkt
- En serverslutpunkt kan endast skapas på en NTFS-volym. ReFS, FAT, FAT32 och andra filsystem stöds inte av Azure File Sync för närvarande.
- Nivåindelade filer blir oanvändbara om filerna inte återställas innan du tar bort Serverslutpunkten.
- Molnet lagringsnivåer stöds inte på systemvolymen. Inaktivera molnlagringsnivåer när du skapar Serverslutpunkten för att skapa en serverslutpunkt på systemvolymen.
- Redundansklustring stöds endast med klustrade diskar, inte med klusterdelade volymer (CSV).
- Serverslutpunkter får inte vara kapslade. De får dock finnas på samma volym parallellt med varandra.
- Lagra inte ett operativsystem eller ett programs växlingsfil i en serverslutpunkt.
- Servernamnet i portalen uppdateras inte om servern har bytt namn. Uppdatera servernamnet i portalen genom att avregistrera och registrera om servern.

### <a name="cloud-endpoint"></a>Molnslutpunkt
- Azure File Sync stöder gör ändringar i Azure-filresursen direkt. Ändringar som görs på Azure-filresursen måste dock först identifieras av ett jobb med Azure File Sync ändra identifiering. Ett jobb för identifiering av ändring initieras för en molnslutpunkt en gång per dygn. Dessutom kommer inte att uppdatera SMB tid för senaste ändring ändringar som gjorts i en Azure-filresurs via REST-protokollet och kan inte ses som en ändring av synkronisering.
- Storage sync-tjänsten och/eller storage-konto kan flyttas till en annan resursgrupp eller prenumeration inom de befintliga Azure AD-klienten. Om lagringskontot har flyttats, måste du ge Hybrid Filsynkroniseringstjänstens åtkomst till lagringskontot (se [se till att Azure File Sync har åtkomst till lagringskontot](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

    > [!Note]  
    > Azure File Sync stöder inte flytta prenumerationen till en annan Azure AD-klient.

### <a name="cloud-tiering"></a>Lagringsnivåer för moln
- Om en nivåindelad fil kopieras till en annan plats med Robocopy så kommer den kopierade filen inte att vara nivåindelad. Offline-attributet kan anges eftersom Robocopy felaktigt tar med det attributet i kopieringsåtgärder.
- När du visar filegenskaper på en SMB-klient kan det verka som att offline-attributet har konfigurerats felaktigt på grund av SMB-cachelagring av filens metadata.
