---
title: Begränsa nätverksåtkomst till PaaS-resurser – Azure PowerShell | Microsoft Docs
description: I den här artikeln lär du dig att begränsa nätverksåtkomst till Azure-resurser, till exempel Azure Storage och Azure SQL Database med virtuella nätverksslutpunkter med Azure PowerShell.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want only resources in a virtual network subnet to access an Azure PaaS resource, such as an Azure Storage account.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/14/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: b3977e045751165947243c67291e81b998b5fcb5
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38606122"
---
# <a name="restrict-network-access-to-paas-resources-with-virtual-network-service-endpoints-using-powershell"></a>Begränsa nätverksåtkomst till PaaS-resurser med virtuella nätverksslutpunkter med hjälp av PowerShell

Med virtuella nätverksslutpunkter kan du begränsa nätverksåtkomsten till vissa Azure-tjänsters resurser till ett undernät för virtuella datorer. Du kan också ta bort resursernas internetåtkomst. Tjänstslutpunkterna möjliggör direktanslutning från ditt virtuella nätverk till Azure-tjänster som stöds, så att du kan använda det privata adressutrymmet i det virtuella nätverket för åtkomst till Azure-tjänsterna. Trafik till Azure-resurser genom tjänstslutpunkterna finns alltid kvar i Microsoft Azure-stamnätverket. I den här artikeln kan du se hur du:

* Skapa ett virtuellt nätverk med ett undernät
* Lägga till ett undernät och aktivera en tjänstslutpunkt
* Skapa en Azure-resurs och tillåt nätverksåtkomst till den från enbart ett undernät
* Distribuera en virtuell dator (VM) till varje undernät
* Bekräfta åtkomst till en resurs från ett undernät
* Bekräfta att åtkomst nekas för en resurs från ett undernät och internet

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Om du väljer att installera och använda PowerShell lokalt kräver den här artikeln version 5.4.1 eller senare av Azure PowerShell-modulen. Kör ` Get-Module -ListAvailable AzureRM` för att hitta den installerade versionen. Om du behöver uppgradera kan du läsa [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps) (Installera Azure PowerShell-modul). Om du kör PowerShell lokalt måste du också köra `Connect-AzureRmAccount` för att skapa en anslutning till Azure.

## <a name="create-a-virtual-network"></a>Skapa ett virtuellt nätverk

Du måste skapa en resursgrupp för det virtuella nätverket och alla andra resurser som skapats i den här artikeln innan du skapar ett virtuellt nätverk. Skapa en resursgrupp med [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). I följande exempel skapas en resursgrupp med namnet *myResourceGroup*: 

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

Skapa ett virtuellt nätverk med [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). I följande exempel skapas ett virtuellt nätverk med namnet *myVirtualNetwork* med adressprefixet *10.0.0.0/16*.

```azurepowershell-interactive
$virtualNetwork = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

Skapa en undernätskonfiguration med [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). I följande exempel skapas en undernätskonfiguration för ett undernät med namnet *offentliga*:

```azurepowershell-interactive
$subnetConfigPublic = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Public `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork
```

Skapa undernätet i det virtuella nätverket genom att skriva Undernätskonfigurationen till det virtuella nätverket med [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork):

```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="enable-a-service-endpoint"></a>Aktivera en tjänstslutpunkt 

Du kan aktivera Tjänsteslutpunkter bara för tjänster som stöder Tjänsteslutpunkter. Visa tjänstens slutpunkt-aktiverade tjänster som är tillgängliga i en Azure-plats med [Get-AzureRmVirtualNetworkAvailableEndpointService](/powershell/module/azurerm.network/get-azurermvirtualnetworkavailableendpointservice). I följande exempel returneras en lista över service-slutpunkt-aktiverade tjänster som är tillgängliga i den *eastus* region. I listan över tjänster som returneras kommer att växa med tiden när flera Azure-tjänster blir tjänstslutpunkt aktiverad.

```azurepowershell-interactive
Get-AzureRmVirtualNetworkAvailableEndpointService -Location eastus | Select Name
``` 

Skapa en ytterligare undernät i det virtuella nätverket. I det här exemplet, ett undernät med namnet *privata* skapas med en tjänstslutpunkt för *Microsoft.Storage*: 

```azurepowershell-interactive
$subnetConfigPrivate = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Private `
  -AddressPrefix 10.0.1.0/24 `
  -VirtualNetwork $virtualNetwork `
  -ServiceEndpoint Microsoft.Storage

$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="restrict-network-access-for-a-subnet"></a>Begränsa nätverksåtkomst för ett undernät

Skapa nätverkssäkerhet grupp säkerhetsregler med [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig). Följande regel tillåter utgående åtkomst till de offentliga IP-adresser tilldelade till Azure Storage-tjänsten: 

```azurepowershell-interactive
$rule1 = New-AzureRmNetworkSecurityRuleConfig `
  -Name Allow-Storage-All `
  -Access Allow `
  -DestinationAddressPrefix Storage `
  -DestinationPortRange * `
  -Direction Outbound `
  -Priority 100 `
  -Protocol * `
  -SourceAddressPrefix VirtualNetwork `
  -SourcePortRange *
```

Följande regel nekar åtkomst till alla offentliga IP-adresser. Den föregående regeln åsidosätter den här regeln på grund av dess högre prioritet, vilket ger åtkomst till de offentliga IP-adresserna i Azure Storage.

```azurepowershell-interactive
$rule2 = New-AzureRmNetworkSecurityRuleConfig `
  -Name Deny-Internet-All `
  -Access Deny `
  -DestinationAddressPrefix Internet `
  -DestinationPortRange * `
  -Direction Outbound `
  -Priority 110 `
  -Protocol * `
  -SourceAddressPrefix VirtualNetwork `
  -SourcePortRange *
```

Följande regel tillåter Remote Desktop Protocol (RDP) trafik inkommande i undernätet från var som helst. Fjärrskrivbordsanslutningar tillåts i undernätet, så att du kan bekräfta nätverksåtkomst till en resurs i ett senare steg.

```azurepowershell-interactive
$rule3 = New-AzureRmNetworkSecurityRuleConfig `
  -Name Allow-RDP-All `
  -Access Allow `
  -DestinationAddressPrefix VirtualNetwork `
  -DestinationPortRange 3389 `
  -Direction Inbound `
  -Priority 120 `
  -Protocol * `
  -SourceAddressPrefix * `
  -SourcePortRange *
```

Skapa en nätverkssäkerhetsgrupp med [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup). I följande exempel skapas en nätverkssäkerhetsgrupp med namnet *myNsgPrivate*.

```azurepowershell-interactive
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myNsgPrivate `
  -SecurityRules $rule1,$rule2,$rule3
```

Associera nätverkssäkerhetsgruppen till den *privata* undernätet med [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) och skriv sedan Undernätskonfigurationen till det virtuella nätverket. I följande exempel kopplar den *myNsgPrivate* nätverkssäkerhetsgrupp till det *privata* undernät:

```azurepowershell-interactive
Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $VirtualNetwork `
  -Name Private `
  -AddressPrefix 10.0.1.0/24 `
  -ServiceEndpoint Microsoft.Storage `
  -NetworkSecurityGroup $nsg

$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="restrict-network-access-to-a-resource"></a>Begränsa nätverksåtkomst till en resurs

De steg som behövs för att begränsa nätverksåtkomsten till resurser som har skapats via Azure-tjänster som är aktiverade för tjänstslutpunkter varierar från tjänst till tjänst. Läs dokumentationen för enskilda tjänster för specifika åtgärder för varje tjänst. Resten av den här artikeln innehåller steg för att begränsa nätverksåtkomsten för ett Azure Storage-konto, som exempel.

### <a name="create-a-storage-account"></a>skapar ett lagringskonto

Skapa ett Azure storage-konto med [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount). Ersätt `<replace-with-your-unique-storage-account-name>` med ett namn som är unikt i alla Azure-platser, mellan 3 och 24 tecken långt, med hjälp av endast siffror och gemener.

```azurepowershell-interactive
$storageAcctName = '<replace-with-your-unique-storage-account-name>'

New-AzureRmStorageAccount `
  -Location EastUS `
  -Name $storageAcctName `
  -ResourceGroupName myResourceGroup `
  -SkuName Standard_LRS `
  -Kind StorageV2
```

När lagringskontot har skapats kan du hämta nyckeln för lagringskontot i en variabel med [Get-AzureRmStorageAccountKey](/powershell/module/azurerm.storage/get-azurermstorageaccountkey):

```azurepowershell-interactive
$storageAcctKey = (Get-AzureRmStorageAccountKey `
  -ResourceGroupName myResourceGroup `
  -AccountName $storageAcctName).Value[0]
```

Nyckeln används för att skapa en filresurs i ett senare steg. Ange `$storageAcctKey` och anteckna värdet för, eftersom du måste också ange den manuellt i ett senare steg när du mappar filresursen till en enhet i en virtuell dator.

### <a name="create-a-file-share-in-the-storage-account"></a>Skapa en filresurs i lagringskontot

Skapa en kontext för ditt lagringskonto och nyckel med [New-AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext). Kontexten innehåller lagringskontots namn och åtkomstnyckel:

```azurepowershell-interactive
$storageContext = New-AzureStorageContext $storageAcctName $storageAcctKey
```

Skapa en filresurs med [New-AzureStorageShare](/powershell/module/azure.storage/new-azurestorageshare):

$share = New-AzureStorageShare my-file-share - kontexten $storageContext

### <a name="deny-all-network-access-to-a-storage-account"></a>Neka alla åtkomst till ett lagringskonto

Som standard godkänner lagringskonton nätverksanslutningar från klienter i alla nätverk. För att begränsa åtkomsten till valda nätverk, ändrar du åtgärden du *neka* med [uppdatering AzureRmStorageAccountNetworkRuleSet](/powershell/module/azurerm.storage/update-azurermstorageaccountnetworkruleset). Lagringskontot är inte tillgänglig från alla nätverk när nätverksåtkomst nekas.

```azurepowershell-interactive
Update-AzureRmStorageAccountNetworkRuleSet  `
  -ResourceGroupName "myresourcegroup" `
  -Name $storageAcctName `
  -DefaultAction Deny
```

### <a name="enable-network-access-from-a-subnet"></a>Aktivera nätverksåtkomst från ett undernät

Hämta det skapade virtuellt nätverket med [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork) och sedan hämta privata Undernätsobjektet till en variabel med [Get-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig):

```azurepowershell-interactive
$privateSubnet = Get-AzureRmVirtualNetwork `
  -ResourceGroupName "myResourceGroup" `
  -Name "myVirtualNetwork" `
  | Get-AzureRmVirtualNetworkSubnetConfig `
  -Name "Private"
```

Tillåt nätverksåtkomst till storage-konto från den *privata* undernätet med [Lägg till AzureRmStorageAccountNetworkRule](/powershell/module/azurerm.network/add-azurermnetworksecurityruleconfig).

```azurepowershell-interactive
Add-AzureRmStorageAccountNetworkRule `
  -ResourceGroupName "myresourcegroup" `
  -Name $storageAcctName `
  -VirtualNetworkResourceId $privateSubnet.Id
```

## <a name="create-virtual-machines"></a>Skapa virtuella datorer

Om du vill testa nätverksåtkomsten till ett lagringskonto distribuerar du en virtuell dator till varje undernät.

### <a name="create-the-first-virtual-machine"></a>Skapa din första virtuella dator

Skapa en virtuell dator i den *offentliga* undernätet med [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). När du kör kommandot nedan uppmanas du att ange autentiseringsuppgifter. De värden som du anger konfigureras som användarnamn och lösenord för den virtuella datorn. Alternativet `-AsJob` skapar den virtuella datorn i bakgrunden, så att du kan fortsätta till nästa steg.

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Location "East US" `
    -VirtualNetworkName "myVirtualNetwork" `
    -SubnetName "Public" `
    -Name "myVmPublic" `
    -AsJob
```

Utdata som liknar följande Exempelutdata returneras:

```powershell
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command                  
--     ----            -------------   -----         -----------     --------             -------                  
1      Long Running... AzureLongRun... Running       True            localhost            New-AzureRmVM     
```

### <a name="create-the-second-virtual-machine"></a>Skapa den andra virtuella datorn

Skapa en virtuell dator i den *privata* undernät:

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Location "East US" `
    -VirtualNetworkName "myVirtualNetwork" `
    -SubnetName "Private" `
    -Name "myVmPrivate"
```

Det tar några minuter för Azure för att skapa den virtuella datorn. Fortsätt inte till nästa steg förrän Azure har skapat den virtuella datorn och returnerar utdata till PowerShell. 

## <a name="confirm-access-to-storage-account"></a>Bekräfta åtkomst till lagringskontot

Använd [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) för att returnera den offentliga IP-adressen för en virtuell dator. I följande exempel returneras den offentliga IP-adressen för den *myVmPrivate* VM:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmPrivate `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Ersätt `<publicIpAddress>` i följande kommando med den offentliga IP-adressen som returnerades från föregående kommando och ange sedan följande kommando: 

```powershell
mstsc /v:<publicIpAddress>
```

En RDP-fil (Remote Desktop Protocol) skapas och laddas ned till datorn. Öppna den nedladdade RDP-filen. Välj **Anslut** om du uppmanas att göra det. Ange användarnamnet och lösenordet du angav när du skapade den virtuella datorn. Du kan behöva välja **Fler alternativ** och sedan **Använd ett annat konto** för att ange autentiseringsuppgifterna du angav när du skapade den virtuella datorn. Välj **OK**. Du kan få en certifikatvarning under inloggningen. Om du ser varningen väljer du **Ja** eller **Fortsätt** för att fortsätta med anslutningen.

På den virtuella datorn *myVmPrivate* mappar du Azure-fildelningen till enhet Z med PowerShell. Ersätt innan du kör kommandona som följer `<storage-account-key>` och `<storage-account-name>` med värden från du angav och hämtade i [skapa ett lagringskonto](#create-a-storage-account).

```powershell
$acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
New-PSDrive -Name Z -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.windows.net\my-file-share" -Credential $credential
```
PowerShell returnerar utdata som liknar följande exempelutdata:

```powershell
Name           Used (GB)     Free (GB) Provider      Root
----           ---------     --------- --------      ----
Z                                      FileSystem    \\vnt.file.core.windows.net\my-f...
```

Azure-fildelningen har mappats till enhet Z.

Bekräfta att den virtuella datorn inte har någon utgående anslutning till alla andra offentliga IP-adresser:

```powershell
ping bing.com
```

Du får inga svar eftersom nätverkssäkerhetsgruppen som är kopplad till det *privata* undernätet inte tillåter utgående åtkomst till offentliga IP-adresser förutom de adresser som är kopplade till Azure Storage-tjänsten.

Stäng fjärrskrivbordssessionen för den virtuella datorn *myVmPrivate*.

## <a name="confirm-access-is-denied-to-storage-account"></a>Bekräfta att åtkomst till lagringskontot nekas

Hämta den offentliga IP-adressen för den *myVmPublic* VM:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmPublic `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Ersätt `<publicIpAddress>` i följande kommando med den offentliga IP-adressen som returnerades från föregående kommando och ange sedan följande kommando: 

```powershell
mstsc /v:<publicIpAddress>
```

På den *myVmPublic* virtuell dator, försöker mappa Azure-filresursen till enhet Z. Ersätt innan du kör kommandona som följer `<storage-account-key>` och `<storage-account-name>` med värden från du angav och hämtade i [skapa ett lagringskonto](#create-a-storage-account).

```powershell
$acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
New-PSDrive -Name Z -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.windows.net\my-file-share" -Credential $credential
```

Nekad åtkomst till resursen, och du får en `New-PSDrive : Access is denied` fel. Åtkomst nekas eftersom den virtuella datorn *myVmPublic* distribueras i det *offentliga* undernätet. Det *offentliga* undernätet har ingen tjänstslutpunkt aktiverad för Azure Storage, och lagringskontot tillåter endast nätverksåtkomst från det *privata* undernätet, inte det *offentliga*.

Stäng fjärrskrivbordssessionen för den virtuella datorn *myVmPublic*.

Försök att visa filresurserna i lagringskontot med följande kommando från din dator:

```powershell-interactive
Get-AzureStorageFile `
  -ShareName my-file-share `
  -Context $storageContext
```

Åtkomst nekas och du får en *Get-AzureStorageFile: fjärrservern returnerade ett fel: (403) förbjuden. HTTP-statuskod: 403 - HTTP-felmeddelande: den här begäran har inte behörighet att utföra den här åtgärden* fel, eftersom datorn inte är i den *privata* undernät för den *MyVirtualNetwork* virtuellt nätverk.

## <a name="clean-up-resources"></a>Rensa resurser

Du kan använda [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) för att ta bort resursgruppen och alla resurser den innehåller när de inte längre behövs:

```azurepowershell-interactive 
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Nästa steg

I den här artikeln har aktiverat du en tjänstslutpunkt för ett virtuellt nätverksundernät. Du har lärt dig att tjänstslutpunkter kan aktiveras för resurser som distribueras med flera Azure-tjänster. Du har skapat ett Azure Storage-konto och begränsat nätverksåtkomst för lagringskontot till enbart resurser inom ett undernät för ett virtuellt nätverk. Om du vill veta mer om tjänstslutpunkter går du till [Översikt över tjänstslutpunkter](virtual-network-service-endpoints-overview.md) och [Hantera undernät](virtual-network-manage-subnet.md).

Om du har flera virtuella nätverk i ditt konto kanske du vill ansluta två virtuella nätverk så att resurserna i vart och ett kan kommunicera med varandra. Läs hur genom att läsa [ansluta virtuella nätverk](tutorial-connect-virtual-networks-powershell.md).
