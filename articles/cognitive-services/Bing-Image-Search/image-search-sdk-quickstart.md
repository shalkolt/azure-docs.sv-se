---
title: 'Snabbstart: Sök efter bilder med API för bildsökning i Bing och C#'
description: Använd den här snabbstarten till att göra din första bildsökning med API för bildsökning i Bing, som är en adapterklass för API:t och innehåller samma funktioner. Den här enkla C#-appen skickar en bildsökningsfråga, parsar JSON-svaret och visar webbadressen till den första bild som returneras.
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: aahi
ms.openlocfilehash: edebd1361e39a338672b4249dd159e5c1d4078ce
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/19/2018
ms.locfileid: "46294160"
---
# <a name="quickstart-search-for-images-with-the-bing-image-search-sdk-and-c"></a>Snabbstart: Sök efter bilder med API för bildsökning i Bing och C#

Använd den här snabbstarten till att göra din första bildsökning med API för bildsökning i Bing, som är en adapterklass för API:t och innehåller samma funktioner. Den här enkla C#-appen skickar en bildsökningsfråga, parsar JSON-svaret och visar webbadressen till den första bild som returneras.

Källkoden för det här exemplet finns på [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingImageSearch) tillsammans med ytterligare felhantering och kommentarer.

## <a name="prerequisites"></a>Nödvändiga komponenter

* Valfri version av [Visual Studio 2017](https://visualstudio.microsoft.com/vs/whatsnew/).
* [Cognitive Image Search NuGet-paketet](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.ImageSearch/1.2.0).

Om du vill installera SDK:n för Bildsökning i Bing i Visual Studio, så använd alternativet `Manage NuGet Packages` i Solution Explorer i Visual Studio.

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]


## <a name="create-and-initialize-the-application"></a>Skapa och initiera appen

Skapa först ett nytt C#-konsolprogram i Visual Studio. Lägg sedan till följande paket i projektet.

```csharp
using System;
using System.Linq;
using Microsoft.Azure.CognitiveServices.Search.ImageSearch;
using Microsoft.Azure.CognitiveServices.Search.ImageSearch.Models;
```

Skapa variabler för en giltig prenumerationsnyckel, bildresultatet som ska returneras av Bing och en sökterm i main-metoden i projektet. Sedan skapa en instans av bildsökningsklienten med hjälp av nyckeln.

```csharp
//IMPORTANT: replace this variable with your Cognitive Services subscription key
string subscriptionKey = "ENTER YOUR KEY HERE";
//stores the image results returned by Bing
Images imageResults = null;
// the image search term to be used in the query
string searchTerm = "canadian rockies";
//initialize the client
var client = new ImageSearchAPI(new ApiKeyServiceClientCredentials(subscriptionKey));
```

## <a name="send-a-search-query-using-the-client"></a>Skicka en sökfråga med hjälp av klienten

Använd klienten för att söka med en frågetext:

```csharp
// make the search request to the Bing Image API, and get the results"
imageResults = client.Images.SearchAsync(query: searchTerm).Result; //search query
```

## <a name="parse-and-view-the-first-image-result"></a>Parsa och visa det första avbildningresultatet

Parsa bild esultatet som returneras i svaret.
Om svaret innehåller sökresultat ska du lagra och skriva ut information om det första resultatet, som en webbadress till miniatyrbilden, en webbadress till originalet och det totala antalet returnerade bilder.  

```csharp
if (imageResults != null)
{
    //display the details for the first image result.
    var firstImageResult = imageResults.Value.First();
    Console.WriteLine($"\nTotal number of returned images: {imageResults.Value.Count}\n");
    Console.WriteLine($"Copy the following URLs to view these images on your browser.\n");
    Console.WriteLine($"URL to the first image:\n\n {firstImageResult.ContentUrl}\n");
    Console.WriteLine($"Thumbnail URL for the first image:\n\n {firstImageResult.ThumbnailUrl}");
    Console.ReadKey();
}
```

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [Självstudie om enkel app för bildsökning i Bing](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app)

## <a name="see-also"></a>Se även

* [Vad är bildsökning i Bing?](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)  
* [Prova en interaktiv demo online](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
* [Hämta en kostnadsfri åtkomstnyckel för Cognitive Services](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)  
* [.NET-exempel för Azure Cognitive Services SDK](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
* [Dokumentation om Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services)
* [API-referens för Bildsökning i Bing](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)
