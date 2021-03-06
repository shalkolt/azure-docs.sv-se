---
title: Skillnader och överväganden för Managed Disks i Azure Stack | Microsoft Docs
description: Läs mer om skillnader och överväganden när du arbetar med Managed Disks i Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2018
ms.author: sethm
ms.reviewer: jiahan
ms.openlocfilehash: 2870bf8911c48ac4cbb442278f172b37a474152b
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/10/2018
ms.locfileid: "49078061"
---
# <a name="azure-stack-managed-disks-differences-and-considerations"></a>Azure Stack Managed Disks: Skillnader och överväganden
Den här artikeln sammanfattas de kända skillnaderna mellan Azure Stack Managed Disks och Managed Disks för Azure. Läs mer om övergripande skillnader mellan Azure Stack och Azure, i den [viktiga överväganden](azure-stack-considerations.md) artikeln.

Hanterade diskar förenklar Diskhantering för virtuella IaaS-datorer genom att hantera den [lagringskonton](/azure/azure-stack/azure-stack-manage-storage-accounts) som är associerade med de Virtuella diskarna.
  

## <a name="cheat-sheet-managed-disk-differences"></a>Lathund: Managed disk skillnader

| Funktion | Azure (global) | Azure Stack |
| --- | --- | --- |
|Kryptering för vilande Data |Azure Storage Service Encryption (SSE), Azure Disk Encryption (ADE)     |BitLocker 128-bitars AES-kryptering      |
|Bild          | Stöd för anpassade avbildningar |Stöds inte än|
|Alternativ för säkerhetskopiering |Stöd för Azure Backup-tjänsten |Stöds inte än |
|Alternativen för katastrofåterställning |Stöd för Azure Site Recovery |Stöds inte än|
|Disktyper     |Premium SSD, Standard SSD (förhandsversion) och Standard-Hårddisk |Premium SSD, Standard-Hårddisk |
|Premium-diskar  |Stöds fullt ut |Kan tillhandahållas, men ingen prestandagräns eller garanterar  |
|Premium-diskar IOPs  |Beror på diskstorleken  |2300 IOPs per disk |
|Dataflöde för Premium-diskar |Beror på diskstorleken |145 MB/sekund per disk |
|Maxstorlek för disk  |4 TB       |1 TB       |
|Diskar prestanda analytiska |Aggregera mått och per disk-mått som stöds |Stöds inte än |
|Migrering      |Innehåller verktyg för att migrera från befintliga ohanterade virtuella Azure Resource Manager-datorer utan att behöva återskapa den virtuella datorn  |Stöds inte än |


## <a name="metrics"></a>Mått
Det finns också skillnader med mätvärden i storage:
- Med Azure Stack skilja inte transaktionsdata i lagringsmått interna eller externa nätverksbandbredd.
- Azure Stack-transaktionsdata i lagringsmått omfattar inte VM-åtkomst till de monterade diskarna.


## <a name="api-versions"></a>API-versioner
Managed Disks för Azure Stack stöd för följande API-versioner:
- 2017-03-30


## <a name="next-steps"></a>Nästa steg
[Lär dig mer om Azure Stack-datorer](azure-stack-compute-overview.md)
