---
title: "Zabalení a nasazení aplikace Service Fabric kontejnery | Microsoft Docs"
description: "Zjistěte, jak vygenerovat definice aplikace Azure Service Fabric pomocí Yeoman a balíček aplikace."
services: service-fabric
documentationcenter: 
author: suhuruli
manager: timlt
editor: suhuruli
tags: servicefabric
keywords: "Docker kontejnery, Mikroslužeb, Service Fabric, Azure"
ms.assetid: 
ms.service: service-fabric
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/12/2017
ms.author: suhuruli
ms.custom: mvc
ms.openlocfilehash: caa7f58860c4540fa6914b1c0f0cfcba437468fa
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/11/2018
---
# <a name="package-and-deploy-containers-as-a-service-fabric-application"></a>Zabalení a nasazení kontejnerů jako aplikace Service Fabric

Tento kurz je určen dvěma součástí řady. V tomto kurzu nástroj Generátor šablony (Yeoman) slouží ke generování definici aplikace Service Fabric. Tuto aplikaci lze potom k nasazení kontejnerů do Service Fabric. V tomto kurzu se naučíte: 

> [!div class="checklist"]
> * Nainstalujte Yeoman  
> * Vytvoření balíčku aplikace pomocí Yeoman
> * Konfigurace nastavení v balíčku aplikace pro použití s kontejnery
> * Sestavení aplikace  
> * Nasazení a spuštění aplikace 
> * Vyčištění aplikace

## <a name="prerequisites"></a>Požadavky

- Kontejner imagí nabídnutých do registru kontejner Azure vytvořené v [část 1](service-fabric-tutorial-create-container-images.md) v tomto kurzu se používají řady.
- Vývojové prostředí Linux je [nastavit](service-fabric-tutorial-create-container-images.md).

## <a name="install-yeoman"></a>Nainstalujte Yeoman
Service fabric nabízí nástroje pro generování uživatelského rozhraní k usnadnění vytváření aplikací z terminálu pomocí Yeoman generátor šablony. Postupujte podle následujících kroků a zajistěte, abyste měli Yeoman generátor šablony. 

1. Nainstalujte nodejs a NPM na váš počítač. Všimněte si, že Mac OSX uživatelé budou muset použít Správce balíčků Homebrew

    ```bash
    sudo apt-get install npm && sudo apt install nodejs-legacy
    ```
2. Nainstalujte Yeoman generátor šablony na počítači z NPM 

    ```bash
    sudo npm install -g yo
    ```
3. Nainstalujte generátor kontejneru Service Fabric Yeoman

    ```bash 
    sudo npm install -g generator-azuresfcontainer
    ```

## <a name="package-a-docker-image-container-with-yeoman"></a>Zabalení kontejneru image Dockeru pomocí Yeomana

1. Chcete-li vytvořit kontejner Service Fabric aplikace, v adresáři, kontejner tutorial' klonovaný úložiště, spusťte následující příkaz.

    ```bash
    yo azuresfcontainer
    ```
2. Zadejte prosím "TestContainer" název vaší aplikace
3. Zadejte prosím "azurevotefront" název aplikace služby.
4. Zadat cestu image kontejneru v ACR front-endu úložiště – například '\<acrName >.azurecr.io / azure-hlas – přední: v1'. \<AcrName > pole musí být stejná jako hodnota, který byl použit v předchozí kurzu.
5. Stiskněte klávesu Enter a nechte příkazy části prázdný.
6. Zadejte počet instancí 1.

Následující příklad zobrazuje vstupní a výstupní spuštění příkazu je:

```bash
? Name your application TestContainer
? Name of the application service: azurevotefront
? Input the Image Name: <acrName>.azurecr.io/azure-vote-front:v1
? Commands: 
? Number of instances of guest container application: 1
   create TestContainer/TestContainer/ApplicationManifest.xml
   create TestContainer/TestContainer/azurevotefrontPkg/ServiceManifest.xml
   create TestContainer/TestContainer/azurevotefrontPkg/config/Settings.xml
   create TestContainer/TestContainer/azurevotefrontPkg/code/Dummy.txt
   create TestContainer/install.sh
   create TestContainer/uninstall.sh
```

Pokud chcete přidat jiné služby kontejneru na aplikaci již vytvořený Yeoman, proveďte následující kroky:

1. Změňte adresář na jednu úroveň **TestContainer** adresáři, například *. / TestContainer*
2. Spusťte `yo azuresfcontainer:AddService`. 
3. Název služby azurevoteback
4. Zadejte cestu k bitové kopie kontejner pro Redis - ' alpine: redis.
5. Stiskněte klávesu Enter a nechte příkazy části prázdný
6. Jako počet instancí zadejte 1.

Položky pro přidání služby použít všechny zobrazeny:

```bash
? Name of the application service: azurevoteback
? Input the Image Name: alpine:redis
? Commands: 
? Number of instances of guest container application: 1
   create TestContainer/azurevotebackPkg/ServiceManifest.xml
   create TestContainer/azurevotebackPkg/config/Settings.xml
   create TestContainer/azurevotebackPkg/code/Dummy.txt
```

Pro zbývající část tohoto kurzu, pracujeme **TestContainer** adresáře. Například *./TestContainer/TestContainer*. Obsah tohoto adresáře by měl vypadat takto.
```bash
$ ls
ApplicationManifest.xml azurevotefrontPkg azurevotebackPkg
```

## <a name="configure-the-application-manifest-with-credentials-for-azure-container-registry"></a>Manifest aplikace nakonfigurovat přihlašovací údaje pro registru kontejner Azure
Pro Service Fabric načítat kontejneru bitové kopie z registru kontejner Azure, je potřeba zadat přihlašovací údaje v **ApplicationManifest.xml**. 

Přihlaste se k instanci ACR. Použití **az acr přihlášení** příkaz k dokončení operace. Zadejte jedinečný název zadané registru kontejneru v okamžiku vytvoření.

```bash
az acr login --name <acrName>
```

Příkaz vrátí **bylo přihlášení úspěšné** zprávu po dokončení.

Potom spusťte následující příkaz získat heslo registr kontejneru. Toto heslo slouží k ověření s ACR vyžádání kontejneru bitové kopie pomocí Service Fabric.

```bash
az acr credential show -n <acrName> --query passwords[0].value
```

V **ApplicationManifest.xml**, Přidání fragmentu kódu zobrazeném v části **ServiceManifestImport** element pro službu front-endu. Vložit vaše **acrName** pro **AccountName** pole a vrácené z předchozího příkazu heslo se používá pro **heslo** pole. Úplné **ApplicationManifest.xml** je k dispozici na konci tohoto dokumentu. 

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code">
    <RepositoryCredentials AccountName="<acrName>" Password="<password>" PasswordEncrypted="false"/>
  </ContainerHostPolicies>
</Policies>
```
## <a name="configure-communication-and-container-port-to-host-port-mapping"></a>Konfigurace komunikace a mapování portu kontejneru na port hostitele

### <a name="configure-communication-port"></a>Konfigurace portů pro komunikaci

Nakonfigurujte koncový bod HTTP, aby klienti mohli komunikovat s vaší službou. Otevřete *./TestContainer/azurevotefrontPkg/ServiceManifest.xml* souborů a deklarovat prostředek koncového bodu v **ServiceManifest** element.  Přidejte protokol, port a název. V tomto kurzu službu naslouchá na portu 80. Následující fragment kódu je umístěn v *ServiceManifest* značky v prostředku.
  
```xml
<Resources>
  <Endpoints>
    <!-- This endpoint is used by the communication listener to obtain the port on which to 
            listen. Please note that if your service is partitioned, this port is shared with 
            replicas of different partitions that are placed in your code. -->
    <Endpoint Name="azurevotefrontTypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
  </Endpoints>
</Resources>

```
  
Podobně upravte Service Manifest pro back-end službu. Otevřete *./TestContainer/azurevotebackPkg/ServiceManifest.xml* a deklarovat prostředek koncového bodu v **ServiceManifest** element. V tomto kurzu zachovaný výchozí redis 6379. Následující fragment kódu je umístěn v *ServiceManifest* značky v prostředku.

```xml
<Resources>
  <Endpoints>
    <!-- This endpoint is used by the communication listener to obtain the port on which to 
            listen. Please note that if your service is partitioned, this port is shared with 
            replicas of different partitions that are placed in your code. -->
    <Endpoint Name="azurevotebackTypeEndpoint" Port="6379" Protocol="tcp"/>
  </Endpoints>
</Resources>
```
Poskytuje **UriScheme**koncový bod kontejneru službu Service Fabric Naming pro možnosti rozpoznání zaregistruje automaticky. Úplný příklad souboru ServiceManifest.xml pro back-end službu k dispozici na konci tohoto článku jako příklad. 

### <a name="map-container-ports-to-a-service"></a>Mapování portů kontejneru služby
Tak, aby získal kontejnery v clusteru, je potřeba také vytvořit vazbu portu v 'ApplicationManifest.xml'. **PortBinding** zásad odkazy **koncové body** definovaného v **ServiceManifest.xml** soubory. Příchozí žádosti s těmito koncovými body získat namapované na kontejneru porty, které jsou otevřené a ohraničenou sem. V **ApplicationManifest.xml** soubor, přidejte následující kód k vytvoření vazby porty 80 a 6379 koncových bodů. Úplné **ApplicationManifest.xml** je k dispozici na konci tohoto dokumentu. 
  
```xml
<ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="80" EndpointRef="azurevotefrontTypeEndpoint"/>
</ContainerHostPolicies>
```

```xml
<ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="6379" EndpointRef="azurevotebackTypeEndpoint"/>
</ContainerHostPolicies>
```

### <a name="add-a-dns-name-to-the-backend-service"></a>Přidat název DNS ke službě back-end
  
Pro Service Fabric přiřadit tento název DNS ke službě back-end, je třeba zadat v názvu **ApplicationManifest.xml**. Přidat **ServiceDnsName** atribut **služby** element, jak je znázorněno: 
  
```xml
<Service Name="azurevoteback" ServiceDnsName="redisbackend.testapp">
  <StatelessService ServiceTypeName="azurevotebackType" InstanceCount="1">
    <SingletonPartition/>
  </StatelessService>
</Service>
```

Služba front-endu přečte proměnné prostředí znát název DNS instance Redis. Tato proměnná prostředí je již definována v soubor Docker, která byla použita k vytvoření bitové kopie Docker a mají být provedeny, zde není nutné žádnou akci.
  
```Dockerfile
ENV REDIS redisbackend.testapp
```
  
Následující fragment kódu ukazuje, jak kód front-endu Python převezme popsané v soubor Docker proměnné prostředí. Mají být provedeny, zde není nutné žádnou akci. 

```python
# Get DNS Name
redis_server = os.environ['REDIS'] 

# Connect to the Redis store
r = redis.StrictRedis(host=redis_server, port=6379, db=0)
```

V tomto okamžiku v tomto kurzu je k dispozici pro nasazení do clusteru s podporou šablonu pro balíček služby aplikací. V tomto kurzu následné tuto aplikaci nasazuje a Cluster Service Fabric se spustil.

## <a name="create-a-service-fabric-cluster"></a>Vytvořit cluster Service Fabric
Pokud chcete nasadit aplikaci do clusteru v Azure, použijte vlastní cluster nebo Party Cluster.

Party Clustery jsou bezplatné, časově omezené clustery Service Fabric hostované v Azure. Je zachována tým Service Fabric, kde každý, kdo můžete nasadit aplikace a další informace o platformě. Pokud chcete získat přístup k Party Clusteru, [postupujte podle těchto pokynů](http://aka.ms/tryservicefabric). 

Informace o vytvoření vlastního clusteru najdete v tématu věnovaném [vytvoření clusteru Service Fabric v Azure](service-fabric-tutorial-create-vnet-and-linux-cluster.md).

## <a name="build-and-deploy-the-application-to-the-cluster"></a>Sestavení a nasazení aplikací do clusteru
Můžete nasadit aplikaci Azure clusteru pomocí rozhraní příkazového řádku služby prostředků infrastruktury. Pokud služba Fabric rozhraní příkazového řádku není nainstalovaný na počítači, postupujte podle pokynů [sem](service-fabric-get-started-linux.md#set-up-the-service-fabric-cli) k její instalaci. 

Připojte se ke clusteru Service Fabric v Azure. Nahraďte zástupný symbol koncový bod vlastní. Koncový bod musí být podobná té následující úplnou adresu URL.

```bash
sfctl cluster select --endpoint <http://lin4hjim3l4.westus.cloudapp.azure.com:19080>
```

Pomocí skriptu instalace součástí **TestContainer** adresář ke zkopírování balíčku aplikace do úložiště bitových kopií clusteru, registrace typu aplikace a vytvořit instanci aplikace.

```bash
./install.sh
```

Otevřete prohlížeč a přejděte do Service Fabric Explorer na http://lin4hjim3l4.westus.cloudapp.azure.com:19080/Explorer. Rozbalte uzel aplikace a Všimněte si, že je položka pro váš typ aplikace a druhý pro instanci.

![Service Fabric Explorer][sfx]

Aby bylo možné připojit k běžící aplikaci, otevřete webový prohlížeč a přejděte na adresu url clusteru – například http://lin0823ryf2he.cloudapp.azure.com:80. Měli byste vidět Voting aplikace webového uživatelského rozhraní.

![votingapp][votingapp]

## <a name="clean-up"></a>Vyčištění
Pomocí odinstalačního skriptu, který je součástí šablony, odstraňte instanci aplikace z clusteru a zrušte registraci typu aplikace. Tento příkaz má chvíli vyčištění instance a okamžitě po tento skript nelze spustit příkaz 'install.sh'. 

```bash
./uninstall.sh
```

## <a name="examples-of-completed-manifests"></a>Příklady dokončené manifestů

### <a name="applicationmanifestxml"></a>ApplicationManifest.xml
```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<ApplicationManifest ApplicationTypeName="TestContainerType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="azurevotefrontPkg" ServiceManifestVersion="1.0.0"/>
    <Policies> 
    <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myaccountname" Password="<password>" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="80" EndpointRef="azurevotefrontTypeEndpoint"/>
    </ContainerHostPolicies>
        </Policies>
  </ServiceManifestImport>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="azurevotebackPkg" ServiceManifestVersion="1.0.0"/>
      <Policies> 
        <ContainerHostPolicies CodePackageRef="Code">
          <PortBinding ContainerPort="6379" EndpointRef="azurevotebackTypeEndpoint"/>
        </ContainerHostPolicies>
      </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <Service Name="azurevotefront">
      <StatelessService ServiceTypeName="azurevotefrontType" InstanceCount="1">
        <SingletonPartition/>
      </StatelessService>
    </Service>
    <Service Name="azurevoteback" ServiceDnsName="redisbackend.testapp">
      <StatelessService ServiceTypeName="azurevotebackType" InstanceCount="1">
        <SingletonPartition/>
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```

### <a name="front-end-servicemanifestxml"></a>Front-endu ServiceManifest.xml 
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="azurevotefrontPkg" Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" >

   <ServiceTypes>
      <StatelessServiceType ServiceTypeName="azurevotefrontType" UseImplicitHost="true">
   </StatelessServiceType>
   </ServiceTypes>
   
   <CodePackage Name="code" Version="1.0.0">
      <EntryPoint>
         <ContainerHost>
            <ImageName>acrName.azurecr.io/azure-vote-front:v1</ImageName>
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
      <Endpoint Name="azurevotefrontTypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
    </Endpoints>
  </Resources>

 </ServiceManifest>
```

### <a name="redis-servicemanifestxml"></a>Redis ServiceManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="azurevotebackPkg" Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" >

   <ServiceTypes>
      <StatelessServiceType ServiceTypeName="azurevotebackType" UseImplicitHost="true">
   </StatelessServiceType>
   </ServiceTypes>
   
   <CodePackage Name="code" Version="1.0.0">
      <EntryPoint>
         <ContainerHost>
            <ImageName>alpine:redis</ImageName>
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
      <Endpoint Name="azurevotebackTypeEndpoint" Port="6379" Protocol="tcp"/>
    </Endpoints>
  </Resources>
 </ServiceManifest>
```
## <a name="next-steps"></a>Další postup

V tomto kurzu se do aplikace Service Fabric pomocí Yeoman zabalené několika kontejnerů. Tato aplikace byla pak nasadit a spustit na cluster Service Fabric. Dokončili jste následující kroky:

> [!div class="checklist"]
> * Nainstalujte Yeoman  
> * Vytvoření balíčku aplikace pomocí Yeoman
> * Konfigurace nastavení v balíčku aplikace pro použití s kontejnery
> * Sestavení aplikace  
> * Nasazení a spuštění aplikace 
> * Vyčištění aplikace

Přechodu na v dalším kurzu se dozvíte o převzetí služeb při selhání a škálování aplikace v Service Fabric.

> [!div class="nextstepaction"]
> [Další informace o převzetí služeb při selhání a škálování aplikací](service-fabric-tutorial-containers-failover.md)

[votingapp]: ./media/service-fabric-tutorial-deploy-run-containers/votingapp.png
[sfx]: ./media/service-fabric-tutorial-deploy-run-containers/containerspackagetutorialsfx.png


