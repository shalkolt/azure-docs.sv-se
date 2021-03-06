---
title: Visualisering av beroenden i Azure Migrate | Microsoft Docs
description: Översikt över utvärderingsberäkningar i Azure Migrate-tjänsten.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 09/25/2018
ms.author: raynew
ms.openlocfilehash: 923a2a137bb4510e9490ce4077f744a43619a2c6
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47165032"
---
# <a name="dependency-visualization"></a>Visualisering av beroenden

Den [Azure Migrate](migrate-overview.md) tjänsterna utvärderar grupper av lokala datorer för migrering till Azure. Du kan använda beroendevisualiseringsfunktionen i Azure Migrate för att skapa grupper. Den här artikeln innehåller information om den här funktionen.


## <a name="overview"></a>Översikt

Visualisering av beroenden i Azure Migrate kan du skapa grupper med hög exakthet för migreringsutvärdering. Med hjälp av beroendevisualisering du kan visa nätverksberoenden för datorer och identifiera relaterade datorer som behövde migreras tillsammans till Azure. Den här funktionen är användbar i scenarier där du inte är helt känner till de datorer som utgör ditt program och måste migreras tillsammans till Azure.

## <a name="how-does-it-work"></a>Hur fungerar det?

Azure Migrate använder den [Tjänstkarta](../operations-management-suite/operations-management-suite-service-map.md) lösning i [Log Analytics](../log-analytics/log-analytics-overview.md) för visualisering av beroenden.
- Om du vill använda visualisering av beroenden, måste du koppla en Log Analytics-arbetsyta, ny eller befintlig, med ett Azure Migrate-projekt.
- Du kan bara skapa eller koppla en arbetsyta i samma prenumeration där migration-projekt skapas.
- Om du vill koppla en Log Analytics-arbetsyta till ett projekt, gå till **Essentials** avsnittet projektets **översikt** och klicka på **kräver konfiguration**

    ![Associera arbetsytan för Log Analytics](./media/concepts-dependency-visualization/associate-workspace.png)

- När du skapar en ny arbetsyta kan behöva du ange ett namn för arbetsytan. Arbetsytan skapas i en region i samma [Azure geografi](https://azure.microsoft.com/global-infrastructure/geographies/) som migration-projekt.
- Arbetsytan associerade märks med nyckeln **migreringsprojektet**, och värdet **projektnamn**, som du kan använda för att söka i Azure-portalen.
- Om du vill gå till arbetsytan som är kopplade till projektet, går du till **Essentials** avsnittet projektets **översikt** sidan och få åtkomst till arbetsytan

    ![Navigera Log Analytics-arbetsyta](./media/concepts-dependency-visualization/oms-workspace.png)

Om du vill använda visualisering av beroenden, måste du ladda ned och installera agenter på varje lokal dator som du vill analysera.  

## <a name="do-i-need-to-pay-for-it"></a>Behöver jag betala för det?

Azure Migrate är tillgänglig utan extra kostnad. Användning av funktionen beroendevisualisering i Azure Migrate kräver Tjänstkarta och kräver att du associerar en Log Analytics-arbetsyta, ny eller befintlig, med Azure Migrate-projektet. Beroendevisualiseringsfunktionen i Azure Migrate är kostnadsfri under de första 180 dagarna i Azure Migrate.

1. Användning av några lösningar än Tjänstkartan i den här Log Analytics-arbetsytan medför [standard logganalys](https://azure.microsoft.com/pricing/details/log-analytics/) avgifter.
2. För att stödja Migreringsscenarier utan extra kostnad, behöver lösningen Tjänstkarta inte betala något under de första 180 dagarna från dagen för att associera Log Analytics-arbetsytan med Azure Migrate-projektet. Efter 180-dagarsperioden tillkommer Log Analytics standardkostnader.

När du registrerar agenter till arbetsytan använder du ID: T och nyckeln som anges av projektet på sidan Installera agenten steg.

När Azure Migrate-projekt tas bort, raderas inte arbetsytan tillsammans med den. Publicera projektet borttagningen Service Map användning är inte kostnadsfria och varje nod debiteras enligt den kostnadsfria nivån av Log Analytics-arbetsyta.

> [!NOTE]
> Funktionen beroendevisualisering använder Tjänstkarta via en Log Analytics-arbetsyta. Eftersom den 28 februari 2018 är med tillkännagivandet av Azure Migrate allmänt tillgängliga, funktionen nu tillgänglig utan extra kostnad. Du måste skapa ett nytt projekt att använda arbetsytan kostnadsfri användning. Befintliga arbetsytor före den allmänt tillgängliga är fortfarande begränsad omfattning, därför rekommenderar vi att du flyttar till ett nytt projekt.

Mer information om priser för Azure Migrate finns [här](https://azure.microsoft.com/pricing/details/azure-migrate/).

## <a name="how-do-i-manage-the-workspace"></a>Hur hanterar jag arbetsytan?

Du kan använda Log Analytics-arbetsytan utanför Azure Migrate. Den tas inte bort om du tar bort migration-projekt där den skapades. Om du inte längre behöver arbetsytan [ta bort den](../log-analytics/log-analytics-manage-access.md) manuellt.

Ta inte bort den arbetsyta som skapats av Azure Migrate, såvida inte du ta bort migreringsprojektet. Om du gör det, kommer beroendevisualiseringsfunktionen inte fungerar som förväntat.

## <a name="next-steps"></a>Nästa steg
- [Gruppera datorer med datorberoenden](how-to-create-group-machine-dependencies.md)
- [Läs mer](https://docs.microsoft.com/azure/migrate/resources-faq#dependency-visualization) om frågor och svar på beroendevisualisering.
