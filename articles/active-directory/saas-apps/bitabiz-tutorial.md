---
title: 'Självstudier: Azure Active Directory-integration med BitaBIZ | Microsoft Docs'
description: Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och BitaBIZ.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 1a51e677-c62b-4aee-9c61-56926aaaa899
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: jeedes
ms.openlocfilehash: 2a05a4f1b9162a69e074bf6243236df48c8ce536
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39429670"
---
# <a name="tutorial-azure-active-directory-integration-with-bitabiz"></a>Självstudier: Azure Active Directory-integration med BitaBIZ

I den här självstudien får du lära dig hur du integrerar BitaBIZ med Azure Active Directory (AD Azure).

Integrera BitaBIZ med Azure AD ger dig följande fördelar:

- Du kan styra i Azure AD som har åtkomst till BitaBIZ.
- Du kan aktivera användarna att automatiskt få loggat in på BitaBIZ (Single Sign-On) med sina Azure AD-konton.
- Du kan hantera dina konton på en central plats – Azure portal.

Om du vill veta mer om integrering av SaaS-app med Azure AD finns i [vad är programåtkomst och enkel inloggning med Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Förutsättningar

Om du vill konfigurera Azure AD-integrering med BitaBIZ, behöver du följande objekt:

- En Azure AD-prenumeration
- En BitaBIZ enkel inloggning aktiverat prenumeration

> [!NOTE]
> Om du vill testa stegen i den här självstudien rekommenderar vi inte med hjälp av en produktionsmiljö.

Om du vill testa stegen i den här självstudien bör du följa dessa rekommendationer:

- Använd inte din produktionsmiljö, om det inte behövs.
- Om du inte har en Azure AD-utvärderingsmiljö, kan du [få en månads utvärdering](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I den här självstudien kan du testa Azure AD enkel inloggning i en testmiljö. Det scenario som beskrivs i den här självstudien består av två viktigaste byggstenarna:

1. Att lägga till BitaBIZ från galleriet
1. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-bitabiz-from-the-gallery"></a>Att lägga till BitaBIZ från galleriet
För att konfigurera integrering av BitaBIZ i Azure AD, som du behöver lägga till BitaBIZ från galleriet i din lista över hanterade SaaS-appar.

**Utför följande steg för att lägga till BitaBIZ från galleriet:**

1. I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon. 

    ![Azure Active Directory-knappen][1]

1. Gå till **företagsprogram**. Gå till **alla program**.

    ![Bladet för Enterprise-program][2]
    
1. Lägg till nytt program, klicka på **nytt program** knappen överst i dialogrutan.

    ![Knappen Nytt program][3]

1. I sökrutan skriver **BitaBIZ**väljer **BitaBIZ** resultatet panelen klickar **Lägg till** för att lägga till programmet.

    ![BitaBIZ i resultatlistan](./media/bitabiz-tutorial/tutorial_bitabiz_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet ska du konfigurera och testa Azure AD enkel inloggning med BitaBIZ baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning att fungera, behöver Azure AD du veta vad användaren motsvarighet i BitaBIZ är till en användare i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och relaterade användaren i BitaBIZ upprättas.

I BitaBIZ, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** att upprätta länken-relation.

Om du vill konfigurera och testa Azure AD enkel inloggning med BitaBIZ, måste du utföra följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  – om du vill ge användarna använda den här funktionen.
1. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  – om du vill testa Azure AD enkel inloggning med Britta Simon.
1. **[Skapa en testanvändare BitaBIZ](#create-a-bitabiz-test-user)**  – du har en motsvarighet för Britta Simon i BitaBIZ som är länkad till en Azure AD-representation av användaren.
1. **[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  – om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.
1. **[Testa enkel inloggning](#test-single-sign-on)**  – om du vill kontrollera om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt BitaBIZ program.

**Utför följande steg för att konfigurera Azure AD enkel inloggning med BitaBIZ:**

1. I Azure-portalen på den **BitaBIZ** program integration-sidan klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning för länken][4]

1. På den **enkel inloggning** dialogrutan **läge** som **SAML-baserad inloggning** att aktivera enkel inloggning.
 
    ![Enkel inloggning för dialogrutan](./media/bitabiz-tutorial/tutorial_bitabiz_samlbase.png)

1. På den **BitaBIZ domän och URL: er** avsnittet, utför följande steg om du vill konfigurera programmet i IDP-initierad läge:

    ![BitaBIZ domän och URL: er med enkel inloggning för information](./media/bitabiz-tutorial/tutorial_bitabiz_url.png)

    I den **identifierare** textrutan anger du ett URL med hjälp av följande mönster: `https://www.bitabiz.com/<instanceId>`

    > [!NOTE] 
    > Värdet i URL: en ovan är bara exempel. Uppdatera värdet med den faktiska identifieraren som beskrivs senare i självstudien.

1. Kontrollera **visa avancerade URL-inställningar** och utföra följande steg om du vill konfigurera programmet i **SP** initierade läge:

    ![BitaBIZ domän och URL: er med enkel inloggning för information](./media/bitabiz-tutorial/tutorial_bitabiz_url1.png)

    I den **inloggnings-URL** textrutan anger du URL: `https://www.bitabiz.com/dashboard`

1. På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.

    ![Länk för hämtning av certifikat](./media/bitabiz-tutorial/tutorial_bitabiz_certificate.png) 

1. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning spara-knapp](./media/bitabiz-tutorial/tutorial_general_400.png)
    
1. På den **BitaBIZ Configuration** klickar du på **konfigurera BitaBIZ** att öppna **konfigurera inloggning** fönster. Kopiera den **SAML entitets-ID och SAML enkel inloggning för tjänst-URL** från den **Snabbreferens avsnittet.**

    ![BitaBIZ konfiguration](./media/bitabiz-tutorial/tutorial_bitabiz_configure.png) 

1. I ett annat webbläsarfönster inloggning till BitaBIZ-klienten som administratör.

1. Klicka på **installationsprogrammet ADMIN**.

    ![BitaBIZ konfiguration](./media/bitabiz-tutorial/settings1.png)

1. Klicka på **Microsoft integreringar** under **mervärde** avsnittet.

    ![BitaBIZ konfiguration](./media/bitabiz-tutorial/settings2.png)

1. Rulla ned till avsnittet **Microsoft Azure AD (aktivera enkel inloggning)** och utför följande steg:

    ![BitaBIZ konfiguration](./media/bitabiz-tutorial/settings3.png)

    a. Kopiera värdet från den **entitets-ID (”identifierare” i Azure AD)** textrutan och klistra in den i den **identifierare** textrutan på den **BitaBIZ domän och URL: er** avsnitt i Azure-portalen. 
    
    b. I den **Azure AD enkel inloggning för tjänst-URL** textrutan klistra in **SAML enkel inloggning för tjänst-URL**, som du har kopierat från Azure-portalen.
    
    c. I den **entitets-ID för Azure AD SAML** textrutan klistra in **SAML entitets-ID**, som du har kopierat från Azure-portalen.

    d. Öppna din hämtade **Certificate(Base64)** filen i anteckningar, kopiera innehållet i den till Urklipp och klistra in den till den **Azure AD-signeringscertifikat (Base64-kodad)** textrutan.

    e. Lägg till din e-postdomän företag namnge det vill säga tilldelats i **domännamn** textrutan ska tilldelas användare i företaget med den här e-postdomän SSO (ej obligatoriskt).
    
    f. Markera **enkel inloggning aktiverat** BitaBIZ-konto.
    
    g. Klicka på **spara konfigurationen av Azure AD** att spara och aktivera SSO-konfiguration.

> [!TIP]
> Du kan läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du ställer in appen!  När du lägger till den här appen från den **Active Directory > företagsprogram** bara klickar du på den **enkel inloggning** fliken och komma åt den inbäddade dokumentationen genom den  **Konfigurationen** avsnittet längst ned. Du kan läsa mer om här funktionen embedded-dokumentation: [Azure AD embedded-dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Skapa en Azure AD-testanvändare

Målet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.

   ![Skapa en Azure AD-testanvändare][100]

**Utför följande steg för att skapa en testanvändare i Azure AD:**

1. I Azure-portalen, i den vänstra rutan klickar du på den **Azure Active Directory** knappen.

    ![Azure Active Directory-knappen](./media/bitabiz-tutorial/create_aaduser_01.png)

1. Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.

    ![”Användare och grupper” och ”alla användare”-länkar](./media/bitabiz-tutorial/create_aaduser_02.png)

1. Öppna den **användaren** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.

    ![Knappen Lägg till](./media/bitabiz-tutorial/create_aaduser_03.png)

1. I den **användaren** dialogrutan utför följande steg:

    ![Dialogrutan användare](./media/bitabiz-tutorial/create_aaduser_04.png)

    a. I den **namn** skriver **BrittaSimon**.

    b. I den **användarnamn** skriver användarens Britta Simon e-postadress.

    c. Välj den **visa lösenord** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** box.

    d. Klicka på **Skapa**.
 
### <a name="create-a-bitabiz-test-user"></a>Skapa en BitaBIZ testanvändare

Om du vill aktivera Azure AD-användare att logga in på BitaBIZ, måste de etableras i BitaBIZ.  
När det gäller BitaBIZ är etablering en manuell aktivitet.

**Utför följande steg för att etablera ett användarkonto:**

1. Logga in på webbplatsen BitaBIZ företag som administratör.

1. Klicka på **installationsprogrammet ADMIN**.

    ![BitaBIZ lägga till användare](./media/bitabiz-tutorial/settings1.png)

1. Klicka på **lägga till användare** under **organisation** avsnittet.

    ![BitaBIZ lägga till användare](./media/bitabiz-tutorial/user1.png)

1. Klicka på **Lägg till ny medarbetare**.

    ![BitaBIZ lägga till användare](./media/bitabiz-tutorial/user2.png)

1. På den **”Lägg till ny medarbetare”** dialogrutan utför följande steg:

    ![BitaBIZ lägga till användare](./media/bitabiz-tutorial/user3.png)

    a. I den **Förnamn** textrutan Ange först namnet på användaren som Britta.

    b. I den **efternamn** textrutan anger efternamn för användaren som Simon.

    c. I den **e-post** textrutan typ e-postadressen för användaren som Brittasimon@contoso.com.

    d. Välj ett datum i **datum för anställningen**.

    e. Det finns andra icke-obligatorisk användarattribut som kan ställas in för användaren. Finns det [medarbetare installationsprogrammet Doc](https://help.bitabiz.dk/manage-or-set-up-your-account/on-boarding-employees/new-employee) för mer information.    
    
    f. Klicka på **spara medarbetare**.
    
    > [!NOTE]
    > Azure Active Directory-kontoinnehavare tar emot ett e-postmeddelande och följer en länk för att bekräfta sina konton innan den blir aktiv.
    
### <a name="assign-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändare

I det här avsnittet ska aktivera du Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till BitaBIZ.

![Tilldela rollen][200] 

**Om du vill tilldela Britta Simon BitaBIZ, utför du följande steg:**

1. Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** klickar **alla program**.

    ![Tilldela användare][201] 

1. I listan med program väljer **BitaBIZ**.

    ![Länken BitaBIZ i listan med program](./media/bitabiz-tutorial/tutorial_bitabiz_app.png)  

1. I menyn till vänster, klickar du på **användare och grupper**.

    ![Länken ”användare och grupper”][202]

1. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg till tilldelning** dialogrutan.

    ![Fönstret Lägg till tilldelning][203]

1. På **användare och grupper** dialogrutan **Britta Simon** på listan användare.

1. Klicka på **Välj** knappen **användare och grupper** dialogrutan.

1. Klicka på **tilldela** knappen **Lägg till tilldelning** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet ska testa du Azure AD enkel inloggning för konfigurationen med hjälp av åtkomstpanelen.

När du klickar på panelen BitaBIZ i åtkomstpanelen du bör få automatiskt loggat in på ditt BitaBIZ program.
Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över guider om hur du integrerar SaaS-appar med Azure Active Directory](tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/bitabiz-tutorial/tutorial_general_01.png
[2]: ./media/bitabiz-tutorial/tutorial_general_02.png
[3]: ./media/bitabiz-tutorial/tutorial_general_03.png
[4]: ./media/bitabiz-tutorial/tutorial_general_04.png

[100]: ./media/bitabiz-tutorial/tutorial_general_100.png

[200]: ./media/bitabiz-tutorial/tutorial_general_200.png
[201]: ./media/bitabiz-tutorial/tutorial_general_201.png
[202]: ./media/bitabiz-tutorial/tutorial_general_202.png
[203]: ./media/bitabiz-tutorial/tutorial_general_203.png

