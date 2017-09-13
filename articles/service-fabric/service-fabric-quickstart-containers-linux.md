---
title: "Vytvoření aplikace Azure Service Fabric typu kontejner v Linuxu | Dokumentace Microsoftu"
description: "Vytvoříte svou první aplikaci typu kontejner pro Linux na platformě Azure Service Fabric.  Sestavíte image Dockeru s vaší aplikací, nahrajete image do registru kontejneru a sestavíte a nasadíte aplikaci Service Fabric typu kontejner."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/05/2017
ms.author: ryanwi
ms.translationtype: HT
ms.sourcegitcommit: eeed445631885093a8e1799a8a5e1bcc69214fe6
ms.openlocfilehash: 78306672e812745fd1902ae264c2adea196ab721
ms.contentlocale: cs-cz
ms.lasthandoff: 09/07/2017

---

# <a name="deploy-a-service-fabric-linux-container-application-on-azure"></a>Nasazení aplikace Service Fabric typu kontejner pro Linux v Azure
Azure Service Fabric je platforma distribuovaných systémů pro nasazování a správu škálovatelných a spolehlivých mikroslužeb a kontejnerů. 

Spuštění existující aplikace v kontejneru Linux v clusteru Service Fabric nevyžaduje žádné změny aplikace. Tento rychlý start ukazuje, jak nasadit předem připravenou image kontejneru Dockeru v aplikaci Service Fabric. Až budete hotovi, budete mít funkční kontejner nginx.  Tento rychlý start popisuje nasazení kontejneru Linuxu. Pokud chcete nasadit kontejner Windows, přečtěte si [tento rychlý start](service-fabric-quickstart-containers.md).

![Nginx][nginx]

V tomto rychlém startu se naučíte:
> [!div class="checklist"]
> * Zabalení kontejneru image Dockeru
> * Konfigurace komunikace
> * Sestavení a zabalení aplikace Service Fabric
> * Nasazení aplikace typu kontejner do Azure

## <a name="prerequisites"></a>Požadavky
Nainstalujte [sadu Service Fabric SDK, Service Fabric CLI a generátory šablon Service Fabric yeoman](service-fabric-get-started-linux.md).
  
## <a name="package-a-docker-image-container-with-yeoman"></a>Zabalení kontejneru image Dockeru pomocí Yeomana
Sada Service Fabric SDK pro Linux zahrnuje generátor [Yeoman](http://yeoman.io/), který usnadňuje vytvoření aplikace a přidání image kontejneru. 

Pokud chcete vytvořit aplikaci Service Fabric typu kontejner, otevřete okno terminálu a spusťte příkaz `yo azuresfcontainer`.  

Pojmenujte aplikaci MyFirstContainer a aplikační službu MyContainerService.

Zadejte název image kontejneru nginx:latest (nejnovější [image kontejneru nginx](https://hub.docker.com/r/_/nginx/) na Docker Hubu. 

Tato image má definovaný vstupní bod úloh, takže je potřeba explicitně zadat vstupní příkazy. 

Jako počet instancí zadejte 1.

![Generátor Service Fabric Yeoman pro kontejnery][sf-yeoman]

## <a name="configure-communication-and-container-port-to-host-port-mapping"></a>Konfigurace komunikace a mapování portu kontejneru na port hostitele
Nakonfigurujte koncový bod HTTP, aby klienti mohli komunikovat s vaší službou.  Otevřete soubor *./MyFirstContainer/MyContainerServicePkg/ServiceManifest.xml* a v elementu **ServiceManifest** deklarujte prostředek koncového bodu.  Přidejte protokol, port a název. Pro účely tohoto rychlého startu služba naslouchá na portu 80: 

```xml
<Resources>
  <Endpoints>
    <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
    <Endpoint Name="myserviceTypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
  </Endpoints>
</Resources>

```
Pokud zadáte `UriScheme`, koncový bod kontejneru se automaticky zaregistruje ve Službě pojmenování Service Fabric, aby byl zjistitelný. Úplný ukázkový soubor ServiceManifest.xml najdete na konci tohoto článku. 

Namapujte port kontejneru na `Endpoint` služby pomocí zásady `PortBinding` v části `ContainerHostPolicies` souboru ApplicationManifest.xml.  Pro účely tohoto rychlého startu je `ContainerPort` nastavený na 80 (kontejner zpřístupňuje port 80) a `EndpointRef` je myserviceTypeEndpoint (koncový bod definovaný dříve v manifestu služby).  Příchozí požadavky na službu na portu 80 se mapují na port 80 v kontejneru.  

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="80" EndpointRef="myserviceTypeEndpoint"/>
  </ContainerHostPolicies>
</Policies>
```

## <a name="build-and-package-the-service-fabric-application"></a>Sestavení a zabalení aplikace Service Fabric
Šablony Service Fabric Yeoman zahrnují skript sestavení pro [Gradle](https://gradle.org/), který můžete použít k sestavení aplikace z terminálu. Uložte všechny provedené změny.  Pokud chcete sestavit a zabalit aplikaci, spusťte následující:

```bash
cd MyFirstContainer
gradle
```
## <a name="create-a-cluster"></a>Vytvoření clusteru
Pokud chcete nasadit aplikaci do clusteru v Azure, můžete vytvořit vlastní cluster nebo použít party cluster.

Party clustery jsou bezplatné, časově omezené clustery Service Fabric hostované v Azure a provozované týmem Service Fabric, na kterých může kdokoli nasazovat aplikace a seznamovat se s platformou. Pokud chcete získat přístup k party clusteru, [postupujte podle těchto pokynů](http://aka.ms/tryservicefabric).  

Informace o vytvoření vlastního clusteru najdete v tématu [Vytvoření vašeho prvního clusteru Service Fabric v Azure](service-fabric-get-started-azure-cluster.md).

Poznamenejte si koncový bod připojení, který použijete v následujícím kroku.

## <a name="deploy-the-application-to-azure"></a>Nasazení aplikace v Azure
Jakmile je aplikace sestavená, můžete ji nasadit do clusteru Azure pomocí Service Fabric CLI.

Připojte se ke clusteru Service Fabric v Azure.

```bash
sfctl cluster select --endpoint http://lnxt10vkfz6.westus.cloudapp.azure.com:19080
```

Pomocí instalačního skriptu, který je součástí šablony, zkopírujte balíček aplikace do úložiště imagí clusteru, zaregistrujte typ aplikace a vytvořte její instanci.

```bash
./install.sh
```

Otevřete prohlížeč a přejděte na Service Fabric Explorer na adrese http://lnxt10vkfz6.westus.cloudapp.azure.com:19080/Explorer. Rozbalte uzel Aplikace a všimněte si, že už obsahuje položku pro váš typ aplikace a další položku pro první instanci tohoto typu.

![Service Fabric Explorer][sfx]

Připojte se ke spuštěnému kontejneru.  Otevřete webový prohlížeč a přejděte na vrácenou IP adresu na portu 80, například lnxt10vkfz6.westus.cloudapp.azure.com:80. V prohlížeči by se měla zobrazit úvodní stránka serveru nginx.

![Nginx][nginx]

## <a name="clean-up"></a>Vyčištění
Pomocí odinstalačního skriptu, který je součástí šablony, odstraňte instanci aplikace z clusteru a zrušte registraci typu aplikace.

```bash
./uninstall.sh
```

## <a name="complete-example-service-fabric-application-and-service-manifests"></a>Kompletní příklad manifestů služby a aplikace Service Fabric
Tady jsou kompletní manifesty aplikace a služby použité v tomto rychlém startu.

### <a name="servicemanifestxml"></a>ServiceManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="MyContainerServicePkg" Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" >

   <ServiceTypes>
      <StatelessServiceType ServiceTypeName="MyContainerServiceType" UseImplicitHost="true">
   </StatelessServiceType>
   </ServiceTypes>
   
   <CodePackage Name="code" Version="1.0.0">
      <EntryPoint>
         <ContainerHost>
            <ImageName>nginx:latest</ImageName>
            <Commands></Commands>
         </ContainerHost>
      </EntryPoint>
      <EnvironmentVariables> 
      </EnvironmentVariables> 
   </CodePackage>
<Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="myserviceTypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
    </Endpoints>
  </Resources>
 </ServiceManifest>

```
### <a name="applicationmanifestxml"></a>ApplicationManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest  ApplicationTypeName="MyFirstContainerType" ApplicationTypeVersion="1.0.0"
                      xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
   
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyContainerServicePkg" ServiceManifestVersion="1.0.0" />
   <Policies>
      <ContainerHostPolicies CodePackageRef="Code">
        <PortBinding ContainerPort="80" EndpointRef="myserviceTypeEndpoint"/>
      </ContainerHostPolicies>
    </Policies>
</ServiceManifestImport>
   
   <DefaultServices>
      <Service Name="MyContainerService">
        <!-- On a local development cluster, set InstanceCount to 1.  On a multi-node production 
        cluster, set InstanceCount to -1 for the container service to run on every node in 
        the cluster.
        -->
        <StatelessService ServiceTypeName="MyContainerServiceType" InstanceCount="1">
            <SingletonPartition />
        </StatelessService>
      </Service>
   </DefaultServices>
   
</ApplicationManifest>

```

## <a name="next-steps"></a>Další kroky
V tomto rychlém startu se naučíte:
> [!div class="checklist"]
> * Zabalení kontejneru image Dockeru
> * Konfigurace komunikace
> * Sestavení a zabalení aplikace Service Fabric
> * Nasazení aplikace typu kontejner do Azure

* Další informace o spouštění [kontejnerů v Service Fabric](service-fabric-containers-overview.md).
* Přečtěte si kurz [Nasazení aplikace .NET v kontejneru](service-fabric-host-app-in-a-container.md).
* Informace o [životním cyklu aplikace](service-fabric-application-lifecycle.md) Service Fabric.
* Prohlédněte si [ukázky kódu kontejneru Service Fabric](https://github.com/Azure-Samples/service-fabric-dotnet-containers) na GitHubu.

[sfx]: ./media/service-fabric-quickstart-containers-linux/SFX.png
[nginx]: ./media/service-fabric-quickstart-containers-linux/nginx.png
[sf-yeoman]: ./media/service-fabric-quickstart-containers-linux/YoSF.png

