---
title: Uppdatera Windows Defender Antivirus på Azure Stack
description: Information om hur antivirus hålls uppdaterad på Azure Stack
services: azure-stack
author: PatAltimore
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 09/26/2018
ms.author: patricka
ms.reviewer: fiseraci
ms.openlocfilehash: 59c59705e840d3cdd0d3c5310d84d711cd00c96a
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47394020"
---
# <a name="update-windows-defender-antivirus-on-azure-stack"></a>Uppdatera Windows Defender Antivirus på Azure Stack

[Windows Defender Antivirus](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10) är en lösning för program mot skadlig kod som ger skydd för säkerhet och virus. Varje Azure Stack-infrastrukturkomponenter (Hyper-V-värdar och virtuella datorer) är skyddad med Windows Defender Antivirus. För skyddet kräva periodiska uppdateringar av Windows Defender Antivirus definitioner, engine och plattform. Hur uppdateringar tillämpas beror på din konfiguration.

## <a name="connected-scenario"></a>Scenariot

För program mot skadlig kod definitioner och motor uppdateringar, Azure Stack [uppdatering resursprovidern](azure-stack-updates.md#the-update-resource-provider) laddar ned definitioner för skadlig kod och motoruppdateringar flera gånger per dag. Varje Azure Stack-infrastrukturkomponenter hämtar uppdateringen från resursprovidern uppdatering och tillämpar uppdateringen automatiskt.

Uppdateringar för plattformen för program mot skadlig kod, tillämpa den [månatliga Azure Stack-uppdatering](azure-stack-apply-updates.md). Månatliga Azure Stack uppdateringen innehåller uppdateringar för Windows Defender Antivirus-plattform för månaden.

## <a name="disconnected-scenario"></a>Frånkopplade scenario

 Tillämpa den [månatliga Azure Stack-uppdatering](azure-stack-apply-updates.md). Den månatliga Azure Stack-uppdateringen innehåller Windows Defender Antivirus definitioner, engine och plattformsuppdateringar för månaden.

## <a name="next-steps"></a>Nästa steg

[Läs mer om Azure Stack-säkerhet](azure-stack-security-foundations.md)
