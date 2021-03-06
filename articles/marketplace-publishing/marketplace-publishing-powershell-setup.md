---
title: Konfigurera PowerShell för att skapa en virtuell dator för Marketplace | Microsoft Docs
description: Anvisningar om hur du konfigurerar Azure PowerShell och använda det som en valfri process flow för att skapa avbildningar av Virtuella datorer att distribuera till och sälja på Azure Marketplace
services: marketplace-publishing
documentationcenter: ''
author: HannibalSII
manager: hascipio
editor: ''
ms.assetid: e19d6cda-76df-4e42-be84-c9fe47a636db
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/04/2016
ms.author: hascipio
ms.openlocfilehash: bbcce5093d2bbd5326523063db7d0e565fe4de6d
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/08/2018
ms.locfileid: "39713643"
---
# <a name="set-up-azure-powershell-to-create-an-offer-for-the-azure-marketplace"></a>Konfigurera Azure PowerShell för att skapa ett erbjudande för Azure Marketplace
Detaljerad information om hur du ställer in PowerShell i Azure finns i [hur du installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview). En enkel metod är att använda metoden certifikat, som hämtar och importerar ett certifikat som krävs för autentisering. Hämta nödvändiga certifikat med den **Get-AzurePublishSettingsFile** cmdlet. Spara filen när du uppmanas. Om du vill importera certifikatet till en PowerShell-session, använda den **Import AzurePublishSettingsFile** cmdlet.

Konfigurera och lagra vanliga inställningar för Microsoft Azure-prenumeration för PowerShell-session genom att använda den **Set-AzureSubscription** och **Select-AzureSubscription** cmdletar:

        Set-AzureSubscription -SubscriptionName “mySubName” -CurrentStorageAccountName “mystorageaccount”
        Select-AzureSubscription -SubscriptionName "mySubName" –Current

Det första kommandot associerar ett standardkonto för lagring med prenumerationen (som behövs för VM etablering åtgärder).  Andra blir prenumerationen som den aktuella artikeln (som identifieras av andra cmdlet: ar).

## <a name="see-also"></a>Se också
* [Komma igång: hur du publicerar ett erbjudande på Azure Marketplace](marketplace-publishing-getting-started.md)
* [Skapa en avbildning av virtuell dator för Marketplace](marketplace-publishing-vm-image-creation.md)

