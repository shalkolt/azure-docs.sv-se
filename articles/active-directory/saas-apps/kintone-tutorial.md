---
title: 'Självstudier: Azure Active Directory-integration med Kintone | Microsoft Docs'
description: Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Kintone.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: c2b947dc-e1a8-4f5f-b40e-2c5180648e4f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jeedes
ms.openlocfilehash: 693211245ee98849548bee4a52f7e424dd8b4cc6
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39430465"
---
# <a name="tutorial-azure-active-directory-integration-with-kintone"></a>Självstudier: Azure Active Directory-integration med Kintone

I den här självstudien får du lära dig hur du integrerar Kintone med Azure Active Directory (AD Azure).

Integrera Kintone med Azure AD ger dig följande fördelar:

- Du kan styra i Azure AD som har åtkomst till Kintone
- Du kan aktivera användarna att automatiskt få loggat in på Kintone (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton på en central plats – Azure portal

Om du vill veta mer om integrering av SaaS-app med Azure AD finns i [vad är programåtkomst och enkel inloggning med Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Förutsättningar

Om du vill konfigurera Azure AD-integrering med Kintone, behöver du följande objekt:

- En Azure AD-prenumeration
- En Kintone enkel inloggning aktiverat prenumeration

> [!NOTE]
> Om du vill testa stegen i den här självstudien rekommenderar vi inte med hjälp av en produktionsmiljö.

Om du vill testa stegen i den här självstudien bör du följa dessa rekommendationer:

- Använd inte din produktionsmiljö, om det inte behövs.
- Om du inte har en Azure AD-utvärderingsmiljö kan du få en månads utvärdering [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I den här självstudien kan du testa Azure AD enkel inloggning i en testmiljö. Det scenario som beskrivs i den här självstudien består av två viktigaste byggstenarna:

1. Att lägga till Kintone från galleriet
1. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-kintone-from-the-gallery"></a>Att lägga till Kintone från galleriet
För att konfigurera integrering av Kintone i Azure AD, som du behöver lägga till Kintone från galleriet i din lista över hanterade SaaS-appar.

**Utför följande steg för att lägga till Kintone från galleriet:**

1. I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon. 

    ![Active Directory][1]

1. Gå till **företagsprogram**. Gå till **alla program**.

    ![Program][2]
    
1. Lägg till nytt program, klicka på **nytt program** knappen överst i dialogrutan.

    ![Program][3]

1. I sökrutan skriver **Kintone**.

    ![Skapa en Azure AD-användare för testning](./media/kintone-tutorial/tutorial_kintone_search.png)

1. I resultatpanelen väljer **Kintone**, och klicka sedan på **Lägg till** för att lägga till programmet.

    ![Skapa en Azure AD-användare för testning](./media/kintone-tutorial/tutorial_kintone_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet ska du konfigurera och testa Azure AD enkel inloggning med Kintone baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning att fungera, behöver Azure AD du veta vad användaren motsvarighet i Kintone är till en användare i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och relaterade användaren i Kintone upprättas.

I Kintone, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** att upprätta länken-relation.

Om du vill konfigurera och testa Azure AD enkel inloggning med Kintone, måste du utföra följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  – om du vill ge användarna använda den här funktionen.
1. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  – om du vill testa Azure AD enkel inloggning med Britta Simon.
1. **[Skapa en testanvändare för Kintone](#creating-a-kintone-test-user)**  – du har en motsvarighet för Britta Simon i Kintone som är länkad till en Azure AD-representation av användaren.
1. **[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  – om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.
1. **[Testa enkel inloggning](#testing-single-sign-on)**  – om du vill kontrollera om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet ska du aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt program för Kintone.

**Utför följande steg för att konfigurera Azure AD enkel inloggning med Kintone:**

1. I Azure-portalen på den **Kintone** program integration-sidan klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

1. På den **enkel inloggning** dialogrutan **läge** som **SAML-baserad inloggning** att aktivera enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/kintone-tutorial/tutorial_kintone_samlbase.png)

1. På den **Kintone domän och URL: er** avsnittet, utför följande steg:

    ![Konfigurera enkel inloggning](./media/kintone-tutorial/tutorial_kintone_url.png)

    a. I den **inloggnings-URL** textrutan anger du ett URL med hjälp av följande mönster: `https://<companyname>.kintone.com`

    b. I den **identifierare** textrutan anger du ett URL med hjälp av följande mönster:
    | |
    |--|
    | `https://<companyname>.cybozu.com`|
    | `https://<companyname>.kintone.com`|

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med de faktiska inloggnings-URL och identifierare. Kontakta [Kintone klienten supportteamet](https://www.kintone.com/contact/) att hämta dessa värden. 
 
1. På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.

    ![Konfigurera enkel inloggning](./media/kintone-tutorial/tutorial_kintone_certificate.png) 

1. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/kintone-tutorial/tutorial_general_400.png)

1. På den **Kintone Configuration** klickar du på **konfigurera Kintone** att öppna **konfigurera inloggning** fönster. Kopiera den **URL: en för utloggning och SAML enkel inloggning för tjänst-URL** från den **Snabbreferens avsnittet.**

    ![Konfigurera enkel inloggning](./media/kintone-tutorial/tutorial_kintone_configure.png) 

1. I ett annat webbläsarfönster, loggar du in på ditt **Kintone** företagets plats som administratör.

1. Klicka på **inställningar**.
   
    ![Inställningar för](./media/kintone-tutorial/ic785879.png "inställningar")

1. Klicka på **användare & systemadministration**.
   
    ![Användare och systemadministration](./media/kintone-tutorial/ic785880.png "användare & systemadministration")

1. Under **systemadministration \> Security** klickar du på **inloggning**.
   
    ![Logga in](./media/kintone-tutorial/ic785881.png "inloggning")

1. Klicka på **aktivera SAML-autentisering**.
   
    ![SAML-autentisering](./media/kintone-tutorial/ic785882.png "SAML-autentisering")

1. Utför följande steg i avsnittet SAML-autentisering:
    
    ![SAML-autentisering](./media/kintone-tutorial/ic785883.png "SAML-autentisering")
    
    a. I den **inloggnings-URL** textrutan klistra in värdet för **SAML inloggnings-tjänst-URL för enkel** som du har kopierat från Azure-portalen.
   
    b. I den **URL för utloggning** textrutan klistra in värdet för **URL: en för utloggning** som du har kopierat från Azure-portalen.
    
    c. Klicka på **Bläddra** att ladda upp din hämtade certifikatet.
    
    d. Klicka på **Spara**.

> [!TIP]
> Du kan läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du ställer in appen!  När du lägger till den här appen från den **Active Directory > företagsprogram** bara klickar du på den **enkel inloggning** fliken och komma åt den inbäddade dokumentationen genom den  **Konfigurationen** avsnittet längst ned. Du kan läsa mer om här funktionen embedded-dokumentation: [Azure AD embedded-dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en Azure AD-användare för testning
Målet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.

![Skapa en Azure AD-användare][100]

**Utför följande steg för att skapa en testanvändare i Azure AD:**

1. I den **Azure-portalen**, i det vänstra navigeringsfönstret klickar du på **Azure Active Directory** ikon.

    ![Skapa en Azure AD-användare för testning](./media/kintone-tutorial/create_aaduser_01.png) 

1. Om du vill visa en lista över användare, gå till **användare och grupper** och klicka på **alla användare**.
    
    ![Skapa en Azure AD-användare för testning](./media/kintone-tutorial/create_aaduser_02.png) 

1. Öppna den **användaren** dialogrutan klickar du på **Lägg till** överst i dialogrutan.
 
    ![Skapa en Azure AD-användare för testning](./media/kintone-tutorial/create_aaduser_03.png) 

1. På den **användaren** dialogrutan utför följande steg:
 
    ![Skapa en Azure AD-användare för testning](./media/kintone-tutorial/create_aaduser_04.png) 

    a. I den **namn** textrutan typ **BrittaSimon**.

    b. I den **användarnamn** textrutan skriver den **e-postadress** av BrittaSimon.

    c. Välj **visa lösenord** och anteckna värdet för den **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-kintone-test-user"></a>Skapa en testanvändare för Kintone

Om du vill aktivera Azure AD-användare att logga in på Kintone, måste de etableras i Kintone.  
När det gäller Kintone är etablering en manuell aktivitet.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Utför följande steg för att etablera ett användarkonto:

1. Logga in på din **Kintone** företagets plats som administratör.

1. Klicka på **inställningen**.
   
    ![Inställningar för](./media/kintone-tutorial/ic785879.png "inställningar")

1. Klicka på **användare & systemadministration**.
   
    ![Användaren & systemadministration](./media/kintone-tutorial/ic785880.png "användaren & systemadministration")

1. Under **Användaradministration**, klickar du på **avdelningar och användare**.
   
    ![Avdelning och användare](./media/kintone-tutorial/ic785888.png "avdelning och användare")

1. Klicka på **ny användare**.
   
    ![Nya användare](./media/kintone-tutorial/ic785889.png "nya användare")

1. I den **ny användare** avsnittet, utför följande steg:
   
    ![Nya användare](./media/kintone-tutorial/ic785890.png "nya användare")
   
    a. Ange ett **visningsnamn**, **inloggningsnamn**, **nytt lösenord**, **Bekräfta lösenord**, **e-postadress**, och annan information om ett giltigt AAD-konto som du vill att etablera i den relaterade textrutor.
 
    b. Klicka på **Spara**.

> [!NOTE]
> Du kan använda alla andra Kintone användare konto verktyg för att skapa eller API: er som tillhandahålls av Kintone att etablera AAD-användarkonton.

### <a name="assigning-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändare

I det här avsnittet ska aktivera du Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Kintone.

![Tilldela användare][200] 

**Om du vill tilldela Britta Simon Kintone, utför du följande steg:**

1. Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** klickar **alla program**.

    ![Tilldela användare][201] 

1. I listan med program väljer **Kintone**.

    ![Konfigurera enkel inloggning](./media/kintone-tutorial/tutorial_kintone_app.png) 

1. I menyn till vänster, klickar du på **användare och grupper**.

    ![Tilldela användare][202] 

1. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg till tilldelning** dialogrutan.

    ![Tilldela användare][203]

1. På **användare och grupper** dialogrutan **Britta Simon** på listan användare.

1. Klicka på **Välj** knappen **användare och grupper** dialogrutan.

1. Klicka på **tilldela** knappen **Lägg till tilldelning** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

Målet med det här avsnittet är att prova Azure AD enkel inloggning för konfigurationen med hjälp av åtkomstpanelen.

När du klickar på panelen Kintone i åtkomstpanelen du bör få automatiskt loggat in på ditt program för Kintone.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över guider om hur du integrerar SaaS-appar med Azure Active Directory](tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/kintone-tutorial/tutorial_general_01.png
[2]: ./media/kintone-tutorial/tutorial_general_02.png
[3]: ./media/kintone-tutorial/tutorial_general_03.png
[4]: ./media/kintone-tutorial/tutorial_general_04.png

[100]: ./media/kintone-tutorial/tutorial_general_100.png

[200]: ./media/kintone-tutorial/tutorial_general_200.png
[201]: ./media/kintone-tutorial/tutorial_general_201.png
[202]: ./media/kintone-tutorial/tutorial_general_202.png
[203]: ./media/kintone-tutorial/tutorial_general_203.png

