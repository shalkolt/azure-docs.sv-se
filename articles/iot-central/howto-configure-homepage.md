---
title: Konfigurera webbsida programmets Azure IoT Central | Microsoft Docs
description: Lär dig hur du konfigurerar du startsidan för Azure IoT Central programmet som en builder.
author: tbhagwat3
ms.author: tanmayb
ms.date: 07/10/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 609dc8aa1305833fb3677de6fbe95cf490237a4a
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/17/2018
ms.locfileid: "45731129"
---
## <a name="configuring-homepage"></a>Konfigurera startsida

Startsidan är den sida som läses in när användare som har åtkomst navigera till programmets URL. Om du har valt antingen ”exempel Contoso” eller ”exempel Devkits” programmallar när du skapar ditt program, ska ditt program ha fördefinierade startsidor. Om du valde å andra sidan programmallen ”anpassade program”, kommer din startsida måste anges.

Här är till exempel startsidan för programmen baserat på mallen ”Contoso Sample”. Välj först för att anpassa startsidan för ditt program, **redigera** längst upp till höger. 

![Startsida för programmen baserat på mallen ”Contoso exemplet”](media\howto-configure-homepage\image1.png)

Att välja **redigera**, öppnas instrumentpanelen biblioteket på en panel till vänster. Det finns många typer av paneler och instrumentpanelen primitiver som kan läggas till att anpassa din startsida.

![Instrumentpanelen bibliotek](media\howto-configure-homepage\image2.png)

Du kan till exempel lägga till en **inställningar och egenskaper** rutan för att visa ett urval av de aktuella värdena för inställningar och egenskaper. Om du vill göra det väljer du först en **enheten mallen** Välj sedan en **enhetsinstansen**. Efter som tillhandahåller panelen en rubrik och väljer en **inställningen** eller en **egenskapen** ska visas. I det här fallet har vi valt **ange temperatur**. Klicka på **klar** leder till den här panelen visas på startsidan.

![”Konfigurera enhetsinformation” formuläret med information om inställningar och egenskaper](media\howto-configure-homepage\image3.png)

När en operatör visar startsidan, kan de nu se den här panelen som visar de egenskaper eller inställningarna för enheten:

![Fliken ”instrumentpanel” med visas inställningar och egenskaper för panelen](media\howto-configure-homepage\image4.png)

Experimentera med de olika andra typerna av paneler i biblioteket att upptäcka hur du kan anpassa ditt programs startsida ännu mer.

## <a name="next-steps"></a>Nästa steg

Nu när du har lärt dig hur du konfigurerar din Azure IoT Central startsida, kan du:

> [!div class="nextstepaction"]
> [Lär dig hur du förbereder och ladda upp bilder](howto-prepare-images.md)
