---
title: "Služba Azure Service Fabric DNS | Microsoft Docs"
description: "Použijte službu dns Service Fabric pro zjišťování mikroslužeb z v clusteru."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: vturecek
ms.assetid: 47f5c1c1-8fc8-4b80-a081-bc308f3655d3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/27/2017
ms.author: msfussell
ms.openlocfilehash: 9871bc5aa4e74ab0faef401d67c4e9558eb5e14b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="dns-service-in-azure-service-fabric"></a>Služba DNS v Azure Service Fabric
Služba DNS je volitelné systému služba, kterou můžete povolit v clusteru pro zjišťování dalších služeb pomocí protokolu DNS.

Mnoho služeb, zejména kontejnerizované služeb, může mít název existující adresu URL a schopnost vyřešit pomocí standardní protokol DNS (a nikoli protokol služby DNS) je žádoucí, zejména ve scénářích "navýšení a posunutí". Služba DNS umožňuje mapování názvů DNS pro název služby a proto přeložit koncový bod IP adresy. 

Služba DNS mapuje názvy DNS pro názvy služby, které se pak vyřeší služby DNS k vrácení koncového bodu služby. V době vytvoření je zadaný název DNS pro službu. 

![Koncové body služby][0]

## <a name="enabling-the-dns-service"></a>Povolení služby DNS
Je třeba nejprve povolit službu DNS v clusteru. Získáte šablonu pro cluster, který chcete nasadit. Můžete použít [ukázkových šablon](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) nebo vytvořit šablonu Resource Manager. Můžete povolit službu DNS pomocí následujících kroků:

1. Zkontrolujte, zda `apiversion` je nastaven na `2017-07-01-preview` pro `Microsoft.ServiceFabric/clusters` prostředků a pokud ne, aktualizovat, je znázorněné v následujícím fragmentu kódu:

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. Teď povolit službu DNS přidáním následující `addonFeatures` části po `fabricSettings` části, jak je znázorněno v následujícím fragmentu kódu: 

    ```json
        "fabricSettings": [
        ...      
        ],
        "addonFeatures": [
            "DnsService"
        ],
    ```

3. Jakmile clusteru šablony aktualizovaly s předchozí změny, je použít a umožní upgradu dokončení. Po dokončení spuštění služby DNS systému běžící v clusteru, který se nazývá `fabric:/System/DnsService` části systému služby v Service Fabric Exploreru. 

Alternativně můžete povolit službu DNS prostřednictvím portálu v době vytváření clusteru. Služba DNS lze povolit zaškrtnutím zaškrtávacího políčka pro `Include DNS service` v `Cluster configuration` nabídky, jak je znázorněno na následujícím snímku obrazovky:

![Povolení služby DNS prostřednictvím portálu][2]


## <a name="setting-the-dns-name-for-your-service"></a>Nastavení názvu DNS pro vaši službu
Jakmile je spuštěna služba DNS v clusteru můžete nastavit název DNS pro vaše služby buď deklarativně pro výchozí služby v `ApplicationManifest.xml` nebo pomocí příkazů prostředí Powershell.

### <a name="setting-the-dns-name-for-a-default-service-in-the-applicationmanifestxml"></a>Nastavení názvu DNS pro výchozí služba v ApplicationManifest.xml
Otevřete projekt v sadě Visual Studio nebo svém oblíbeném editoru a otevřete `ApplicationManifest.xml` souboru. Přejděte do části výchozí služby a pro každou službu přidat `ServiceDnsName` atribut. Následující příklad ukazuje, jak nastavit název DNS služby`service1.application1`

```xml
    <Service Name="Stateless1" ServiceDnsName="service1.application1">
    <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
        <SingletonPartition />
    </StatelessService>
    </Service>
```
Jakmile je aplikace nasazená, instance služby v Service Fabric explorer zobrazuje název DNS pro tuto instanci, jak je znázorněno na následujícím obrázku: 

![Koncové body služby][1]

### <a name="setting-the-dns-name-for-a-service-using-powershell"></a>Nastavení názvu DNS pro službu pomocí prostředí Powershell
Název DNS pro službu, můžete nastavit při vytváření pomocí `New-ServiceFabricService` prostředí Powershell. Následující příklad vytvoří nový bezstavové služby s názvem DNS`service1.application1`

```powershell
    New-ServiceFabricService `
    -Stateless `
    -PartitionSchemeSingleton `
    -ApplicationName `fabric:/application1 `
    -ServiceName fabric:/application1/service1 `
    -ServiceTypeName Service1Type `
    -InstanceCount 1 `
    -ServiceDnsName service1.application1
```

## <a name="using-dns-in-your-services"></a>Pomocí DNS v službách
Pokud nasazujete více než jedna služba, můžete najít koncové body jiných služeb, ke komunikaci s pomocí názvu DNS. Služba DNS používá se pouze pro bezstavové služby, protože protokol DNS nemůže komunikovat s stavové služby. Pro stavové služby můžete použít předdefinované reverzní proxy server pro volání protokolu http k volání oddíl konkrétní službu.

Následující kód ukazuje, jak volat jiné služby, která je jednoduše volání regulární http, kde zadáte port a všechny cesty jako část adresy URL.

```csharp
public class ValuesController : Controller
{
    // GET api
    [HttpGet]
    public async Task<string> Get()
    {
        string result = "";
        try
        {
            Uri uri = new Uri("http://service1.application1:8080/api/values");
            HttpClient client = new HttpClient();
            var response = await client.GetAsync(uri);
            result = await response.Content.ReadAsStringAsync();
            
        }
        catch (Exception e)
        {
            Console.Write(e.Message);
        }

        return result;
    }
}
```

## <a name="next-steps"></a>Další kroky
Další informace o komunikaci služby v rámci clusteru s [připojení a komunikovat se službami](service-fabric-connect-and-communicate-with-services.md)

[0]: ./media/service-fabric-connect-and-communicate-with-services/dns.png
[1]: ./media/service-fabric-dnsservice/servicefabric-explorer-dns.PNG
[2]: ./media/service-fabric-dnsservice/DNSService.PNG
