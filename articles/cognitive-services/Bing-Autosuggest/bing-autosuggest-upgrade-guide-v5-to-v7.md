---
title: Uppgradera automatiska förslag i Bing API v5 till v7
titlesuffix: Azure Cognitive Services
description: Identifierar delarna av programmet som du behöver uppdatera om du vill använda version 7.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-autosuggest
ms.topic: conceptual
ms.date: 01/12/2017
ms.author: scottwhi
ms.openlocfilehash: 8ada9ab6622ea12e48475230787168d90e95ec0d
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2018
ms.locfileid: "48829876"
---
# <a name="autosuggest-api-upgrade-guide"></a>Uppgraderingsguide för API för automatiska förslag

Den här uppgraderingshandboken identifierar ändringar mellan version 5 och version 7 för automatiska förslag i Bing. Använd den här guiden hjälper dig att identifiera delarna av programmet som du behöver uppdatera om du vill använda version 7.

## <a name="breaking-changes"></a>Icke-bakåtkompatibla ändringar

### <a name="endpoints"></a>Slutpunkter

- Versionsnumret för den slutpunkt som har ändrats från v5 till v7. Till exempel https://api.cognitive.microsoft.com/bing/\ * \*v7.0 ** / förslag.

### <a name="error-response-objects-and-error-codes"></a>Fel svarsobjekt och felkoder

- Alla misslyckade begäranden ska nu innehålla en `ErrorResponse` objekt i svarstexten.

- Lagt till följande fält i den `Error` objekt.  
  - `subCode`&mdash;Partitionerar felkoden i diskreta buckets, om möjligt
  - `moreDetails`&mdash;Mer information om felet som beskrivs i den `message` fält

- Ersatt v5-felkoder med följande möjliga `code` och `subCode` värden.

|Kod|Obligatorisk|Beskrivning
|-|-|-
|ServerError|UnexpectedError<br/>ResourceError<br/>NotImplemented|Bing returnerar ServerError när något av de underordnade kod inträffar. Svaret innehåller de här felen om HTTP-statuskoden är 500.
|InvalidRequest|ParameterMissing<br/>ParameterInvalidValue<br/>HttpNotAllowed<br/>Blockerad|Bing returnerar InvalidRequest när någon del av begäran inte är giltig. Till exempel en obligatorisk parameter saknas eller ett parametervärde är inte giltig.<br/><br/>Om felet är ParameterMissing eller ParameterInvalidValue, är HTTP-statuskod 400.<br/><br/>Om felet är HttpNotAllowed, HTTP-statuskod 410.
|RateLimitExceeded||Bing returnerar RateLimitExceeded varje gång du överskrider din frågor per sekund (QPS) eller frågor per månad (QPM) kvot.<br/><br/>Bing returnerar HTTP-statuskod 429 om du har överskridit Indexlagring och 403 om du har överskridit QPM.
|InvalidAuthorization|AuthorizationMissing<br/>AuthorizationRedundancy|Bing returnerar InvalidAuthorization när Bing inte kan autentisera anroparen. Till exempel den `Ocp-Apim-Subscription-Key` huvud saknas eller prenumerationsnyckeln är inte giltig.<br/><br/>Redundans inträffar om du anger mer än en autentiseringsmetod.<br/><br/>Om felet är InvalidAuthorization, är HTTP-statuskod 401.
|InsufficientAuthorization|AuthorizationDisabled<br/>AuthorizationExpired|Bing returnerar InsufficientAuthorization när anroparen inte har behörighet att komma åt resursen. Detta kan inträffa om prenumerationsnyckeln har inaktiverats eller har upphört att gälla. <br/><br/>Om felet är InsufficientAuthorization, är HTTP-statuskod 403.

- Följande mappar tidigare felkoder till de nya koderna. Om du har gått ett beroende på v5 felkoder, måste din kod uppdateras också.

|Versionskod 5|Version 7 code.subCode
|-|-
|RequestParameterMissing|InvalidRequest.ParameterMissing
RequestParameterInvalidValue|InvalidRequest.ParameterInvalidValue
ResourceAccessDenied|InsufficientAuthorization
ExceededVolume|RateLimitExceeded
ExceededQpsLimit|RateLimitExceeded
Disabled|InsufficientAuthorization.AuthorizationDisabled
UnexpectedError|ServerError.UnexpectedError
DataSourceErrors|ServerError.ResourceError
AuthorizationMissing|InvalidAuthorization.AuthorizationMissing
HttpNotAllowed|InvalidRequest.HttpNotAllowed
UserAgentMissing|InvalidRequest.ParameterMissing
NotImplemented|ServerError.NotImplemented
InvalidAuthorization|InvalidAuthorization
InvalidAuthorizationMethod|InvalidAuthorization
MultipleAuthorizationMethod|InvalidAuthorization.AuthorizationRedundancy
ExpiredAuthorizationToken|InsufficientAuthorization.AuthorizationExpired
InsufficientScope|InsufficientAuthorization
Blockerad|InvalidRequest.Blocked

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [Använd och visa krav](./UseAndDisplayRequirements.md)
