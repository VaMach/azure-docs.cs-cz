---
title: "Azure Service Fabric s úvodní API Management | Microsoft Docs"
description: "Tento průvodce vám ukáže, jak rychle začít s Azure API Management a Service Fabric."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 96176149-69bb-4b06-a72e-ebbfea84454b
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/01/2017
ms.author: vturecek
ms.openlocfilehash: 2969834713fc7c2f1a2e281a6c988158d803dc45
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="service-fabric-with-azure-api-management-quick-start"></a>Service Fabric s Azure API Management rychlý Start

Tento průvodce vám ukáže postup nastavení Azure API Management s Service Fabric a konfigurace vašeho prvního rozhraní API operaci odesílat provoz do back endové služby v Service Fabric. Další informace o scénářích Azure API Management s Service Fabric najdete v tématu [přehled](service-fabric-api-management-overview.md) článku. 

## <a name="deploy-api-management-and-service-fabric-to-azure"></a>Nasazení do Azure API Management a Service Fabric

Prvním krokem je k nasazení API Management a cluster Service Fabric na Azure ve sdílené virtuální síti. To umožňuje správu rozhraní API a komunikovat přímo s Service Fabric, aby mohl vykonávat zjišťování služby, řešení oddílu služby a předat dál provoz přímo do jakékoli back-end službu v Service Fabric.

### <a name="topology"></a>topologie

Tento průvodce nasadí do Azure, ve které jsou v podsítě ve stejné virtuální síti API Management a Service Fabric následující topologie:

 ![Popisek obrázku][sf-apim-topology-overview]

Abyste mohli rychle začít, šablony správce prostředků slouží pro každého kroku nasazení:

 - Topologie sítě:
    - [Network.JSON][network-arm]
    - [Network.Parameters.JSON][network-parameters-arm]
 - Cluster Service Fabric:
    - [cluster.JSON][cluster-arm]
    - [cluster.Parameters.JSON][cluster-parameters-arm]
 - API Management:
    - [APIM.JSON][apim-arm]
    - [APIM.Parameters.JSON][apim-parameters-arm]

### <a name="sign-in-to-azure-and-select-your-subscription"></a>Přihlaste se k Azure a vybrat své předplatné

Tato příručka používá [prostředí Azure PowerShell][azure-powershell]. Když spustíte novou relaci prostředí PowerShell, přihlaste se k účtu Azure a vybrat své předplatné před spuštěním příkazů Azure.
 
Přihlaste se k účtu Azure:

```powershell
Login-AzureRmAccount
```

Vyberte předplatné:

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte novou skupinu prostředků pro vaše nasazení. Poskytněte název a umístění.

```powershell
New-AzureRmResourceGroup -Name <my-resource-group> -Location westus
```

### <a name="deploy-the-network-topology"></a>Nasazení síťové topologie

Prvním krokem je nastavit topologie sítě, do které bude nasazen API Management a cluster Service Fabric. [Network.json] [ network-arm] šablony Resource Manageru nastaven tak, aby vytvořit virtuální síť (VNET) se dvěma podsítěmi a skupiny zabezpečení dvě sítě (NSG). 

[Network.parameters.json] [ network-parameters-arm] soubor parametrů obsahuje názvy podsítí a skupin Nsg, které API Management a Service Fabric se nasadí do. Tato příručka hodnoty parametru není nutné změnit. Použití šablony API Management a služby Správce prostředků infrastruktury tyto hodnoty, takže pokud jsou upraveny zde, je třeba je změnit v jiných šablonách Resource Manageru odpovídajícím způsobem. 

 1. Stáhněte si následující soubor šablony a parametry Resource Manager:

    - [Network.JSON][network-arm]
    - [Network.Parameters.JSON][network-parameters-arm]

 2. Použijte následující příkaz prostředí PowerShell pro nasazení Resource Manager soubory šablony a parametr pro nastavení sítě:

    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\network.json -TemplateParameterFile .\network.parameters.json -Verbose
    ```

### <a name="deploy-the-service-fabric-cluster"></a>Nasazení clusteru Service Fabric

Po dokončení síťové prostředky nasazení, dalším krokem je k nasazení clusteru Service Fabric v podsíti virtuální sítě a NSG určené pro cluster Service Fabric. V tomto kurzu je předem nakonfigurovaný k použití názvy virtuální sítě, podsítě a NSG, které jste nastavili v předchozím kroku šablony služby Správce prostředků infrastruktury. 

Šablony Resource Manageru clusteru Service Fabric je nakonfigurován k vytvoření clusteru s podporou zabezpečení s certifikát zabezpečení. Certifikát se používá k zabezpečení komunikace mezi uzly clusteru a ke správě přístupu uživatelů ke svému clusteru Service Fabric. API Management používá tento certifikát pro přístup ke službě názvy prostředků infrastruktury služby pro zjišťování služby.

Tento krok vyžaduje použití certifikátu v Key Vault pro zabezpečení clusteru. Další informace o nastavení zabezpečení clusteru s Key Vault najdete v tématu [Tato příručka týkající se vytvoření clusteru v Azure pomocí Resource Manager](service-fabric-cluster-creation-via-arm.md)

> [!NOTE]
> Můžete přidat ověřování Azure Active Directory kromě certifikát používaný pro přístup ke clusteru. Azure Active Directory je doporučeným způsobem, jak spravovat přístup uživatelů k cluster Service Fabric, ale není nutné k dokončení tohoto kurzu. Certifikát je nutný v obou případech pro zabezpečení mezi uzly clusteru a pro ověřování Azure API Management, které aktuálně nepodporuje ověřování pomocí služby Azure Active Directory pro Service Fabric back-end.

 1. Stáhněte si následující soubor šablony a parametry Resource Manager:
 
    - [cluster.JSON][cluster-arm]
    - [cluster.Parameters.JSON][cluster-parameters-arm]

 2. Vyplňte prázdné parametry v `cluster.parameters.json` soubor pro vaše nasazení, včetně [Key Vault informace](service-fabric-cluster-creation-via-arm.md#set-up-a-key-vault) pro váš cluster certifikát.

 3. Použijte následující příkaz prostředí PowerShell pro nasazení Resource Manager šablony a parametr soubory a vytvořte cluster Service Fabric:

    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\cluster.json -TemplateParameterFile .\cluster.parameters.json -Verbose
    ```

### <a name="deploy-api-management"></a>Nasadit službu API Management

Nakonec do virtuální sítě v podsíti nasazení API Management a NSG určená pro API Management. Nemusíte čekat na nasazení clusteru Service Fabric na Dokončit před nasazením API Management. 

V tomto kurzu šablony Resource Manageru rozhraní API Management je předem nakonfigurovaný použít názvy virtuální sítě, podsítě a NSG, které jste nastavili v předchozím kroku. 

 1. Stáhněte si následující soubor šablony a parametry Resource Manager:
 
    - [APIM.JSON][apim-arm]
    - [APIM.Parameters.JSON][apim-parameters-arm]

 2. Vyplňte prázdné parametry v `apim.parameters.json` pro vaše nasazení.

 3. Použijte následující příkaz prostředí PowerShell k nasazení Resource Manager soubory šablony a parametrů pro API Management:

    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\apim.json -TemplateParameterFile .\apim.parameters.json -Verbose
    ```

## <a name="configure-api-management"></a>Konfigurace správy rozhraní API

Jakmile cluster API Management a Service Fabric se nasazuje, můžete nakonfigurovat back-end Service Fabric ve službě API Management. Umožňuje vytvořit zásadu služby back-end, která odešle přenosů do clusteru Service Fabric.

### <a name="api-management-security"></a>Zabezpečení rozhraní API Management

Konfigurace back-end Service Fabric, musíte nejprve konfigurovat nastavení zabezpečení rozhraní API správy. Chcete-li nakonfigurovat nastavení zabezpečení, přejděte do služby API Management na portálu Azure.

#### <a name="enable-the-api-management-rest-api"></a>Povolení správy rozhraní API REST API

Rozhraní API REST API správy je aktuálně jedinou možností, jak konfigurovat back-end službu. Prvním krokem je povolit rozhraní API REST API pro správu a zabezpečte ji.

 1. Ve službě API Management vyberte **rozhraní API pro správu - PREVIEW** pod **zabezpečení**.
 2. Zkontrolujte **povolit rozhraní API REST API pro správu** zaškrtávací políčko.
 3. Poznamenejte si adresu URL rozhraní API správy – Toto je adresa URL použijeme později nastavit back-end Service Fabric
 4. Generovat **přístupový Token** tak, že vyberete datum vypršení platnosti a klíč, klikněte **generování** tlačítko ve spodní části stránky.
 5. Kopírování **přístupový token** a uložte ho – použijeme ho v následujících krocích. Všimněte si, že se to neliší od primární klíč a sekundární klíč.

#### <a name="upload-a-service-fabric-client-certificate"></a>Nahrajte certifikát klienta Service Fabric

API Management musí ověřit k vašemu clusteru Service Fabric pro zjišťování služby pomocí klientského certifikátu, který má přístup ke clusteru. Pro zjednodušení tento kurz používá stejný certifikát zadaný při vytváření clusteru Service Fabric, která ve výchozím nastavení může být použit pro přístup clusteru.

 1. Ve službě API Management vyberte **klientské certifikáty - PREVIEW** pod **zabezpečení**.
 2. Klikněte **+ přidat** tlačítko
 2. Vyberte privátní klíč souboru (.pfx) clusteru certifikátu, který jste zadali při vytváření clusteru Service Fabric, zadejte jeho název a zadejte heslo soukromého klíče.

> [!NOTE]
> Tento kurz používá stejný certifikát pro ověřování klientů a zabezpečení mezi uzly clusteru. Pokud nemáte nakonfigurovaná pro přístup k vaší cluster Service Fabric, můžete použít samostatné klientský certifikát.

### <a name="configure-the-backend"></a>Konfigurace back-end

Teď, když je nakonfigurovaná zabezpečení rozhraní API Management, můžete nakonfigurovat back-end Service Fabric. Cluster Service Fabric Service Fabric back-EndY, je back-end, nikoli konkrétní službu Service Fabric. To umožňuje jedna zásada směrování do více než jedna služba v clusteru.

Tento krok vyžaduje přístupový token, který jste vygenerovali dříve a kryptografický otisk pro svůj cluster certifikát, který jste nahráli do rozhraní API správy v předchozím kroku.

> [!NOTE]
> Pokud jste použili samostatný klientský certifikát v předchozím kroku pro rozhraní API Management, musíte kryptografický otisk certifikátu klienta se kromě clusteru kryptografický otisk certifikátu v tomto kroku.

Odeslání žádosti o následující HTTP PUT na adresu URL rozhraní API správy rozhraní API, že jste si předtím poznamenali při povolování REST API pro správu rozhraní API konfigurace back-end službu Service Fabric. Měli byste vidět `HTTP 201 Created` odpovědi při úspěšném provedení příkazu. Další informace o každé pole v rozhraní API pro správu [referenční dokumentace rozhraní API back-end](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend).

Příkaz HTTP a adresy URL:
```http
PUT https://your-apim.management.azure-api.net/backends/servicefabric?api-version=2016-10-10
```

Hlavičky požadavku:
```http
Authorization: SharedAccessSignature <your access token>
Content-Type: application/json
```

Text žádosti:
```http
{
    "description": "<description>",
    "url": "<fallback service name>",
    "protocol": "http",
    "resourceId": "<cluster HTTP management endpoint>",
    "properties": {
        "serviceFabricCluster": {
            "managementEndpoints": [ "<cluster HTTP management endpoint>" ],
            "clientCertificateThumbprint": "<client cert thumbprint>",
            "serverCertificateThumbprints": [ "<cluster cert thumbprint>" ],
            "maxPartitionResolutionRetries" : 5
        }
    }
}
```

**Url** parametr tady je služba plně kvalifikovaný název služby v clusteru, všechny požadavky jsou směrovány do ve výchozím nastavení, pokud není zadán žádný název služby v zásadách back-end. Můžete použít název falešných služby, jako například "fabric: / falešných/služby" Pokud nemáte v úmyslu tak, aby měl záložní služby. Mějte na paměti, **url** musí být ve formátu "fabric: / aplikace/služby" i v případě, že je to falešných záložní služba.

Odkazovat na rozhraní API pro správu [referenční dokumentace rozhraní API back-end](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend) další podrobnosti o každé pole.

#### <a name="example"></a>Příklad

```http
PUT https://your-apim.management.azure-api.net/backends/servicefabric?api-version=2016-10-10
Authorization: SharedAccessSignature 230948023984&Ld93cRGcNU6KZ4uVz7JlfTec4eX43Q9Nu8ndatOgBzs6+f559Pkf3iHX2cSge+r42pn35qGY3TitjrIl13hwcQ==
Content-Type: application/json

{
    "description": "My Service Fabric backend",
    "url": "fabric:/myapp/myservice",
    "protocol": "http",
    "resourceId": "https://your-cluster.westus.cloudapp.azure.com:19080",
    "properties": {
        "serviceFabricCluster": {
            "managementEndpoints": ["https://your-cluster.westus.cloudapp.azure.com:19080"],
            "clientCertificateThumbprint": "57bc463aba3aea3a12a18f36f44154f819f0fe32",
            "serverCertificateThumbprints": ["57bc463aba3aea3a12a18f36f44154f819f0fe32"],
            "maxPartitionResolutionRetries" : 5
        }
    }
}
```

## <a name="deploy-a-service-fabric-back-end-service"></a>Nasazení služby back-end Service Fabric

Teď, když máte Service Fabric nakonfigurovaný jako back-end pro API Management, můžete vytvořit zásady back-end pro vaše rozhraní API, který odesílá data na vaše služby Service Fabric. Ale nejdřív je potřeba služby spuštěné v Service Fabric tak, aby přijímal požadavky.

### <a name="create-a-service-fabric-service-with-an-http-endpoint"></a>Vytvoření služby Service Fabric se koncový bod HTTP

V tomto kurzu vytvoříme základní bezstavové ASP.NET Core spolehlivé služby pomocí výchozí šablona projektu webového rozhraní API. Tím se vytvoří koncový bod HTTP pro vaši službu, která budete vystavit pomocí Azure API Management:

```
/api/values
```

Začněte tím, že [nastavení vývojového prostředí pro vývoj ASP.NET Core](service-fabric-add-a-web-frontend.md#set-up-your-environment-for-aspnet-core).

Až nastavíte vývojového prostředí Visual Studio spustíte jako správce a vytvoření služby ASP.NET Core:

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

    To umožňuje Service Fabric zadejte port dynamicky z rozsahu portů aplikace, který jsme otevřít prostřednictvím skupinu zabezpečení sítě v šabloně Resource Manager clusteru, umožňuje provoz mají být předány z API Management.
 
 6. Stisknutím klávesy F5 ve Visual Studiu ověřte webové rozhraní API není k dispozici místně. 

    Otevřete Service Fabric Explorer a podrobnostem konkrétní instanci služby ASP.NET Core najdete v části Základní adresa služby naslouchá na. Přidat `/api/values` s jejím základem adresy a otevřete v prohlížeči. Tím se spustí metodu Get na ValuesController v šabloně webového rozhraní API. Vrátí výchozí odpovědi, které poskytuje šablony, pole JSON, který obsahuje dva řetězce:

    ```json
    ["value1", "value2"]`
    ```

    Toto je koncový bod, který budete vystavit přes správu rozhraní API v Azure.

 7. Nakonec nasazení aplikace na cluster v Azure. [Pomocí sady Visual Studio](service-fabric-publish-app-remote-cluster.md#to-publish-an-application-using-the-publish-service-fabric-application-dialog-box), klikněte pravým tlačítkem na projekt aplikace a vyberte **publikovat**. Zadejte svůj koncový bod clusteru (například `mycluster.westus.cloudapp.azure.com:19000`) k nasazení aplikace na cluster Service Fabric v Azure.

Bezstavové služby ASP.NET Core s názvem `fabric:/ApiApplication/WebApiService` teď by měl být spuštěn v clusteru Service Fabric v Azure.

## <a name="create-an-api-operation"></a>Vytvoření operace rozhraní API

Nyní je připraven k vytvoření operace ve službě API Management, který externí klienti používat pro komunikaci se službou ASP.NET Core bezstavové spuštěných v clusteru Service Fabric.

 1. Přihlaste se k portálu Azure a přejděte do vašeho nasazení služby API Management.
 2. V okně služby API Management vyberte **rozhraní API – náhled**
 3. Přidání nového rozhraní API kliknutím **prázdné API** pole a vyplníte dialogové okno:

     - **Adresu URL webové služby**: pro Service Fabric back-EndY, není tato hodnota adresy URL používá. Zde můžete zadejte libovolnou hodnotu. V tomto kurzu použít: `http://servicefabric`.
     - **Název**: zadat libovolný název pro rozhraní API. V tomto kurzu použít `Service Fabric App`.
     - **Schéma adresy URL**: Vyberte HTTP, HTTPS nebo obě možnosti. V tomto kurzu použít `both`.
     - **Přípona adresy URL rozhraní API**: Zadejte příponu pro naše rozhraní API. V tomto kurzu použít `myapp`.
 
 4. Po vytvoření rozhraní API klikněte na tlačítko **+ operace přidání** přidat front-end operace rozhraní API. Vyplňte hodnoty:
    
     - **Adresa URL**: vyberte `GET` a zadejte cestu adresy URL pro rozhraní API. V tomto kurzu použít `/api/values`.
     
       Ve výchozím nastavení zadat cestu adresy URL zde bude zaslána cestu adresy URL back-end služby Service Fabric. Pokud používáte stejnou adresu URL cestu, zde která používá službu, v takovém případě `/api/values`, pak operaci funguje bez další úpravy. Můžete také určit cestu adresy URL tady, se liší od cesty URL používá váš back-end služby Service Fabric, v takovém případě bude také musíte zadat cestu přepisování v zásadě operaci později.
     - **Zobrazovaný název**: zadat libovolný název pro rozhraní API. V tomto kurzu použít `Values`.

## <a name="configure-a-backend-policy"></a>Nakonfigurujte zásady back-end

Zásady back-end sváže všechno, co společně. Toto je, kde můžete konfigurovat službě back-end Service Fabric, do které směrují požadavky. Tyto zásady můžete použít pro všechny operace rozhraní API. [Konfiguraci back-end pro Service Fabric](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService) poskytuje následující žádosti o směrování ovládací prvky: 
 - Služba instance výběru zadáním Service Fabric název instance služby, buď pevně zakódované (například `"fabric:/myapp/myservice"`) nebo vygenerovat z požadavku HTTP (například `"fabric:/myapp/users/" + context.Request.MatchedParameters["name"]`).
 - Oddíl rozlišení vygenerováním klíč oddílu pomocí žádné schéma rozdělení oddílů Service Fabric.
 - Výběr repliky pro stavové služby.
 - Řešení opakování podmínky, které můžete určit podmínky pro přeložit umístění služby a odešlete žádost.

V tomto kurzu vytvořte zásadu back-end, který směruje požadavky přímo na bezstavové služby ASP.NET Core dříve nasazené:

 1. Vyberte a upravit **příchozí zásady** pro `Values` operaci klepnutím na ikonu pro úpravu a potom výběrem **zobrazení kódu**.
 2. V editoru zásad kódu přidat `set-backend-service` zásad v části příchozí zásady, jak je vidět tady a klikněte na tlačítko **Uložit** tlačítko:
    
    ```xml
    <policies>
      <inbound>
        <base/>
        <set-backend-service 
           backend-id="servicefabric"
           sf-service-instance-name="fabric:/ApiApplication/WebApiService"
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

### <a name="add-the-api-to-a-product"></a>Přidání rozhraní API do produktu. 

Než bude možné volat rozhraní API, musí být přidané pro určitý produkt, kde můžete udělit přístup uživatelům. 

 1. Ve službě API Management vyberte **produkty - PREVIEW**.
 2. Ve výchozím nastavení, produkty dva poskytovatelé pro správu rozhraní API: úvodní a bez omezení. Vyberte neomezená produktu.
 3. Vyberte rozhraní API.
 4. Klikněte **+ přidat** tlačítko.
 5. Vyberte `Service Fabric App` rozhraní API, které jste vytvořili v předchozích krocích a klikněte na **vyberte** tlačítko.

### <a name="test-it"></a>otestovat

Nyní můžete zkusit odesílání požadavku do back-end služby v Service Fabric pomocí rozhraní API správy přímo z portálu Azure.

 1. Ve službě API Management vyberte **API – PREVIEW**.
 2. V `Service Fabric App` rozhraní API, které jste vytvořili v předchozím kroku, vyberte **Test** kartě.
 3. Klikněte **odeslat** tlačítko poslat žádost o test ke službě back-end.

## <a name="next-steps"></a>Další kroky

V tomto okamžiku byste měli mít základní instalaci pomocí Service Fabric a API Management.

Tento kurz používá ověřování basic uživatelů na základě certifikátů pro váš cluster Service Fabric si rychle nastavit. Pokročilejší ověřování uživatelů pro váš cluster Service Fabric je vhodnější s [ověřování Azure Active Directory](service-fabric-cluster-creation-via-arm.md#set-up-azure-active-directory-for-client-authentication). 

Dále [vytvoření a konfigurace pokročilých nastavení produktu v Azure API Management](https://docs.microsoft.com/azure/api-management/api-management-howto-product-with-rules) Příprava aplikace pro provoz skutečném světě.

<!-- links -->
[azure-powershell]:https://azure.microsoft.com/documentation/articles/powershell-install-configure/

[network-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.json
[network-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.parameters.json

[apim-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/apim.json
[apim-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/apim.parameters.json

[cluster-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.json
[cluster-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.parameters.json


<!-- pics -->
[sf-apim-topology-overview]: ./media/service-fabric-api-management-quickstart/sf-apim-topology-overview.png
