---
title: ta med fil
description: ta med fil
services: automation
author: georgewallace
ms.service: automation
ms.topic: include
ms.date: 04/05/2018
ms.author: gwallace
ms.custom: include file
ms.openlocfilehash: 34cae9172d9b024bd6866742d39d82ad496bfc52
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45570438"
---
| Resurs | Övre gräns |Anteckningar|
| --- | --- |---|
| Maximalt antal nya jobb som ska skickas med 30 sekunders mellanrum per Automation-konto (icke schemalagda jobb) |100 |När den här gränsen träffar, misslyckas de efterföljande begärandena om att skapa ett jobb. Klienten får ett felsvar.|
| Maximalt antal samtidiga jobb som körs på samma instans av tid per Automation-konto (icke schemalagda jobb) |200 |När den här gränsen träffar, misslyckas de efterföljande begärandena om att skapa ett jobb. Klienten får ett felsvar.|
| Maximalt antal moduler som kan importeras med 30 sekunders mellanrum per Automation-konto |5 ||
| Maxstorlek på en modul |100 MB ||
| Kör jobbtid - kostnadsfri nivå |500 minuter per prenumeration per kalendermånad ||
| Högsta mängd diskutrymme som tillåts per sandbox  **<sup>1</sup>** |1 GB |Gäller för Azure sandboxar|
| Högsta mängd minne som tilldelas en sandbox  **<sup>1</sup>** |400 MB |Gäller för Azure sandboxar|
| Maxantal nätverk sockets tillåts per sandbox  **<sup>1</sup>** |1000 |Gäller för Azure sandboxar|
| Maximala körtid som tillåts per runbook  **<sup>1</sup>** |tre timmar |Gäller för Azure sandboxar|
| Maxantal Automation-konton i en prenumeration |Obegränsat ||
|Maximalt antal samtidiga jobb som ska köras på en enda Hybrid Runbook Worker|50 ||
| Max Runbook-jobbet parametrar storlek   | 512 kb||
| Max Runbook-parametrar   | 50|Du kan skicka en JSON- eller XML-sträng till en parameter och parsa den med runbook om du har nått gränsen för 50 parametern|
| Maxstorlek för webhook-nyttolasten |  512 kb|

**<sup>1</sup>**  en Sandbox-miljön är en delad miljö som kan användas av flera jobb, jobb med hjälp av samma sandlåda är bundna av resursbegränsningar av sandbox-miljön.
