---
title: Verifiera Azure Graph-integrering för Azure Stack
description: Använda Azure Stack-beredskap layout för att verifiera graph-integrering för Azure Stack.
services: azure-stack
documentationcenter: ''
author: PatAltimore
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/22/2018
ms.author: patricka
ms.reviewer: jerskine
ms.openlocfilehash: e1c1ba0a065a20874bf51d7464cbcfdfa13a571d
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/23/2018
ms.locfileid: "49947337"
---
# <a name="validate-graph-integration-for-azure-stack"></a>Verifiera graph-integrering för Azure Stack

Använd verktyget Azure Stack-beredskap för installation (AzsReadinessChecker) för att verifiera att din miljö är redo för graph-integrering med Azure Stack. Innan du börjar center dataintegrering eller före ett Azure Stack-distributionen bör du kontrollera graph-integrering.

Validerar beredskap för installation:

* Autentiseringsuppgifterna för tjänstkontot som skapats för graph-integrering har rätt behörigheter att fråga Active Directory.
* Den *global katalog* kan lösas och är beröras.
* KDC kan lösas och är beröras.
* Nödvändiga nätverksanslutningen är på plats.

Läs mer om integrering av Azure Stack data center [Azure Stack datacenter-integrering - identitet](azure-stack-integrate-identity.md)

## <a name="get-the-readiness-checker-tool"></a>Hämta verktyget för diagnostisk för installation

Ladda ned den senaste versionen av verktyget Azure Stack-beredskap för installation (AzsReadinessChecker) från den [PSGallery](https://aka.ms/AzsReadinessChecker).

## <a name="prerequisites"></a>Förutsättningar

Följande krav måste vara på plats.

**Den dator där verktyget körs:**

* Windows 10 eller Windows Server 2016 med domänanslutning.
* PowerShell 5.1 eller senare. Om du vill kontrollera vilken version du kör följande PowerShell-cmd och granska de *större* version och *mindre* versioner:  
   > `$PSVersionTable.PSVersion`
* Active Directory PowerShell-modulen
* Den senaste versionen av den [Microsoft Azure Stack-beredskap Checker](https://aka.ms/AzsReadinessChecker) verktyget

**Active Directory-miljö:**

* Identifiera användarnamnet och lösenordet för ett konto för graph-tjänsten i befintliga Active Directory
* Identifiera Active Directory-skogens rot FQDN

## <a name="validate-graph"></a>Verifiera graph

1. Öppna en administrativ PowerShell-kommandotolk och kör sedan följande kommando för att installera AzsReadinessChecker på en dator som uppfyller kraven.

     `Install-Module Microsoft.AzureStack.ReadinessChecker -Force`

1. Från PowerShell-Kommandotolken kör du följande för att ange *$graphCredential* variabeln till graph-kontot. Ersätt `contoso\graphservice` med ditt konto med hjälp av den `domain\username` format.

    `$graphCredential = Get-Credential contoso\graphservice -Message "Enter Credentials for the Graph Service Account"`

1. Från PowerShell-Kommandotolken kör du följande om du vill starta verifieringen av diagrammet. Ange värdet för **- ForestFQDN** som det fullständiga Domännamnet för skogsroten:

     `Invoke-AzsGraphValidation -ForestFQDN contoso.com -Credential $graphCredential`

1. Granska utdata när verktyget körs. Bekräfta att statusen är OK för integreringskraven för diagrammet. En lyckad validering visas liknar följande:

    ```
    Testing Graph Integration (v1.0)
            Test Forest Root:            OK
            Test Graph Credential:       OK
            Test Global Catalog:         OK
            Test KDC:                    OK
            Test LDAP Search:            OK
            Test Network Connectivity:   OK

    Details:

    [-] In standalone mode, some tests should not be considered fully indicative of connectivity or readiness the Azure Stack Stamp requires prior to Data Center Integration.

    Additional help URL: https://aka.ms/AzsGraphIntegration

    AzsReadinessChecker Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log

    AzsReadinessChecker Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json

    Invoke-AzsGraphValidation Completed
    ```

I produktionsmiljöer, Testa nätverksanslutningen från en arbetsstation för operatörer kan inte betraktas som fullständigt indikation på anslutningen som är tillgängliga för Azure Stack. Azure Stack-stämpel offentliga VIP-nätverket behöver anslutning för LDAP-trafik att utföra identitetsintegrering.

## <a name="report-and-log-file"></a>Rapporten och loggfilen

Varje tidsverifiering körs, loggas resultaten till **AzsReadinessChecker.log** och **AzsReadinessCheckerReport.json**. Platsen för filerna visas med verifieringsresultat i PowerShell.

Verifiering-filer kan hjälpa dig att dela status innan du distribuerar Azure Stack eller undersöka problem med verifieringen. Båda filerna spara resultatet av varje efterföljande valideringskontrollen. Rapporten innehåller din distribution team bekräftelse av identity-konfigurationen. Loggfilen kan hjälpa din distribution eller support-teamet undersöka verifieringsproblem.

Som standard skrivs båda filerna till `C:\Users\<username>\AppData\Local\Temp\AzsReadinessChecker\`

Användning:

* **-OutputPath** *sökväg* parameter i slutet av kör kommandoraden för att ange en annan plats.
* **-CleanReport** parameter i slutet av kommandot Kör för att rensa *AzsReadinessCheckerReport.json* av föregående rapportinformation. Mer information finns i [Azure Stack verifieringsrapport](azure-stack-validation-report.md).

## <a name="validation-failures"></a>Verifieringsfel

Om en verifieringskontroll misslyckas, visas information om felet i PowerShell-fönstret. Verktyget också loggar information till den *AzsGraphIntegration.log*.

## <a name="next-steps"></a>Nästa steg

[Visa rapport om distributionsberedskap](azure-stack-validation-report.md)  
[Överväganden för allmänna Azure Stack-integrering](azure-stack-datacenter-integration.md)  
