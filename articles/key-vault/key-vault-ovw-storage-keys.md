---
ms.assetid: ''
title: Azure Key Vault-Lagringskontonycklar
description: Lagringskontonycklar ge en seemless integrering mellan Azure Key Vault och viktiga baserat åtkomst till Azure Storage-konto.
ms.topic: conceptual
services: key-vault
ms.service: key-vault
author: bryanla
ms.author: bryanla
manager: mbaldwin
ms.date: 10/03/2018
ms.openlocfilehash: 02fffe7c4a3acff6ce6d68046eee4286003b1766
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50232230"
---
# <a name="azure-key-vault-storage-account-keys"></a>Azure Key Vault-Lagringskontonycklar

> [!NOTE]
> [Azure storage stöder nu AAD auktorisering](https://docs.microsoft.com/azure/storage/common/storage-auth-aad). Vi rekommenderar att du använder Azure Active Directory för autentisering och auktorisering till lagring som användarna inte behöver bekymra sig om roterar sina nycklar för Lagringskonto.

- Azure Key Vault hanterar nycklar för ett Azure Storage konto (ASA).
    - Azure Key Vault kan internt lista nycklar (sync) med Azure Storage-kontot.    
    - Genererar om Azure Key Vault (roterar) nycklarna regelbundet.
    - Nyckelvärden returneras aldrig i svaret till anroparen.
    - Azure Key Vault hanterar nycklarna för både Storage-konton och klassiska Lagringskonton.

<a name="prerequisites"></a>Förutsättningar
--------------
1. [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) installera Azure CLI   
2. [Skapa ett Lagringskonto](https://azure.microsoft.com/services/storage/)
    - Följ stegen i den här [dokumentet](https://docs.microsoft.com/azure/storage/) att skapa ett lagringskonto  
    - **Riktlinjer för namngivning:** lagringskontonamn måste vara mellan 3 och 24 tecken långt och får innehålla siffror och gemener.        
      
<a name="step-by-step-instructions"></a>Steg för steg-instruktioner
-------------------------
I den nedan information vi tilldela Key Vault som en tjänst har operatorn behörigheter för ditt storage-konto

1. När du har skapat ett lagringskonto som kör följande kommando för att hämta resurs-ID för lagringskontot som vill du hantera

    ```
    az storage account show -n storageaccountname (Copy ID out of the result of this command)
    ```
    
2. Hämta program-ID för Azure Key Vault-tjänstens huvudnamn 

    ```
    az ad sp show --id cfa8b339-82a2-471a-a3c9-0fc0be7a4093
    ```
    
3. Tilldela lagring nyckeln operatörsrollen till Azure Key Vault Identity

    ```
    az role assignment create --role "Storage Account Key Operator Service Role"  --assignee-object-id hhjkh --scope idofthestorageaccount
    ```
    
4. Skapa Key Vault hanteras Storage-konto.     <br /><br />
   Nedan har anger vi en återskapandeperiod 90 dagar. Efter 90 dagar, Key Vault återskapa ”key1” och växla den aktiva nyckeln från ”key2” till ”key1”.
   
    ```
    az keyvault storage add --vault-name <YourVaultName> -n <StorageAccountName> --active-key-name key2 --auto-regenerate-key --regeneration-period P90D --resource-id <Resource-id-of-storage-account>
    ```
    Om användaren inte har skapat lagringskontot och inte har behörighet att storage-konto, ange behörigheter för ditt konto så att du kan hantera alla behörigheter som lagring i Key Vault i stegen nedan.
 > [!NOTE] 
    I det fallet att användaren inte har behörighet att storage-konto kan hämta vi först objekt-Id för användaren

    ```
    az ad user show --upn-or-object-id "developer@contoso.com"

    az keyvault set-policy --name <YourVaultName> --object-id <ObjectId> --storage-permissions backup delete list regeneratekey recover     purge restore set setsas update
    ```

### <a name="relevant-powershell-cmdlets"></a>Relevanta Powershell-cmdletar

- [Get-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.keyvault/get-azurekeyvaultmanagedstorageaccount)
- [Add-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Add-AzureKeyVaultManagedStorageAccount)
- [Get-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Get-AzureKeyVaultManagedStorageSasDefinition)
- [Update-AzureKeyVaultManagedStorageAccountKey](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Update-AzureKeyVaultManagedStorageAccountKey)
- [Remove-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.keyvault/remove-azurekeyvaultmanagedstorageaccount)
- [Remove-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Remove-AzureKeyVaultManagedStorageSasDefinition)
- [Set-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Set-AzureKeyVaultManagedStorageSasDefinition)

## <a name="see-also"></a>Se också

- [Om nycklar, hemligheter och certifikat](https://docs.microsoft.com/rest/api/keyvault/)
- [Key Vault-teamets blogg](https://blogs.technet.microsoft.com/kv/)
