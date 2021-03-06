---
title: Hantera logic apps i Visual Studio – Azure Logic Apps | Microsoft Docs
description: Hantera logic apps och andra Azure-tillgångar med Visual Studio Cloud Explorer
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.topic: article
ms.custom: mvc
ms.date: 03/15/2018
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: d4de75238e48b8eb955095b5a3823f2fed799fae
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/22/2018
ms.locfileid: "42445651"
---
# <a name="manage-logic-apps-with-visual-studio"></a>Hantera logic apps i Visual Studio

Även om du kan skapa, redigera, hantera och distribuera logic apps i den <a href="https://portal.azure.com" target="_blank">Azure-portalen</a>, du kan också använda Visual Studio när du vill lägga till logikappar för att köra källkontroll, publicera olika versioner och skapa [Azure-resurs Manager](../azure-resource-manager/resource-group-overview.md) mallar för olika distributionsmiljöer. Du kan hitta och hantera dina logic apps tillsammans med andra Azure-resurser med Visual Studio Cloud Explorer. Du kan till exempel öppna, hämta, redigera, kör, visa körningshistoriken, inaktivera och aktivera logic apps som redan har distribuerats i Azure-portalen. Om du inte har arbetat med Azure Logic Apps i Visual Studio, lär du dig [hur du skapar logikappar med Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md).

> [!IMPORTANT]
> Distribuera eller publicera en logikapp från Visual Studio skriver du över versionen av appen i Azure-portalen. Så om du gör ändringar i Azure portal som du vill behålla, se till att du [uppdatera logikappen i Visual Studio](#refresh) från Azure-portalen innan nästa gång du distribuerar eller publicera från Visual Studio.

<a name="requirements"></a>

## <a name="prerequisites"></a>Förutsättningar

* Om du heller inte har någon Azure-prenumeration kan du <a href="https://azure.microsoft.com/free/" target="_blank">registrera ett kostnadsfritt Azure-konto</a>.

* Hämta och installera följande verktyg, om du inte redan har dem: 

  * <a href="https://www.visualstudio.com/downloads" target="_blank">Visual Studio 2017 eller Visual Studio 2015 – Community Edition eller senare</a>. 
  I denna snabbstart används Visual Studio Community 2017 som är tillgängligt utan kostnad.

  * <a href="https://azure.microsoft.com/downloads/" target="_blank">Azure SDK (2.9.1 eller senare)</a> och <a href="https://github.com/Azure/azure-powershell#installation" target="_blank">Azure PowerShell</a>

  * <a href="https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio-18551" target="_blank">Azure Logic Apps Tools för Visual Studio 2017</a> eller för <a href="https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio" target="_blank">Visual Studio 2015</a> 
  
    Du kan hämta och installera Azure Logic Apps Tools direkt från Visual Studio Marketplace, eller läsa mer om <a href="https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions" target="_blank">hur du installerar tillägget från Visual Studio</a>. 
    Se till att starta om Visual Studio när installationen är klar.

* Åtkomst till Internet när du använder den inbäddade Logic Apps Designer

  Designer kräver en Internetanslutning för att kunna skapa resurser i Azure och läsa in egenskaper och data från anslutningarna i din logikapp. 
  Om du t.ex. använder Dynamics CRM Online-anslutningen kontrollerar Designer om CRM-instansen har några standardegenskaper och anpassade egenskaper.

<a name="find-logic-apps-vs"></a>

## <a name="find-your-logic-apps"></a>Hitta dina logikappar

Du kan hitta alla logikappar som är associerade med din Azure-prenumeration och distribueras i Azure-portalen med hjälp av Cloud Explorer i Visual Studio.

1. Öppna Visual Studio. På den **visa** menyn och välj **Cloud Explorer**.

2. I Cloud Explorer väljer **kontohantering**. Ange den prenumeration som är associerade med dina logikappar och välj sedan **tillämpa**. Exempel:

   ![Välj ”hantering”](./media/manage-logic-apps-with-visual-studio/account-management-select-Azure-subscription.png)

2. Baserat på om du vill söka av **resursgrupper** eller **resurstyper**, Följ dessa steg:

   * **Resursgrupper**: din Azure-prenumeration, Cloud Explorer visar alla resursgrupper som är associerade med den aktuella prenumerationen. 
   Expandera resursgruppen som innehåller logikappen och välj sedan din logikapp.

   * **Resurstyper**: din Azure-prenumeration, expandera **Logikappar**. När Cloud Explorer visar alla distribuerade logic-appar som är associerade med din prenumeration, väljer du din logikapp.

<a name="open-designer"></a>

## <a name="open-in-visual-studio"></a>Öppna i Visual Studio

I Visual Studio kan du öppna logikappar tidigare skapas och distribueras direkt via Azure portal eller Azure Resource Manager-projekt med Visual Studio.

1. Öppna Utforskaren i molnet och hitta din logikapp. 

2. På den logikapp snabbmenyn väljer **öppna med Logic App Editor**.

   Det här exemplet visar logikappar efter resurstyp, så att dina logikappar visas under den **Logic Apps** avsnittet.

  ![Öppna distribuerade logikapp från Azure-portalen](./media/manage-logic-apps-with-visual-studio/open-logic-app-in-editor.png)

   När logikappen öppnas i Logikappdesigner längst ned i Designern kan du välja **kodvy** så att du kan granska den underliggande strukturen av logic app-definition. 
   Om du vill skapa en Distributionsmall för logikappen kan du läsa [hur du hämtar en Azure Resource Manager-mall](#download-logic-app) för den logikappen. Läs mer om [Resource Manager-mallar](../azure-resource-manager/resource-group-overview.md#template-deployment).

<a name="download-logic-app"></a>

## <a name="download-from-azure"></a>Ladda ned från Azure

Du kan hämta logikappar från den <a href="https://portal.azure.com" target="_blank">Azure-portalen</a> och spara dem som [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) mallar. Du kan sedan lokalt Redigera mallar med Visual Studio och anpassa logikappar för olika distributionsmiljöer. Ladda ned logikappar automatiskt *funktionsfråga* deras definitioner i [Resource Manager-mallar](../azure-resource-manager/resource-group-overview.md#template-deployment), som också använder JavaScript Object Notation (JSON).

1. Öppna Cloud Explorer i Visual Studio och sedan söka efter och välj logikappen som du vill ladda ned från Azure.

2. På appens snabbmenyn väljer **öppna med Logic App Editor**.

   Logic App Designer öppnas och visar logikappen. 
   Om du vill granska underliggande definitionen och struktur, längst ned i designern för logic app väljer **kodvy**. 

3. I verktygsfältet för appdesignern väljer **hämta**.

   ![Välj ”Hämta”](./media/manage-logic-apps-with-visual-studio/download-logic-app.png)

4. När du uppmanas att ange en plats, bläddra till den platsen och spara Resource Manager-mall för logikappsdefinitionen i JSON (.json)-format. 

Din logikappens definition visas i den `resources` underavsnitt i Resource Manager-mallen. Du kan nu redigera logikappsdefinitionen och Resource Manager-mall med Visual Studio. Du kan också lägga till mallen som ett projekt med Azure Resource Manager en Visual Studio-lösning. Lär dig mer om [Resource Manager-projekt för logic apps i Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md). 

<a name="refresh"></a>

## <a name="refresh-from-azure"></a>Uppdatera från Azure

Om du redigerar din logikapp i Azure-portalen och vill behålla ändringarna, se till att du uppdaterar appens version i Visual Studio med dessa ändringar. 

* I Visual Studio Logic App Designer-verktygsfältet väljer **uppdatera**.

  ELLER

* Öppna snabbmenyn för din logikapp i Visual Studio Cloud Explorer och välj **uppdatera**. 

![Uppdatera logikapp med uppdateringar](./media/manage-logic-apps-with-visual-studio/refresh-logic-app.png)

## <a name="publish-logic-app-updates"></a>Publicera logic app-uppdateringar

När du är redo att distribuera dina logic appuppdateringar från Visual Studio till Azure, i verktygsfältet Logic App Designer väljer **publicera**.

![Publicera uppdaterade logic app](./media/manage-logic-apps-with-visual-studio/publish-logic-app.png)

## <a name="manually-run-your-logic-app"></a>Manuellt köra din logikapp

Du kan manuellt utlösa en logikapp som distribueras i Azure från Visual Studio. Välj Logic App Designer-verktygsfältet **kör utlösaren**.

![Köra logikappen manuellt](./media/manage-logic-apps-with-visual-studio/manually-run-logic-app.png)

## <a name="review-run-history"></a>Granska körningshistorik

För att kontrollera status och diagnostisera problem med logikapp-körningar, hittar du mer information, t.ex. indata och utdata för de som körs i Visual Studio.

1. Öppna snabbmenyn för din logikapp i Cloud Explorer och välj **öppen körningshistorik**.

   ![Öppna körningshistorik](./media/manage-logic-apps-with-visual-studio/view-run-history.png)

2. Dubbelklicka på en körning för att visa information om en specifik körning. Exempel:

   ![Detaljerad körningshistorik](./media/manage-logic-apps-with-visual-studio/view-run-history-details.png)
  
   > [!TIP]
   > Om du vill sortera tabellen efter egenskap, väljer du kolumnrubriken för egenskapen. 

3. Expandera stegen vars indata och utdata som du vill granska. Exempel:

   ![Visa indata och utdata för varje steg](./media/manage-logic-apps-with-visual-studio/run-inputs-outputs.png)

## <a name="disable-or-enable-logic-app"></a>Inaktivera eller aktivera logikappen

Du kan stoppa utlösaren från startas nästa gång när utlösarvillkoret är uppfyllt utan att ta bort din logikapp. Inaktiverar logikappen förhindrar Logic Apps-motorn från att skapa och köra framtida arbetsflödesinstanser för din logikapp.
Öppna snabbmenyn för din logikapp i Cloud Explorer och välj **inaktivera**.

![Inaktivera logikappen](./media/manage-logic-apps-with-visual-studio/disable-logic-app.png)

> [!NOTE]
> När du inaktiverar en logikapp kan instansieras inga nya körningar. Alla pågående och väntande körningar fortsätter tills de är klara, vilket kan ta tid att slutföra. 

När du är redo för din logikapp att fortsätta kan återaktivera du din logikapp. Öppna snabbmenyn för din logikapp i Cloud Explorer och välj **aktivera**.

![Aktivera din logikapp](./media/manage-logic-apps-with-visual-studio/enable-logic-app.png)

## <a name="delete-your-logic-app"></a>Ta bort din logikapp

Om du vill ta bort din logikapp från Azure-portalen i Cloud Explorer, öppna snabbmenyn för din logikapp och välj **ta bort**.

![Ta bort din logikapp](./media/manage-logic-apps-with-visual-studio/delete-logic-app.png)

> [!NOTE]
> När du tar bort en logikapp kan instansieras inga nya körningar. Alla pågående och väntande körningar har avbrutits. Om du har tusentals körningar ta annullering betydande tid att slutföra. 

## <a name="next-steps"></a>Nästa steg

I den här artikeln har du lärt dig hur du hanterar distribuerade logic apps i Visual Studio. Därefter lär du dig om hur du anpassar logic app-definitioner för distribution:

> [!div class="nextstepaction"]
> [Skapa logic app-definitioner i JSON](../logic-apps/logic-apps-author-definitions.md)
