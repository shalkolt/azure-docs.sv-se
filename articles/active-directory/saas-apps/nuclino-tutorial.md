---
title: 'Självstudier: Azure Active Directory-integration med Nuclino | Microsoft Docs'
description: Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Nuclino.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 74bbab82-5581-4dcf-8806-78f77c746968
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2018
ms.author: jeedes
ms.openlocfilehash: 1a5346b98de48b1a2f8928c3c2bf30730588e9c1
ms.sourcegitcommit: a1140e6b839ad79e454186ee95b01376233a1d1f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/28/2018
ms.locfileid: "43146168"
---
# <a name="tutorial-azure-active-directory-integration-with-nuclino"></a>Självstudier: Azure Active Directory-integration med Nuclino

I den här självstudien får du lära dig hur du integrerar Nuclino med Azure Active Directory (AD Azure).

Integrera Nuclino med Azure AD ger dig följande fördelar:

- Du kan styra i Azure AD som har åtkomst till Nuclino.
- Du kan aktivera användarna att automatiskt få loggat in på Nuclino (Single Sign-On) med sina Azure AD-konton.
- Du kan hantera dina konton på en central plats – Azure portal.

Om du vill veta mer om integrering av SaaS-app med Azure AD finns i [vad är programåtkomst och enkel inloggning med Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Förutsättningar

Om du vill konfigurera Azure AD-integrering med Nuclino, behöver du följande objekt:

- En Azure AD-prenumeration
- En Nuclino enkel inloggning aktiverat prenumeration

> [!NOTE]
> Om du vill testa stegen i den här självstudien rekommenderar vi inte med hjälp av en produktionsmiljö.

Om du vill testa stegen i den här självstudien bör du följa dessa rekommendationer:

- Använd inte din produktionsmiljö, om det inte behövs.
- Om du inte har en Azure AD-utvärderingsmiljö, kan du [få en månads utvärdering](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning

I den här självstudien kan du testa Azure AD enkel inloggning i en testmiljö. Det scenario som beskrivs i den här självstudien består av två viktigaste byggstenarna:

1. Att lägga till Nuclino från galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-nuclino-from-the-gallery"></a>Att lägga till Nuclino från galleriet

För att konfigurera integrering av Nuclino i Azure AD, som du behöver lägga till Nuclino från galleriet i din lista över hanterade SaaS-appar.

**Utför följande steg för att lägga till Nuclino från galleriet:**

1. I den **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon. 

    ![Azure Active Directory-knappen][1]

2. Gå till **företagsprogram**. Gå till **alla program**.

    ![Bladet för Enterprise-program][2]

3. Lägg till nytt program, klicka på **nytt program** knappen överst i dialogrutan.

    ![Knappen Nytt program][3]

4. I sökrutan skriver **Nuclino**väljer **Nuclino** resultatet panelen klickar **Lägg till** för att lägga till programmet.

    ![Nuclino i resultatlistan](./media/nuclino-tutorial/tutorial_nuclino_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet ska du konfigurera och testa Azure AD enkel inloggning med Nuclino baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning att fungera, behöver Azure AD du veta vad användaren motsvarighet i Nuclino är till en användare i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och relaterade användaren i Nuclino upprättas.

Om du vill konfigurera och testa Azure AD enkel inloggning med Nuclino, måste du utföra följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  – om du vill ge användarna använda den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  – om du vill testa Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Nuclino](#create-a-nuclino-test-user)**  – du har en motsvarighet för Britta Simon i Nuclino som är länkad till en Azure AD-representation av användaren.
4. **[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  – om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  – om du vill kontrollera om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Nuclino program.

**Utför följande steg för att konfigurera Azure AD enkel inloggning med Nuclino:**

1. I Azure-portalen på den **Nuclino** program integration-sidan klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning för länken][4]

2. På den **enkel inloggning** dialogrutan **läge** som **SAML-baserad inloggning** att aktivera enkel inloggning.

    ![Enkel inloggning för dialogrutan](./media/nuclino-tutorial/tutorial_nuclino_samlbase.png)

3. På den **Nuclino domän och URL: er** avsnittet, utför följande steg om du vill konfigurera programmet i **IDP** initierade läge:

    ![Nuclino domän och URL: er med enkel inloggning för information](./media/nuclino-tutorial/tutorial_nuclino_url1.png)

    a. I den **identifierare** textrutan anger du ett URL med hjälp av följande mönster: `https://api.nuclino.com/api/sso/<UNIQUE-ID>/metadata`

    b. I den **svars-URL** textrutan anger du ett URL med hjälp av följande mönster: `https://api.nuclino.com/api/sso/<UNIQUE-ID>/acs`

    > [!NOTE]
    > Dessa värden är inte verkliga. Uppdatera dessa värden med de faktiska identifierare och svars-URL från den **autentisering** som beskrivs senare i den här självstudien.

4. Kontrollera **visa avancerade URL-inställningar** och utföra följande steg om du vill konfigurera programmet i **SP** initierade läge:

    ![Nuclino domän och URL: er med enkel inloggning för information](./media/nuclino-tutorial/tutorial_nuclino_url2.png)

    I den **inloggnings-URL** textrutan anger du ett URL med hjälp av följande mönster: `https://app.nuclino.com/<UNIQUE-ID>/login`

    > [!NOTE]
    > Det här värdet är inte verkliga. Uppdatera det här värdet med faktiska inloggnings-URL: en. Kontakta [Nuclino klienten supportteamet](mailto:contact@nuclino.com) att hämta det här värdet.

5. Nuclino program som förväntar SAML-intyg i ett visst format. Konfigurera följande anspråk för det här programmet. Du kan hantera värdena för dessa attribut från den ”**användarattribut**” på sidan för integrering av program. Följande skärmbild visar ett exempel för detta.

    ![Konfigurera enkel inloggning](./media/Nuclino-tutorial/tutorial_attribute.png)

6. Klicka på **visa och redigera alla andra användarattribut** kryssrutan i den **användarattribut** avsnitt för att expandera attribut. Utför följande steg på varje visas attribut-

    | Attributnamn | Attributvärde |
    | ---------------| --------------- |
    | Förnamn | User.givenName |
    | Efternamn | User.surname |

    a. Klicka på **Lägg till attribut** att öppna den **lägga till attributet** dialogrutan.

    ![Konfigurera enkel inloggning](./media/nuclino-tutorial/tutorial_attribute_04.png)

    ![Konfigurera enkel inloggning](./media/nuclino-tutorial/tutorial_attribute_05.png)

    b. I den **namn** textrutan skriver den **attributnamnet** visas för den raden.

    c. Från den **värdet** anger attributvärdet som visas för den raden.

    d. Klicka på **OK**.

7. På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.

    ![Länk för hämtning av certifikat](./media/nuclino-tutorial/tutorial_nuclino_certificate.png)

8. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning spara-knapp](./media/nuclino-tutorial/tutorial_general_400.png)

9. På den **Nuclino Configuration** klickar du på **konfigurera Nuclino** att öppna **konfigurera inloggning** fönster. Kopiera den **SAML entitets-ID och SAML enkel inloggning för tjänst-URL** från den **Snabbreferens avsnittet.**

    ![Nuclino konfiguration](./media/nuclino-tutorial/tutorial_nuclino_configure.png)

10. I ett annat webbläsarfönster, loggar du in din Nuclino företagets webbplats som administratör.

11. Klicka på den **IKONEN**.

    ![Nuclino konfiguration](./media/nuclino-tutorial/configure1.png)

12. Klicka på den **Azure AD SSO** och välj **Team inställningar** i listrutan.

    ![Nuclino konfiguration](./media/nuclino-tutorial/configure2.png)

13. Välj **autentisering** vänstra navigeringsfönstret.

    ![Nuclino konfiguration](./media/nuclino-tutorial/configure3.png)

14. I den **autentisering** avsnittet, utför följande steg:

    ![Nuclino konfiguration](./media/nuclino-tutorial/configure4.png)

    a. Välj **SAML-baserad enkel inloggning (SSO)**.

    b. Kopiera **ACS URL (du måste kopiera och klistra in det till din provider för enkel inloggning)** värde och klistra in den i den **svars-URL** textrutan för den **Nuclino domän och URL: er** avsnitt i Azure portalen.

    c. Kopiera **entitets-ID (du måste kopiera och klistra in det till din provider för enkel inloggning)** värde och klistra in den i den **identifierare** textrutan för den **Nuclino domän och URL: er** avsnitt i Azure portalen.

    d. I den **SSO URL** textrutan klistra in den **SAML enkel inloggning för tjänst-URL** värde som du har kopierat från Azure-portalen.

    e. I den **entitets-ID** textrutan klistra in den **SAML entitets-ID** värde som du har kopierat från Azure-portalen.

    f. Öppna din hämtade **Certificate(Base64)** -filen i anteckningar. Kopiera innehållet i den till Urklipp och klistra in den till den **offentligt certifikat** textrutan.

    g. Klicka på **spara ändringar**.

### <a name="create-an-azure-ad-test-user"></a>Skapa en Azure AD-testanvändare

Målet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.

   ![Skapa en Azure AD-testanvändare][100]

**Utför följande steg för att skapa en testanvändare i Azure AD:**

1. I Azure-portalen, i den vänstra rutan klickar du på den **Azure Active Directory** knappen.

    ![Azure Active Directory-knappen](./media/nuclino-tutorial/create_aaduser_01.png)

2. Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.

    ![”Användare och grupper” och ”alla användare”-länkar](./media/nuclino-tutorial/create_aaduser_02.png)

3. Öppna den **användaren** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.

    ![Knappen Lägg till](./media/nuclino-tutorial/create_aaduser_03.png)

4. I den **användaren** dialogrutan utför följande steg:

    ![Dialogrutan användare](./media/nuclino-tutorial/create_aaduser_04.png)

    a. I den **namn** skriver **BrittaSimon**.

    b. I den **användarnamn** skriver användarens Britta Simon e-postadress.

    c. Välj den **visa lösenord** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** box.

    d. Klicka på **Skapa**.

### <a name="create-a-nuclino-test-user"></a>Skapa en Nuclino testanvändare

Målet med det här avsnittet är att skapa en användare som kallas Britta Simon i Nuclino. Nuclino stöder just-in-time-etablering, vilket är som standard aktiverat. Det finns inga uppgift åt dig i det här avsnittet. En ny användare har skapats under ett försök att komma åt Nuclino om det inte finns ännu.

> [!Note]
> Om du vill skapa en användare manuellt kan du kontakta [Nuclino supportteamet](mailto:contact@nuclino.com).

### <a name="assign-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändare

I det här avsnittet ska aktivera du Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Nuclino.

![Tilldela rollen][200]

**Om du vill tilldela Britta Simon Nuclino, utför du följande steg:**

1. Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** klickar **alla program**.

    ![Tilldela användare][201]

2. I listan med program väljer **Nuclino**.

    ![Länken Nuclino i listan med program](./media/nuclino-tutorial/tutorial_nuclino_app.png)  

3. I menyn till vänster, klickar du på **användare och grupper**.

    ![Länken ”användare och grupper”][202]

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg till tilldelning** dialogrutan.

    ![Fönstret Lägg till tilldelning][203]

5. På **användare och grupper** dialogrutan **Britta Simon** på listan användare.

6. Klicka på **Välj** knappen **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen **Lägg till tilldelning** dialogrutan.

### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet ska testa du Azure AD enkel inloggning för konfigurationen med hjälp av åtkomstpanelen.

När du klickar på panelen Nuclino i åtkomstpanelen du bör få automatiskt loggat in på ditt Nuclino program.
Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över guider om hur du integrerar SaaS-appar med Azure Active Directory](tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/nuclino-tutorial/tutorial_general_01.png
[2]: ./media/nuclino-tutorial/tutorial_general_02.png
[3]: ./media/nuclino-tutorial/tutorial_general_03.png
[4]: ./media/nuclino-tutorial/tutorial_general_04.png

[100]: ./media/nuclino-tutorial/tutorial_general_100.png

[200]: ./media/nuclino-tutorial/tutorial_general_200.png
[201]: ./media/nuclino-tutorial/tutorial_general_201.png
[202]: ./media/nuclino-tutorial/tutorial_general_202.png
[203]: ./media/nuclino-tutorial/tutorial_general_203.png