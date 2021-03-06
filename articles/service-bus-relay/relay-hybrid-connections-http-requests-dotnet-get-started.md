---
title: Komma igång med HTTP-begäranden för Azure Relay-hybridanslutningar i .NET | Microsoft Docs
description: Skriva ett C#-konsolprogram för HTTP-begäranden för Azure Relay-hybridanslutningar i .NET.
services: service-bus-relay
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: d1386900-b942-4abf-acfc-38d2ef826253
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 08/16/2018
ms.author: spelluru
ms.openlocfilehash: e66a1651a46cfaeb7fb8b232eeb7cf6a2fb8044d
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/28/2018
ms.locfileid: "47451230"
---
# <a name="get-started-with-relay-hybrid-connections-http-requests-in-net"></a>Komma igång med HTTP-begäranden för Relay-hybridanslutningar i .NET
[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

Den här självstudien innehåller en introduktion till [Azure Relay-hybridanslutningar](relay-what-is-it.md#hybrid-connections). Lär dig att skapa ett klientprogram med Microsoft .NET som skickar begäranden till ett motsvarande lyssnarprogram. 

## <a name="what-will-be-accomplished"></a>Detta kommer att utföras
Hybridanslutningar kräver både en klient- och en serverkomponent. I den här självstudien har du slutfört de här stegen för att skapa två konsolprogram:

1. Skapa ett Relay-namnområde med Azure Portal.
2. Skapa en hybridanslutning i det namnområdet med Azure Portal.
3. Skriva ett serverkonsolprogram (lyssnare) för att ta emot begäranden.
4. Skriva ett klientkonsolprogram (avsändare) för att skicka begäranden.

## <a name="prerequisites"></a>Nödvändiga komponenter

För att slutföra den här självstudien, finns följande förhandskrav:

* [Visual Studio 2015 eller senare](http://www.visualstudio.com). I exemplen i den här självstudiekursen används Visual Studio 2017.
* En Azure-prenumeration.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-by-using-the-azure-portal"></a>1. Skapa ett namnområde med Azure Portal
Om du redan har skapat ett Relay-namnområde går du till [Skapa en hybridanslutning med Azure Portal](#2-create-a-hybrid-connection-using-the-azure-portal).

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-by-using-the-azure-portal"></a>2. Skapa en hybridanslutning med Azure Portal
Om du redan har skapat en hybridanslutning går du till [Skapa ett serverprogram](#3-create-a-server-application-listener).

[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a>3. Skapa ett serverprogram (lyssnare)
För att lyssna på och ta emot meddelanden från Relay skriver du ett C#-konsolprogram i Visual Studio.

[!INCLUDE [relay-hybrid-connections-http-requests-dotnet-get-started-server](../../includes/relay-hybrid-connections-http-requests-dotnet-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a>4. Skapa ett klientprogram (avsändare)
För att skicka meddelanden till Relay skriver du ett C#-konsolprogram i Visual Studio.

[!INCLUDE [relay-hybrid-connections-http-requests-dotnet-get-started-client](../../includes/relay-hybrid-connections-http-requests-dotnet-get-started-client.md)]

## <a name="5-run-the-applications"></a>5. Köra programmen
1. Kör serverprogrammet. Du kan se följande text i konsolfönstret:

    ```
    Online
    Server listening
    ```
1. Kör klientprogrammet. `hello!` visas i klientfönstret. Klienten har skickat en HTTP-begäran till servern och servern svarade med en `hello!`. 
3. Om du vill stänga konsolfönstret nu trycker du på **RETUR** i båda konsolfönsterna. 

Grattis, du har skapat ett end-to-end hybridanslutningsprogram!

## <a name="next-steps"></a>Nästa steg

* [Vanliga frågor och svar om Relay](relay-faq.md)
* [Skapa ett namnområde](relay-create-namespace-portal.md)
* [Kom igång med Node](relay-hybrid-connections-node-get-started.md)

