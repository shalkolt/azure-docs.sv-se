---
title: Kodning i molnet med Azure Media Services | Microsoft Docs
description: Det här avsnittet beskriver hur du kodning med Azure Media Services
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: bee74f0399def142915aa26d15ecfa671925f405
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50025590"
---
# <a name="encoding-with-azure-media-services"></a>Kodning med Azure Media Services

Azure Media Services kan du koda din digitala mediefiler med hög kvalitet till format som kan spelas upp på en mängd olika webbläsare och enheter. Du kanske vill strömma ditt innehåll i Apples HLS- eller MPEG DASH-formaten. Media Services kan du analysera video- eller ljudinnehåll innehållet. Det här avsnittet ger vägledning om att koda ditt innehåll med Media Services v3.

Om du vill koda med Media Services v3, måste du skapa en transformering och ett jobb. En transformering definierar receptet för kodning inställningar och utdata och jobbet är en instans av receptet. Mer information finns i [transformeringar och jobb](transform-concept.md)

När kodning med Azure Media Services, använder du förinställningar som talar om kodaren hur inkommande mediefiler ska bearbetas. Du kan till exempel ange video upplösning och/eller antalet ljud kanaler som du vill i det kodade innehållet. 

Du kan komma igång snabbt med en av de rekommenderade inbyggda förinställningar baserat på branschens bästa praxis eller du kan välja att skapa en anpassad förinställning om du vill rikta in dina specifika krav för scenario eller enhet. Mer information finns i [koda med en anpassad transformering](customize-encoder-presets-how-to.md). 

## <a name="built-in-presets"></a>Inbyggda förinställningar

Media Services stöder för närvarande följande inbyggda förinställningar för kodning:  

|**Förinställda namnet**|**Scenario**|**Detaljer**|
|---|---|---|
|**AudioAnalyzerPreset**|Analysera ljud|Förinställningen gäller en fördefinierad uppsättning AI-baserade analysis-åtgärder, inklusive taltranskription. Förinställningen stöder för närvarande, bearbetning av innehåll med en enda ljudspår.<br/>Du kan ange språket för ljud nyttolasten i indata i BCP-47 formatet för ”språk tagg-regioner” (till exempel ”en-US”). Listan över språk som stöds är ”en-US”, ”en-GB”, ”es-ES”, ”es-MX”, fr-FR, it-IT, ja-JP, pt-BR, zh-CN.|
|**VideoAnalyzerPreset**|Analysera ljud och video|Extraherar insikter (omfattande metadata) från både ljud och video och matar ut en fil i JSON-format. Du kan ange om du bara vill lyfta ut kunskaper ljud vid bearbetning av en videofil. Mer information finns i [analysera video](analyze-videos-tutorial-with-api.md).|
|**BuiltInStandardEncoderPreset**|Strömning|Används för att ange en inbyggd förinställning för encoding indatavideon med Standard-kodare. <br/>Följande förinställningar stöds för närvarande:<br/>**EncoderNamedPreset.AdaptiveStreaming** (rekommenderas). Mer information finns i [autogenerering en bithastighetsstege](autogen-bitrate-ladder.md).<br/>**EncoderNamedPreset.AACGoodQualityAudio** -producerar en enda MP4-fil som innehåller endast stereo ljud kodade med 192 kbit/s.<br/>**EncoderNamedPreset.H264MultipleBitrate1080p** -producerar en uppsättning 8 GOP-justerad MP4-filer, sträcker sig från 6000 kbit/s till 400 kbit/s och stereo AAC-ljud. Lösning startas vid 1080p och ned 360 p.<br/>**EncoderNamedPreset.H264MultipleBitrate720p** -producerar en uppsättning 6 GOP-justerad MP4-filer, sträcker sig från 3400 kbit/s till 400 kbit/s och stereo AAC-ljud. Lösning startas vid 720p och ned 360 p.<br/>**EncoderNamedPreset.H264MultipleBitrateSD** -producerar en uppsättning 5 GOP-justerad MP4-filer, sträcker sig från 1600 kbit/s till 400 kbit/s och stereo AAC-ljud. Lösning startas på 480 pixlar och ned 360 p.<br/><br/>Mer information finns i [överför, koda och strömma filer](stream-files-tutorial-with-api.md).|
|**StandardEncoderPreset**|Strömning|Beskriver inställningarna som ska användas vid kodning indatavideon med Standard-kodare. <br/>Använd denna förinställning när du anpassar transformeringen förinställningar. Mer information finns i [hur du anpassar transformeringen förinställningar](customize-encoder-presets-how-to.md).|

## <a name="custom-presets"></a>Anpassade förinställningar

Media Services stöder helt anpassa alla värden i förinställningar för att uppfylla dina specifika kodning behov och krav. Du använder den **StandardEncoderPreset** förinställda när du anpassar transformeringen förinställningar. För en detaljerade förklaringar och exempel finns i [hur du anpassar encoder förinställningar](customize-encoder-presets-how-to.md).

## <a name="scaling-encoding-in-v3"></a>Skala kodning i v3

För närvarande kan kunder har du använder Azure portal eller AMS v2 API: er för att ange ru: er (enligt beskrivningen i [skala mediebearbetning](../previous/media-services-scale-media-processing-overview.md). 

## <a name="next-steps"></a>Nästa steg

### <a name="tutorials"></a>Självstudier

Följande tutorals visar att koda ditt innehåll med Media Services:

* [Ladda upp, koda och strömma med Azure Media Services](stream-files-tutorial-with-api.md)
* [Analysera videoklipp med Azure Media Services](analyze-videos-tutorial-with-api.md)

### <a name="code-samples"></a>Kodexempel

Följande kodexempel innehåller koden som visar hur du koda med Media Services:

* [.NET Core](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore)
* [Azure CLI](https://github.com/Azure/azure-docs-cli-python-samples/tree/master/media-services)

### <a name="sdks"></a>SDK:er

Du kan använda någon av följande stöds Media Services v3 SDK: erna för att koda ditt innehåll.

* [Azure CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
* [REST](https://docs.microsoft.com/rest/api/media/transforms)
* [NET](https://docs.microsoft.com/dotnet/api/overview/azure/mediaservices/management?view=azure-dotnet)
* [Java](https://docs.microsoft.com/java/api/overview/azure/mediaservices)
* [Python](https://docs.microsoft.com/python/api/overview/azure/media-services?view=azure-python)

