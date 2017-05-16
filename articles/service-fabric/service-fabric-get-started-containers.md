---
title: "Vytvoření aplikace Azure Service Fabric typu kontejner | Dokumentace Microsoftu"
description: "Vytvoříte svou první aplikaci typu kontejner na Azure Service Fabric.  Sestavíte image Dockeru s vaší aplikací, nahrajete image do registru kontejneru a sestavíte a nasadíte aplikaci Service Fabric typu kontejner."
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
ms.date: 05/08/2017
ms.author: ryanwi
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: acb68b274228aa647dc7be5d36b2b077bd213c1b
ms.contentlocale: cs-cz
ms.lasthandoff: 05/10/2017


---

# <a name="create-your-first-service-fabric-container-app"></a>Vytvoření první aplikace Service Fabric typu kontejner
Spuštění existující aplikace v kontejneru Windows v clusteru Service Fabric nevyžaduje žádné změny aplikace. Tento Rychlý start vás provede vytvořením image Dockeru obsahující webovou aplikaci, nahráním nové image do služby Azure Container Registry, vytvořením aplikace Service Fabric typu kontejner a nasazením aplikace typu kontejner do clusteru Service Fabric.  Tento článek předpokládá základní znalost Dockeru. Informace o Dockeru najdete v článku [Docker Overview](https://docs.docker.com/engine/understanding-docker/) (Přehled Dockeru).

## <a name="prerequisites"></a>Požadavky
Vývojový počítač s:
* Visual Studio 2015 nebo Visual Studio 2017.
* [Sada Service Fabric SDK a nástroje](service-fabric-get-started.md).
*  Docker pro Windows.  [Získejte Docker CE pro Windows (stabilní verze)](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description). Po nainstalování a spuštění Dockeru klikněte pravým tlačítkem myši na ikonu na hlavním panelu a vyberte **Switch to Windows containers** (Přepnout na kontejnery Windows). To se vyžaduje pro spuštění imagí Dockeru založených na Windows.

Cluster Windows se třemi nebo více uzly spuštěnými na Windows Serveru 2016 s kontejnery – [Vytvořte cluster](service-fabric-get-started-azure-cluster.md) nebo [vyzkoušejte Service Fabric zdarma](http://tryazureservicefabric.westus.cloudapp.azure.com/). 

Registr ve službě Azure Container Registry – [Vytvořte registr kontejneru](../container-registry/container-registry-get-started-portal.md) ve svém předplatném Azure. 

## <a name="create-a-simple-web-app"></a>Vytvoření jednoduché webové aplikace
Shromážděte na jednom místě všechny prostředky, které potřebujete nahrát do image Dockeru. Pro účely tohoto Rychlého startu vytvořte na vývojovém počítači webovou aplikaci Hello World.

1. Vytvořte adresář, například *c:\temp\helloworldapp*.
2. Vytvořte podadresář *c:\temp\helloworldapp\content*.
3. V adresáři *c:\temp\helloworldapp\content* vytvořte soubor *index.html*.
4. Upravte soubor *index.html* a přidejte následující řádek:
    ```
    <h1>Hello World!</h1>
    ```
5. Uložte změny souboru *index.html*.

## <a name="build-the-docker-image"></a>Sestavení image Dockeru
Sestavte image založenou na [imagi microsoft/iis](https://hub.docker.com/r/microsoft/iis/), která se nachází na Docker Hubu. Image microsoft/iis je odvozená od základní image operačního systému Windows Server Core a obsahuje Internetovou informační službu (služba IIS).  Spuštění této image v kontejneru automaticky spustí službu IIS a nainstalované weby.

Definujte vaši image Dockeru v souboru Dockerfile. Soubor Dockerfile obsahuje pokyny k sestavení image a nahrání aplikace, kterou chcete spustit. Soubor Dockerfile je vstupem příkazu ```docker build```, který vytvoří image. 

1. V adresáři *c:\temp\helloworldapp* vytvořte soubor *Dockerfile* (bez přípony souboru) a přidejte do něj následující:

    ```
    # The `FROM` instruction specifies the base image. You are
    # extending the `microsoft/iis` image.
    FROM microsoft/iis

    # Create a directory to hold the web app in the container.
    RUN mkdir C:\site

    # Create a new IIS site.
    RUN powershell -NoProfile -Command \
        Import-module IISAdministration; \
        New-IISSite -Name "Site" -PhysicalPath C:\site -BindingInformation "*:8000:"

    # Opens port 8000 on the container.
    EXPOSE 8000

    # The final instruction copies the web app you created earlier into the container.
    ADD content/ /site
    ```

    V tomto souboru Dockerfile není žádný příkaz ```ENTRYPOINT```. Žádný nepotřebujete. Při spouštění Windows Serveru se službou IIS je vstupním bodem proces služby IIS, jehož spuštění je nakonfigurováno v základní imagi.

    Další informace najdete v [referenčních informacích k souboru Dockerfile](https://docs.docker.com/engine/reference/builder/).

2. Spuštěním příkazu ```docker build``` vytvořte image spouštějící vaši webovou aplikaci. Otevřete okno PowerShellu a přejděte do adresáře *c:\temp\helloworldapp*. Spusťte následující příkaz:

    ```
    docker build -t helloworldapp .
    ```
    Tento příkaz sestaví novou image podle pokynů ve vašem souboru Dockerfile a pojmenuje ji (označení -t) „helloworldapp“. Sestavení image si z Docker Hubu stáhne základní image a vytvoří novou image, která přidá vaši aplikaci nad základní image.  [Image microsoft/iis](https://hub.docker.com/r/microsoft/iis/) a základní image operačního systému mají 10,5 GB a jejich stažení a extrahování na vývojovém počítači chvíli trvá.  Můžete si zajít na oběd nebo šálek kávy.  Stahování bude trvat kratší dobu, pokud jste na vývojovém počítači již dříve stáhli základní image operačního systému.

3. Po dokončení příkazu pro sestavení spusťte příkaz `docker images` a zobrazte informace o nové imagi:

    ```
    docker images
    
    REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
    helloworldapp              latest              86838648aab6        2 minutes ago       10.1 GB
    ```

## <a name="verify-the-image-runs-locally"></a>Ověření, že image lze spustit místně
Než image nahrajete do registru kontejneru, ověřte ji místně.  

1. Spusťte kontejner pomocí příkazu ```docker run```:

    ```
    docker run -d -p 8000:8000 --name my-web-site helloworldapp
    ```

    Parametr *name* udává název spuštěného kontejneru (namísto ID kontejneru).

2. Po spuštění kontejneru vyhledejte jeho IP adresu, abyste se ke spuštěnému kontejneru mohli připojit z prohlížeče:
    ```
    docker inspect -f "{{ .NetworkSettings.Networks.nat.IPAddress }}" my-web-site
    ```

3. Připojte se ke spuštěnému kontejneru.  Otevřete webový prohlížeč a přejděte na vrácenou IP adresu na portu 8000, například http://172.31.194.61:8000. V prohlížeči by se měl zobrazit nadpis „Hello World!“.

4. Pokud chcete kontejner zastavit, spusťte:

    ```
    docker stop my-web-site
    ```

5. Odstraňte kontejner z vývojového počítače:

    ```
    docker rm my-web-site
    ```

## <a name="push-the-image-to-the-container-registry"></a>Nahrání image do registru kontejneru
Po ověření, že se kontejner spustí na vývojovém počítači, nahrajte image do vašeho registru ve službě Azure Container Service.

1. Spusťte příkaz ``docker login`` a přihlaste se ke svému registru kontejnerů pomocí [přihlašovacích údajů registru](../container-registry/container-registry-authentication.md).

    Následující příklad předá ID a heslo [instančního objektu](../active-directory/active-directory-application-objects.md) Azure Active Directory. Instanční objekt jste k registru mohli přiřadit například pro účely scénáře automatizace.

    ```
    docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
    ```

2. Následující příkaz vytvoří značku, neboli alias, image s plně kvalifikovanou cestou k vašemu registru. Tento příklad umístí image do oboru názvů ```samples```, aby se zabránilo nepořádku v kořenovém adresáři registru.

    ```
    docker tag helloworldapp myregistry.azurecr.io/samples/helloworldapp
    ```

3.  Nahrajte image do registru kontejneru:

    ```
    docker push myregistry.azurecr.io/samples/helloworldapp
    ```

## <a name="create-and-package-the-containerized-service-in-visual-studio"></a>Vytvoření a zabalení kontejnerizované služby v sadě Visual Studio
Sada Service Fabric SDK a nástroje poskytují šablonu služby, která vám pomůže s nasazením kontejneru do clusteru Service Fabric.

1. Spusťte Visual Studio.  Vyberte **Soubor** > **Nový** > **Projekt**.
2. Vyberte **Aplikace Service Fabric**, pojmenujte ji MyFirstContainer a klikněte na **OK**.
3. Ze **seznamu šablon** služeb vyberte **Guest Container**.
4. Do pole **Název image** zadejte „myregistry.azurecr.io/samples/helloworldapp“, tedy image, kterou jste nahráli do úložiště kontejnerů. 
5. Zadejte název služby a klikněte na **OK**.
6. Pokud vaše kontejnerizovaná služba potřebuje koncový bod pro komunikaci, můžete teď přidat protokol, port a typ v části ```Endpoint``` v souboru ServiceManifest.xml. Pro účely tohoto Rychlého startu kontejnerizovaná služba naslouchá na portu 80: 

    ```xml
    <Endpoint Name="Guest1TypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
    ```
    Pokud zadáte ```UriScheme```, koncový bod kontejneru se automaticky zaregistruje ve Službě pojmenování Service Fabric, aby byl zjistitelný. Úplný ukázkový soubor ServiceManifest.xml najdete na konci tohoto článku. 
7. Nakonfigurujte v kontejneru mapování portů na hostitele určením zásady ```PortBinding``` v části ```ContainerHostPolicies``` souboru APplicationManifest.xml.  Pro účely tohoto Rychlého startu je ```ContainerPort``` nastaven na 8000 (kontejner zpřístupňuje port 8000, jak je zadáno v souboru Dockerfile) a ```EndpointRef``` je „Guest1TypeEndpoint“ (koncový bod definovaný v manifestu služby).  Příchozí požadavky na službu na portu 80 se mapují na port 8000 v kontejneru.  Pokud se váš kontejner potřebuje ověřovat v privátním úložišti, přidejte ```RepositoryCredentials```.  Pro účely tohoto Rychlého startu přidejte název a heslo účtu pro registr kontejneru myregistry.azurecr.io. 

    ```xml
    <Policies>
        <ContainerHostPolicies CodePackageRef="Code">
            <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
            <PortBinding ContainerPort="8000" EndpointRef="Guest1TypeEndpoint"/>
        </ContainerHostPolicies>
    </Policies>
    ```

    Úplný ukázkový soubor ApplicationManifest.xml najdete na konci tohoto článku.
8. Nakonfigurujte koncový bod připojení ke clusteru, abyste aplikaci mohli publikovat do clusteru.  Koncový bod připojení ke klientu najdete v okně Přehled pro váš cluster na webu [Azure Portal](https://portal.azure.com). V Průzkumníku řešení otevřete soubor *Cloud.xml* v části **MyFirstContainer** -> **PublishProfiles**.  Přidejte název clusteru a port připojení do části **ClusterConnectionParameters**.  Například:
    ```xml
    <ClusterConnectionParameters ConnectionEndpoint="containercluster.westus2.cloudapp.azure.com:19000" />
    ```
    
9. Uložte všechny soubory a sestavte projekt.  

10. Pokud chcete aplikaci zabalit, klikněte pravým tlačítkem myši na **MyFirstContainer** v Průzkumníku řešení a vyberte **Zabalit**. 

## <a name="deploy-the-container-app"></a>Nasazení aplikace typu kontejner
1. Pokud chcete aplikaci publikovat, klikněte pravým tlačítkem myši na **MyFirstContainer** v Průzkumníku řešení a vyberte **Publikovat**.

2. [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) je webový nástroj pro kontrolu a správu aplikací a uzlů v clusteru Service Fabric. Otevřete prohlížeč, přejděte na adresu http://containercluster.westus2.cloudapp.azure.com:19080/Explorer/ a sledujte nasazení aplikace.  Aplikace se nasadí, ale bude v chybovém stavu, dokud se image nestáhne na uzlech clusteru (což v závislosti na velikosti image může nějakou dobu trvat):  ![Chyba][1]

3. Aplikace je připravena, když je ve stavu ```Ready```:  ![Připraveno][2]

4. Otevřete prohlížeč a přejděte na adresu http://containercluster.westus2.cloudapp.azure.com. V prohlížeči by se měl zobrazit nadpis „Hello World!“.

## <a name="clean-up"></a>Vyčištění
Za spuštěný cluster se vám stále účtují poplatky, proto zvažte [odstranění clusteru](service-fabric-get-started-azure-cluster.md#remove-the-cluster).  [Party clustery](http://tryazureservicefabric.westus.cloudapp.azure.com/) se automaticky odstraní po několika hodinách.

Po nahrání image do registru kontejneru můžete odstranit místní image z vývojového počítače:

```
docker rmi helloworldapp
docker rmi myregistry.azurecr.io/samples/helloworldapp
```

## <a name="complete-example-service-fabric-application-and-service-manifests"></a>Kompletní příklad manifestů služby a aplikace Service Fabric
Tady jsou kompletní manifesty aplikace a služby použité v tomto Rychlém startu.

### <a name="servicemanifestxml"></a>ServiceManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="Guest1Pkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         The UseImplicitHost attribute indicates this is a guest service. -->
    <StatelessServiceType ServiceTypeName="Guest1Type" UseImplicitHost="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <!-- Follow this link for more information about deploying Windows containers to Service Fabric: https://aka.ms/sfguestcontainers -->
      <ContainerHost>
        <ImageName>myregistry.azurecr.io/samples/helloworldapp</ImageName>
      </ContainerHost>
    </EntryPoint>
    <!-- Pass environment variables to your container: -->
    <!--
    <EnvironmentVariables>
      <EnvironmentVariable Name="VariableName" Value="VariableValue"/>
    </EnvironmentVariables>
    -->
  </CodePackage>

  <!-- Config package is the contents of the Config directoy under PackageRoot that contains an 
       independently-updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="Guest1TypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
    </Endpoints>
  </Resources>
</ServiceManifest>
```
### <a name="applicationmanifestxml"></a>ApplicationManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="MyFirstContainerType"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Parameters>
    <Parameter Name="Guest1_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion 
       should match the Name and Version attributes of the ServiceManifest element defined in the 
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Guest1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="8000" EndpointRef="Guest1TypeEndpoint"/>
      </ContainerHostPolicies>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types, when an instance of this 
         application type is created. You can also create one or more instances of service type using the 
         ServiceFabric PowerShell module.
         
         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="Guest1">
      <StatelessService ServiceTypeName="Guest1Type" InstanceCount="[Guest1_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```

## <a name="next-steps"></a>Další kroky
* Další informace o spouštění [kontejnerů v Service Fabric](service-fabric-containers-overview.md).
* Informace o [životním cyklu aplikace](service-fabric-application-lifecycle.md) Service Fabric.
* Prohlédněte si [ukázky kódu kontejneru Service Fabric](https://github.com/Azure-Samples/service-fabric-dotnet-containers) na GitHubu.

[1]: ./media/service-fabric-get-started-containers/MyFirstContainerError.png
[2]: ./media/service-fabric-get-started-containers/MyFirstContainerReady.png

