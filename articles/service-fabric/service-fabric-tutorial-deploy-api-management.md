---
title: "Azure Service Fabric integraci se službou API Management | Microsoft Docs"
description: "Zjistěte, jak rychle začít s Azure API Management a Service Fabric."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/10/2017
ms.author: ryanwi
ms.openlocfilehash: b3bf91a7ae5ed70456352ecdba9c0324f20f3555
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="deploy-api-management-with-service-fabric"></a>Nasazení správy rozhraní API pomocí Service Fabric
V tomto kurzu je součástí série, tři.  Nasazení aplikace Azure API Management s Service Fabric je pokročilý scénář.  API Management je užitečné, když je potřeba publikovat rozhraní API s bohatou sadu pravidla směrování pro váš back endové služby Service Fabric. Cloudové aplikace obvykle nutné front-endu brány zajistit jediný bod příjem příchozích dat pro uživatele, zařízení nebo jiné aplikace. V Service Fabric brány může být jakékoli bezstavové služby určený pro příchozí provoz například APP.NET základní aplikace, služby Event Hubs, IoT Hub nebo Azure API Management. 

V tomto kurzu se dozvíte, jak nastavit [Azure API Management](../api-management/api-management-key-concepts.md) s Service Fabric přesměrovat provoz na back-end služby v Service Fabric.  Jakmile budete hotovi, nasazené API Management k virtuální síti, nakonfigurované operace rozhraní API odesílat provoz do bezstavové služby back-end. Další informace o scénářích Azure API Management s Service Fabric najdete v tématu [přehled](service-fabric-api-management-overview.md) článku.

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Nasadit službu API Management
> * Konfigurace správy rozhraní API
> * Vytvoření operace rozhraní API
> * Nakonfigurujte zásady back-end
> * Přidání rozhraní API do produktu

V této série kurzu zjistíte, jak:
> [!div class="checklist"]
> * Vytvoření zabezpečeného [clusteru se systémem Windows](service-fabric-tutorial-create-vnet-and-windows-cluster.md) nebo [Linux clusteru](service-fabric-tutorial-create-vnet-and-linux-cluster.md) v Azure pomocí šablony
> * [Škálování clusteru příchozí nebo odchozí](/service-fabric-tutorial-scale-cluster.md)
> * Nasazení správy rozhraní API pomocí Service Fabric

## <a name="prerequisites"></a>Požadavky
Před zahájením tohoto kurzu:
- Pokud nemáte předplatné Azure, vytvořte [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Nainstalujte [prostředí Azure Powershell verze modulu 4.1 nebo vyšší](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) nebo [Azure CLI 2.0](/cli/azure/install-azure-cli).
- Vytvoření zabezpečeného [clusteru se systémem Windows](service-fabric-tutorial-create-vnet-and-windows-cluster.md) nebo [Linux clusteru](service-fabric-tutorial-create-vnet-and-linux-cluster.md) v Azure
- Pokud nasadíte clusteru se systémem Windows, nastavení vývojového prostředí systému Windows. Nainstalujte [Visual Studio 2017](http://www.visualstudio.com) a **Azure development**, **ASP.NET a webové vývoj**, a **vývoj pro různé platformy .NET Core**úlohy.  Potom nastavit [vývojové prostředí .NET](service-fabric-get-started.md).
- Pokud nasadíte Linux cluster, nastavit vývojové prostředí Java na [Linux](service-fabric-get-started-linux.md) nebo [systému MacOS](service-fabric-get-started-mac.md).  Nainstalujte [služby Fabric rozhraní příkazového řádku](service-fabric-cli.md). 

## <a name="network-topology"></a>Topologie sítě
Teď, když máte zabezpečený [clusteru se systémem Windows](service-fabric-tutorial-create-vnet-and-windows-cluster.md) nebo [Linux clusteru](service-fabric-tutorial-create-vnet-and-linux-cluster.md) v Azure, a nasadit API Management na virtuální síť (VNET) v podsíti a NSG navržený pro API Management. V tomto kurzu je předem nakonfigurovaný k použití názvy virtuální sítě, podsítě a NSG, které jste nastavili v předchozím šablony Resource Manageru rozhraní API správy [kurzu clusteru Windows](service-fabric-tutorial-create-vnet-and-windows-cluster.md) nebo [kurz Linux clusteru](service-fabric-tutorial-create-vnet-and-linux-cluster.md). V tomto kurzu nasadí do Azure, ve které jsou v podsítě ve stejné virtuální síti API Management a Service Fabric následující topologie:

 ![Popisek obrázku][sf-apim-topology-overview]

## <a name="sign-in-to-azure-and-select-your-subscription"></a>Přihlaste se k Azure a vybrat své předplatné
Přihlaste se ke svému účtu Azure vyberte předplatné, před spuštěním příkazů Azure.

```powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

```azurecli
az login
az account set --subscription <guid>
```

## <a name="deploy-a-service-fabric-back-end-service"></a>Nasazení služby back-end Service Fabric

Před konfigurací správy rozhraní API pro směrování provozu back endové službě Service Fabric, je třeba nejprve spuštěnou službu tak, aby přijímal požadavky.  Pokud jste dříve vytvořili [clusteru se systémem Windows](service-fabric-tutorial-create-vnet-and-windows-cluster.md), nasazení služby .NET Service Fabric.  Pokud jste dříve vytvořili [Linux clusteru](service-fabric-tutorial-create-vnet-and-linux-cluster.md), nasazení služby Java Service Fabric.

### <a name="deploy-a-net-service-fabric-service"></a>Nasazení služby .NET Service Fabric

V tomto kurzu vytvořte základní bezstavové ASP.NET Core spolehlivé služby pomocí výchozí šablona projektu webového rozhraní API. Tím se vytvoří koncový bod HTTP pro vaši službu, která vystavit pomocí Azure API Management.

Spuštění sady Visual Studio jako správce a vytvoření služby ASP.NET Core:

 1. V sadě Visual Studio vyberte soubor -> Nový projekt.
 2. Vyberte šablonu aplikace Service Fabric v cloudu a pojmenujte ji **"ApiApplication"**.
 3. Vyberte šablonu služby ASP.NET Core a název projektu **"WebApiService"**.
 4. Výběr šablony projektu webového rozhraní API ASP.NET Core 1.1.
 5. Po vytvoření projektu otevřete `PackageRoot\ServiceManifest.xml` a odebrat `Port` atribut z prostředků konfigurace koncového bodu:
 
    ```xml
    <Resources>
      <Endpoints>
        <Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" />
      </Endpoints>
    </Resources>
    ```

    Odebrání port umožňuje Service Fabric zadejte port dynamicky z rozsahu portů aplikace, otevřít prostřednictvím skupinu zabezpečení sítě v šabloně Resource Manager clusteru, umožňuje provoz mají být předány z API Management.
 
 6. Stisknutím klávesy F5 ve Visual Studiu ověřte webové rozhraní API není k dispozici místně. 

    Otevřete Service Fabric Explorer a podrobnostem konkrétní instanci služby ASP.NET Core najdete v části Základní adresa služby naslouchá na. Přidat `/api/values` s jejím základem adresy a otevřete v prohlížeči, který volá metodu Get na ValuesController v šabloně webového rozhraní API. Vrátí výchozí odpovědi, které poskytuje šablony, pole JSON, který obsahuje dva řetězce:

    ```json
    ["value1", "value2"]`
    ```

    Toto je koncový bod, který vystavit přes správu rozhraní API v Azure.

 7. Nakonec nasazení aplikace na cluster v Azure. V sadě Visual Studio, klikněte pravým tlačítkem na projekt aplikace a vyberte **publikovat**. Zadejte svůj koncový bod clusteru (například `mycluster.southcentralus.cloudapp.azure.com:19000`) k nasazení aplikace na cluster Service Fabric v Azure.

Bezstavové služby ASP.NET Core s názvem `fabric:/ApiApplication/WebApiService` teď by měl být spuštěn v clusteru Service Fabric v Azure.

### <a name="create-a-java-service-fabric-service"></a>Vytvoření služby Java Service Fabric
Pro tento kurz nasazení základní webový server, který vrátí zprávy zpět na uživatele. Ukázkovou aplikaci echo server obsahuje koncový bod HTTP pro vaši službu, která vystavit pomocí Azure API Management.

1. Klon Java získávání spuštění ukázky.

   ```bash
   git clone https://github.com/Azure-Samples/service-fabric-java-getting-started.git
   cd service-fabric-java-getting-started
   ```

2. Upravit *Services/EchoServer/EchoServer1.0/EchoServerApplication/EchoServerPkg/ServiceManifest.xml*. Aktualizujte koncový bod, aby služba naslouchá na portu 8081.

   ```xml
   <Endpoint Name="WebEndpoint" Protocol="http" Port="8081" />
   ```

3. Uložit *ServiceManifest.xml*, pak sestavení EchoServer1.0 aplikace.

   ```bash
   cd Services/EchoServer/EchoServer1.0/
   gradle
   ```

4. Nasazení aplikací do clusteru.

   ```bash
   cd Scripts
   sfctl cluster select --endpoint http://mycluster.southcentralus.cloudapp.azure.com:19080
   ./install.sh
   ```

   Java bezstavové služby s názvem `fabric:/EchoServerApplication/EchoServerService` teď by měl být spuštěn v clusteru Service Fabric v Azure.

5. Otevřete prohlížeč a zadejte v http://mycluster.southcentralus.cloudapp.azure.com:8081/getMessage, měli byste vidět "[verze 1.0] Hello, World!" zobrazí.

## <a name="download-and-understand-the-resource-manager-template"></a>Stažení a pochopení šablony Resource Manageru
Stáhněte a uložte následující Resource Manager soubor šablony a parametry:
 
- [APIM.JSON][apim-arm]
- [APIM.Parameters.JSON][apim-parameters-arm]

Následující části popisují prostředky se definované *apim.json* šablony. Další informace najdete na následujících odkazech na referenční dokumentaci šablony v každém oddílu. Konfigurovat parametry definované v metadatech *apim.parameters.json* souboru parametrů jsou nastavené později v tomto článku.

### <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service
[Microsoft.ApiManagement/service](/azure/templates/microsoft.apimanagement/service) popisuje instance služby API Management: název, SKU nebo vrstvy, umístění skupiny prostředků, informace o vydavateli a virtuální sítě.

### <a name="microsoftapimanagementservicecertificates"></a>Microsoft.ApiManagement/service/certificates
[Microsoft.ApiManagement/service/certificates](/azure/templates/microsoft.apimanagement/service/certificates) nakonfiguruje zabezpečení rozhraní API Management. API Management musí ověřit k vašemu clusteru Service Fabric pro zjišťování služby pomocí klientského certifikátu, který má přístup ke clusteru. Tento kurz používá stejný certifikát zadaný dříve při vytváření [clusteru se systémem Windows](service-fabric-tutorial-create-vnet-and-windows-cluster.md#createvaultandcert_anchor) nebo [Linux clusteru](service-fabric-tutorial-create-vnet-and-linux-cluster.md#createvaultandcert_anchor), který ve výchozím nastavení lze použít pro přístup k vaší clusteru. 

Tento kurz používá stejný certifikát pro ověřování klientů a zabezpečení mezi uzly clusteru. Pokud nemáte nakonfigurovaná pro přístup k vaší cluster Service Fabric, můžete použít samostatné klientský certifikát. Zadejte **název**, **heslo**, a **data** (řetězec s kódováním base-64) privátní klíč souboru (.pfx) clusteru certifikátu, který jste zadali při vytváření vašeho Cluster Service Fabric.

### <a name="microsoftapimanagementservicebackends"></a>Microsoft.ApiManagement/service/backends
[Microsoft.ApiManagement/service/backends](/azure/templates/microsoft.apimanagement/service/backends) popisuje back-end službu předanou do provozu. 

Cluster Service Fabric Service Fabric back-EndY, je back-end místo specifické služby Service Fabric. To umožňuje jedna zásada směrování do více než jedna služba v clusteru. **Url** pole v tomto poli je název plně kvalifikovaný služby služby v clusteru, všechny požadavky jsou směrovány do ve výchozím nastavení, pokud není zadán žádný název služby v zásadách back-end. Můžete použít název falešných služby, jako například "fabric: / falešných/služby" Pokud nemáte v úmyslu tak, aby měl záložní služby. **resourceId** Určuje koncový bod správy clusteru.  **clientCertificateThumbprint** a **serverCertificateThumbprints** identifikovat certifikáty slouží k ověření u clusteru.

### <a name="microsoftapimanagementserviceproducts"></a>Microsoft.ApiManagement/service/products
[Microsoft.ApiManagement/service/products](/azure/templates/microsoft.apimanagement/service/products) vytvoří produkt. Ve službě Azure API Management produkt obsahuje jeden nebo více rozhraní API a také kvóty využití a podmínky použití. Po publikování produktu vývojáři můžou přihlásit k produktu a začít používat rozhraní API produktu. 

Zadejte popisný **displayName** a **popis** pro produkt. V tomto kurzu vyžaduje se předplatné ale schválení předplatného pomocí správce není.  Tento produkt **stavu** je "publikovat" a je viditelná pro odběratele. 

### <a name="microsoftapimanagementserviceapis"></a>Microsoft.ApiManagement/service/apis
[Microsoft.ApiManagement/service/apis](/azure/templates/microsoft.apimanagement/service/apis) vytvoří rozhraní API. Rozhraní API ve službě API Management představuje sadu operací, které můžete vyvolat klientské aplikace. Jakmile se operace, které jsou přidány, rozhraní API se přidá do produktu a může být publikována. Po publikování rozhraní API se můžete přihlásit k odběru a používají vývojáři.

- **displayName** může být jakýkoli název pro rozhraní API. V tomto kurzu použijte "Service Fabric aplikace".
- **název** poskytuje jedinečný a popisný název pro rozhraní API, jako je například "aplikace service fabric". Zobrazí se na vývojáře a vydavatele portálech. 
- **serviceUrl** odkazuje na službu HTTP implementace rozhraní API. Rozhraní API správy předá požadavky na tuto adresu. Pro Service Fabric back-EndY není tato hodnota adresy URL používá. Zde můžete zadejte libovolnou hodnotu. Pro účely tohoto kurzu, například "http://servicefabric". 
- **cesta** se připojí k základní adresu URL pro službu správy rozhraní API. Základní adresa URL je společný pro všechny rozhraní API hostovaná v instanci služby API Management. API Management odlišuje rozhraní API podle jejich přípona a proto přípona musí být jedinečný pro každé rozhraní API pro daného vydavatele. 
- **protokoly** určit protokoly, které lze použít pro přístup k rozhraní API. V tomto kurzu seznamu **http** a **https**.
- **cesta** je příponu pro rozhraní API. V tomto kurzu použijte "Moje aplikace".

### <a name="microsoftapimanagementserviceapisoperations"></a>Microsoft.ApiManagement/service/apis/operations
[Microsoft.ApiManagement/service/apis/operations](/azure/templates/microsoft.apimanagement/service/apis/operations) před k rozhraní API ve službě API Management může být použita, operace musí být přidaný do rozhraní API.  Externí klienti používají ke komunikaci s ASP.NET Core bezstavové služby spuštěné v clusteru Service Fabric operace.

Pokud chcete přidat front-end operace rozhraní API, vyplňte hodnoty:

- **displayName** a **popis** popisují operaci. V tomto kurzu použijte "Hodnoty".
- **Metoda** Určuje příkaz HTTP.  V tomto kurzu zadejte **získat**.
- **urlTemplate** se připojí k základní adresu URL rozhraní API a identifikuje jednu operaci HTTP.  V tomto kurzu použít `/api/values` Pokud jste přidali rozhraní .NET back-end službu nebo `getMessage` Pokud jste přidali Java back-end službu.  Ve výchozím nastavení zadat cestu adresy URL zde bude zaslána cestu adresy URL back-end služby Service Fabric. Pokud použijete stejné cesty URL sem používající služby, jako je například "/ api/hodnoty" operaci funguje bez další úpravy. Můžete také určit cestu adresy URL tady, se liší od cesty URL používá váš back-end službu Service Fabric, v takovém případě také musíte zadat cestu přepisování v zásadě operaci později.

### <a name="microsoftapimanagementserviceapispolicies"></a>Microsoft.ApiManagement/service/apis/policies
[Microsoft.ApiManagement/service/apis/policies](/azure/templates/microsoft.apimanagement/service/apis/policies) vytvoří zásadu back-end, která sváže všechno, co společně. Toto je, kde můžete konfigurovat službě back-end Service Fabric, do které směrují požadavky. Tyto zásady můžete použít pro všechny operace rozhraní API.  Další informace najdete v tématu [přehled zásad](/azure/api-management/api-management-howto-policies). 

[Konfiguraci back-end pro Service Fabric](/azure/api-management/api-management-transformation-policies#SetBackendService) poskytuje následující žádosti o směrování ovládací prvky: 
 - Služba instance výběru zadáním Service Fabric název instance služby, buď pevně zakódované (například `"fabric:/myapp/myservice"`) nebo vygenerovat z požadavku HTTP (například `"fabric:/myapp/users/" + context.Request.MatchedParameters["name"]`).
 - Oddíl rozlišení vygenerováním klíč oddílu pomocí žádné schéma rozdělení oddílů Service Fabric.
 - Výběr repliky pro stavové služby.
 - Řešení opakování podmínky, které můžete určit podmínky pro přeložit umístění služby a odešlete žádost.

**policyContent** je uvozena ve formátu Json obsah XML zásad.  V tomto kurzu vytvořte zásadu back-end pro směrování požadavků přímo ke službě .NET nebo Java bezstavové předtím. Přidat `set-backend-service` zásadu v příchozími zásadami.  Nahraďte "service-name" `fabric:/ApiApplication/WebApiService` Pokud jste předtím nasadili službě back-end .NET nebo `fabric:/EchoServerApplication/EchoServerService` Pokud jste nasadili službu Java.
    
```xml
<policies>
  <inbound>
    <base/>
    <set-backend-service 
        backend-id="servicefabric"
        sf-service-instance-name="service-name"
        sf-resolve-condition="@((int)context.Response.StatusCode != 200)" />
  </inbound>
  <backend>
    <base/>
  </backend>
  <outbound>
    <base/>
  </outbound>
</policies>
```

Úplnou sadu atributů Service Fabric back-end zásady, najdete v části [dokumentace ke službě back endové rozhraní API Management](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService)

## <a name="set-parameters-and-deploy-api-management"></a>Nastavit parametry a nasadit API Management
Vyplňte následující prázdné parametry v *apim.parameters.json* pro vaše nasazení. 

|Parametr|Hodnota|
|---|---|
|apimInstanceName|SF apim|
|apimPublisherEmail|myemail@contosos.com|
|apimSku|Developer|
|serviceFabricCertificateName|sfclustertutorialgroup320171031144217|
|CertificatePassword|q6D7nN %6ck@6| 
|serviceFabricCertificateThumbprint|C4C1E541AD512B8065280292A8BA6079C3F26F10 |
|serviceFabricCertificate|&lt;řetězec s kódováním Base-64&gt;|
|cesta_URL|/ api/hodnoty|
|clusterHttpManagementEndpoint|https://mysfcluster.southcentralus.cloudapp.Azure.com:19080|
|inbound_policy|&lt;Řetězec XML&gt;|

*certificatePassword* a *serviceFabricCertificateThumbprint* clusteru certifikát používaný k nastavení clusteru se musí shodovat.  

*serviceFabricCertificate* je certifikát jako řetězec kódovaný kódování base-64, který se dá vygenerovat pomocí následující skript:

```powershell
$bytes = [System.IO.File]::ReadAllBytes("C:\mycertificates\sfclustertutorialgroup220171109113527.pfx");
$b64 = [System.Convert]::ToBase64String($bytes);
[System.Io.File]::WriteAllText("C:\mycertificates\sfclustertutorialgroup220171109113527.txt", $b64);
```

V *inbound_policy*, nahraďte "service-name" `fabric:/ApiApplication/WebApiService` Pokud jste předtím nasadili službě back-end .NET nebo `fabric:/EchoServerApplication/EchoServerService` Pokud jste nasadili službu Java.

```xml
<policies>
  <inbound>
    <base/>
    <set-backend-service 
        backend-id="servicefabric"
        sf-service-instance-name="service-name"
        sf-resolve-condition="@((int)context.Response.StatusCode != 200)" />
  </inbound>
  <backend>
    <base/>
  </backend>
  <outbound>
    <base/>
  </outbound>
</policies>
```

Použijte následující skript nasazení Resource Manager soubory šablony a parametrů pro API Management:

```powershell
$ResourceGroupName = "sfclustertutorialgroup"
New-AzureRmResourceGroupDeployment -ResourceGroupName $ResourceGroupName -TemplateFile .\apim.json -TemplateParameterFile .\apim.parameters.json -Verbose
```

```azurecli
ResourceGroupName="sfclustertutorialgroup"
az group deployment create --name ApiMgmtDeployment --resource-group $ResourceGroupName --template-file apim.json --parameters @apim.parameters.json 
```

## <a name="test-it"></a>otestovat

Nyní můžete zkusit odesílání požadavku do back-end služby v Service Fabric pomocí rozhraní API správy přímo z [portál Azure](https://portal.azure.com).

 1. Ve službě API Management vyberte **rozhraní API**.
 2. V **aplikace Service Fabric** rozhraní API, které jste vytvořili v předchozím kroku, vyberte **Test** kartu a potom **hodnoty** operaci.
 3. Klikněte **odeslat** tlačítko poslat žádost o test ke službě back-end.  Měli byste vidět odpovědi HTTP podobně jako:

    ```http
    HTTP/1.1 200 OK

    Transfer-Encoding: chunked

    Content-Type: application/json; charset=utf-8

    Vary: Origin

    Access-Control-Allow-Origin: https://apimanagement.hosting.portal.azure.net

    Access-Control-Allow-Credentials: true

    Access-Control-Expose-Headers: Transfer-Encoding,Date,Server,Vary,Ocp-Apim-Trace-Location

    Ocp-Apim-Trace-Location: https://apimgmtstuvyx3wa3oqhdbwy.blob.core.windows.net/apiinspectorcontainer/RaVVuJBQ9yxtdyH55BAsjQ2-1?sv=2015-07-08&sr=b&sig=Ab6dPyLpTGAU6TdmlEVu32DMfdCXTiKAASUlwSq3jcY%3D&se=2017-09-15T05%3A49%3A53Z&sp=r&traceId=ed9f1f4332e34883a774c34aa899b832

    Date: Thu, 14 Sep 2017 05:49:56 GMT


    [
    "value1",
    "value2"
    ]
    ```

## <a name="clean-up-resources"></a>Vyčištění prostředků

Cluster se kromě vlastního prostředku clusteru skládá z dalších prostředků Azure. Nejjednodušší způsob, jak odstranit cluster a všechny prostředky, které využívá, je odstranit příslušnou skupinu prostředků.

Přihlaste se k Azure a vybrat ID předplatného, pro který chcete odebrat cluster.  Můžete najít svoje ID předplatného přihlášením k [portál Azure](http://portal.azure.com). Odstranit skupinu prostředků a všechny prostředky clusteru pomocí [rutinu Remove-AzureRMResourceGroup](/en-us/powershell/module/azurerm.resources/remove-azurermresourcegroup).

```powershell
$ResourceGroupName = "sfclustertutorialgroup"
Remove-AzureRmResourceGroup -Name $ResourceGroupName -Force
```

```azurecli
ResourceGroupName="sfclustertutorialgroup"
az group delete --name $ResourceGroupName
```

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Nasadit službu API Management
> * Konfigurace správy rozhraní API
> * Vytvoření operace rozhraní API
> * Nakonfigurujte zásady back-end
> * Přidání rozhraní API do produktu

V dalším kroku přechodu na následující kurzu se dozvíte, jak upgradovat runtime clusteru.
> [!div class="nextstepaction"]
> [Upgrade clusteru runtime Azure Service Fabric](service-fabric-tutorial-upgrade-cluster.md)

[azure-powershell]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/

[apim-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/apim.json
[apim-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/apim.parameters.json

[network-arm]: https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.json
[network-parameters-arm]: https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.parameters.json

[cluster-arm]: https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.json
[cluster-parameters-arm]: https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.parameters.json

<!-- pics -->
[sf-apim-topology-overview]: ./media/service-fabric-tutorial-deploy-api-management/sf-apim-topology-overview.png
