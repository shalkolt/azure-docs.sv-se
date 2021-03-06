---
title: Återaktivering av skydd redundansväxlade virtuella Azure-datorer till den primära Azure-regionen med Azure Site Recovery | Microsoft Docs
description: Beskriver hur du skyddar virtuella Azure-datorer i en sekundär region efter redundansväxling från en primär region, med hjälp av Azure Site Recovery.
services: site-recovery
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: rajanaki
ms.openlocfilehash: 9759e209f15622d70aaa833a993234863ac1053c
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/09/2018
ms.locfileid: "37918874"
---
# <a name="reprotect-failed-over-azure-vms-to-the-primary-region"></a>Återaktivering av skydd redundansväxlade virtuella Azure-datorer till den primära regionen


När du [redundansväxla](site-recovery-failover.md) Azure virtuella datorer från en region till en annan med [Azure Site Recovery](site-recovery-overview.md), virtuella datorer startas i den sekundära regionen oskyddad. Om de virtuella datorerna återställa till den primära regionen, måste du göra följande:

- Återaktivera skyddet av de virtuella datorerna i den sekundära regionen så att de börjar replikera till den primära regionen. 
- När återaktiveringen av skyddet har slutförts och de virtuella datorerna replikeras, kan du växla över dem från sekundär till primär region.

> [!WARNING]
> Om du har [migreras](migrate-overview.md#what-do-we-mean-by-migration) datorer från primärt till den sekundära regionen flyttas den virtuella datorn till en annan resursgrupp eller ta bort den virtuella Azure-datorn kan du inte återaktivera skyddet för den virtuella datorn eller inte återställa.


## <a name="prerequisites"></a>Förutsättningar
1. VM-redundans från primärt till sekundära region måste bekräftas.
2. Den primära målplatsen ska vara tillgängliga och du ska kunna komma åt eller skapa resurser i den regionen.

## <a name="reprotect-a-vm"></a>Återaktivera skyddet av en virtuell dator

1. I **Vault** > **replikerade objekt**, högerklicka på den redundansväxlade virtuella datorn och välj **skydda igen**. Riktning för återaktiveringen av skyddet ska visa från sekundär till primär. 

  ![Återaktivering av skydd](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotect.png)

2. Granska resource group, nätverk, lagring och tillgänglighet uppsättningar. Klicka sedan på **OK**. Om det finns några resurser som är markerade som ny, skapas de som en del av återaktiveringen av skyddet.
3. Återaktivering av skydd jobbet lägger till målplatsen den senaste informationen. Efter som har slutförts sker deltareplikering. Sedan kan du växla över till den primära platsen. Du kan välja storage-konto eller det nätverk som du vill använda under skydda igen med alternativet Anpassa.

  ![Anpassa alternativ](./media/site-recovery-how-to-reprotect-azure-to-azure/customize.png)

### <a name="customize-reprotect-settings"></a>Anpassa inställningar för återaktivering av skydd

Du kan anpassa följande egenskaper för mål VMe under återaktiveringen av skyddet.

![Anpassa](./media/site-recovery-how-to-reprotect-azure-to-azure/customizeblade.png)

|Egenskap  |Anteckningar  |
|---------|---------|
|Målresursgrupp     | Ändra målresursgruppen som den virtuella datorn har skapats. Den Virtuella måldatorn som en del av återaktiveringen av skyddet har tagits bort. Du kan välja en ny resursgrupp som du skapar den virtuella datorn efter redundans.        |
|Virtuellt Målnätverk     | Målnätverket kan inte ändras under jobbet för återaktivering av skydd. Om du vill ändra nätverket, gör du om nätverksmappningen.         |
|Mål-Lagringskontot (sekundära virtuella datorn inte använder hanterade diskar)     | Du kan ändra det lagringskonto som den virtuella datorn använder efter en redundansväxling.         |
|Hanterade replikeringsdiskar (sekundära virtuella datorn använder hanterade diskar)    | Hanterade replikeringsdiskar skapar site Recovery i den primära regionen för spegling av hanterade diskar för den sekundära virtuella datorn.         | 
|Cachelagring     | Du kan ange ett cachelagringskonto som ska användas vid replikering. Som standard är ett nytt cachelagringskonto skapas, om det inte finns.         |
|Tillgänglighetsuppsättning     |Om den virtuella datorn i den sekundära regionen är en del av en tillgänglighetsuppsättning, kan du välja en tillgänglighetsuppsättning för den Virtuella måldatorn i den primära regionen. Som standard Site Recovery försöker hitta befintliga tillgänglighetsuppsättningen i den primära regionen och använda den. Vid anpassning, kan du ange en ny tillgänglighetsuppsättning.         |


### <a name="what-happens-during-reprotection"></a>Vad händer under återaktiveringen av skyddet?

Som standard inträffar följande:

1. Ett cachelagringskonto har skapats i den primära regionen
2. Om mål-lagringskontot (ursprungliga lagringskontot i den primära regionen) inte finns, skapas en ny. Tilldelade lagringskontonamn är namnet på det lagringskonto som används av den sekundära virtuella datorn, suffix med ”asr”.
3. Om den virtuella datorn använder hanterade diskar, hanterade diskar skapas i den primära regionen att lagra de data som replikeras från den sekundära Virtuella diskar. 
4. Om målets tillgänglighetsuppsättning inte finns, skapas en ny som en del av jobbet återaktivering av skydd om det behövs. Om du har anpassade inställningar för återaktivering av skydd, används den valda uppsättningen.

När du utlöser ett jobb för återaktivering av skydd och de mål som den virtuella datorn finns, inträffar följande:

1. Komponenterna som krävs skapas som en del av återaktivering av skydd. Om det redan finns, så återanvänds.
2. Målsidan virtuella datorn stängs av om den körs.
3. Måldisken sida VM kopieras till en behållare som en seedblob av Site Recovery.
4. Målsidan VM raderas sedan.
5. Seedbloben används av den aktuella källan sida (sekundär) virtuell dator för att replikera. Detta säkerställer att endast deltan replikeras.
6. Större ändringar mellan källdisken och seedbloben synkroniseras. Det kan ta lite tid att slutföra.
7. När återaktivering av skydd jobbet har slutförts, deltareplikeringen börjar och skapar en återställningspunkt i enlighet med replikeringsprincipen.
8. När jobbet återaktivering av skydd har genomförts, försätts den virtuella datorn i ett skyddat läge.

## <a name="next-steps"></a>Nästa steg

När den virtuella datorn är skyddad, kan du initiera redundans. Redundansen stänger av den virtuella datorn i den sekundära regionen och skapar och startar virtuella datorer i den primära regionen, med vissa små avbrott. Vi rekommenderar att du väljer en tid i enlighet med detta och att du kör ett redundanstest men initiera en fullständig växling till den primära platsen. [Läs mer](site-recovery-failover.md) om redundans.

