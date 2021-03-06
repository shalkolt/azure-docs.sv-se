---
title: Med hjälp av API-versionsprofiler med Ruby i Azure Stack | Microsoft Docs
description: Lär dig mer om hur du använder API-versionsprofiler med Ruby i Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: B82E4979-FB78-4522-B9A1-84222D4F854B
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2018
ms.author: sethm
ms.reviewer: sijuman
ms.openlocfilehash: a000a54f79e479567168992cdd0786eb9e8b5c32
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/16/2018
ms.locfileid: "42055801"
---
# <a name="use-api-version-profiles-with-ruby-in-azure-stack"></a>Använd API-versionsprofiler med Ruby i Azure Stack

*Gäller för: integrerade Azure Stack-system och Azure Stack Development Kit*

## <a name="ruby-and-api-version-profiles"></a>Ruby- och API-versionsprofiler

Ruby SDK för Azure Stack Resource Manager innehåller verktyg som hjälper dig att skapa och hantera infrastrukturen. Resursprovidrar i SDK innehåller beräkning, virtuella nätverk och lagring med Ruby-språket. API-profiler i Ruby SDK aktivera hybrid molnutveckling genom att hjälpa dig att växla mellan globala Azure-resurser och resurser på Azure Stack.

En API-profil är en kombination av resursprovidrar och tjänstversioner. Du kan använda en API-profil för att kombinera olika resurstyper.

 - Att göra använder de senaste versionerna av alla tjänster kan använda den **senaste** profilen för Azure SDK samlad gem.
 - Om du vill använda tjänsterna som är kompatibla med Azure Stack, använda den **V2017_03_09** profilen för Azure SDK samlad gem.
 - Om du vill använda den senaste api-versionen av en tjänst, använda den **senaste** profilen för den specifika gem. Till exempel om du vill använda den senaste api-versionen av beräkningstjänst enbart använda de **senaste** profilen för den **Compute** gem.
 - Om du vill använda specifika api-versionen för en tjänst, använder du de specifika API-versioner som definierats i symbolen.

> [!Note]   
> Du kan kombinera alla alternativ i samma program.

## <a name="install-the-azure-ruby-sdk"></a>Installera Azure SDK för Ruby

 - Följ officiella anvisningarna för att installera [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
 - Följ officiella anvisningarna för att installera [Ruby](https://www.ruby-lang.org/en/documentation/installation/).
    - När du installerar väljer **lägga till Ruby PATH-variabeln**
    - Installera Dev-paket under Ruby-installationen när du tillfrågas.
    - Installera bundler med följande kommando:  
      `Gem install bundler`
 - Om det inte finns skapar du en prenumeration och spara prenumerations-ID som ska användas senare. Instruktioner för att skapa en prenumeration är [här](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm). 
 - Skapa ett huvudnamn för tjänsten och spara dess ID och hemlighet. Instruktioner för att skapa ett huvudnamn för tjänsten för Azure Stack är [här](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals). 
 - Kontrollera att din tjänstens huvudnamn har rollen deltagare/ägare av prenumerationen. Instruktioner om hur du tilldelar roll till tjänstens huvudnamn är [här](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals).

## <a name="install-the-rubygem-packages"></a>Installera rubygem-paket

Du kan installera azure rubygem-paket direkt.

````Ruby  
gem install azure_mgmt_compute
gem install azure_mgmt_storage
gem install azure_mgmt_resources
gem install azure_mgmt_network
Or use them in your Gemfile.
gem 'azure_mgmt_storage'
gem 'azure_mgmt_compute'
gem 'azure_mgmt_resources'
gem 'azure_mgmt_network'
````

Tänk Ruby SDK för Azure Resource Manager är en förhandsversion och kommer troligen att ha större ändringar i användargränssnittet i kommande versioner. Fler och fler i delversion kan tyda på större ändringar.

## <a name="usage-of-the-azuresdk-gem"></a>Användning av azure_sdk gem

Symbolen azure_sdk, är en sammanslagning av alla stöds gems i Ruby SDK. Den här symbolen består av en **senaste** -profil som har stöd för den senaste versionen av alla tjänster. Det inför en profil för en ny version **V2017_03_09** -profil som har skapats för Azure Stack.

Du kan installera azure_sdk samlad symbolen med följande kommando:  

````Ruby  
  gem install 'azure_sdk
````

## <a name="prerequisite"></a>Krav

För att kunna använda Azure SDK för Ruby med Azure Stack, måste du ange följande värden och ange sedan värden med miljövariabler. Se anvisningarna efter tabellen för operativsystemet på ställa in miljövariabler. 

| Värde | Miljövariabler | Beskrivning | 
| --- | --- | --- | --- |
| Klient-ID:t | AZURE_TENANT_ID | Värdet för Azure Stack [klient-ID](https://docs.microsoft.com/azure/azure-stack/azure-stack-identity-overview). |
| Klient-ID | AZURE_CLIENT_ID | Tjänsten huvudnamn program-ID sparas när tjänstens huvudnamn har skapats i föregående avsnitt i det här dokumentet.  |
| Prenumerations-ID:t | AZURE_SUBSCRIPTION_ID | Den [prenumerations-ID](https://docs.microsoft.com/azure/azure-stack/azure-stack-plan-offer-quota-overview#subscriptions) är hur du kommer åt erbjudanden i Azure Stack. |
| Klienthemlighet | AZURE_CLIENT_SECRET | Huvudnamn tjänstprogrammet hemlighet sparas när tjänstens huvudnamn har skapats. |
| Resource Manager-slutpunkten | ARM_ENDPOINT | Se [The Azure Stack resource manager endpoin](#The-azure-stack-resource-manager-endpoint).  |

### <a name="the-azure-stack-resource-manager-endpoint"></a>Azure Stack resource manager-slutpunkt

Microsoft Azure Resource Manager är en management-ramverk som gör att administratörer kan distribuera, hantera och övervaka Azure-resurser. Azure Resource Manager kan hantera dessa uppgifter som en grupp i stället, i en enda åtgärd.

Du kan hämta metadata-information från Resource Manager-slutpunkten. Slutpunkten som returnerar en JSON-fil med den information som krävs för att köra din kod.

  > [!Note]  
  > Den **ResourceManagerUrl** i Azure Stack Development Kit (ASDK) är: `https://management.local.azurestack.external/`  
  > Den **ResourceManagerUrl** integrerade system är: `https://management.<location>.ext-<machine-name>.masd.stbtest.microsoft.com/`  
  > Att hämta de metadata som krävs: `<ResourceManagerUrl>/metadata/endpoints?api-version=1.0`
  
  JSON-exempelfilen:

  ```json
  { "galleryEndpoint": "https://portal.local.azurestack.external:30015/",  
    "graphEndpoint": "https://graph.windows.net/",  
    "portal Endpoint": "https://portal.local.azurestack.external/", 
    "authentication": {
      "loginEndpoint": "https://login.windows.net/", 
      "audiences": ["https://management.<yourtenant>.onmicrosoft.com/3cc5febd-e4b7-4a85-a2ed-1d730e2f5928"]
    }
  }
  ```

### <a name="set-environmental-variables"></a>Ange miljövariabler

**Microsoft Windows**  
Ange miljövariabler, i Windows kommandotolk genom att använda följande format:  
`set AZURE_TENANT_ID=<YOUR_TENANT_ID>`

**macOS, Linux och Unix-baserade system**  
I Unix-baserat system, kan du använda kommandot som:  
`export AZURE_TENANT_ID=<YOUR_TENANT_ID>`

## <a name="existing-api-profiles"></a>Befintliga API-profiler

Azure_sdk samlad gem har följande två profiler:

1. **V2017_03_09**  
  Profil som skapats för Azure Stack. Använd den här profilen för tjänster som är mest kompatibla med Azure Stack.
2. **senaste**  
  Profil består av senaste versionerna av alla tjänster. Använd de senaste versionerna av alla tjänster.

Mer information om Azure Stack och API-profiler finns i en [sammanfattning av API-profiler](azure-stack-version-profiles.md#summary-of-api-profiles).

## <a name="azure-ruby-sdk-api-profile-usage"></a>Användningen av Azure Ruby SDK API-profil

Följande rader ska användas för att skapa en instans av en profil-klient. Den här parametern är endast krävs för Azure Stack eller andra privata moln. Global Azure har redan inställningarna som standard.

````Ruby  
active_directory_settings = get_active_directory_settings(ENV['ARM_ENDPOINT'])

provider = MsRestAzure::ApplicationTokenProvider.new(
    ENV['AZURE_TENANT_ID'],
    ENV['AZURE_CLIENT_ID'],
    ENV['AZURE_CLIENT_SECRET'],
    active_directory_settings
)
credentials = MsRest::TokenCredentials.new(provider)
options = {
    credentials: credentials,
    subscription_id: subscription_id,
    active_directory_settings: active_directory_settings,
    base_url: ENV['ARM_ENDPOINT']
}

# Target profile built for Azure Stack
client = Azure::Resources::Profiles::V2017_03_09::Mgmt::Client.new(options)
````

Profil-klienten kan användas för åtkomst till enskilda resource-leverantörer, till exempel beräkning, lagring och nätverk.

````Ruby  
# To access the operations associated with Compute
profile_client.compute.virtual_machines.get 'RESOURCE_GROUP_NAME', 'VIRTUAL_MACHINE_NAME'

# Option 1: To access the models associated with Compute
purchase_plan_obj = profile_client.compute.model_classes.purchase_plan.new

# Option 2: To access the models associated with Compute
# Notice Namespace: Azure::Profiles::<Profile Name>::<Service Name>::Mgmt::Models::<Model Name>
purchase_plan_obj = Azure::Profiles::V2017_03_09::Compute::Mgmt::Models::PurchasePlan.new
````

## <a name="define-azurestack-environment-setting-functions"></a>Definiera AzureStack miljö inställningen funktioner

För att autentisera tjänstobjektet i Azure Stack-miljön, definiera slutpunkter med **get_active_directory_settings()**. Den här metoden använder den **ARM_Endpoint** miljövariabeln som du anger när din miljövariabler.

````Ruby  
# Get Authentication endpoints using Arm Metadata Endpoints
def get_active_directory_settings(armEndpoint)
    settings = MsRestAzure::ActiveDirectoryServiceSettings.new
    response = Net::HTTP.get_response(URI("#{armEndpoint}/metadata/endpoints?api-version=1.0"))
    status_code = response.code
    response_content = response.body
    unless status_code == "200"
        error_model = JSON.load(response_content)
        fail MsRestAzure::AzureOperationError.new("Getting Azure Stack Metadata Endpoints", response, error_model)
    end
    result = JSON.load(response_content)
    settings.authentication_endpoint = result['authentication']['loginEndpoint'] unless result['authentication']['loginEndpoint'].nil?
    settings.token_audience = result['authentication']['audiences'][0] unless result['authentication']['audiences'][0].nil?
    settings
end
````

## <a name="samples-using-api-profiles"></a>Exempel med hjälp av API-profiler

Du kan använda följande exempel finns i GitHub repositoreis som en referens som skapar lösningar med Ruby- och API: T för Azure Stack-profiler:

 - [Hantera Azure-resurser och -resursgrupper med Ruby](https://github.com/Azure-Samples/resource-manager-ruby-resources-and-groups/tree/master/Hybrid)
 - [Hantera virtuella datorer med hjälp av Ruby](https://github.com/Azure-Samples/compute-ruby-manage-vm/tree/master/Hybrid)
 - [Distribuera en SSH aktiverad virtuell dator med en mall i Ruby](https://github.com/Azure-Samples/resource-manager-ruby-template-deployment/tree/master/Hybrid)

### <a name="sample-resource-manager-and-groups"></a>Exemplet Resource Manager och grupper

Se till att du har installerat Ruby för att köra exemplet. Om du använder Visual Studio Code, hämta Ruby SDK: N som en utökning av samt. 

> [!Note]  
> Du kan hämta lagringsplatsen för exemplet på ”[hantera Azure-resurser och resursgrupper med Ruby](https://github.com/Azure-Samples/resource-manager-ruby-resources-and-groups/tree/master/Hybrid)”.

1. Klona databasen.

    ````Bash
    git clone https://github.com/Azure-Samples/resource-manager-ruby-resources-and-groups.git
    ````

2. Installera beroenden med hjälp av paket.

    ````Bash
    cd resource-manager-ruby-resources-and-groups\Hybrid\
    bundle install
    ````

3. Skapa ett tjänstobjekt i Azure med hjälp av PowerShell och hämta de värden som behövs. 

  Anvisningar om hur du skapar ett huvudnamn för tjänsten finns i [med Azure PowerShell för att skapa ett huvudnamn för tjänsten med ett certifikat](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals).

  Värden som behövs är:
  - Klient-ID:t
  - Klient-ID
  - Klienthemlighet
  - Prenumerations-ID:t
  - Resource Manager-slutpunkten

  Ange följande miljövariabler med hjälp av informationen som du hämtade från tjänstens huvudnamn du skapade.

  - Exportera AZURE_TENANT_ID = {ditt klient-id}
  - Exportera AZURE_CLIENT_ID = {din klient-id}
  - Exportera AZURE_CLIENT_SECRET = {klienthemlighet}
  - Exportera AZURE_SUBSCRIPTION_ID = {ditt prenumerations-id}
  - Exportera ARM_ENDPOINT = {din AzureStack Resource manager-url}

  > [!Note]  
  > På Windows, Använd i stället för export.

4. Kontrollera plats-variabeln anges till din AzureStack plats. Till exempel lokala = ”local”

5. Lägga till i följande rad med kod om du använder Azure Stack eller andra privata moln och ange rätt active directory-slutpunkter.

  ````Ruby  
  active_directory_settings = get_active_directory_settings(ENV['ARM_ENDPOINT'])
  ````

6. Lägga till inställningarna för active directory och den grundläggande Webbadressen ska fungera med Azure Stack i alternativ-variabeln. 

  ````Ruby  
  options = {
    credentials: credentials,
    subscription_id: subscription_id,
    active_directory_settings: active_directory_settings,
    base_url: ENV['ARM_ENDPOINT']
  }
  ````

7. Skapa profil-klient som riktar sig mot Azure Stack-profil:

  ````Ruby  
    client = Azure::Resources::Profiles::V2017_03_09::Mgmt::Client.new(options)
  ````

8. För att autentisera tjänstens huvudnamn med Azure Stack slutpunkterna ska definieras med hjälp av **get_active_directory_settings()**. Den här metoden använder den **ARM_Endpoint** miljövariabeln som du anger när din miljövariabler.

  ````Ruby  
  def get_active_directory_settings(armEndpoint)
    settings = MsRestAzure::ActiveDirectoryServiceSettings.new
    response = Net::HTTP.get_response(URI("#{armEndpoint}/metadata/endpoints?api-version=1.0"))
    status_code = response.code
    response_content = response.body
    unless status_code == "200"
      error_model = JSON.load(response_content)
      fail MsRestAzure::AzureOperationError.new("Getting Azure Stack Metadata Endpoints", response, error_model)
    end
    result = JSON.load(response_content)
    settings.authentication_endpoint = result['authentication']['loginEndpoint'] unless result['authentication']['loginEndpoint'].nil?
    settings.token_audience = result['authentication']['audiences'][0] unless result['authentication']['audiences'][0].nil?
    settings
  end
  ````

9. Kör exemplet.

  ````Ruby
    bundle exec ruby example.rb
  ````

## 

## <a name="next-steps"></a>Nästa steg

* [Installera PowerShell för Azure Stack](azure-stack-powershell-install.md)
* [Konfigurera PowerShell-miljö för Azure Stack-användare](azure-stack-powershell-configure-user.md)  
