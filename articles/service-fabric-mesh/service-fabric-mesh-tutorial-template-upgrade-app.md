---
title: Självstudie – uppgradera en app som körs i Azure Service Fabric Mesh | Microsoft Docs
description: I den här självstudien lär du dig hur du uppgraderar ett program som körs i Service Fabric Mesh.
services: service-fabric-mesh
documentationcenter: .net
author: rwike77
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/18/2018
ms.author: ryanwi
ms.custom: mvc, devcenter
ms.openlocfilehash: c82e0cd5bd6a15ff33f51b4e88f68c13080f595d
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46967973"
---
# <a name="tutorial-upgrade-a-service-fabric-application-running-in-service-fabric-mesh"></a>Självstudie: uppgradera ett program som körs i Service Fabric Mesh

Den här självstudiekursen är den tredje delen i en serie. Du får lära dig hur du uppgraderar ett Service Fabric-program som [tidigare har distribuerat till Service Fabric Mesh](service-fabric-mesh-tutorial-template-deploy-app.md) genom att öka de tilldelade CPU-resurserna.  När du är klar har du en webbtjänst för klientdelen som körs med högre CPU-resurser.

I den tredje delen i serien får du lära dig att:

> [!div class="checklist"]
> * Ändra programkonfigurationerna
> * Uppgradera ett program som körs i Service Fabric Mesh

I den här självstudieserien får du lära du dig att:
> [!div class="checklist"]
> * [Distribuera ett program till Service Fabric Mesh med en mall](service-fabric-mesh-tutorial-template-deploy-app.md)
> * [Skala ett program som körs i Service Fabric Mesh](service-fabric-mesh-tutorial-template-scale-services.md)
> * Uppgradera ett program som körs i Service Fabric Mesh
> * [Ta bort ett program](service-fabric-mesh-tutorial-template-remove-app.md)

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="prerequisites"></a>Nödvändiga komponenter

Innan du börjar den här självstudien:

* Om du inte har någon Azure-prenumeration kan du [skapa ett kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

* Öppna [Azure Cloud Shell](service-fabric-mesh-howto-setup-cli.md), eller [installera Azure CLI och Service Fabric Mesh CLI lokalt](service-fabric-mesh-howto-setup-cli.md#install-the-service-fabric-mesh-cli-locally).

## <a name="upgrade-application-configurations"></a>Uppgradera programkonfigurationerna

En av de största fördelarna med att distribuera program till Service Fabric Mesh är att du enkelt kan uppdatera din programkonfiguration.  Till exempel CPU- eller minnesresurser för dina tjänster.

I den här självstudien används exemplet med att göra-listan, som [distribuerades tidigare](service-fabric-mesh-tutorial-template-deploy-app.md) och som nu bör vara igång och köras. Programmet har två tjänster: WebFrontEnd och ToDoService. Varje tjänst distribuerades från början med CPU-resurser på 0,5.  Om du vill visa CPU-resurserna för WebFrontEnd-tjänsten kör du följande:

```azurecli
az mesh service show --resource-group myResourceGroup --name WebFrontEnd --app-name todolistapp
```

I distributionsmallen för programresursen har varje tjänst en *cpu*-egenskap som kan användas för att ange önskade CPU-resurser. Ett program kan bestå av flera tjänster där varje tjänst har en unik *CPU*-inställning som distribueras och hanteras tillsammans. För att öka processorresurserna för webbtjänsten för klientdelen, ändrar du *cpue*-värdet i distributionsmallen eller parametrarfilen.  Sedan uppgraderar du programmet.

### <a name="modify-the-deployment-template-parameters"></a>Ändra parametrarna för distributionsmallen

Om det finns värden i mallen som förväntas ändras när programmet har distribuerats, eller som du vill ha möjlighet att ändra beroende på distribution (om du planerar att återanvända mallen för andra distributioner), bör du parameterisera värdena.

Tidigare distribuerades programmet med hjälp av [mesh_rp.windows.json-distributionsmallen](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/todolist/mesh_rp.windows.json) och [mesh_rp.windows.parameter.json-parameterfilen](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/todolist/mesh_rp.windows.parameters.json).

Öppna [mesh_rp.windows.parameter.json-parameterfilen](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/todolist/mesh_rp.windows.parameters.json) lokalt och ange *frontEndCpu*-värdet till 1:

```json
      "frontEndCpu":{
        "value": "1"
      }
```

Spara dina ändringar i parameterfilen.  

Parametern *frontEndCpu* deklareras i *parametrar*-avsnittet i [distributionsmallen mesh_rp.windows.json](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/todolist/mesh_rp.windows.json):

```json
"frontEndCpu": {
    "defaultValue": "0.5",
    "type": "string",
    "metadata": {
        "description": "The CPU resources for the front end web service."
    }
}
```

WebFrontEnd-tjänsten *codePackages->resurser->begäranden->cpu* egenskapen refererar till *frontEndCpu*-parametern:

```json
    "services": [
          {
            "name": "WebFrontEnd",
            "properties": {
              "description": "WebFrontEnd description.",
              "osType": "Windows",
              "codePackages": [
                {
                  "name": "WebFrontEnd",
                  "image": "[parameters('frontEndImage')]",
                  ...
                  "resources": {
                    "requests": {
                      "cpu": "[parameters('frontEndCpu')]",
                      "memoryInGB": "[parameters('frontEndMemory')]"
                    }
                  },
                  "imageRegistryCredential": {
                    "server": "[parameters('registryServer')]",
                    "username": "[parameters('registryUserName')]",
                    "password": "[parameters('registryPassword')]"
                  }
                }
              ],
              ...
```

### <a name="upgrade-your-application"></a>Uppgraderar tillämpningen

När programmet körs kan du uppgradera det genom att omdistribuera mallen och den uppdaterade parameterfilen:

```azurecli
az mesh deployment create --resource-group myResourceGroup --template-file c:\temp\mesh_rp.windows.json --parameters c:\temp\mesh_rp.windows.parameters.json
```

Det här startar en rullande uppgraderingen av ditt program och du bör se hur dina CPU-resurser ökar inom några minuter.  Om du vill visa CPU-resurserna för WebFrontEnd-tjänsten kör du följande:

```azurecli
az mesh service show --resource-group myResourceGroup --name WebFrontEnd --app-name todolistapp
```

## <a name="next-steps"></a>Nästa steg

I den här självstudiedelen lärde du dig att:

> [!div class="checklist"]
> * Ändra programkonfigurationerna
> * Uppgradera ett program som körs i Service Fabric Mesh

Gå vidare till nästa kurs:
> [!div class="nextstepaction"]
> [Ta bort ett Service Fabric Mesh-program](service-fabric-mesh-tutorial-template-remove-app.md)
