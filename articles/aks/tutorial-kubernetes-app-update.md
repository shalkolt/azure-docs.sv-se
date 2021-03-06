---
title: Självstudie om Kubernetes i Azure – Uppdatera ett program
description: I den här självstudien om Azure Kubernetes Service (AKS) lär du dig hur du uppdaterar en befintlig programdistribution till AKS med en ny version av programkoden.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: tutorial
ms.date: 08/14/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: b2dd52fec112b879e072d3ac5598dd7978e68cbc
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/15/2018
ms.locfileid: "41918843"
---
# <a name="tutorial-update-an-application-in-azure-kubernetes-service-aks"></a>Självstudie: Uppdatera ett program i Azure Kubernetes Service (AKS)

När ett program har distribuerats i Kubernetes kan du uppdatera det genom att ange en ny containeravbildning eller avbildningsversion. När du gör det mellanlagras uppdateringen så att endast en del av distributionen uppdateras samtidigt. Den här mellanlagrade uppdateringen gör att programmet kan fortsätta att köras under uppdateringen. Det ger också en mekanism för återställning om ett distributionsfel inträffar.

I den här självstudien, som är del sex av sju, uppdateras Azure Vote-exempelappen. Lär dig att:

> [!div class="checklist"]
> * Uppdatera klientdelens programkod
> * Skapa en uppdaterad containeravbildning
> * Överföra containeravbildningen till Azure Container Registry
> * Distribuera den uppdaterade containeravbildningen

## <a name="before-you-begin"></a>Innan du börjar

I tidigare självstudier paketerades ett program i en behållaravbildning, avbildningen laddades upp till Azure Container Registry (ACR) och ett Kubernetes-kluster skapades. Programmet kördes därefter i Kubernetes-klustret.

En programlagringsplats klonades också som inkluderar programmets källkod och en färdig Docker Compose-fil som används i den här självstudien. Verifiera att du har skapat en klon av lagringsplatsen och att du har ändrat kataloger i den klonade katalogen. Om du inte har slutfört dessa steg och vill hänga med återgår du till [Självstudie 1 – Skapa containeravbildningar][aks-tutorial-prepare-app].

I den här självstudien måste du köra Azure CLI version 2.0.44 eller senare. Kör `az --version` för att hitta versionen. Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI][azure-cli-install].

## <a name="update-an-application"></a>Uppdatera ett program

Vi gör en ändring i exempelprogrammet och uppdaterar sedan den version som redan har distribuerats till ditt AKS-kluster. Exempelprogrammets källkod finns i katalogen *azure-vote*. Öppna filen *config_file.cfg* med en textredigerare, till exempel `vi`:

```console
vi azure-vote/azure-vote/config_file.cfg
```

Ändra värdena för *VOTE1VALUE* och *VOTE2VALUE* till olika färger. I följande exempel visas de uppdaterade färgvärdena:

```
# UI Configurations
TITLE = 'Azure Voting App'
VOTE1VALUE = 'Blue'
VOTE2VALUE = 'Purple'
SHOWHOST = 'false'
```

Spara och stäng filen.

## <a name="update-the-container-image"></a>Uppdatera containeravbildningen

För att återskapa klientdelsavbildningen och testa det uppdaterade programmet använder du [docker-compose][docker-compose]. Argumentet `--build` används till att instruera Docker Compose att återskapa programavbildningen:

```console
docker-compose up --build -d
```

## <a name="test-the-application-locally"></a>Testa appen lokalt

För att kontrollera att den uppdaterade containeravbildningen visar dina ändringar öppnar du en lokal webbläsare till http://localhost:8080.

![Bild av Kubernetes-kluster i Azure](media/container-service-kubernetes-tutorials/vote-app-updated.png)

De uppdaterade färgvärdena som anges i filen *config_file.cfg* visas i det program som körs.

## <a name="tag-and-push-the-image"></a>Tagga och överföra avbildningen

För att använda den uppdaterade avbildningen på rätt sätt taggar du avbildningen *azure-vote-front* med inloggningsservernamnet för ACR-registret. Hämta inloggningsservernamnet med kommandot [az acr list](/cli/azure/acr#az_acr_list):

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Använd [dockertagg][docker-tag] för att tagga avbildningen. Ersätt `<acrLoginServer>` med namnet på ACR-inloggningsservern eller värdnamnet för det offentliga registret och uppdatera avbildningsversionen till *:v2* på följande sätt:

```console
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:v2
```

Använd nu [docker push][docker-push] för att ladda upp avbildningen till registret. Ersätt `<acrLoginServer>` med namnet på din ACR-inloggningsserver. Om du får problem med push-överföring till ACR-registret, så kontrollera att du har kört kommandot [az acr login][az-acr-login].

```console
docker push <acrLoginServer>/azure-vote-front:v2
```

## <a name="deploy-the-updated-application"></a>Distribuera det uppdaterade programmet

Du får minimala störningar om flera instanser av programpodden körs. Kontrollera antalet klientdelsinstanser som körs med kommandot [kubectl get pods][kubectl-get]:

```
$ kubectl get pods

NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-217588096-5w632    1/1       Running   0          10m
azure-vote-front-233282510-b5pkz   1/1       Running   0          10m
azure-vote-front-233282510-dhrtr   1/1       Running   0          10m
azure-vote-front-233282510-pqbfk   1/1       Running   0          10m
```

Om du inte har flera klientdelspoddar skalar du *azure-vote-front*-distributionen på följande sätt:

```console
kubectl scale --replicas=3 deployment/azure-vote-front
```

När du ska uppdatera programmet använder du kommandot [kubectl set][kubectl-set]. Uppdatera `<acrLoginServer>` med inloggningsservern eller värdnamnet på ditt containerregister, och ange programversionen *v2*:

```console
kubectl set image deployment azure-vote-front azure-vote-front=<acrLoginServer>/azure-vote-front:v2
```

Du övervakar distributionen av kommandot [kubectl get pod][kubectl-get]. Eftersom det uppdaterade programmet är distribuerat avslutas dina poddar och återskapas med den nya containeravbildningen.

```console
kubectl get pods
```

Följande exempelutdata visar poddar som avslutas och nya instanser som körs medan distributionen pågår:

```
$ kubectl get pods

NAME                               READY     STATUS        RESTARTS   AGE
azure-vote-back-2978095810-gq9g0   1/1       Running       0          5m
azure-vote-front-1297194256-tpjlg  1/1       Running       0          1m
azure-vote-front-1297194256-tptnx  1/1       Running       0          5m
azure-vote-front-1297194256-zktw9  1/1       Terminating   0          1m
```

## <a name="test-the-updated-application"></a>Testa det uppdaterade programmet

Om du vill visa det uppdaterade programmet hämtar du först den externa IP-adressen för tjänsten `azure-vote-front`:

```console
kubectl get service azure-vote-front
```

Nu öppnar du en lokal webbläsare till IP-adressen.

![Bild av Kubernetes-kluster i Azure](media/container-service-kubernetes-tutorials/vote-app-updated-external.png)

## <a name="next-steps"></a>Nästa steg

I den här självstudien har du uppdaterat ett program och distribuerat uppdateringen till ett Kubernetes-kluster. Du har lärt dig att:

> [!div class="checklist"]
> * Uppdatera klientdelens programkod
> * Skapa en uppdaterad containeravbildning
> * Överföra containeravbildningen till Azure Container Registry
> * Distribuera den uppdaterade containeravbildningen

Gå vidare till nästa självstudie och lär dig hur du uppgraderar ett AKS-kluster till en ny version av Kubernetes.

> [!div class="nextstepaction"]
> [Uppgradera Kubernetes][aks-tutorial-upgrade]

<!-- LINKS - external -->
[docker-compose]: https://docs.docker.com/compose/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-set]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#set

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-upgrade]: ./tutorial-kubernetes-upgrade-cluster.md
[az-acr-login]: /cli/azure/acr#az_acr_login
[azure-cli-install]: /cli/azure/install-azure-cli
