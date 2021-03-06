---
title: Självstudie – minska Azure-kostnader med optimering rekommendationer | Microsoft Docs
description: Den här kursen hjälper dig att minska kostnaderna för Azure när du vidta åtgärder för optimering av rekommendationer.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 09/21/2018
ms.topic: conceptual
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: 4d9e47d6da45eaba19cbe089de3fdf053c36046a
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47030686"
---
# <a name="tutorial-optimize-costs-from-recommendations"></a>Självstudie: Optimera kostnaderna från rekommendationer

Azure Cost Management fungerar med Azure Advisor för att tillhandahålla kostnadsrekommendationer för optimering. Azure Advisor hjälper dig att optimera och förbättra effektiviteten genom att identifiera inaktiv och underutnyttjade resurser. Den här självstudien visar ett exempel där du identifiera underutnyttjade Azure-resurser och sedan du vidta åtgärder för att minska kostnaderna.

I den här guiden får du lära dig att:

> [!div class="checklist"]
> * Visa optimering rekommendationer att visa potentiella ineffektivitet
> * Agera på en rekommendation att ändra storlek på en virtuell dator till ett mer kostnadseffektivt alternativ
> * Bekräfta åtgärden för att säkerställa att den virtuella datorn har har ändrats

## <a name="prerequisites"></a>Förutsättningar
Rekommendationer är tillgänglig för alla [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/) kunder. Du måste minst ha skrivskyddad åtkomst till en eller flera av följande omfattningar för att visa kostnadsdata.

- Prenumeration
- Resursgrupp

Du måste ha aktiva virtuella datorer med minst 14 dagars aktivitet.

## <a name="sign-in-to-azure"></a>Logga in på Azure
Logga in på Azure Portal på [https://portal.azure.com](https://portal.azure.com/).

## <a name="view-cost-optimization-recommendations"></a>Visa rekommendationer för optimering

Klicka på **Kostnadshantering + fakturering** i listan med tjänster i Azure Portal. Sedan i listan under **kostnadshantering**väljer **Advisor-rekommendationer**. Advisor kostnadsrekommendationer visas.

![Advisor-rekommendationer](./media/tutorial-acm-opt-recommendations/advisor-recommendations.png)

Lista över rekommendationer identifierar ineffektivitet eller visar rekommendationer för köp som kan hjälpa dig att spara ytterligare pengar. Den sammanräknad **potentiella årliga besparingar** visar det totala beloppet som du kan spara om du stänger ner eller frigör alla dina virtuella datorer som uppfyller reglerna för rekommendation. Om du inte vill att stänga av dem, bör du överväga att ändra storlek på dem till en mindre VM-SKU.

Den **inverkan** kategori, tillsammans med den **potentiella årliga besparingar**, är utformade för att identifiera rekommendationer som kan spara så mycket som möjligt. Rekommendationer för hög påverkan är [köp reserverade instanser av virtuella datorer att spara pengar jämfört med betala per användning](../advisor/advisor-cost-recommendations.md#buy-reserved-virtual-machine-instances-to-save-money-over-pay-as-you-go-costs) och [ägna åt att optimera virtuella datorn genom att ändra storlek på eller stänga av underutnyttjade instanser](../advisor/advisor-cost-recommendations.md#optimize-virtual-machine-spend-by-resizing-or-shutting-down-underutilized-instances). Medelstor inverkan rekommendationerna är [minska kostnaderna genom att eliminera icke etablerade ExpressRoute-kretsar](../advisor/advisor-cost-recommendations.md#reduce-costs-by-eliminating-unprovisioned-expressroute-circuits) och [minska kostnaderna genom att ta bort eller konfigurera om inaktiva virtuella nätverksgatewayer](../advisor/advisor-cost-recommendations.md#reduce-costs-by-deleting-or-reconfiguring-idle-virtual-network-gateways).

## <a name="act-on-a-recommendation"></a>Agera på en rekommendation

Azure Advisor övervakar din användning av virtuella datorer under 14 dagar och identifierar underutnyttjade virtuella datorer. Virtuella datorer vars CPU-belastningen är fem procent eller mindre och nätverkets användning är sju MB eller mindre för fyra eller flera dagar betraktas som låg användning virtuella datorer.

5% eller mindre CPU användning är standardinställningen, men du kan justera inställningarna. Mer information om hur du justerar inställningen finns i den [konfigurera regeln för Genomsnittlig CPU-användning](../advisor/advisor-get-started.md#configure-the-average-cpu-utilization-rule-for-the-low-usage-virtual-machine-recommendation) artikeln [för den virtuella dator med låg användning-rekommendationen](../advisor/advisor-get-started.md#configure-the-average-cpu-utilization-rule-for-the-low-usage-virtual-machine-recommendation).

Även om vissa scenarier kan resultera i låg användning av design, kan du ofta spara pengar genom att ändra storleken på dina virtuella datorer till billigare storlekar. Dina faktiska besparingar kan variera om du väljer en åtgärd för storleksändring. Låt oss gå igenom ett exempel på att ändra storlek på en virtuell dator.

I listan över rekommendationer, klickar du på den **rätt storlek för eller Stäng av underutnyttjade virtuella datorer** rekommendation. I listan över kandidater för virtuell dator, väljer du en virtuell dator för att ändra storlek på och klicka sedan på den virtuella datorn. Den virtuella datorns information visas så att du kan kontrollera mätvärden för resursutnyttjande. Den **potentiella årliga besparingar** värdet är vad du kan spara om du stänger ner eller ta bort den virtuella datorn. Ändra storlek på en virtuell dator kommer förmodligen att spara pengar, men du kommer inte att spara med potentiella årliga besparingar.

![Information om rekommendation](./media/tutorial-acm-opt-recommendations/recommendation-details.png)

Kontrollera användningen av den virtuella datorn för att bekräfta att det är en lämplig storleksändring kandidat i VM-information.

![VM-information](./media/tutorial-acm-opt-recommendations/vm-details.png)

Observera den aktuella virtuella datorns storlek. När du har kontrollerat att den virtuella datorn ska ändras, Stäng VM-information så att du ser listan över virtuella datorer.

Välj i listan över kandidater att Stäng av eller ändra storlek på, **ändra storlek på den virtuella datorn**.
![Ändra storlek på den virtuella datorn](./media/tutorial-acm-opt-recommendations/resize-vm.png)

Därefter visas en lista med tillgängliga storleksändring alternativ. Välj det som ger bästa prestanda och kostnadseffektivitet för ditt scenario. I följande exempel visas valt alternativet ändrar storlek från en **DS14\_V2** till en **DS13\_V2**. Följa rekommendationen sparar 551.30 $/ månad eller $6,615.60/ år.

![Välj en storlek](./media/tutorial-acm-opt-recommendations/choose-size.png)

När du väljer en lämplig storlek, klickar du på **Välj** att starta åtgärden storleksändring.

Ändra storlek på kräver ett aktivt aktiv virtuell dator startas om. Om den virtuella datorn är i en produktionsmiljö, rekommenderar vi att du kör storleksändringen efter kontorstid. Schemaläggning av omstarten kan minska avbrott som orsakas av tillfälligt otillgängliga.

## <a name="verify-the-action"></a>Bekräfta åtgärden

När VM storleksändringen är klar visas ett meddelande om Azure.

![Ändrade meddelande](./media/tutorial-acm-opt-recommendations/resized-notification.png)

## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen lärde du dig att:

> [!div class="checklist"]
> * Visa optimering rekommendationer att visa potentiella ineffektivitet
> * Agera på en rekommendation att ändra storlek på en virtuell dator till ett mer kostnadseffektivt alternativ
> * Bekräfta åtgärden för att säkerställa att den virtuella datorn har har ändrats

Om du inte har redan läst artikel kostnadshantering bästa praxis, ger det övergripande vägledning och principer för att tänka på för att hantera kostnader.

> [!div class="nextstepaction"]
> [Kostnad rekommenderade metoder](cost-mgt-best-practices.md)
