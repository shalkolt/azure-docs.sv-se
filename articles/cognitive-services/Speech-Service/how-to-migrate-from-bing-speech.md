---
title: Migrera från Bing-tal till Taltjänst
titleSuffix: Azure Cognitive Services
description: Läs om skillnaderna mellan Bing-taligenkänning och Speech-tjänsten baserat på en utvecklare av och migrera ditt program att använda Speech-tjänsten.
services: cognitive-services
author: wsturman
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: gracez
ms.openlocfilehash: baf9b9cd9b3f57c1d708dd404d59c036df6c169f
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49466655"
---
# <a name="migrate-from-bing-speech-to-the-speech-service"></a>Migrera från Bing-tal till Taltjänst

Använd den här artikeln för att migrera dina program från Bing-Taligenkänning till Speech-tjänsten.

Den här artikeln beskrivs skillnaderna mellan tal-API: er för Bing och Speech-tjänsten och föreslår strategier för att migrera dina program. Din prenumerationsnyckel för taligenkänning för Bing kommer inte godkännas av tal-tjänst. Du behöver en ny Speech Service-prenumeration.

En prenumerationsnyckel för taltjänsten ger åtkomst till följande funktioner. Varje funktion mäts separat, så att du bara debiteras för de funktioner du använder.

* [Tal till text](speech-to-text.md)
* [Anpassat tal till text](https://cris.ai)
* [Text till tal](text-to-speech.md)
* [Anpassad text till talade röster](how-to-customize-voice-font.md)
* [Talöversättning](speech-translation.md) (omfattar inte [textöversättning](../translator/translator-info-overview.md))

Den [tal SDK](speech-sdk.md) är en funktionell ersättning för klientbibliotek för Bing-taligenkänning, men använder ett annat API.

## <a name="comparison-of-features"></a>Jämförelse av funktioner

Speech-tjänsten är i stort sett i funktionen, plattform och programmeringsspråk språkparitet med Bing-taligenkänning med följande skillnader.

Funktion | Bing-taligenkänning | Tjänst för taligenkänning | Information
-|-|-|-
SDK FÖR C++ | : heavy_minus_sign: | : heavy_check_mark: | Speech Service har stöd för Windows och Linux
Java SDK | : heavy_check_mark: | : heavy_check_mark: | Speech Service har stöd för Android- och taligenkänning enheter
C#-SDK | : heavy_check_mark: | : heavy_check_mark: | Speech Service har stöd för Windows 10 UWP och .NET Standard 2.0
Kontinuerlig taligenkänning | 10 minuter | Obegränsat (med SDK) | Både tal för Bing och Speech Service WebSockets protokoll stöd för upp till 10 minuter per anrop. Men SDK tal automatiskt återansluts vid timeout eller koppla från.
Partiell eller mellanliggande resultat | : heavy_check_mark: | : heavy_check_mark: | Med WebSockets-protokollet eller SDK
Anpassade talmodeller | : heavy_check_mark: | : heavy_check_mark: | Bing-taligenkänning kräver en separat prenumeration för anpassat tal
Anpassade rösttyper | : heavy_check_mark: | : heavy_check_mark: | Bing-taligenkänning kräver en separat anpassad Voice-prenumeration
Få en 24-kHz | : heavy_minus_sign: | : heavy_check_mark: 
Igenkänning av talavsikt | Kräver separat LUIS-API-anrop | Integrerat (SDK) |  En LUIS-nyckel kan användas med Speech-tjänsten.
Enkel avsiktsigenkänning | : heavy_minus_sign: | : heavy_check_mark: 
Batch transkription av länge ljudfiler | : heavy_minus_sign: | : heavy_check_mark:
Igenkänningsläge | Manuell via slutpunkt URI | Automatisk | Igenkänning av läge är inte tillgänglig i Speech Service
Slutpunkten ort | Global | Regionala | Regionala slutpunkter förbättra svarstiden. En slutpunkt för global är beaktas för Speech-tjänsten.
REST API:er | : heavy_check_mark: | : heavy_check_mark: | REST API för taligenkänning är kompatibel med Bing-tal (olika slutpunkt). REST API: er stöd för text till tal och begränsade funktioner för tal till text.
WebSockets protokoll | : heavy_check_mark: | : heavy_check_mark: | API för taligenkänning Service WebSockets är kompatibel med Bing-tal (olika slutpunkt). Migrera till tal SDK: N om det är möjligt att förenkla din kod.
Tjänst-till-tjänst-API-anrop | : heavy_check_mark: | : heavy_minus_sign: | Tillhandahålla i Bing-taligenkänning via C#-Tjänstbibliotek. 
SDK med öppen källkod | : heavy_check_mark: | : heavy_minus_sign: |

Tal-tjänsten använder en prismodell som baseras på tidpunkt (i stället för en transaktion som bygger modell). Mer information finns i [prissättning för taltjänsten](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

## <a name="migration-strategies"></a>Migreringsstrategier

Om du eller din organisation har program i utveckling eller produktion som använder en Bing-Taligenkänning, bör du uppdatera dem för att använda Speech-tjänsten så snart som möjligt. Se den [Speech Service dokumentation](index.yml) för tillgängliga SDK: er, kodexempel och självstudier.

Med Taltjänsten [REST API: er](rest-apis.md) är kompatibla med API: er för Bing-tal. Om du använder Bing Speech REST-API: er, behöver du bara ändra REST-slutpunkten och växla till en prenumerationsnyckel för Speech Service.

Speech Service WebSockets-protokoll är också kompatibla med de som används av Bing-taligenkänning. Vi rekommenderar att utvecklingen av nya mål till tal-SDK, snarare än att använder WebSockets vi rekommenderar att du migrerar befintliga koden i SDK samt. Som med REST API: er kräver befintlig kod som använder Bing-taligenkänning via WebSockets dock endast en ändring i slutpunkt och en nyckel som har uppdaterats.

Om du använder ett klientbibliotek för Bing-taligenkänning för ett specifikt programmeringsspråk, migrera till den [tal SDK](speech-sdk.md) kräver ändringar i ditt program eftersom API: et är olika. Tal-SDK kan göra din kod enklare samtidigt som också ger dig tillgång till nya funktioner.

Tal-SDK stöder för närvarande, C# (Windows 10, UWP, .NET Standard), Java (Android och anpassade enheter), Objective C (iOS), C++-Windows och Linux- och JavaScript. API: er för alla plattformar som liknar varandra, underlätta utveckling för flera plattformar.

Speech-tjänsten tillhandahåller inte för närvarande en global slutpunkt. Du behöver att avgöra om ditt program fungerar effektivt med hjälp av en enda regional slutpunkt för alla dess trafik. Annars kan du använda geoplats för att fastställa den effektivaste slutpunkten. Du behöver en separat Speech Service-prenumeration i varje region som du använder.

Om ditt program använder långlivade anslutningar och kan inte använda en tillgängliga SDK: N, kan du använda en WebsSockets-anslutning och hantera 10 minuters timeout-gränsen genom att ansluta vid lämplig tidpunkter.

Kom igång med tal-SDK:

1. Ladda ned den [tal SDK](speech-sdk.md).
1. Gå igenom Taltjänsten [snabbstartguider](quickstart-csharp-dotnet-windows.md), [självstudier](how-to-recognize-intents-from-speech-csharp.md), och titta på den [kodexempel](samples.md) att få med de nya API: er.
1. Uppdatera appen till tal-tjänst och API: er.

## <a name="support"></a>Support

Bing-taligenkänning kunder bör kontakta kundsupporten genom att öppna en [supportärende](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest). Du kan även kontakta oss om ditt behov av support kräver en [teknisk supportavtalet](https://azure.microsoft.com/support/plans/).

Speech Service, SDK och API-stöd finns Taltjänsten [supportsidan](support.md).

## <a name="next-steps"></a>Nästa steg

* [Prova Speech Service kostnadsfritt](get-started.md)
* [Snabbstart: Känna igen tal i en UWP-app med hjälp av tal-SDK](quickstart-csharp-uwp.md)

## <a name="see-also"></a>Se också
* [Viktig information Speech-tjänsten](releasenotes.md)
* [Vad är Speech-tjänsten](overview.md)
* [Med Taltjänsten och SDK-dokumentation](speech-sdk.md#get-the-sdk)
