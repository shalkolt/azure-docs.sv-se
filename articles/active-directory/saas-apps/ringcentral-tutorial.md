---
title: 'Självstudier: Azure Active Directory-integration med RingCentral | Microsoft Docs'
description: Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och RingCentral.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 5848c875-5185-4f91-8279-1a030e67c510
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2018
ms.author: jeedes
ms.openlocfilehash: 35033e52fb54177428f8869ebcc462bd9465ad4c
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/08/2018
ms.locfileid: "48871773"
---
# <a name="tutorial-azure-active-directory-integration-with-ringcentral"></a>Självstudier: Azure Active Directory-integration med RingCentral

I den här självstudien får du lära dig hur du integrerar RingCentral med Azure Active Directory (AD Azure).

Integrera RingCentral med Azure AD ger dig följande fördelar:

- Du kan styra i Azure AD som har åtkomst till RingCentral.
- Du kan aktivera användarna att automatiskt få loggat in på RingCentral (Single Sign-On) med sina Azure AD-konton.
- Du kan hantera dina konton på en central plats – Azure portal.

Om du vill veta mer om integrering av SaaS-app med Azure AD finns i [vad är programåtkomst och enkel inloggning med Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Förutsättningar

Om du vill konfigurera Azure AD-integrering med RingCentral, behöver du följande objekt:

- En Azure AD-prenumeration
- En RingCentral enkel inloggning aktiverat prenumeration

> [!NOTE]
> Om du vill testa stegen i den här självstudien rekommenderar vi inte med hjälp av en produktionsmiljö.

Om du vill testa stegen i den här självstudien bör du följa dessa rekommendationer:

- Använd inte din produktionsmiljö, om det inte behövs.
- Om du inte har en Azure AD-utvärderingsmiljö, kan du [få en månads utvärdering](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I den här självstudien kan du testa Azure AD enkel inloggning i en testmiljö. Det scenario som beskrivs i den här självstudien består av två viktigaste byggstenarna:

1. Att lägga till RingCentral från galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-ringcentral-from-the-gallery"></a>Att lägga till RingCentral från galleriet
För att konfigurera integrering av RingCentral i Azure AD, som du behöver lägga till RingCentral från galleriet i din lista över hanterade SaaS-appar.

**Utför följande steg för att lägga till RingCentral från galleriet:**

1. I den **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon. 

    ![image](./media/ringcentral-tutorial/selectazuread.png)

2. Gå till **företagsprogram**. Gå till **alla program**.

    ![image](./media/ringcentral-tutorial/a_select_app.png)
    
3. Lägg till nytt program, klicka på **nytt program** knappen överst i dialogrutan.

    ![image](./media/ringcentral-tutorial/a_new_app.png)

4. I sökrutan skriver **RingCentral**väljer **RingCentral** resultatet panelen klickar **Lägg till** för att lägga till programmet.

     ![image](./media/ringcentral-tutorial/a_add_app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet ska du konfigurera och testa Azure AD enkel inloggning med RingCentral baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning att fungera, behöver Azure AD du veta vad användaren motsvarighet i RingCentral är till en användare i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och relaterade användaren i RingCentral upprättas.

Om du vill konfigurera och testa Azure AD enkel inloggning med RingCentral, måste du utföra följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  – om du vill ge användarna använda den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  – om du vill testa Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare RingCentral](#create-a-ringcentral-test-user)**  – du har en motsvarighet för Britta Simon i RingCentral som är länkad till en Azure AD-representation av användaren.
4. **[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  – om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  – om du vill kontrollera om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt RingCentral-program.

**Utför följande steg för att konfigurera Azure AD enkel inloggning med RingCentral:**

1. I den [Azure-portalen](https://portal.azure.com/)på den **RingCentral** application integration markerar **enkel inloggning**.

    ![image](./media/ringcentral-tutorial/b1_b2_select_sso.png)

2. Klicka på **ändra enkel inloggning läge** på skärmen för att välja den **SAML** läge.

      ![image](./media/ringcentral-tutorial/b1_b2_saml_ssso.png)

3. På den **väljer du en metod för enkel inloggning** dialogrutan Välj **SAML** läge för att aktivera enkel inloggning.

    ![image](./media/ringcentral-tutorial/b1_b2_saml_sso.png)

4. På den **ange in enkel inloggning med SAML** klickar du på **redigera** knappen för att öppna **SAML grundkonfiguration** dialogrutan.

    ![image](./media/ringcentral-tutorial/b1-domains_and_urlsedit.png)

5. På den **SAML grundkonfiguration** om du har **tjänstleverantör metadatafil**, utför följande steg:

    a. Klicka på **ladda upp metadatafilen**.

    ![image](./media/ringcentral-tutorial/b9_saml.png)

    b. Klicka på **mappen logotyp** att välja metadatafilen och klicka på **överför**.

    ![image](./media/ringcentral-tutorial/b9(1)_saml.png)

    c. När metadatafilen har laddats upp den **identifierare** och **svars-URL** värden får automatiskt ifylld i **SAML grundkonfiguration** avsnittet textrutan som visas nedan :

    ![image](./media/ringcentral-tutorial/b21-domains_and_urls.png)

    d. I den **inloggnings-URL** textrutan anger du ett URL:
    | |
    |--|
    | `https://service.ringcentral.com` |
    | `https://service.ringcentral.com.au` |
    | `https://service.ringcentral.co.uk` |
    | `https://service.ringcentral.eu` |

    > [!NOTE]
    > Du får den **tjänstleverantör metadatafil** på sidan RingCentral SSO-konfiguration som beskrivs senare i självstudien.

6. Om du inte har **tjänstleverantör metadatafil**, utför följande steg:

    a. I den **inloggnings-URL** textrutan anger du ett URL:
    | |
    |--|
    | `https://service.ringcentral.com` |
    | `https://service.ringcentral.com.au` |
    | `https://service.ringcentral.co.uk` |
    | `https://service.ringcentral.eu` |

    b. I den **identifierare** textrutan anger du ett URL:
    | |
    |--|
    |  `https://sso.ringcentral.com` |
    | `https://ssoeuro.ringcentral.com` |

    c. I den **svars-URL** textrutan anger du ett URL:
    | |
    |--|
    | `https://sso.ringcentral.com/sp/ACS.saml2` |
    | `https://ssoeuro.ringcentral.com/sp/ACS.saml2` |

    ![image](./media/ringcentral-tutorial/b2-domains_and_urls.png)

7. På den **ange in enkel inloggning med SAML** sidan den **SAML-signeringscertifikat** klickar du på **hämta** att hämta certifikatet från de angivna alternativen enligt dina krav och spara den på din dator.

    ![image](./media/ringcentral-tutorial/certificatebase64.png)
    
8. I ett annat webbläsarfönster, logga in på RingCentral som en administratör.

9. På överst, klickar du på **verktyg**.

    ![image](./media/ringcentral-tutorial/ringcentral1.png)

10. Gå till **enkel inloggning**.

    ![image](./media/ringcentral-tutorial/ringcentral2.png)

11. På den **enkel inloggning** sidan under **SSO-konfiguration** avsnittet från **steg 1** klickar du på **redigera** och utför följande steg:

    ![image](./media/ringcentral-tutorial/ringcentral3.png)

12. På den **Konfigurera enkel inloggning** utför följande steg:

    ![image](./media/ringcentral-tutorial/ringcentral4.png)

    a. Klicka på **Bläddra** att ladda upp metadatafilen som du har hämtat från Azure-portalen.

    b. När du har överfört metadata hämta värdena fylls i **SSO allmän Information om** avsnittet.

    c. Under **attributmappning** väljer **kartan e-attributet till** som `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    d. Klicka på **Spara**.

    e. Från **steg 2** klickar du på **hämta** att ladda ned den **tjänstleverantör metadatafil** och ladda upp den i **SAML grundkonfiguration** avsnittet för automatisk polulate den **identifierare** och **svars-URL** värdena i Azure-portalen.

    ![image](./media/ringcentral-tutorial/ringcentral6.png) 

    f. På samma sida, navigerar du till **aktivera SSO** avsnittet och utför följande steg:

    ![image](./media/ringcentral-tutorial/ringcentral5.png)

    a. Välj **aktivera SSO-tjänsten**.
    
    b. Välj **Tillåt användare att logga in med autentiseringsuppgifter för enkel inloggning eller RingCentral**.

    c. Klicka på **Spara**.

### <a name="create-an-azure-ad-test-user"></a>Skapa en Azure AD-testanvändare

Målet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.

1. I Azure-portalen, i den vänstra rutan väljer **Azure Active Directory**väljer **användare**, och välj sedan **alla användare**.

    ![image](./media/ringcentral-tutorial/d_users_and_groups.png)

2. Välj **ny användare** överst på skärmen.

    ![image](./media/ringcentral-tutorial/d_adduser.png)

3. Utför följande steg i egenskaperna för användaren.

    ![image](./media/ringcentral-tutorial/d_userproperties.png)

    a. I den **namn** ange **BrittaSimon**.
  
    b. I den **användarnamn** fälttyp **brittasimon@yourcompanydomain.extension**  
    Till exempel, BrittaSimon@contoso.com

    c. Välj **egenskaper**väljer den **Show lösenord** kryssrutan och sedan skriva ned det värde som visas i rutan lösenord.

    d. Välj **Skapa**.
 
### <a name="create-a-ringcentral-test-user"></a>Skapa en RingCentral testanvändare

I det här avsnittet skapar du en användare som kallas Britta Simon i RingCentral. Arbeta med [RingCentral klienten supportteamet](https://success.ringcentral.com/RCContactSupp) att lägga till användare i RingCentral-plattformen. Användare måste skapas och aktiveras innan du använder enkel inloggning.

### <a name="assign-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändare

I det här avsnittet ska aktivera du Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till RingCentral.

1. I Azure-portalen väljer du **företagsprogram**väljer **alla program**.

    ![image](./media/ringcentral-tutorial/d_all_applications.png)

2. I listan med program väljer **RingCentral**.

    ![image](./media/ringcentral-tutorial/d_all_proapplications.png)

3. I menyn till vänster väljer **användare och grupper**.

    ![image](./media/ringcentral-tutorial/d_leftpaneusers.png)

4. Välj den **Lägg till** knappen och välj **användare och grupper** i den **Lägg till tilldelning** dialogrutan.

    ![image](./media/ringcentral-tutorial/d_assign_user.png)

4. I den **användare och grupper** dialogrutan Välj **Britta Simon** i listan över användare och klicka på den **Välj** längst ned på skärmen.

5. I den **Lägg till tilldelning** dialogrutan Välj den **tilldela** knappen.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet ska testa du Azure AD enkel inloggning för konfigurationen med hjälp av åtkomstpanelen.

När du klickar på panelen RingCentral i åtkomstpanelen du bör få automatiskt loggat in på ditt RingCentral-program.
Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över guider om hur du integrerar SaaS-appar med Azure Active Directory](tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/ringcentral-tutorial/tutorial_general_01.png
[2]: ./media/ringcentral-tutorial/tutorial_general_02.png
[3]: ./media/ringcentral-tutorial/tutorial_general_03.png
[4]: ./media/ringcentral-tutorial/tutorial_general_04.png

[100]: ./media/ringcentral-tutorial/tutorial_general_100.png

[200]: ./media/ringcentral-tutorial/tutorial_general_200.png
[201]: ./media/ringcentral-tutorial/tutorial_general_201.png
[202]: ./media/ringcentral-tutorial/tutorial_general_202.png
[203]: ./media/ringcentral-tutorial/tutorial_general_203.png

