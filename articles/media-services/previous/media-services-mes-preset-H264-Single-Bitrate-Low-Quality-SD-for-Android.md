---
title: H264, Enkel bithastighet, låg kvalitet, SD för Android | Microsoft Docs
description: Avsnittet ger en översikt över den **H264 enkel bithastighet låg kvalitet SD för Android** uppgiften förinställd.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: 67d3446d-e3b8-419f-bbf8-e5149c108531
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2018
ms.author: juliako
ms.openlocfilehash: 4c69923518a0e886bb9174750be201b05be442cd
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50250337"
---
# <a name="h264-single-bitrate-low-quality-sd-for-android"></a>H264 Enkel bithastighet låg kvalitet, SD för Android
`Media Encoder Standard` definierar en uppsättning kodning förinställningar som du kan använda när du skapar kodningsjobb. Du kan använda en `preset name` att ange i vilket format du vill koda din mediefil. Du kan också skapa egna JSON eller XML-baserade förinställningar (med UTF-8- eller UTF-16-kodning. Sedan skickar du den anpassade förinställda till kodaren. Lista över alla förvalda namn som stöds av det här `Media Encoder Standard` encoder, se [uppgift förinställningar för Media Encoder Standard](media-services-mes-presets-overview.md).  
  
 Det här avsnittet beskrivs de `H264 Single Bitrate Low Quality SD for Android` förinställda i XML- och JSON-format.  
  
 Den här förinställda ger en enda MP4-filen med en bithastighet 56 kbit/s och stereo AAC-ljud. Detaljerad information om profilen med flera bithastigheter, sampling hastighet och så vidare på detta förinställda ska undersöka XML eller JSON som definieras nedan. Förklaringar av vad varje element i dessa förinställningar innebär och de giltiga värdena för varje element finns i den [Media Encoder Standard schemat](media-services-mes-schema.md) avsnittet.  
  
 XML  
  
```  
<?xml version="1.0" encoding="utf-16"?>  
<Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">  
  <Encoding>  
    <H264Video>  
      <KeyFrameInterval>00:00:05</KeyFrameInterval>  
      <SceneChangeDetection>true</SceneChangeDetection>  
      <H264Layers>  
        <H264Layer>  
          <Bitrate>56</Bitrate>  
          <Width>176</Width>  
          <Height>144</Height>  
          <FrameRate>12/1</FrameRate>  
          <Profile>Baseline</Profile>  
          <Level>2</Level>  
          <BFrames>0</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>false</AdaptiveBFrame>  
          <EntropyMode>Cavlc</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>56</MaxBitrate>  
        </H264Layer>  
      </H264Layers>  
      <Chapters />  
    </H264Video>  
    <AACAudio>  
      <Profile>HEAACV2</Profile>  
      <Channels>2</Channels>  
      <SamplingRate>48000</SamplingRate>  
      <Bitrate>24</Bitrate>  
    </AACAudio>  
  </Encoding>  
  <Outputs>  
    <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">  
      <MP4Format />  
    </Output>  
  </Outputs>  
</Preset>  
```  
  
 JSON  
  
```  
{  
  "Version": 1.0,  
  "Codecs": [  
    {  
      "KeyFrameInterval": "00:00:05",  
      "SceneChangeDetection": true,  
      "H264Layers": [  
        {  
          "Profile": "Baseline",  
          "Level": "2",  
          "Bitrate": 56,  
          "MaxBitrate": 56,  
          "BufferWindow": "00:00:05",  
          "Width": 176,  
          "Height": 144,  
          "ReferenceFrames": 3,  
          "EntropyMode": "Cavlc",  
          "Type": "H264Layer",  
          "FrameRate": "12/1"  
        }  
      ],  
      "Type": "H264Video"  
    },  
    {  
      "Profile": "HEAACV2",  
      "Channels": 2,  
      "SamplingRate": 48000,  
      "Bitrate": 24,  
      "Type": "AACAudio"  
    }  
  ],  
  "Outputs": [  
    {  
      "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",  
      "Format": {  
        "Type": "MP4Format"  
      }  
    }  
  ]  
}  
```
