---
title: Användningsscenarier och överväganden vid distribution för Azure AD Join | Microsoft Docs
description: Beskriver hur administratörer kan konfigurera Azure AD Join för slutanvändarna (anställda, studenter, andra användare). Det diskuterar även olika verkliga scenarier för att använda Azure AD Join.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
tags: azure-classic-portal
ms.component: devices
ms.assetid: 81d4461e-21c8-4fdd-9076-0e4991979f62
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2018
ms.author: markvi
ms.openlocfilehash: a145b4184fbebd9c4f447face1c47bec20c0ec32
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39414891"
---
# <a name="usage-scenarios-and-deployment-considerations-for-azure-ad-join"></a>Användningsscenarier och överväganden vid distribution för Azure AD Join
## <a name="usage-scenarios-for-azure-ad-join"></a>Användningsscenarier för Azure AD Join
### <a name="scenario-1-businesses-largely-in-the-cloud"></a>Scenario 1: Företag huvudsakligen i molnet
Azure Active Directory Join (Azure AD Join) kan hjälpa dig om du för närvarande fungerar och hantera identiteter för din verksamhet i molnet eller flytt till molnet snart. Du kan använda ett konto som du har skapat i Azure AD för att logga in på Windows 10. Via [den första körningen uppstår (FRX) processen](azuread-joined-devices-frx.md), eller genom att ansluta till Azure AD från [inställningsmenyn](../user-help/device-management-azuread-joined-devices-setup.md), dina användare kan ansluta till sina datorer till Azure AD.  Användarna kan också dra nytta av enkel inloggning (SSO) åtkomst till molnresurser som Office 365, antingen i webbläsaren eller i Office-program.

### <a name="scenario-2-educational-institutions"></a>Scenario 2: Undervisningsinstitutioner
Utbildningsinstitutioner har vanligtvis två användartyper: lärare, personal och studenter. Lärare och annan personal anses långsiktiga medlemmar i organisationen. Det är önskvärt att skapa lokala konton för dessa. Men studenter shorter-term som ingår i organisationen och deras konton kan hanteras i Azure AD. Det innebär att directory skala kan skickas till molnet i stället för att lagras lokalt. Det innebär också att studenter kommer att kunna logga in på Windows med sina Azure AD-konton och få åtkomst till Office 365-resurser i Office-program.

### <a name="scenario-3-retail-businesses"></a>Scenario 3: Detaljhandel företag
Detaljhandel företag har säsongsarbetare och långsiktig anställda. Du vanligtvis skapa lokala konton och använda domänanslutna datorer för långsiktiga heltidsanställda. Men säsongsarbetare shorter-term som ingår i organisationen, och det är önskvärt att hantera sina konton där användarlicenser kan enkelt flytta runt. När du skapar sina konton i molnet med Office 365-licenser kan få användarna fördelarna med att logga in på Windows och Office-program med en Azure AD-konto, samtidigt som du behåller större flexibilitet med sina licenser när de lämnar.

### <a name="scenario-4-additional-scenarios"></a>Scenario 4: Ytterligare scenarier
Tillsammans med fördelar som beskrivs ovan, dra nytta av att användare ansluta sina enheter till Azure AD på grund av en förenklad sammanbinder miljö, effektiv enhetshantering, automatisk registrering av mobil hantering och enkel inloggning till Azure AD och lokala resurser.  

## <a name="deployment-considerations-for-azure-ad-join"></a>Överväganden vid distribution för Azure AD Join
### <a name="enable-your-users-to-join-a-company-owned-device-directly-to-azure-ad"></a>Ge användarna att ansluta till en företagsägd enhet direkt till Azure AD
Företag kan ge molnbaserad konton till partnerföretag och organisationer. Dessa partner kan sedan enkelt komma åt företagets appar och resurser med enkel inloggning. Det här scenariot gäller för användare som har åtkomst till resurser i första hand i molnet, till exempel Office 365 eller SaaS-appar som förlitar sig på Azure AD för autentisering.

### <a name="prerequisites"></a>Förutsättningar
**På företagsnivå (administratör)**

* Azure-prenumeration med Azure Active Directory  

**På nivån**

* Windows 10 (Professional och Enterprise Edition)

### <a name="administrator-tasks"></a>Administratörsåtgärder
* [Konfigurera enhetsregistrering](device-management-azure-portal.md)

### <a name="user-tasks"></a>Aktiviteter för användare
* [Konfigurera en ny Windows 10-enhet med Azure AD under installationen](azuread-joined-devices-frx.md)
* [Konfigurera en Windows 10-enhet med Azure AD från inställningsmenyn](../user-help/device-management-azuread-registered-devices-windows10-setup.md)
* [Ansluta en egen Windows 10-enhet till din organisation](../user-help/device-management-azuread-joined-devices-setup.md)

## <a name="enable-byod-in-your-organization-for-windows-10"></a>Aktivera BYOD i din organisation för Windows 10
Du kan ställa in dina användare och anställda använda sina egna Windows-enheter (BYOD) för att få åtkomst till företagets appar och resurser. Användarna kan lägga till sina Azure AD-konton (arbets- eller skolkonto-konton) till en personlig Windows-enhet för att komma åt resurser på en säker och kompatibel sätt.

### <a name="prerequisites"></a>Förutsättningar
**På företagsnivå (administratör)**

* Azure AD-prenumeration

**På nivån**

* Windows 10 (Professional och Enterprise Edition)

### <a name="administrator-tasks"></a>Administratörsåtgärder
* [Konfigurera enhetsregistrering](device-management-azure-portal.md)

### <a name="user-tasks"></a>Aktiviteter för användare
* [Ansluta en egen Windows 10-enhet till din organisation](../user-help/device-management-azuread-joined-devices-setup.md)

## <a name="next-steps"></a>Nästa steg

- [Enhetshantering](overview.md)

