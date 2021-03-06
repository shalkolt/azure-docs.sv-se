---
title: Hämta marketplace-objekt från Azure | Microsoft Docs
description: Operatorn molnet kan hämta marketplace-objekt från Azure till min Azure Stack-distribution.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/19/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: b5c2c51429e37eea2473ae5966b1f41295875cb6
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/22/2018
ms.locfileid: "49638179"
---
# <a name="download-marketplace-items-from-azure-to-azure-stack"></a>Hämta marketplace-objekt från Azure till Azure Stack

*Gäller för: integrerade Azure Stack-system och Azure Stack Development Kit*

Du kan hämta objekt från Azure Marketplace och göra dem tillgängliga i Azure Stack som en cloud-operator. Det är de objekt som du kan välja från en granskad lista över Azure Marketplace-objekt som testats och har stöd för att fungera med Azure Stack. Ytterligare objekt är ofta läggs till i listan kan fortsätta så Håll utkik efter nytt innehåll. 

Det finns två scenarier för att ansluta till Azure Marketplace: 

- **Ett scenario med anslutna** -som kräver Azure Stack-miljön vara ansluten till internet. Du kan använda Azure Stack-portalen för att leta upp och hämta objekt. 
- **Ett scenario med frånkopplade eller anslutna delvis** -som kräver att du ansluter till Internet med hjälp av verktyget för marketplace-syndikering att ladda ned marketplace-objekt. Sedan kan överföra du dina nedladdningar till den frånkopplade Azure Stack-installationen. Det här scenariot används PowerShell.

Se [Azure Marketplace-objekt för Azure Stack](azure-stack-marketplace-azure-items.md) en lista över marketplace-objekt som du kan ladda ned.


## <a name="connected-scenario"></a>Scenariot
Om Azure Stack ansluter till internet kan använda du admin portal för att ladda ned marketplace-objekt.

### <a name="prerequisites"></a>Förutsättningar
Azure Stack-distributioner måste ha Internetanslutning och vara [registrerad med Azure](azure-stack-register.md).

### <a name="use-the-portal-to-download-marketplace-items"></a>Använda portalen för att hämta marketplace-objekt  
1. Logga in på Azure Stack-administratörsportalen.

2.  Granska det tillgängliga lagringsutrymmet innan du laddar ned marketplace-objekt. Senare, när du väljer objekt för nedladdning, kan du jämföra hämtningsstorleken med tillgängliga lagringskapaciteten. Om kapaciteten är begränsad, Överväg alternativ för [hantera tillgängligt utrymme](azure-stack-manage-storage-shares.md#manage-available-space). 

    Att granska tillgängligt utrymme i **regionshantering** väljer du den region som du vill utforska och gå sedan till **Resursprovidrar** > **Storage**.

    [ ![Granska lagringsutrymme](media/azure-stack-download-azure-marketplace-item/storagesm.png "granska lagringsutrymme") ](media/azure-stack-download-azure-marketplace-item/storage.png#lightbox)

    
3. Öppna Azure Stack Marketplace och Anslut till Azure. Om du vill göra det, Välj **Marketplace management**, och välj sedan **Lägg till från Azure**.

    [ ![Lägg till från Azure](media/azure-stack-download-azure-marketplace-item/marketplacesm.png "Lägg till från Azure") ](media/azure-stack-download-azure-marketplace-item/marketplace.png#lightbox)

    Portalen visar en lista över tillgängliga för nedladdning från Azure Marketplace. Du kan klicka på varje objekt för att visa dess beskrivning och ytterligare information om det, inklusive dess hämtningsstorlek. 

    [ ![Marketplace-lista](media/azure-stack-download-azure-marketplace-item/image03sm.png "Marketplace-lista") ](media/azure-stack-download-azure-marketplace-item/image03.png#lightbox)

4. Det objekt du vill och välj sedan **hämta**. Filhämtningar variera.

    [ ![Hämta meddelande](media/azure-stack-download-azure-marketplace-item/image04.png "hämta meddelande") ](media/azure-stack-download-azure-marketplace-item/image04.png#lightbox)

    När nedladdningen är klar kan du distribuera det nya marketplace-objektet som ett Azure Stack-operatör eller användare.

5. Om du vill distribuera hämtade objektet, Välj **+ skapa en resurs**, och sedan söka bland olika kategorier av nya marknadsplats-objektet. Sedan väljer du objektet för att starta distributionsprocessen. Processen varierar för olika marketplace-objekt. 

## <a name="disconnected-or-a-partially-connected-scenario"></a>Frånkopplad eller en delvis ansluten scenario

Om Azure Stack är i frånkopplat läge och utan Internetanslutning kan du använda PowerShell och *marketplace syndikering verktyget* att ladda ned marketplace-objekt till en dator med Internetanslutning. Du kan sedan överföra objekt till Azure Stack-miljön. I en frånkopplad miljö kan du ladda ned marketplace-objekt med hjälp av Azure Stack-portalen. 

Marketplace-syndikering verktyget kan också användas i ett scenario med anslutna. 

Det finns två delar i det här scenariot:
- **Del 1:** ladda ned från Azure Marketplace. På datorn med Internetåtkomst konfigurera PowerShell, ladda ned verktyget syndikering och sedan hämta objekt formuläret Azure Marketplace.  
- **Del 2:** överföring och publicering på Azure Stack Marketplace. Du flyttar de filer som du hämtade till Azure Stack-miljön, importera dem till Azure Stack och sedan publicera dem på Azure Stack Marketplace.  


### <a name="prerequisites"></a>Förutsättningar
- Azure Stack-distributioner måste vara [registrerad med Azure](azure-stack-register.md).  

- Den dator som är ansluten till internet måste ha **Azure Stack PowerShell-modulen version 1.2.11** eller högre. Om den inte redan finns [installera PowerShell-moduler med Azure Stack specifika](azure-stack-powershell-install.md).  

- Att aktivera import av ett hämtade marketplace-objekt i [PowerShell-miljö för Azure Stack-operatör](azure-stack-powershell-configure-admin.md) måste konfigureras.  

- Du måste ha en [lagringskonto](azure-stack-manage-storage-accounts.md) i Azure Stack som har en offentligt tillgänglig behållare (vilket är en lagringsblob). Du använder behållare som temporär lagring för marketplace objekt galleriet filer. Om du inte är bekant med lagringskonton och behållare finns i [arbeta med blobar – Azure-portalen](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) i Azure-dokumentationen.

- Verktyget för marketplace-syndikering hämtas vid den första proceduren. 

### <a name="use-the-marketplace-syndication-tool-to-download-marketplace-items"></a>Använd verktyget för marketplace-syndikering för att hämta marketplace-objekt

1. Öppna en PowerShell-konsol som administratör på en dator med en Internetanslutning.

2. Lägg till Azure-konto som du använde för att registrera Azure Stack. Att lägga till kontot i PowerShell kör `Add-AzureRmAccount` utan några parametrar. Du uppmanas att ange dina autentiseringsuppgifter för Azure-konto och du kan behöva använda 2-faktor autentisering baserat på konfigurationen för ditt konto.

3. Om du har flera prenumerationer kör du följande kommando för att välja den du har använt för registrering:  

   ```PowerShell  
   Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   $AzureContext = Get-AzureRmContext
   ```

4. Ladda ned den senaste versionen av verktyget för marketplace-syndikering genom att använda följande skript:  

   ```PowerShell
   # Download the tools archive.
   [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12 
   invoke-webrequest https://github.com/Azure/AzureStack-Tools/archive/master.zip `
     -OutFile master.zip

   # Expand the downloaded files.
   expand-archive master.zip `
     -DestinationPath . `
     -Force

   # Change to the tools directory.
   cd .\AzureStack-Tools-master

   ```

5. Importera modulen syndikering och starta verktyget genom att köra följande kommandon. Ersätt `Destination folder path` med en plats att lagra de filer som hämtas från Azure Marketplace.   

   ```PowerShell  
   Import-Module .\Syndication\AzureStack.MarketplaceSyndication.psm1

   Sync-AzSOfflineMarketplaceItem 
      -Destination "Destination folder path in quotes" `
      -AzureTenantID $AzureContext.Tenant.TenantId ` 
      -AzureSubscriptionId $AzureContext.Subscription.Id 
   ```

6. När verktyget körs, bör du se en skärm som liknar följande bild, med listan över tillgängliga marketplace-objekt:

   [ ![Azure Marketplace objekt popup](media/azure-stack-download-azure-marketplace-item/image05.png "Azure Marketplace-objekt") ](media/azure-stack-download-azure-marketplace-item/image05.png#lightbox)

7. Välj ett objekt som du vill ladda ned och anteckna den *version*. Du kan hålla den *Ctrl* för att markera flera avbildningar. Du refererar till den *version* när du importerar objekt i nästa procedur. 
   
   Du kan också filtrera listan över avbildningar med hjälp av den **lägga till villkor** alternativet.

8. Välj **OK**, och sedan granska och Godkänn de juridiska villkoren. 

9. Den tid som nedladdningen tar beror på storleken på objektet. När nedladdningen är klar är artikeln tillgänglig i den mapp som du angav i skriptet. Nedladdningen innehåller en VHD-fil (för virtuella datorer) eller en .zip-fil (för tillägg till virtuella datorer). Det kan även innehålla ett gallery-paket i den *.azpkg* format, vilket är bara en .zip-fil.

### <a name="import-the-download-and-publish-to-azure-stack-marketplace"></a>Importera nedladdningen och publicera Azure Stack Marketplace

1. Filer för avbildningar av virtuella datorer eller mallar för lösningar som du har [tidigare hämtade](#use-the-marketplace-syndication-tool-to-download-marketplace-items) måste vara tillgängliga lokalt till Azure Stack-miljön.  

2. Du kan använda administrationsportalen för att överföra marketplace objekt paketet (.azpkg-fil) och den virtuella hårddiskavbildningen (VHD-fil) till Azure Stack Blob storage. Ladda upp av paketet och diskfiler gör dem tillgängliga för Azure Stack så att du kan senare publicera objektet i Azure Stack Marketplace.

   Ladda upp måste du ha ett lagringskonto med en offentligt tillgänglig behållare (se kraven för det här scenariot).  
   1. I Azure Stack-administratörsportalen, går du till **alla tjänster** och sedan under den **DATA + lagring** kategori, väljer **lagringskonton**.  
   
   2. Välj ett lagringskonto från din prenumeration och sedan under **BLOBTJÄNSTEN**väljer **behållare**.  
      [ ![BLOB-tjänst](media/azure-stack-download-azure-marketplace-item/blob-service.png "Blob service") ](media/azure-stack-download-azure-marketplace-item/blob-service.png#lightbox)  
   
   3. Markera den behållare som du vill använda och välj sedan **överför** att öppna den **ladda upp blob** fönstret.  
      [ ![Behållaren](media/azure-stack-download-azure-marketplace-item/container.png "behållare") ](media/azure-stack-download-azure-marketplace-item/container.png#lightbox)  
   
   4. Bläddra till paketet och disk-filer att läsa in till lagring och välj sedan i fönstret ladda upp blob **överför**: [ ![överför](media/azure-stack-download-azure-marketplace-item/uploadsm.png "ladda upp") ](media/azure-stack-download-azure-marketplace-item/upload.png#lightbox)  

   5. Filer som du överför visas i fönstret behållare. Välj en fil och kopiera Webbadressen från den **Blobegenskaper** fönstret. Du ska använda den här URL: en i nästa steg när du importerar marketplace-objekt till Azure Stack.  I följande bild, behållaren är *test blobblagring* och filen *Microsoft.WindowsServer2016DatacenterServerCore ARM.1.0.801.azpkg*.  Filen URL: en är *https://testblobstorage1.blob.local.azurestack.external/blob-test-storage/Microsoft.WindowsServer2016DatacenterServerCore-ARM.1.0.801.azpkg*.  
      [ ![Blobegenskaper](media/azure-stack-download-azure-marketplace-item/blob-storagesm.png "Blobegenskaper") ](media/azure-stack-download-azure-marketplace-item/blob-storage.png#lightbox)  

3. Importera VHD-avbildning till Azure Stack med hjälp av den **Lägg till AzsPlatformimage** cmdlet. När du använder den här cmdleten ersätter den *publisher*, *erbjuder*, och andra parametervärden med värdena för den avbildning som du importerar. 

   Du kan hämta den *publisher*, *erbjuder*, och *sku* värdena för avbildningen från textfilen som hämtar filen AZPKG. Filen lagras på målplatsen. Den *version* värde är den version som anges när du laddar ned objektet från Azure i föregående procedur. 
 
   I följande exempelskript används värdena för den Windows Server 2016 Datacenter - Server Core-VM. Värdet för *- Osuri* är ett exempel på sökväg till blob-lagringsplats för objektet.

   ```PowerShell  
   Add-AzsPlatformimage `
    -publisher "MicrosoftWindowsServer" `
    -offer "WindowsServer" `
    -sku "2016-Datacenter-Server-Core" `
    -osType Windows `
    -Version "2016.127.20171215" `
    -OsUri "https://mystorageaccount.blob.local.azurestack.external/cont1/Microsoft.WindowsServer2016DatacenterServerCore-ARM.1.0.801.vhd"  
   ```
   **Om lösningsmallar:** vissa mallar kan innehålla en liten 3 MB. VHD-fil med namnet **fixed3.vhd**. Du behöver inte importera den till Azure Stack. Fixed3.VHD.  Den här filen som ingår i vissa lösningsmallar att uppfylla publishing krav för Azure Marketplace.

   Granska beskrivningen mallar och hämta och importera sedan ytterligare krav som virtuella hårddiskar som krävs för att arbeta med lösningsmallen.  
   
   **Om tillägg:** när du arbetar med tillägg till virtuella datorer kan använda följande parametrar:
   - *Utgivare*
   - *Typ*
   - *Version*  

   Du använder inte *erbjuder* för tillägg.   


4.  Använd PowerShell för att publicera Azure Stack marketplace-objekt med hjälp av den **Lägg till AzsGalleryItem** cmdlet. Exempel:  
    ```PowerShell  
    Add-AzsGalleryItem `
     -GalleryItemUri "https://mystorageaccount.blob.local.azurestack.external/cont1/Microsoft.WindowsServer2016DatacenterServerCore-ARM.1.0.801.azpkg" `
     –Verbose
    ```
5. När du har publicerat ett galleriobjekt är det nu tillgänglig för användning. För att bekräfta att galleriobjektet har publicerats, gå till **alla tjänster**, och sedan under den **Allmänt** kategori, väljer **Marketplace**.  Om din nedladdning är en lösningsmall, kontrollera att du lägger till alla beroende VHD-avbildning för den lösningsmallen.  
  [ ![Visa marketplace](media/azure-stack-download-azure-marketplace-item/view-marketplacesm.png "visa marketplace") ](media/azure-stack-download-azure-marketplace-item/view-marketplace.png#lightbox)  

Du kan nu lägga till tillägg för virtuell dator med versionen av Azure Stack PowerShell 1.3.0. Exempel:

````PowerShell
Add-AzsVMExtension -Publisher "Microsoft" -Type "MicroExtension" -Version "0.1.0" -ComputeRole "IaaS" -SourceBlob "https://github.com/Microsoft/PowerShell-DSC-for-Linux/archive/v1.1.1-294.zip" -SupportMultipleExtensions -VmOsType "Linux"
````

## <a name="next-steps"></a>Nästa steg

[Skapa och publicera ett Marketplace-objekt](azure-stack-create-and-publish-marketplace-item.md)