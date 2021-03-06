---
title: Använda anpassade roller för Azure-resurser i PIM | Microsoft Docs
description: Lär dig hur du använder anpassade roller för Azure-resurser i Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: pim
ms.date: 03/30/2018
ms.author: rolyon
ms.openlocfilehash: b01e785ac85c71b2982561e8b5e118775750fc69
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189881"
---
# <a name="use-custom-roles-for-azure-resources-in-pim"></a>Använda anpassade roller för Azure-resurser i PIM

Du kan behöva gäller strikta inställningar för Privileged Identity Management (PIM) för vissa medlemmar i en roll, samtidigt som det ger större självständigt för andra. Tänk dig ett scenario där anlitar din organisation flera kontrakt associates att underlätta utvecklingen av ett program som körs i en Azure-prenumeration.

Som administratör av en resurs kan du anställda att vara berättigade till åtkomst utan att kräva godkännande. Alla avtal associates måste vara godkända när de begär åtkomst till organisationens resurser.

Följ stegen som beskrivs i nästa avsnitt för att konfigurera inställningar för riktade PIM för Azure-resursroller.

## <a name="create-the-custom-role"></a>Skapa anpassad roll

Om du vill skapa en anpassad roll för en resurs, följer du stegen som beskrivs i [skapa anpassade roller för rollbaserad åtkomstkontroll i](../role-based-access-control-custom-roles.md).

När du skapar en anpassad roll kan du inkludera ett beskrivande namn så att du lätt kan komma ihåg vilken inbyggd roll som du försöker att duplicera.

> [!NOTE]
> Kontrollera att den anpassade rollen är en dubblett av den inbyggda rollen som du vill duplicera och att den inbyggda rollen som överensstämmer med sitt omfång.

## <a name="apply-pim-settings"></a>Använda PIM-inställningar

När rollen skapas i din klient i Azure-portalen går du till den **Privileged Identity Management - Azure-resurser** fönstret. Välj den resurs som rollen gäller.

![”Privileged Identity Management - Azure-resurser” fönstret](media/azure-pim-resource-rbac/aadpim_manage_azure_resource_some_there.png)

[Konfigurera PIM rollinställningar](pim-resource-roles-configure-role-settings.md) som ska tillämpas på dessa medlemmar i rollen.

Slutligen [tilldela roller](pim-resource-roles-assign-roles.md) till distinkta grupp med medlemmar som du vill använda med de här inställningarna.

## <a name="next-steps"></a>Nästa steg

- [Konfigurera Azure-resurs rollinställningar i PIM](pim-resource-roles-configure-role-settings.md)
- [Anpassade roller i Azure](../../role-based-access-control/custom-roles.md)
