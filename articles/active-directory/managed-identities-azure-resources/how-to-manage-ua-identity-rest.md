---
title: Så här hanterar du Azure användartilldelade hanterade identiteter med hjälp av REST
description: Steg för steg-anvisningar om hur du skapar, lista och ta bort en Användartilldelad hanterad identitet om du vill göra REST API-anrop.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/26/2018
ms.author: daveba
ms.openlocfilehash: dc7abd4bdec30ae870ff6add33d4b9b1c08b5bbd
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44159647"
---
# <a name="create-list-or-delete-a-user-assigned-managed-identity-using-rest-api-calls"></a>Skapa, visa eller ta bort en Användartilldelad hanterad identitet med hjälp av REST API-anrop

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Hanterade identiteter för Azure-resurser som erbjuder Azure-tjänster för autentisering till tjänster som stöder Azure AD-autentisering utan autentiseringsuppgifter i din kod. 

I den här artikeln får du lära dig hur du skapar, lista och ta bort en Användartilldelad hanterad identitet med CURL för att göra REST API-anrop.

## <a name="prerequisites"></a>Förutsättningar

- Om du är bekant med hanterade identiteter för Azure-resurser kan du kolla den [översiktsavsnittet](overview.md). **Se till att granska den [skillnaden mellan en hanterad identitet systemtilldelade och användartilldelade](overview.md#how-does-it-work)**.
- Om du inte redan har ett Azure-konto [registrerar du dig för ett kostnadsfritt konto](https://azure.microsoft.com/free/) innan du fortsätter.
- Om du använder Windows kan du installera den [Windows-undersystem for Linux](https://msdn.microsoft.com/commandline/wsl/about) eller Använd den [Azure Cloud Shell](../../cloud-shell/overview.md) i Azure-portalen.
- Om du använder den [Windows-undersystem for Linux](https://msdn.microsoft.com/commandline/wsl/about) eller en [Linux-distributionsoperativsystem](/cli/azure/install-azure-cli-apt?view=azure-cli-latest), [installera den lokala konsolen i Azure CLI](/cli/azure/install-azure-cli).
- Om du använder Azure CLI lokalt, logga in på Azure med hjälp av `az login` med ett konto som är associerad med den Azure-prenumeration du vill distribuera eller hämta information om användartilldelade hanterad identitet.
- Ditt konto måste följande rolltilldelningar för att utföra vilka hanteringsåtgärder i den här artikeln:
    - [Hanterad Identitetsdeltagare](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rollen kan skapa, läsa (lista), uppdatera och ta bort en hanterad Användartilldelad identitet.
    - [Hanterade Identitetsoperatör](/azure/role-based-access-control/built-in-roles#managed-identity-operator) roll att läsa (lista) egenskaperna för en hanterad Användartilldelad identitet.
- Hämta en ägar-token med `az account get-access-token` att utföra följande åtgärder för användartilldelade hanterad identitet.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-user-assigned-managed-identity"></a>Skapa en hanterad Användartilldelad identitet 

Använd följande CURL-begäran till Azure Resource Manager-API för att skapa en hanterad Användartilldelad identitet. Ersätt den `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, `<USER ASSIGNED IDENTITY NAME>`,`<LOCATION>`, och `<ACCESS TOKEN>` värdena med dina egna värden:

[!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroup
s/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>?api-version=2015-08-31-preview' -X PUT -d '{"loc
ation": "<LOCATION>"}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
```

## <a name="list-user-assigned-managed-identities"></a>Lista användartilldelade hanterade identiteter

Använd följande CURL-begäran till Azure Resource Manager-API för att lista användartilldelade hanterade identiteter. Ersätt den `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, och `<ACCESS TOKEN>` värdena med dina egna värden:

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities?api-version=2015-08-31-preview' -H "Authorization: Bearer <ACCESS TOKEN>"
```
## <a name="delete-a-user-assigned-managed-identity"></a>Ta bort en hanterad Användartilldelad identitet

Använd följande CURL-begäran till Azure Resource Manager-API för att ta bort en hanterad Användartilldelad identitet. Ersätt den `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, och `<ACCESS TOKEN>` parametervärdena med dina egna värden:

> [!NOTE]
> Tar bort en hanterad Användartilldelad identitet raderas inte referensen från alla resurser som den tilldelats. Ta bort en Användartilldelad hanteras från en virtuell dator med hjälp av CURL Se [ta bort en Användartilldelad identitet från en Azure VM](qs-configure-rest-vm.md#remove-a-user-assigned identity-from-an-azure-vm).

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroup
s/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>?api-version=2015-08-31-preview' -X DELETE -H "Authorization: Bearer <ACCESS TOKEN>"
```

## <a name="next-steps"></a>Nästa steg

Information om hur du tilldelar en hanterad Användartilldelad identitet till en Azure VM/VMSS med CURL i, [konfigurera hanterade identiteter för Azure-resurser på en Azure-dator med hjälp av REST API-anrop](qs-configure-rest-vm.md#user-assigned-managed-identity) och [konfigurera hanteras identiteter för Azure-resurser på en VM-skalningsuppsättning med hjälp av REST API-anrop](qs-configure-rest-vmss.md#user-assigned-managed-identity).


