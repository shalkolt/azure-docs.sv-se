---
title: ta med fil
description: ta med fil
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/13/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: 7ff04789a4ba5e5a689b3d3815852bc0fbcdc6a7
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/24/2018
ms.locfileid: "49988375"
---
## <a name="register-your-application"></a>Registrera ditt program

Du kan registrera programmet i något av två sätt, enligt beskrivningen i följande två avsnitt.

### <a name="option-1-express"></a>Alternativ 1: Express

1. Gå till [Microsoft-portalen för appregistrering](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=android&step=configure).
2. I **programnamn**, ange ett namn för ditt program.
3. Se till att den **interaktiva installation** kryssrutan är markerad och välj sedan **skapa**.
4. Följ anvisningarna för att hämta program-ID, och klistra in den i din kod.

### <a name="option-2-advanced"></a>Alternativ 2: avancerad

1. Gå till [Microsoft-portalen för appregistrering](https://apps.dev.microsoft.com/portal/register-app).
2. I rutan **Programnamn** anger du namnet på programmet.
3. Kontrollera att kryssrutan **Interaktiv installation** är avmarkerad och välj sedan **Skapa**.
4. Välj **Lägg till plattform**, välj **Internt program** och välj sedan **Spara**.
5. Under **app** > **java** > **{värden}. { namnområdet}** öppnar `MainActivity`. 
6. Ersätt *[Ange program-Id här]* med ditt program / klient-ID:

    ```java
    final static String CLIENT_ID = "[Enter the application Id here]";
    ```
<!-- Workaround for Docs conversion bug -->
7. Under **app** > **manifest**öppnar den *AndroidManifest.xml* fil.
8. I den `manifest\application`, Lägg till följande aktivitet. Den `BrowserTabActivity` aktivitet som tillåter Microsoft att ringa tillbaka till programmet efter att autentiseringen är klar:

    ```xml
    <!--Intent filter to capture System Browser calling back to our app after sign-in-->
    <activity
        android:name="com.microsoft.identity.client.BrowserTabActivity">
        <intent-filter>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            
            <!--Add in your scheme/host from registered redirect URI-->
            <!--By default, the scheme should be similar to 'msal[appId]' -->
            <data android:scheme="msal[Enter the application Id here]"
                android:host="auth" />
        </intent-filter>
    </activity>
    ```
<!-- Workaround for Docs conversion bug -->
9. I den `BrowserTabActivity`, Ersätt `[Enter the application Id here]` med programmet / klient-ID.
