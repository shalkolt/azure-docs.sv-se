---
title: Så här hanterar du en Användartilldelad hanterad identitet med hjälp av Azure portal
description: Steg för steg instruktioner om hur du kan skapa, visa, ta bort och tilldela en roll till en hanterad Användartilldelad identitet.
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
ms.date: 04/16/2018
ms.author: daveba
ms.openlocfilehash: bdbe15a85ad4d2ef6918b7ab7e16942edde5096e
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47220344"
---
# <a name="create-list-delete-or-assign-a-role-to-a-user-assigned-managed-identity-using-the-azure-portal"></a>Skapa, visa, ta bort eller tilldela en roll till en Användartilldelad hanterad identitet med hjälp av Azure portal

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Hanterade identiteter för Azure-resurser tillhandahåller Azure-tjänster med en hanterad identitet i Azure Active Directory. Du kan använda den här identiteten för att autentisera till tjänster som stöder Azure AD-autentisering, utan att behöva autentiseringsuppgifter i din kod. 

I den här artikeln får du lära dig skapa, visa, ta bort eller tilldela en roll till en Användartilldelad hanterad identitet med hjälp av Azure-portalen.

## <a name="prerequisites"></a>Förutsättningar

- Om du är bekant med hanterade identiteter för Azure-resurser kan du kolla den [översiktsavsnittet](overview.md). **Se till att granska den [skillnaden mellan en hanterad identitet systemtilldelade och användartilldelade](overview.md#how-does-it-work)**.
- Om du inte redan har ett Azure-konto [registrerar du dig för ett kostnadsfritt konto](https://azure.microsoft.com/free/) innan du fortsätter.
- Ditt konto måste följande rolltilldelningar för att utföra vilka hanteringsåtgärder i den här artikeln:
    - [Hanterad Identitetsdeltagare](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rollen kan skapa, läsa (lista), uppdatera och ta bort en hanterad Användartilldelad identitet.
    - [Hanterade Identitetsoperatör](/azure/role-based-access-control/built-in-roles#managed-identity-operator) roll att läsa (lista) egenskaperna för en hanterad Användartilldelad identitet.

## <a name="create-a-user-assigned-managed-identity"></a>Skapa en användartilldelad hanterad identitet

1. Logga in på den [Azure-portalen](https://portal.azure.com) med ett konto som är associerade med Azure-prenumeration för att skapa den hanterade Användartilldelad identitet.
2. I sökrutan skriver *hanterade identiteter*, och under **Services**, klickar du på **hanterade identiteter**.
3. Klicka på **Lägg till** och ange värden i följande fält under **skapa användartilldelade hanteras** identity-fönstret:
   - **Resursnamnet**: det här är namnet på din användartilldelade hanterad identitet, till exempel UAI1.
   - **Prenumeration**: Välj den prenumeration för att skapa hanterade Användartilldelad identitet under
   - **Resursgrupp**: skapa en ny resursgrupp för att innehålla din hanterade Användartilldelad identitet eller välj **Använd befintlig** att skapa den hanterade Användartilldelad identitet i en befintlig resursgrupp.
   - **Plats**: Välj en plats för att distribuera användartilldelade hanterad identitet, till exempel **västra USA**.
4. Klicka på **Skapa**.

![Skapa en användartilldelad hanterad identitet](./media/how-to-manage-ua-identity-portal/create-user-assigned-managed-identity-portal.png)

## <a name="list-user-assigned-managed-identities"></a>Lista användartilldelade hanterade identiteter

1. Logga in på den [Azure-portalen](https://portal.azure.com) med ett konto som är associerade med Azure-prenumeration för att lista de hanterade användartilldelade identiteterna.
2. I sökrutan skriver *hanterade identiteter*, och under tjänster klickar du på **hanterade identiteter**.
3. En lista över hanterade användartilldelade identiteter för din prenumeration returneras.  För att se information om en hanterad Användartilldelad identitet klickar du på namnet.

![Lista användartilldelade hanterad identitet](./media/how-to-manage-ua-identity-portal/list-user-assigned-managed-identity-portal.png)

## <a name="delete-a-user-assigned-managed-identity"></a>Ta bort en hanterad Användartilldelad identitet

1. Logga in på den [Azure-portalen](https://portal.azure.com) med ett konto som är associerade med Azure-prenumeration att ta bort en hanterad Användartilldelad identitet.
2. Välj den hanterade Användartilldelad identitet och klicka på **ta bort**.
3. Under dialogrutan väljer du, **Ja**.

![Ta bort Användartilldelad hanterad identitet](./media/how-to-manage-ua-identity-portal/delete-user-assigned-managed-identity-portal.png)

## <a name="assign-a-role-to-a-user-assigned-managed-identity"></a>Tilldela en roll till en hanterad Användartilldelad identitet 

1. Logga in på den [Azure-portalen](https://portal.azure.com) med ett konto som är associerade med Azure-prenumeration för att lista de hanterade användartilldelade identiteterna.
2. I sökrutan skriver *hanterade identiteter*, och under tjänster klickar du på **hanterade identiteter**.
3. En lista över hanterade användartilldelade identiteter för din prenumeration returneras.  Välj den användartilldelade hanterade identitet som du vill tilldela en roll.
4. Välj **åtkomstkontroll (IAM)** och välj sedan **Lägg till**.

   ![Användartilldelade hanterad identitet start](./media/how-to-manage-ua-identity-portal/assign-role-screenshot1.png)

5. Konfigurera följande värden på bladet Lägg till behörigheter och klicka sedan på **spara**:
   - **Rollen** -roll att tilldela
   - **Tilldela åtkomst till** -resursen för att tilldela den användartilldelade hanterad identitet
   - **Välj** -medlemmen att tilldela åtkomst
   
   ![Användartilldelade hanterad identitet IAM](./media/how-to-manage-ua-identity-portal/assign-role-screenshot2.png)  