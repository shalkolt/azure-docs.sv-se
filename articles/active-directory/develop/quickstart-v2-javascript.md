---
title: Azure AD v2 JavaScript Snabbstart | Microsoft Docs
description: Lär dig hur JavaScript-program kan anropa ett API som kräver åtkomsttoken genom Azure Active Directory v2.0-slutpunkten
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 69c77896f894201d1419aaef33470a02ac45ff91
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/24/2018
ms.locfileid: "49986296"
---
# <a name="quickstart-sign-in-users-and-acquire-an-access-token-from-a-javascript-application"></a>Snabbstart: Logga in användare och hämta en åtkomsttoken från ett JavaScript-program

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

I den här snabbstarten får du lära dig hur du använder ett kodexempel som visar hur ett JavaScript ensidesapplikation (SPA) kan logga in personliga konton, arbetar och skolkonton, få en åtkomsttoken att anropa Microsoft Graph API eller alla webb-API.

![Så här fungerar den exempelapp som skapas med den här snabbstarten](media/quickstart-v2-javascript/javascriptspa-intro.png)

> [!div renderon="docs"]
> ## <a name="register-your-application-and-download-your-quickstart-app"></a>Registrera appen och ladda ned snabbstartsappen
>
> #### <a name="step-1-register-your-application"></a>Steg 1: Registrera ditt program
>
> 1. Logga in på den [Azure-portalen](https://portal.azure.com/) att registrera ett program.
> 1. Om ditt konto får du tillgång till fler än en klient, Välj ditt konto i övre högra hörnet och ange din portal session med önskade Azure AD-klient.
> 1. I det vänstra navigeringsfönstret väljer du den **Azure Active Directory** tjänsten och välj sedan **appregistreringar (förhandsversion) > Ny registrering**.
> 1. När den **registrera ett program** visas, ange ett namn för ditt program.
> 1. Under **stöds kontotyper**väljer **konton alla organisationskatalog och personliga Microsoft-konton**.
> 1. Välj den **Web** plattform under den **omdirigerings-URI** avsnittet och ange värdet `http://localhost:30662/`.
> 1. När du är klar väljer **registrera**.  I appen **översikt** sidan, Skriv ned den **(klient)-ID: T** värde.
> 1. Den här snabbstarten krävs det [flöde beviljat med Implicit](v2-oauth2-implicit-grant-flow.md) aktiveras. I det vänstra navigeringsfönstret för det registrerade programmet, väljer **autentisering**.
> 1. I **avancerade inställningar**under **Implicit beviljande**, aktivera båda **ID-token** och **åtkomsttoken** kryssrutorna. ID-token och åtkomsttoken krävs eftersom den här appen måste du logga in användare och anropa ett API.
> 1. Välj **Spara**.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-the-azure-portal"></a>Steg 1: Konfigurera ditt program i Azure portal
> För kodexempel för den här snabbstarten ska fungera måste du Lägg till en omdirigerings-URI som `http://localhost:30662/` och aktivera **Implicit beviljande**.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Gör dessa ändringar för mig]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Redan konfigurerat](media/quickstart-v2-javascript/green-check.png) ditt program har konfigurerats med dessa attribut.

#### <a name="step-2-download-the-project"></a>Steg 2: Ladda ned projektet

Du kan välja något av följande alternativ som är lämpliga till utvecklingsmiljön.
* [Hämta projektfilerna core - för en webbserver, till exempel Node.js](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip)
* [Ladda ned Visual Studio-projekt](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/vsquickstart.zip)

Extrahera zip-filen till en lokal mapp, till exempel **C:\Azure-Samples**.

#### <a name="step-3-configure-your-javascript-app"></a>Steg 3: Konfigurera JavaScript-app

> [!div renderon="docs"]
> Redigera `index.html` och Ersätt `Enter_the_Application_Id_here` under `applicationConfig` med program-ID för den app som du just registrerade.

> [!div class="sxs-lookup" renderon="portal"]
> Redigera `index.html` och Ersätt `applicationConfig` med:

```javascript
var applicationConfig = {
    clientID: "Enter_the_Application_Id_here",
    graphScopes: ["user.read"],
    graphEndpoint: "https://graph.microsoft.com/v1.0/me"
};
```
> [!NOTE]
>Om du använder [Node.js](https://nodejs.org/en/download/), *server.js* fil har konfigurerats för servern för att påbörja avlyssning av port 30662.
> Om du använder [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/), kodexemplet *.csproj* fil har konfigurerats för servern för att påbörja avlyssning av port 30662.
>

#### <a name="step-4-run-the-project"></a>Steg 4: Kör projektet

* Om du använder Node.js:

    1. Kör följande kommando från projektkatalogen att starta servern:

        ```batch
        npm install
        node server.js
        ```

    1. Öppna en webbläsare och gå till `http://localhost:30662/`.
    1. Klicka på **logga In** knappen för att börja logga in och sedan anropa Microsoft Graph API.


* Om du använder Visual Studio, se till att markera projektet lösningen och tryck sedan på **F5** köra projektet.

## <a name="more-information"></a>Mer information

### <a name="msaljs"></a>*msal.js*

MSAL finns i biblioteket som används för att logga in användare och token som används för att få åtkomst till ett API som skyddas av Microsoft Azure Active Directory (Azure AD)-begäran. I snabbstarten *index.html* innehåller en referens till biblioteket:

```html
<script src="https://secure.aadcdn.microsoftonline-p.com/lib/0.2.3/js/msal.min.js"></script>
```

Om du har noden installerad kan du också hämta den via npm:

```batch
npm install msal
```

### <a name="msal-initialization"></a>MSAL-initiering

Snabbstartskoden visar också hur du initierar biblioteket:

```javascript
var myMSALObj = new Msal.UserAgentApplication(applicationConfig.clientID, null, acquireTokenRedirectCallBack, {storeAuthStateInCookie: true, cacheLocation: "localStorage"});
```

> |Var  |  |
> |---------|---------|
> |`ClientId`     |Program-ID från appen som registrerats i Azure-portalen|
> |`authority`    |Det är URL: en utfärdare. Skicka *null* anger standard-behörighet till `https://login.microsoftonline.com/common`. Om din app är en enda klient (Sök mål konton i en katalog), anger ett värde och `https://login.microsoftonline.com/<tenant name or ID>`|
> |`tokenReceivedCallback`| Motringningsmetoden som anropas när autentiseringen omdirigeras tillbaka till appen. Här kan `acquireTokenRedirectCallBack` skickas. Det här är null om du använder loginPopup.|
> |`options`  |En samling av valfria parametrar. I det här fallet `storeAuthStateInCookie` och `cacheLocation` är valfri konfiguration. Se den [wiki](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/MSAL-basics#configuration-options) för mer information om alternativen. |

### <a name="sign-in-users"></a>Logga in användare

Följande kodfragment visar hur man loggar in användare:

```javascript
myMSALObj.loginPopup(applicationConfig.graphScopes).then(function (idToken) {
    //Callback code here
})
```

> |Var  |  |
> |---------|---------|
> | `scopes`   | (Valfritt) Innehåller omfattningar som begärs för användarens medgivande på inloggningstid. Till exempel `[ "user.read" ]` för Microsoft Graph eller `[ "<Application ID URL>/scope" ]` för anpassade webb-API: er (det vill säga `api://<Application ID>/access_as_user` ). Här kan `applicationConfig.graphScopes` skickas. |

> [!TIP]
> Alternativt kan du använda den `loginRedirect` metod för att omdirigera den aktuella sidan till inloggningssidan i stället för ett popup-fönster.

### <a name="request-tokens"></a>Begära token

MSAL har tre metoder som används för att hämta token: `acquireTokenRedirect`, `acquireTokenPopup` och `acquireTokenSilent`

#### <a name="get-a-user-token-silently"></a>Hämta en token obevakat

Den `acquireTokenSilent` metoden hanterar token anskaffning och förnyelse utan någon användarinteraktion. Efter den `loginRedirect` eller `loginPopup` metod körs för första gången `acquireTokenSilent` är den metod som ofta används för att hämta token som används för att komma åt skyddade resurser för efterföljande anrop. Anrop att begära eller förnya token görs tyst.

```javascript
myMSALObj.acquireTokenSilent(applicationConfig.graphScopes).then(function (accessToken) {
    // Callback code here
})
```

> |Var  |  |
> |---------|---------|
> | `scopes`   | Innehåller omfattningar som begärts som ska returneras i åtkomsttoken för API: et. Till exempel `[ "user.read" ]` för Microsoft Graph eller `[ "<Application ID URL>/scope" ]` för anpassade webb-API: er (det vill säga `api://<Application ID>/access_as_user`). Här kan `applicationConfig.graphScopes` skickas.|

#### <a name="get-a-user-token-interactively"></a>Hämta en användartoken interaktivt

Det finns situationer då du behöver att tvinga användare att interagera med Azure AD v2.0-slutpunkten. Exempel:
* Användare kan behöva ange sina autentiseringsuppgifter igen eftersom deras lösenord har upphört att gälla
* Ditt program begär åtkomst till resurser omfattningar som användaren behöver för att godkänna
* Tvåfaktorsautentisering krävs

Det rekommendera vanliga mönstret för de flesta program är att anropa `acquireTokenSilent` först fånga upp undantaget och sedan anropa `acquireTokenRedirect` (eller `acquireTokenPopup`) att starta en interaktiv förfrågan.

Anropa den `acquireTokenPopup(scope)` resulterar i ett popup-fönster för att logga in (eller `acquireTokenRedirect(scope)` resulterar i att omdirigera användare till Azure AD v2.0-slutpunkten) där användare måste interagera genom att kontrollera sina autentiseringsuppgifter, ge samtycke till den begärda resursen eller du har slutfört tvåfaktorautentisering.

```javascript
myMSALObj.acquireTokenPopup(applicationConfig.graphScopes).then(function (accessToken) {
    // Callback code here
})
```

> [!NOTE]
> Den här snabbstarten används de `loginRedirect` och `acquireTokenRedirect` metoder när webbläsaren som används är Internet Explorer på grund av en [känt problem](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues) rör hantering av popup-fönster genom att webbläsaren Internet Explorer.

## <a name="next-steps"></a>Nästa steg

För en mer detaljerad vägledning om hur du skapar program för den här snabbstarten, kan prova att använda JavaScript kursen nedan.

### <a name="learn-the-steps-to-create-the-application-for-this-quickstart"></a>Lär dig hur du skapar program för den här snabbstarten

> [!div class="nextstepaction"]
> [Självstudie om att anropa Graph API](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-javascriptspa)

### <a name="browse-the-msal-repo-for-documentation-faq-issues-and-more"></a>Bläddra MSAL-lagringsplatsen för dokumentation, vanliga frågor och svar, problem och annat

> [!div class="nextstepaction"]
> [msal.js GitHub-lagringsplatsen](https://github.com/AzureAD/microsoft-authentication-library-for-js)
