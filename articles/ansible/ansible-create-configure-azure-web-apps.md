---
title: Skapa Azure-webbappar med Ansible (förhandsversion)
description: Lär dig att använda Ansible för att skapa en webbapp med Java 8 och Tomcat-körmiljö för container i App Service i Linux
ms.service: ansible
keywords: ansible, azure, devops, bash, playbook, Azure App Service, Web App, Java
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 09/20/2018
ms.openlocfilehash: 48b4c201b2b96bd4662e8c90be7298a4f418af53
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49426565"
---
# <a name="create-azure-app-service-web-apps-by-using-ansible-preview"></a>Skapa Azure App Service-webbappar med Ansible (förhandsversion)
[Azure App Service Web Apps](https://docs.microsoft.com/azure/app-service/app-service-web-overview) (eller bara Web Apps) är värd för webbprogram, REST API:er och mobila serverdelar. Du kan utveckla i det språk du föredrar&mdash; .NET, .NET Core, Java, Ruby, Node.js, PHP eller Python.

Med Ansible kan du automatisera distributionen och konfigurationen av resurser i din miljö. Den här artikeln visar hur du använder Ansible för att skapa en webbapp med Java Runtime. 

## <a name="prerequisites"></a>Nödvändiga komponenter
- **Azure-prenumeration** – Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) konto innan du börjar.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]

> [!Note]
> Ansible 2.7 krävs för att köra följande exempelspelböcker i den här självstudien. Du kan installera Ansible 2.7 RC-versionen genom att köra `sudo pip install ansible[azure]==2.7.0rc2`. När Ansible 2.7 har släppts behöver du inte ange en version här eftersom standardversionen blir 2.7. 

## <a name="create-a-simple-app-service"></a>Skapa en enkel apptjänst
I den här delen finns en Ansible-exempelspelbok som definierar följande resurser:
- Resursgrupp, som din App Service-plan och webbapp distribueras till
- Webbapp med Java 8 och Tomcat-körmiljö för container i App Service i Linux

```
- hosts: localhost
  connection: local
  vars:
    resource_group: myfirstResourceGroup
    webapp_name: myfirstWebApp
    location: eastus
  tasks:
    - name: Create a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        location: "{{ location }}"

    - name: Create App Service on Linux with Java Runtime
      azure_rm_webapp:
        resource_group: "{{ resource_group }}"
        name: "{{ webapp_name }}"
        plan:
          resource_group: "{{ resource_group }}"
          name: myappplan
          is_linux: true
          sku: S1
          number_of_workers: 1
        frameworks:
          - name: "java"
            version: "8"
            settings:
              java_container: tomcat
              java_container_version: 8.5
```
Spara den föregående spelboken som **firstwebapp.yml**.

Om du vill köra spelboken använder du kommandot **ansible-playbook** så här:
```bash
ansible-playbook firstwebapp.yml
```

Utdata efter körningen av Ansible-spelboken visar att webbappen har skapats:

```
TASK [Create a resource group] *************************************************
changed: [localhost]

TASK [Create App Service on Linux with Java Runtime] ******************************
 [WARNING]: Azure API profile latest does not define an entry for
WebSiteManagementClient
changed: [localhost]

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=2    unreachable=0    failed=0   
```

## <a name="create-an-app-service-by-using-traffic-manager"></a>Skapa en apptjänst med hjälp av Traffic Manager
Du kan använda [Azure Traffic Manager](https://docs.microsoft.com/azure/app-service/web-sites-traffic-manager) för att styra hur begäranden från webbklienter distribueras till appar i Azure App Service. När App Service-slutpunkter läggs till i en Azure Traffic Manager-profil spårar Traffic Manager statusen för dina App Service-appar. Statusvärden är körs, stoppad och borttagen. Traffic Manager kan sedan bestämma vilka av dess slutpunkter som ska ta emot trafik.

I App Service körs en app i en [App Service-plan](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview
). En App Service-plan definierar en uppsättning beräkningsresurser som en webbapp ska köra. Du kan hantera din App Service-plan och webbapp i olika grupper.

I den här delen finns en Ansible-exempelspelbok som definierar följande resurser:
- Resursgrupp, som din App Service-plan distribueras till
- App Service-plan
- Sekundär resursgrupp, som din webbapp distribueras till
- Webbapp med Java 8 och Tomcat-körmiljö för container i App Service i Linux
- Traffic Manager-profil
- Traffic Manager-slutpunkt med hjälp av webbplatsen du har skapat

```
- hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    plan_resource_group: planResourceGroup
    app_name: myLinuxWebApp
    location: eastus
    linux_plan_name: myAppServicePlan
    traffic_manager_profile_name: myTrafficManagerProfile
    traffic_manager_endpoint_name: myTrafficManagerEndpoint

  tasks:
  - name: Create resource group
    azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        location: "{{ location }}"

  - name: Create secondary resource group
    azure_rm_resourcegroup:
        name: "{{ plan_resource_group }}"
        location: "{{ location }}"

  - name: Create App Service Plan
    azure_rm_appserviceplan:
      resource_group: "{{ plan_resource_group }}"
      name: "{{ linux_plan_name }}"
      location: "{{ location }}"
      is_linux: true
      sku: S1
      number_of_workers: 1

  - name: Create App Service on Linux with Java Runtime
    azure_rm_webapp:
        resource_group: "{{ resource_group }}"
        name: "{{ app_name }}"
        plan:
          resource_group: "{{ plan_resource_group }}"
          name: "{{ linux_plan_name }}"
          is_linux: true
          sku: S1
          number_of_workers: 1
        app_settings:
          testkey: "testvalue"
        frameworks:
          - name: java
            version: 8
            settings:
              java_container: "Tomcat"
              java_container_version: "8.5"

  - name: Get web app facts
    azure_rm_webapp_facts:
      resource_group: "{{ resource_group }}"
      name: "{{ app_name }}"
    register: webapp
    
  - name: Create Traffic Manager Profile
    azure_rm_trafficmanagerprofile:
      resource_group: "{{ resource_group }}"
      name: "{{ traffic_manager_profile_name }}"
      location: global
      routing_method: performance
      dns_config:
        relative_name: "{{ traffic_manager_profile_name }}"
        ttl:  60
      monitor_config:
        protocol: HTTPS
        port: 80
        path: '/'

  - name: Add endpoint to traffic manager profile, using created web site
    azure_rm_trafficmanagerendpoint:
      resource_group: "{{ resource_group }}"
      profile_name: "{{ traffic_manager_profile_name }}"
      name: "{{ traffic_manager_endpoint_name }}"
      type: azure_endpoints
      location: "{{ location }}"
      target_resource_id: "{{ webapp.webapps[0].id }}"

```
Spara den föregående spelboken som **webapp.yml** eller [ladda ned spelboken](https://github.com/Azure-Samples/ansible-playbooks/blob/master/webapp.yml).

Om du vill köra spelboken använder du kommandot **ansible-playbook** så här:
```bash
ansible-playbook webapp.yml
```

Utdata från körningen av Ansible-spelboken visar att App service-planen, webbappen, Traffic Manager-profilen och slutpunkten har skapats:
```
TASK [Create resource group] ****************************************************************************
changed: [localhost]

TASK [Create resource group for app service plan] ****************************************************************************
changed: [localhost]

TASK [Create App Service Plan] ****************************************************************************
 [WARNING]: Azure API profile latest does not define an entry for WebSiteManagementClient

changed: [localhost]

TASK [Create App Service on Linux with Java Runtime] ****************************************************************************
changed: [localhost]

TASK [Get web app facts] *****************************************************************************
ok: [localhost]

TASK [Create Traffic Manager Profile] *****************************************************************************
 [WARNING]: Azure API profile latest does not define an entry for TrafficManagerManagementClient

changed: [localhost]

TASK [Add endpoint to traffic manager profile, using the web site created above] *****************************************************************************
changed: [localhost]

TASK [Get Traffic Manager Profile facts] ******************************************************************************
ok: [localhost]

PLAY RECAP ******************************************************************************
localhost                  : ok=9    changed=6    unreachable=0    failed=0
```

## <a name="next-steps"></a>Nästa steg
> [!div class="nextstepaction"] 
> [Ansible i Azure](https://docs.microsoft.com/azure/ansible/)