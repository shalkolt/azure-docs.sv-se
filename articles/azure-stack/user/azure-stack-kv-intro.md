---
title: Introduktion till Azure Stack Key Vault | Microsoft Docs
description: Lär dig hur Azure Stack Key Vault hanterar nycklar och hemligheter
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 70f1684a-3fbb-4cd1-bf29-9f9882e98fe9
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/15/2018
ms.author: sethm
ms.openlocfilehash: a6b4e8c3543d4681c92fbbde30eec0a543fcb0fd
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/16/2018
ms.locfileid: "42139345"
---
# <a name="introduction-to-key-vault-in-azure-stack"></a>Introduktion till Nyckelvalv i Azure Stack

## <a name="prerequisites"></a>Förutsättningar 

* Du måste prenumerera på ett erbjudande som innehåller Azure Key Vault-tjänsten.  
* [PowerShell är konfigurerat för användning med Azure Stack](azure-stack-powershell-configure-user.md).
 
## <a name="key-vault-basics"></a>Key Vault-grunderna
Key Vault i Azure Stack hjälper till att skydda kryptografiska nycklar och hemligheter som program och tjänster i molnet använder. Med Key Vault kan kryptera du nycklar och hemligheter, till exempel:
   * Autentiseringsnycklar 
   * Lagringskontonycklar
   * Datakrypteringsnycklar
   * PFX-filer
   * Lösenord

Key Vault förenklar nyckelhanteringen och låter dig behålla kontrollen över nycklar som kommer åt och krypterar data. Utvecklare kan skapa nycklar för utveckling och testning på några minuter och sedan sömlöst migrera dem till produktionsnycklar. Säkerhetsadministratörer kan bevilja (och återkalla) behörighet till nycklar efter behov.

Vem som helst med en Azure Stack-prenumeration kan skapa och använda nyckelvalv. Även om Key Vault hjälper utvecklare och säkerhetsadministratörer, kan en operatör som hanterar andra Azure Stack-tjänster för en organisation implementera och hantera den. Azure Stack operatör kan logga in med ett Azure Stack-prenumerationen kan till exempel skapa ett valv för organisationen att lagra nycklar och sedan är ansvarig för dessa operativa uppgifter:

* Skapa eller importera en nyckel eller hemlighet.
* Återkalla eller ta bort en nyckel eller hemlighet.
* Auktorisera användare eller program åtkomst till nyckelvalvet, så att de kan sedan hantera eller använda sina nycklar och hemligheter.
* Konfigurera nyckelanvändningen (till exempel signera eller kryptera).

Operatören kan sedan ge utvecklare med Uniform Resource Identifier (URI: er) för att anropa från sina program. Operatörer kan också ge säkerhetsadministratörer nyckelanvändning loggningsinformation.

Utvecklare kan också hantera nycklar direkt, med hjälp av API:er. Mer information finns i utvecklarguide för Key Vault.

## <a name="scenarios"></a>Scenarier
Följande scenarier beskriver hur Key Vault kan hjälpa dig att uppfylla kraven från utvecklare och säkerhetsadministratörer.

### <a name="developer-for-an-azure-stack-application"></a>Utvecklare för ett Azure Stack-program
**Problem:** jag vill skriva ett program för Azure Stack som använder nycklar för signering och kryptering. Jag vill att de ska vara externa från mitt program så att lösningen passar ett geografiskt distribuerat program.

**Instruktion:** nycklar lagras i ett valv och anropas av en URI vid behov.

### <a name="developer-for-software-as-a-service-saas"></a>Utvecklare av programvara som en tjänst (SaaS)
**Problem:** jag vill inte ansvar eller hitta potentiella ansvaret för Mina kunders nycklar och hemligheter. Jag vill att kunderna ska äga och hantera sina nycklar så att jag kan koncentrera dig på att göra vad jag gör bäst, dvs. att erbjuda grundläggande funktioner.

**Instruktion:** kunder kan importera sina egna nycklar till Azure Stack och hantera dem sedan. 

### <a name="chief-security-officer-cso"></a>Chief Security Officer (CSO)
**Problem:** jag vill se till att min organisation har kontrollen över nyckelns livscykel och kan övervaka nyckelanvändningen.

**Instruktion:** Key Vault har utformats så att Microsoft varken se eller extrahera dina nycklar. När ett program behöver utföra kryptografiska åtgärder med hjälp av kunders nycklar, använder Key Vault nycklarna åt programmet. Programmet kan inte se kundernas nycklar. Även om vi använder flera Azure Stack-tjänster och resurser, kan du hantera nycklarna från en enda plats i Azure Stack. Valvet innehåller ett enda gränssnitt, oavsett hur många valv du har i Azure Stack, vilka regioner de support och använda dem för vilka program.

## <a name="next-steps"></a>Nästa steg

* [Hantera Nyckelvalv i Azure Stack med hjälp av portalen](azure-stack-kv-manage-portal.md)  
* [Hantera Nyckelvalv i Azure Stack med hjälp av PowerShell](azure-stack-kv-manage-powershell.md)

