---
title: Azure CLI-skriptexempel – Begränsa webbtrafik | Microsoft Docs
description: Azure CLI-skriptexempel – Skapa en programgateway med en brandvägg för webbaserade program och en VM-skalningsuppsättning som använder OWASP-regler till att begränsa trafiken.
services: application-gateway
documentationcenter: networking
author: vhorne
manager: jpconnock
editor: tysonn
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 01/29/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 4201b85ac50f69cc56bbfd4acde685a24f5dee34
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/14/2018
ms.locfileid: "45575889"
---
# <a name="restrict-web-traffic-using-the-azure-cli"></a>Begränsa webbtrafik med hjälp av Azure CLI

Med det här skriptet skapar du en programgateway med en brandvägg för webbaserade program som använder en VM-skalningsuppsättning för serverdelen. Brandväggen för webbaserade program begränsar webbtrafiken baserat på OWASP-regler. När du har kört skriptet kan du testa programgatewayen med hjälp av dess offentliga IP-adress.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exempelskript

[!code-azurecli-interactive[main](../../../cli_scripts/application-gateway/create-vmss/create-vmss-waf.sh "Create application gateway")]

## <a name="clean-up-deployment"></a>Rensa distribution 

Kör följande kommando för att ta bort resursgruppen, programgatewayen och alla relaterade resurser.

```azurecli-interactive 
az group delete --name myResourceGroupAG --yes
```

## <a name="script-explanation"></a>Förklaring av skript

Det här skriptet använder följande kommandon för att skapa distributionen. Varje post i tabellen länkar till kommandospecifik dokumentation.

| Kommando | Anteckningar |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az-group-create) | Skapar en resursgrupp där alla resurser lagras. |
| [az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet#az-net) | Skapar ett virtuellt nätverk. |
| [az network vnet subnet create](https://docs.microsoft.com/cli/azure/network/vnet/subnet#az-network_vnet_subnet_create) | Skapar ett undernät i ett virtuellt nätverk. |
| [az network public-ip create](https://docs.microsoft.com/cli/azure/network/public-ip?view=azure-cli-latest) | Skapar den offentliga IP-adressen för programgatewayen. |
| [az network application-gateway create](https://docs.microsoft.com/cli/azure/network/application-gateway?view=azure-cli-latest) | Skapar en programgateway. |
| [az vmss create](https://docs.microsoft.com/cli/azure/vmss#az-vmss-create) | Skapar en VM-skalningsuppsättning. |
| [az storage account create](https://docs.microsoft.com/cli/azure/storage/account#az-storage-account-create) | Skapar ett lagringskonto. |
| [az monitor diagnostic-settings create](https://docs.microsoft.com/cli/azure/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create) | Skapar ett lagringskonto. |
| [az network public-ip show](https://docs.microsoft.com/cli/azure/network/public-ip#az-network_public_ip_show) | Hämtar programgatewayens offentliga IP-adress. |

## <a name="next-steps"></a>Nästa steg

Mer information om Azure CLI finns i [Azure CLI-dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Du hittar fler CLI-skriptexempel för programgatewayer i [dokumentationen för Azure Application Gateway](../cli-samples.md).
