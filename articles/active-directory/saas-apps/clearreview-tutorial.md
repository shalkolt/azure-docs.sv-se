---
title: 'Självstudier: Azure Active Directory-integration med tydliga granska | Microsoft Docs'
description: Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och rensa granskning.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8264159a-11a2-4a8c-8285-4efea0adac8c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: jeedes
ms.openlocfilehash: 604a557a91176c08a361ffd058adda63f53b30fc
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39433028"
---
# <a name="tutorial-azure-active-directory-integration-with-clear-review"></a>Självstudier: Azure Active Directory-integrering med rensa granskning

Lär dig hur du integrerar rensa granskning med Azure Active Directory (AD Azure) i den här självstudien.

Integrera tydliga granskning med Azure AD ger dig följande fördelar:

- Du kan styra i Azure AD som har åtkomst till rensa granskning.
- Du kan aktivera användarna att automatiskt få loggat in på Rensa granskning (Single Sign-On) med sina Azure AD-konton.
- Du kan hantera dina konton på en central plats – Azure portal.

Om du vill veta mer om integrering av SaaS-app med Azure AD finns i [vad är programåtkomst och enkel inloggning med Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Förutsättningar

Om du vill konfigurera Azure AD-integrering med tydliga granskning, behöver du följande objekt:

- En Azure AD-prenumeration
- En tydlig granska enkel inloggning aktiverat prenumeration

> [!NOTE]
> Om du vill testa stegen i den här självstudien rekommenderar vi inte med hjälp av en produktionsmiljö.

Om du vill testa stegen i den här självstudien bör du följa dessa rekommendationer:

- Använd inte din produktionsmiljö, om det inte behövs.
- Om du inte har en Azure AD-utvärderingsmiljö, kan du [få en månads utvärdering](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I den här självstudien kan du testa Azure AD enkel inloggning i en testmiljö. Det scenario som beskrivs i den här självstudien består av två viktigaste byggstenarna:

1. Att lägga till Rensa granska från galleriet
1. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-clear-review-from-the-gallery"></a>Att lägga till Rensa granska från galleriet
För att konfigurera integrering av rensa granskning i Azure AD, som du behöver lägga till Rensa granska från galleriet i din lista över hanterade SaaS-appar.

**Utför följande steg för att lägga till Rensa granska från galleriet:**

1. I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon. 

    ![Azure Active Directory-knappen][1]

1. Gå till **företagsprogram**. Gå till **alla program**.

    ![Bladet för Enterprise-program][2]
    
1. Lägg till nytt program, klicka på **nytt program** knappen överst i dialogrutan.

    ![Knappen Nytt program][3]

1. I sökrutan skriver **Rensa granska**väljer **Rensa granska** resultatet panelen klickar **Lägg till** för att lägga till programmet.

    ![Rensa granskning i resultatlistan](./media/clearreview-tutorial/tutorial_clearreview_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med tydliga granska baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning att fungera, behöver Azure AD du veta vad du motsvarighet i rensa granskning är till en användare i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och relaterade användaren i rensa granskning upprättas.

Rensa granskning pågår, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** att upprätta länken-relation.

Om du vill konfigurera och testa Azure AD enkel inloggning med tydliga granskning, måste du utföra följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  – om du vill ge användarna använda den här funktionen.
1. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  – om du vill testa Azure AD enkel inloggning med Britta Simon.
1. **[Skapa en tydlig granska testanvändare](#create-a-clear-review-test-user)**  – du har en motsvarighet för Britta Simon i rensa granskning som är länkad till en Azure AD-representation av användaren.
1. **[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  – om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.
1. **[Testa enkel inloggning](#test-single-sign-on)**  – om du vill kontrollera om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i programmets rensa granskning.

**Utför följande steg för att konfigurera Azure AD enkel inloggning med tydliga granskning:**

1. I Azure-portalen på den **Rensa granska** program integration-sidan klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning för länken][4]

1. På den **enkel inloggning** dialogrutan **läge** som **SAML-baserad inloggning** att aktivera enkel inloggning.
 
    ![Enkel inloggning för dialogrutan](./media/clearreview-tutorial/tutorial_clearreview_samlbase.png)

1. På den **Rensa granska domän och URL: er** avsnittet, utför följande steg om du vill konfigurera programmet i **IdP-initierad** läge:

    ![Rensa granska domän och URL: er med enkel inloggning för information](./media/clearreview-tutorial/tutorial_clearreview_url.png)

    a. I den **identifierare** textrutan anger du ett URL med hjälp av följande mönster: `https://<customer name>.clearreview.com/sso/metadata/`

    b. I den **svars-URL** textrutan anger du ett URL med hjälp av följande mönster: `https://<customer name>.clearreview.com/sso/acs/`

1. Kontrollera **visa avancerade URL-inställningar** och utföra följande steg om du vill konfigurera programmet i **SP** initierade läge:

    ![Rensa granska domän och URL: er med enkel inloggning för information](./media/clearreview-tutorial/tutorial_clearreview_url_sp.png)

    I den **inloggnings-URL** textrutan anger du ett URL med hjälp av följande mönster:`https://<customer name>.clearreview.com`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare svars-URL. Kontakta [Rensa granska supportteamet](https://clearreview.com/contact/) att hämta dessa värden.

1. Rensa granska programmet förväntar dig av det unikt värdet för användaridentifierare namnidentifierare anspråket. Du ska mappa användare ID-värde till **user.mail**.

    ![Avsnittet attribut](./media/clearreview-tutorial/attribute.png)


1. På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.

    ![Länk för hämtning av certifikat](./media/clearreview-tutorial/tutorial_clearreview_certificate.png)

1. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning spara-knapp](./media/clearreview-tutorial/tutorial_general_400.png)

1. På den **Rensa Granska konfigurationen** klickar du på **konfigurera Rensa granska** att öppna **konfigurera inloggning** fönster. Kopiera den **URL för utloggning, SAML entitets-ID och SAML enkel inloggning för tjänst-URL** från den **Snabbreferens avsnittet.**

    ![Rensa Granska konfigurationen](./media/clearreview-tutorial/tutorial_clearreview_configure.png) 

1. Att konfigurera enkel inloggning på **Rensa granska** sida, öppna den **Rensa granska** med administratörsautentiseringsuppgifter.

1. Välj **Admin** i det vänstra navigeringsfönstret.

    ![Konfigurera enkel inloggning spara-knapp](./media/clearreview-tutorial/tutorial_clearreview_app_admin1.png)

1. Välj **ändra** längst ned på sidan.

    ![Konfigurera enkel inloggning spara-knapp](./media/clearreview-tutorial/tutorial_clearreview_app_admin2.png)

1. Utför följande steg **inställningar för enkel inloggning** sidan

    ![Konfigurera enkel inloggning spara-knapp](./media/clearreview-tutorial/tutorial_clearreview_app_admin3.png)

    a. I den **utfärdar-URL** textrutan klistra in värdet för **SAML entitets-ID** som du har kopierat från Azure-portalen.

    b. I den **SAML Endpoint** textrutan klistra in värdet för **SAML inloggnings-tjänst-URL för enkel** som du har kopierat från Azure-portalen.    

    c. I den **SLO Endpoint** textrutan klistra in värdet för **inloggnings-tjänstens URL** som du har kopierat från Azure-portalen. 

    d. Öppna det nedladdade certifikatet i anteckningar och klistra in innehållet i den **X.509-certifikat** textrutan.   

1. Klicka på **Spara**.

> [!TIP]
> Du kan läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du ställer in appen!  När du lägger till den här appen från den **Active Directory > företagsprogram** bara klickar du på den **enkel inloggning** fliken och komma åt den inbäddade dokumentationen genom den  **Konfigurationen** avsnittet längst ned. Du kan läsa mer om här funktionen embedded-dokumentation: [Azure AD embedded-dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Skapa en Azure AD-testanvändare

Målet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.

   ![Skapa en Azure AD-testanvändare][100]

**Utför följande steg för att skapa en testanvändare i Azure AD:**

1. I Azure-portalen, i den vänstra rutan klickar du på den **Azure Active Directory** knappen.

    ![Azure Active Directory-knappen](./media/clearreview-tutorial/create_aaduser_01.png)

1. Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.

    ![”Användare och grupper” och ”alla användare”-länkar](./media/clearreview-tutorial/create_aaduser_02.png)

1. Öppna den **användaren** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.

    ![Knappen Lägg till](./media/clearreview-tutorial/create_aaduser_03.png)

1. I den **användaren** dialogrutan utför följande steg:

    ![Dialogrutan användare](./media/clearreview-tutorial/create_aaduser_04.png)

    a. I den **namn** skriver **BrittaSimon**.

    b. I den **användarnamn** skriver användarens Britta Simon e-postadress.

    c. Välj den **visa lösenord** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** box.

    d. Klicka på **Skapa**.
  
### <a name="create-a-clear-review-test-user"></a>Skapa en tydlig granska testanvändare

I det här avsnittet skapar du en användare som kallas Britta Simon i rensa granskning. Kontakta [Rensa granska supportteamet](https://clearreview.com/contact/) att lägga till användare i Rensa granska-plattformen.

### <a name="assign-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändare

I det här avsnittet ska aktivera du Britta Simon att använda Azure enkel inloggning om du beviljar åtkomst till Rensa granska.

![Tilldela rollen][200] 

**Om du vill tilldela Britta Simon rensa granskning, utför du följande steg:**

1. Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** klickar **alla program**.

    ![Tilldela användare][201] 

1. I listan med program väljer **Rensa granska**.

    ![Rensa granskningslänk i programlistan](./media/clearreview-tutorial/tutorial_clearreview_app.png)  

1. I menyn till vänster, klickar du på **användare och grupper**.

    ![Länken ”användare och grupper”][202]

1. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg till tilldelning** dialogrutan.

    ![Fönstret Lägg till tilldelning][203]

1. På **användare och grupper** dialogrutan **Britta Simon** på listan användare.

1. Klicka på **Välj** knappen **användare och grupper** dialogrutan.

1. Klicka på **tilldela** knappen **Lägg till tilldelning** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet ska testa du Azure AD enkel inloggning för konfigurationen med hjälp av åtkomstpanelen.

När du klickar på panelen Rensa granskning i åtkomstpanelen du bör få automatiskt loggat in på programmets rensa granskning.
Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över guider om hur du integrerar SaaS-appar med Azure Active Directory](tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/clearreview-tutorial/tutorial_general_01.png
[2]: ./media/clearreview-tutorial/tutorial_general_02.png
[3]: ./media/clearreview-tutorial/tutorial_general_03.png
[4]: ./media/clearreview-tutorial/tutorial_general_04.png

[100]: ./media/clearreview-tutorial/tutorial_general_100.png

[200]: ./media/clearreview-tutorial/tutorial_general_200.png
[201]: ./media/clearreview-tutorial/tutorial_general_201.png
[202]: ./media/clearreview-tutorial/tutorial_general_202.png
[203]: ./media/clearreview-tutorial/tutorial_general_203.png
