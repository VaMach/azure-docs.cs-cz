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
ms.date: 09/13/2017
ms.author: ryanwi
ms.openlocfilehash: e28ea6df24b9df144552739427427ee14e998584
ms.sourcegitcommit: dcf5f175454a5a6a26965482965ae1f2bf6dca0a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="deploy-api-management-with-service-fabric"></a>Nasazení správy rozhraní API pomocí Service Fabric
V tomto kurzu je součástí série, tři.  Nasazení aplikace Azure API Management s Service Fabric je je pokročilý scénář užitečné, pokud je potřeba publikovat rozhraní API s bohatou sadu pravidel směrování k vašim službám Service Fabric back-end. V tomto kurzu se dozvíte, jak nastavit [Azure API Management](../api-management/api-management-key-concepts.md) s Service Fabric přesměrovat provoz na back-end služby v Service Fabric.  Jakmile budete hotovi, nasazené API Management k virtuální síti, nakonfigurované operace rozhraní API odesílat provoz do bezstavové služby back-end. Další informace o scénářích Azure API Management s Service Fabric najdete v tématu [přehled](service-fabric-api-management-overview.md) článku.

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
> * Nasazení správy rozhraní API pomocí Service Fabric

## <a name="prerequisites"></a>Požadavky
Před zahájením tohoto kurzu:
- Pokud nemáte předplatné Azure, vytvořte [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Nainstalujte [prostředí Azure Powershell verze modulu 4.1 nebo vyšší](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) nebo [Azure CLI 2.0](/cli/azure/install-azure-cli).
- Vytvoření zabezpečeného [clusteru se systémem Windows](service-fabric-tutorial-create-vnet-and-windows-cluster.md) nebo [Linux clusteru](service-fabric-tutorial-create-vnet-and-linux-cluster.md) v Azure
- Pokud nasadíte clusteru se systémem Windows, nastavení vývojového prostředí systému Windows. Nainstalujte [Visual Studio 2017](http://www.visualstudio.com) a **Azure development**, **ASP.NET a webové vývoj**, a **vývoj pro různé platformy .NET Core**úlohy.  Potom nastavit [vývojové prostředí .NET](service-fabric-get-started.md).
- Pokud nasadíte Linux cluster, nastavit vývojové prostředí Java na [Linux](service-fabric-get-started-linux.md) nebo [systému MacOS](service-fabric-get-started-mac.md).  Nainstalujte [služby Fabric rozhraní příkazového řádku](service-fabric-cli.md). 

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

## <a name="deploy-api-management"></a>Nasadit službu API Management
Cloudové aplikace obvykle nutné front-endu brány zajistit jediný bod příjem příchozích dat pro uživatele, zařízení nebo jiné aplikace. V Service Fabric může být bránu, třeba aplikací ASP.NET Core žádné bezstavové služby nebo jiné služby, které jsou určené pro příchozí provoz, například služby Event Hubs, IoT Hub nebo Azure API Management. Tento kurz je určen Úvod do používání Azure API Management jako brána pro vaše aplikace Service Fabric. API Management se integruje přímo s Service Fabric, že vám umožní publikovat rozhraní API s bohatou sadu pravidel směrování k vašim službám Service Fabric back-end. 

Teď, když máte zabezpečený [clusteru se systémem Windows](service-fabric-tutorial-create-vnet-and-windows-cluster.md) nebo [Linux clusteru](service-fabric-tutorial-create-vnet-and-linux-cluster.md) v Azure, a nasadit API Management na virtuální síť (VNET) v podsíti a NSG navržený pro API Management. V tomto kurzu je předem nakonfigurovaný k použití názvy virtuální sítě, podsítě a NSG, které jste nastavili v předchozím šablony Resource Manageru rozhraní API správy [kurzu clusteru Windows](service-fabric-tutorial-create-vnet-and-windows-cluster.md) nebo [kurz Linux clusteru](service-fabric-tutorial-create-vnet-and-linux-cluster.md). 

V tomto kurzu nasadí do Azure, ve které jsou v podsítě ve stejné virtuální síti API Management a Service Fabric následující topologie:

 ![Popisek obrázku][sf-apim-topology-overview]

Stáhněte si následující soubor šablony a parametry Resource Manager:
 
- [APIM.JSON][apim-arm]
- [APIM.Parameters.JSON][apim-parameters-arm]

Vyplňte prázdné parametry v `apim.parameters.json` pro vaše nasazení.

Použijte následující skript nasazení Resource Manager soubory šablony a parametrů pro API Management:

```powershell
$ResourceGroupName = "tutorialgroup"
New-AzureRmResourceGroupDeployment -ResourceGroupName $ResourceGroupName -TemplateFile .\apim.json -TemplateParameterFile .\apim.parameters.json -Verbose
```

```azurecli
ResourceGroupName="tutorialgroup"
az group deployment create --name ApiMgmtDeployment --resource-group $ResourceGroupName --template-file apim.json --parameters @apim.parameters.json 
```

## <a name="configure-api-management"></a>Konfigurace správy rozhraní API

Jakmile cluster API Management a Service Fabric se nasazuje, můžete nakonfigurovat nastavení zabezpečení a back-end Service Fabric ve službě API Management. Umožňuje vytvořit zásadu služby back-end, která odešle přenosů do clusteru Service Fabric.

### <a name="configure-api-management-security"></a>Konfigurace zabezpečení rozhraní API Management

Konfigurace back-end Service Fabric, musíte nejprve konfigurovat nastavení zabezpečení rozhraní API správy. Chcete-li nakonfigurovat nastavení zabezpečení, přejděte do služby API Management na portálu Azure.

#### <a name="enable-the-api-management-rest-api"></a>Povolení správy rozhraní API REST API

Rozhraní API REST API správy je aktuálně jedinou možností, jak konfigurovat back-end službu. Prvním krokem je povolit rozhraní API REST API pro správu a zabezpečte ji.

 1. Ve službě API Management vyberte **rozhraní API pro správu** pod **zabezpečení**.
 2. Zkontrolujte **povolit rozhraní API REST API pro správu** zaškrtávací políčko.
 3. Poznámka: **adresy URL rozhraní API správy**, který jsme později nastavit pomocí Service Fabric back-end.
 4. Generovat **tokenu přístupu** tak, že vyberete datum vypršení platnosti a klíč, klikněte **generování** tlačítko ve spodní části stránky.
 5. Kopírování **přístupový token** a uložte ho.  Přístupový token používáme v následujících krocích. Všimněte si, že se to neliší od primární klíč a sekundární klíč.

#### <a name="upload-a-service-fabric-client-certificate"></a>Nahrajte certifikát klienta Service Fabric

API Management musí ověřit k vašemu clusteru Service Fabric pro zjišťování služby pomocí klientského certifikátu, který má přístup ke clusteru. Pro zjednodušení tento kurz používá stejný certifikát zadaný dříve při vytváření [clusteru se systémem Windows](service-fabric-tutorial-create-vnet-and-windows-cluster.md#createvaultandcert_anchor) nebo [Linux clusteru](service-fabric-tutorial-create-vnet-and-linux-cluster.md#createvaultandcert_anchor), který ve výchozím nastavení lze použít pro přístup k vaší clusteru.

 1. Ve službě API Management vyberte **klientské certifikáty** pod **zabezpečení**.
 2. Klikněte **+ přidat** tlačítko.
 2. Vyberte privátní klíč souboru (.pfx) clusteru certifikátu, který jste zadali při vytváření clusteru Service Fabric, zadejte jeho název a zadejte heslo soukromého klíče.

> [!NOTE]
> Tento kurz používá stejný certifikát pro ověřování klientů a zabezpečení mezi uzly clusteru. Pokud nemáte nakonfigurovaná pro přístup k vaší cluster Service Fabric, můžete použít samostatné klientský certifikát.

### <a name="configure-the-backend"></a>Konfigurace back-end

Teď, když je nakonfigurovaná zabezpečení rozhraní API Management, můžete nakonfigurovat back-end Service Fabric. Cluster Service Fabric Service Fabric back-EndY, je back-end, nikoli konkrétní službu Service Fabric. To umožňuje jedna zásada směrování do více než jedna služba v clusteru.

Tento krok vyžaduje přístupový token, který jste vygenerovali dříve a kryptografický otisk pro váš cluster certifikát, který jste dříve nahráli API Management.

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

**Url** parametr v tomto poli je název plně kvalifikovaný služby služby v clusteru, všechny požadavky jsou směrovány do ve výchozím nastavení, pokud není zadán žádný název služby v zásadách back-end. Můžete použít název falešných služby, jako například "fabric: / falešných/služby" Pokud nemáte v úmyslu tak, aby měl záložní služby.

Odkazovat na rozhraní API pro správu [referenční dokumentace rozhraní API back-end](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend) další podrobnosti o každé pole.

```python
#import requests library for making REST calls
import requests
import json

#specify url
url = 'https://your-apim.management.azure-api.net/backends/servicefabric?api-version=2016-10-10'

token = "SharedAccessSignature integration&201710140514&Lqnbei7n2Sot6doiNtxMFPUi/m9LsNa+1ZK/PdxqFl49JFWjXh1wW5Gd99r/tIOeHp6dU8UV5iZUdOPfcrm5hg=="

payload = {
    "description": "My Service Fabric backend",
    "url": "fabric:/ApiApplication/ApiWebService",
    "protocol": "http",
    "resourceId": "https://tutorialcluster.eastus.cloudapp.azure.com:19080",
    "properties": {
        "serviceFabricCluster": {
            "managementEndpoints": [ "https://tutorialcluster.eastus.cloudapp.azure.com:19080" ],
            "clientCertificateThumbprint": "97EDD7E4979FB072AF3E480A5E5EE34B1B89DD80",
            "serverCertificateThumbprints": [ "97EDD7E4979FB072AF3E480A5E5EE34B1B89DD80" ],
            "maxPartitionResolutionRetries" : 5
        }
    }
}

headers = {'Authorization': token, "Content-Type": "application/json"}

#Call REST API
response = requests.put(url, data=json.dumps(payload), headers=headers)

#Print Response
print(response.status_code)
print(response.text)
```

## <a name="deploy-a-service-fabric-back-end-service"></a>Nasazení služby back-end Service Fabric

Teď, když máte Service Fabric nakonfigurovaný jako back-end pro API Management, můžete vytvořit zásady back-end pro vaše rozhraní API, který odesílá data na vaše služby Service Fabric. Ale nejdřív je potřeba služby spuštěné v Service Fabric tak, aby přijímal požadavky.  Pokud jste dříve vytvořili [clusteru se systémem Windows](service-fabric-tutorial-create-vnet-and-windows-cluster.md), nasazení služby .NET Service Fabric.  Pokud jste dříve vytvořili [Linux clusteru](service-fabric-tutorial-create-vnet-and-linux-cluster.md), nasazení služby Java Service Fabric.

### <a name="deploy-a-net-service-fabric-service"></a>Nasazení služby .NET Service Fabric

V tomto kurzu vytvoříme základní bezstavové ASP.NET Core spolehlivé služby pomocí výchozí šablona projektu webového rozhraní API. Tím se vytvoří koncový bod HTTP pro vaši službu, která vystavit pomocí Azure API Management:

```
/api/values
```

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

    To umožňuje Service Fabric zadejte port dynamicky z rozsahu portů aplikace, který jsme otevřít prostřednictvím skupinu zabezpečení sítě v šabloně Resource Manager clusteru, umožňuje provoz mají být předány z API Management.
 
 6. Stisknutím klávesy F5 ve Visual Studiu ověřte webové rozhraní API není k dispozici místně. 

    Otevřete Service Fabric Explorer a podrobnostem konkrétní instanci služby ASP.NET Core najdete v části Základní adresa služby naslouchá na. Přidat `/api/values` s jejím základem adresy a otevřete v prohlížeči. Tím se spustí metodu Get na ValuesController v šabloně webového rozhraní API. Vrátí výchozí odpovědi, které poskytuje šablony, pole JSON, který obsahuje dva řetězce:

    ```json
    ["value1", "value2"]`
    ```

    Toto je koncový bod, který vystavit přes správu rozhraní API v Azure.

 7. Nakonec nasazení aplikace na cluster v Azure. [Pomocí sady Visual Studio](service-fabric-publish-app-remote-cluster.md#to-publish-an-application-using-the-publish-service-fabric-application-dialog-box), klikněte pravým tlačítkem na projekt aplikace a vyberte **publikovat**. Zadejte svůj koncový bod clusteru (například `mycluster.southcentralus.cloudapp.azure.com:19000`) k nasazení aplikace na cluster Service Fabric v Azure.

Bezstavové služby ASP.NET Core s názvem `fabric:/ApiApplication/WebApiService` teď by měl být spuštěn v clusteru Service Fabric v Azure.

### <a name="create-a-java-service-fabric-service"></a>Vytvoření služby Java Service Fabric
V tomto kurzu jsme nasadit základní webového serveru, který vrátí zprávy zpět na uživatele. Ukázkovou aplikaci echo server obsahuje koncový bod HTTP pro vaši službu, která vystavit pomocí Azure API Management.

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


## <a name="create-an-api-operation"></a>Vytvoření operace rozhraní API

Nyní je připraven k vytvoření operace ve službě API Management, který externí klienti používat pro komunikaci se službou ASP.NET Core bezstavové spuštěných v clusteru Service Fabric.

1. Přihlaste se k portálu Azure a přejděte do vašeho nasazení služby API Management.
2. V okně služby API Management vyberte **rozhraní API**
3. Přidání nového rozhraní API kliknutím **+ API**, pak **prázdné API** pole a naplnění se dialogové okno:

    - **Adresu URL webové služby**: pro Service Fabric back-EndY, není tato hodnota adresy URL používá. Zde můžete zadejte libovolnou hodnotu. V tomto kurzu použít: "http://servicefabric".
    - **Zobrazovaný název**: zadat libovolný název pro rozhraní API. V tomto kurzu použijte "Service Fabric aplikace".
    - **Název**: zadejte "aplikace service fabric".
    - **Schéma adresy URL**: vyberte buď **HTTP**, **HTTPS**, nebo **i**. V tomto kurzu použít **i**.
    - **Přípona adresy URL rozhraní API**: Zadejte příponu pro naše rozhraní API. V tomto kurzu použijte "Moje aplikace".
 
4. Vyberte **aplikace Service Fabric** ze seznamu rozhraní API a klikněte na tlačítko **+ operace přidání** přidat front-end operace rozhraní API. Vyplňte hodnoty:
    
    - **Adresa URL**: vyberte **získat** a zadejte cestu adresy URL pro rozhraní API. V tomto kurzu použijte "/ api/hodnoty".  Ve výchozím nastavení zadat cestu adresy URL zde bude zaslána cestu adresy URL back-end služby Service Fabric. Pokud používáte stejnou cestu adresy URL sem používající služby, v tomto případě "/ api/hodnoty" a pak operaci funguje bez další úpravy. Můžete také určit cestu adresy URL tady, se liší od cesty URL používá váš back-end službu Service Fabric, v takovém případě také musíte zadat cestu přepisování v zásadě operaci později.
    - **Zobrazovaný název**: zadat libovolný název pro rozhraní API. V tomto kurzu použijte "Hodnoty".

5. Klikněte na **Uložit**.

## <a name="configure-a-backend-policy"></a>Nakonfigurujte zásady back-end

Zásady back-end sváže všechno, co společně. Toto je, kde můžete konfigurovat službě back-end Service Fabric, do které směrují požadavky. Tyto zásady můžete použít pro všechny operace rozhraní API. [Konfiguraci back-end pro Service Fabric](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService) poskytuje následující žádosti o směrování ovládací prvky: 
 - Služba instance výběru zadáním Service Fabric název instance služby, buď pevně zakódované (například `"fabric:/myapp/myservice"`) nebo vygenerovat z požadavku HTTP (například `"fabric:/myapp/users/" + context.Request.MatchedParameters["name"]`).
 - Oddíl rozlišení vygenerováním klíč oddílu pomocí žádné schéma rozdělení oddílů Service Fabric.
 - Výběr repliky pro stavové služby.
 - Řešení opakování podmínky, které můžete určit podmínky pro přeložit umístění služby a odešlete žádost.

V tomto kurzu vytvořte zásadu back-end, který směruje požadavky přímo na bezstavové služby ASP.NET Core dříve nasazené:

 1. Vyberte a upravit **příchozí zásady** pro operaci hodnoty klepnutím na ikonu pro úpravu a potom výběrem **zobrazení kódu**.
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

### <a name="add-the-api-to-a-product"></a>Přidání rozhraní API do produktu 

Než bude možné volat rozhraní API, musí být přidané pro určitý produkt, kde můžete udělit přístup uživatelům. 

 1. Ve službě API Management vyberte **produkty**.
 2. Ve výchozím nastavení, produkty dva poskytovatelé pro správu rozhraní API: úvodní a bez omezení. Vyberte neomezená produktu.
 3. Vyberte **+ přidat rozhraní API**.
 4. Vyberte `Service Fabric App` rozhraní API, které jste vytvořili v předchozích krocích a klikněte na **vyberte** tlačítko.

### <a name="test-it"></a>otestovat

Nyní můžete zkusit odesílání požadavku do back-end služby v Service Fabric pomocí rozhraní API správy přímo z portálu Azure.

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
$ResourceGroupName = "tutorialgroup"
Remove-AzureRmResourceGroup -Name $ResourceGroupName -Force
```

```azurecli
ResourceGroupName="tutorialgroup"
az group delete --name $ResourceGroupName
```

## <a name="conclusion"></a>Závěr
V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Nasadit službu API Management
> * Konfigurace správy rozhraní API
> * Vytvoření operace rozhraní API
> * Nakonfigurujte zásady back-end
> * Přidání rozhraní API do produktu

[azure-powershell]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/

[apim-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/apim.json
[apim-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/apim.parameters.json

[network-arm]: https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.json
[network-parameters-arm]: https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.parameters.json

[cluster-arm]: https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.json
[cluster-parameters-arm]: https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.parameters.json

<!-- pics -->
[sf-apim-topology-overview]: ./media/service-fabric-tutorial-deploy-api-management/sf-apim-topology-overview.png
