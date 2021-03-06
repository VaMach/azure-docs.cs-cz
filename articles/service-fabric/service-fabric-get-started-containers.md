---
title: "Vytvoření aplikace Azure Service Fabric typu kontejner | Dokumentace Microsoftu"
description: "Vytvoříte svou první aplikaci typu kontejner pro Windows na platformě Azure Service Fabric.  Sestavíte image Dockeru s aplikací v Pythonu, nahrajete image do registru kontejneru a sestavíte a nasadíte aplikaci Service Fabric typu kontejner."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: vturecek
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 1/19/2018
ms.author: ryanwi
ms.openlocfilehash: 5398605f98c9e115255057cfad0c4c2c2e14737c
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2018
---
# <a name="create-your-first-service-fabric-container-application-on-windows"></a>Vytvoření první aplikace Service Fabric typu kontejner v systému Windows
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started-containers.md)
> * [Linux](service-fabric-get-started-containers-linux.md)

Spuštění existující aplikace v kontejneru Windows v clusteru Service Fabric nevyžaduje žádné změny aplikace. Tento článek vás provede vytvořením image Dockeru obsahující webovou aplikaci Python [Flask](http://flask.pocoo.org/) a jejím nasazením do clusteru Service Fabric.  Kontejnerizovanou aplikaci budete také sdílet prostřednictvím služby [Azure Container Registry](/azure/container-registry/).  Tento článek předpokládá základní znalost Dockeru. Informace o Dockeru najdete v článku [Docker Overview](https://docs.docker.com/engine/understanding-docker/) (Přehled Dockeru).

## <a name="prerequisites"></a>Požadavky
Vývojový počítač s:
* Visual Studio 2015 nebo Visual Studio 2017.
* [Sada Service Fabric SDK a nástroje](service-fabric-get-started.md).
*  Docker pro Windows.  [Získejte Docker CE pro Windows (stabilní verze)](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description). Po nainstalování a spuštění Dockeru klikněte pravým tlačítkem myši na ikonu na hlavním panelu a vyberte **Switch to Windows containers** (Přepnout na kontejnery Windows). To se vyžaduje pro spuštění imagí Dockeru založených na Windows.

Cluster Windows se třemi nebo více uzly spuštěnými na Windows Serveru 2016 s kontejnery – [Vytvořte cluster](service-fabric-cluster-creation-via-portal.md) nebo [vyzkoušejte Service Fabric zdarma](https://aka.ms/tryservicefabric).

Registr ve službě Azure Container Registry – [Vytvořte registr kontejneru](../container-registry/container-registry-get-started-portal.md) ve svém předplatném Azure.

> [!NOTE]
> Nasazení kontejnerů do clusteru Service Fabric v systému Windows 10 nebo do clusteru s Dockerem CE se nepodporuje. Tento názorný průvodce spustí místní testy s využitím modulu Docker ve Windows 10 a nakonec kontejnerové služby nasadí do clusteru Windows Serveru v Azure se spuštěným Dockerem EE. 
>   

> [!NOTE]
> Service Fabric verze 6.1 nabízí podporu verze Preview pro Windows Server verze 1709. Otevřené sítě a služba DNS Service Fabricu s Windows Serverem verze 1709 nefungují. 
> 

## <a name="define-the-docker-container"></a>Definice kontejneru Dockeru
Sestavte image založenou na [imagi Pythonu](https://hub.docker.com/_/python/), která se nachází na Docker Hubu.

Definujte kontejner Dockeru v souboru Dockerfile. Soubor Dockerfile obsahuje pokyny k nastavení prostředí uvnitř kontejneru, načtení aplikace, kterou chcete pustit, a mapování portů. Soubor Dockerfile je vstupem příkazu `docker build`, který vytvoří image.

Vytvořte prázdný adresář a soubor *Dockerfile* (bez přípony souboru). Do souboru *Dockerfile* přidejte následující a uložte změny:

```
# Use an official Python runtime as a base image
FROM python:2.7-windowsservercore

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
ADD . /app

# Install any needed packages specified in requirements.txt
RUN pip install -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
```

Další informace najdete v [referenčních informacích k souboru Dockerfile](https://docs.docker.com/engine/reference/builder/).

## <a name="create-a-simple-web-application"></a>Vytvoření jednoduché webové aplikace
Vytvořte webovou aplikaci Flask, která naslouchá na portu 80 a vrací „Hello World!“.  Ve stejném adresáři vytvořte soubor *requirements.txt*.  Přidejte do něj následující a uložte změny:
```
Flask
```

Vytvořte také soubor *app.py* a přidejte do něj následující:

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello():

    return 'Hello World!'

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=80)
```

<a id="Build-Containers"></a>
## <a name="build-the-image"></a>Sestavení image
Spuštěním příkazu `docker build` vytvořte image spouštějící vaši webovou aplikaci. Otevřete okno PowerShellu a přejděte do adresáře, který obsahuje soubor Dockerfile. Spusťte následující příkaz:

```
docker build -t helloworldapp .
```

Tento příkaz sestaví novou image podle pokynů ve vašem souboru Dockerfile a pojmenuje ji (označení -t) „helloworldapp“. Sestavení image si z Docker Hubu přetáhne základní image a vytvoří novou image, která přidá vaši aplikaci nad základní image.  

Po dokončení příkazu pro sestavení spusťte příkaz `docker images` a zobrazte informace o nové imagi:

```
$ docker images

REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
helloworldapp                 latest              8ce25f5d6a79        2 minutes ago       10.4 GB
```

## <a name="run-the-application-locally"></a>Místní spuštění aplikace
Než image nahrajete do registru kontejneru, ověřte ji místně.  

Spusťte aplikaci:

```
docker run -d --name my-web-site helloworldapp
```

Parametr *name* udává název spuštěného kontejneru (namísto ID kontejneru).

Po spuštění kontejneru vyhledejte jeho IP adresu, abyste se ke spuštěnému kontejneru mohli připojit z prohlížeče:
```
docker inspect -f "{{ .NetworkSettings.Networks.nat.IPAddress }}" my-web-site
```

Připojte se ke spuštěnému kontejneru.  Otevřete webový prohlížeč a přejděte na vrácenou IP adresu, například http://172.31.194.61. V prohlížeči by se měl zobrazit nadpis „Hello World!“.

Pokud chcete kontejner zastavit, spusťte:

```
docker stop my-web-site
```

Odstraňte kontejner z vývojového počítače:

```
docker rm my-web-site
```

<a id="Push-Containers"></a>
## <a name="push-the-image-to-the-container-registry"></a>Nahrání image do registru kontejneru
Po ověření, že se kontejner spustí na vývojovém počítači, nahrajte image do vašeho registru ve službě Azure Container Service.

Spusťte příkaz ``docker login`` a přihlaste se ke svému registru kontejnerů pomocí [přihlašovacích údajů registru](../container-registry/container-registry-authentication.md).

Následující příklad předá ID a heslo [instančního objektu](../active-directory/active-directory-application-objects.md) Azure Active Directory. Instanční objekt jste k registru mohli přiřadit například pro účely scénáře automatizace. Nebo se můžete přihlásit pomocí uživatelského jména a hesla registru.

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

Následující příkaz vytvoří značku, neboli alias, image s plně kvalifikovanou cestou k vašemu registru. Tento příklad umístí image do oboru názvů ```samples```, aby se zabránilo nepořádku v kořenovém adresáři registru.

```
docker tag helloworldapp myregistry.azurecr.io/samples/helloworldapp
```

Nahrajte image do registru kontejneru:

```
docker push myregistry.azurecr.io/samples/helloworldapp
```

## <a name="create-the-containerized-service-in-visual-studio"></a>Vytvoření kontejnerizované služby v sadě Visual Studio
Sada Service Fabric SDK a nástroje poskytují šablonu služby, která vám pomůže s vytvořením kontejnerizované aplikace.

1. Spusťte Visual Studio.  Vyberte **Soubor** > **Nový** > **Projekt**.
2. Vyberte **Aplikace Service Fabric**, pojmenujte ji MyFirstContainer a klikněte na **OK**.
3. Ze seznamu **šablon služeb** vyberte **Kontejner**.
4. Do pole **Název image** zadejte „myregistry.azurecr.io/samples/helloworldapp“, tedy image, kterou jste nahráli do úložiště kontejnerů.
5. Zadejte název služby a klikněte na **OK**.

## <a name="configure-communication"></a>Konfigurace komunikace
Kontejnerizovaná služba potřebuje koncový bod pro komunikaci. Do souboru ServiceManifest.xml přidejte element `Endpoint` s protokolem, portem a typem. Pro účely tohoto článku kontejnerizovaná služba naslouchá na portu 8081.  V tomto příkladu se používá pevný port 8081.  Pokud port není zadaný, zvolí se v rozsahu portů aplikace náhodně.  

```xml
<Resources>
  <Endpoints>
    <Endpoint Name="Guest1TypeEndpoint" UriScheme="http" Port="8081" Protocol="http"/>
  </Endpoints>
</Resources>
```

Když Service Fabric definuje koncový bod, publikuje ho ve službě pojmenování.  Ostatní služby spuštěné v clusteru mohou tento kontejner vyhledat.  Ke komunikaci mezi kontejnery můžete také využít [reverzní proxy server](service-fabric-reverseproxy.md).  Komunikace se provede tak, že se reverznímu proxy serveru poskytne port pro naslouchání HTTP a název služeb, se kterými chcete komunikovat, jako proměnné prostředí.

## <a name="configure-and-set-environment-variables"></a>Konfigurace a nastavení proměnných prostředí
Proměnné prostředí je možné zadat pro každý balíček kódu v manifestu služby. Tato funkce je dostupná pro všechny služby, bez ohledu na to, jestli jsou nasazené jako kontejnery, procesy nebo spustitelné soubory typu Host. Hodnoty proměnných prostředí můžete přepsat v manifestu aplikace, nebo je můžete zadat v průběhu nasazení jako parametry aplikace.

Následující fragment kódu XML manifestu služby ukazuje příklad toho, jak zadat proměnné prostředí pro balíček kódu:
```xml
<CodePackage Name="Code" Version="1.0.0">
  ...
  <EnvironmentVariables>
    <EnvironmentVariable Name="HttpGatewayPort" Value=""/>    
  </EnvironmentVariables>
</CodePackage>
```

Tyto proměnné prostředí je možné přepsat v manifestu aplikace:

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="Guest1Pkg" ServiceManifestVersion="1.0.0" />
    <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
  </EnvironmentOverrides>
  ...
</ServiceManifestImport>
```

## <a name="configure-container-port-to-host-port-mapping-and-container-to-container-discovery"></a>Konfigurace mapování portu kontejneru na port hostitele a zjišťování mezi kontejnery
Nakonfigurujte port hostitele používaný ke komunikaci s kontejnerem. Vazba portu mapuje port, na kterém služba naslouchá uvnitř kontejneru, na port na hostiteli. Přidejte element `PortBinding` do `ContainerHostPolicies` v souboru ApplicationManifest.xml.  Pro účely tohoto článku je `ContainerPort` nastaven na 80 (kontejner zpřístupňuje port 80, jak je uvedené v souboru Dockerfile) a `EndpointRef` je „Guest1TypeEndpoint“ (koncový bod dříve definovaný v manifestu služby).  Příchozí požadavky na službu na portu 8081 se mapují na port 80 v kontejneru.

```xml
<ServiceManifestImport>
    ...
    <Policies>
        <ContainerHostPolicies CodePackageRef="Code">
            <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
        </ContainerHostPolicies>
    </Policies>
    ...
</ServiceManifestImport>
```

## <a name="configure-container-registry-authentication"></a>Konfigurace ověřování registru kontejneru
Nakonfigurujte ověřování registru kontejneru přidáním `RepositoryCredentials` do `ContainerHostPolicies` v souboru ApplicationManifest.xml. Přidejte účet a heslo pro registr kontejneru myregistry.azurecr.io, který službě umožňuje stáhnout image kontejneru z úložiště.

```xml
<ServiceManifestImport>
    ...
    <Policies>
        <ContainerHostPolicies CodePackageRef="Code">
            <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
            <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
        </ContainerHostPolicies>
    </Policies>
    ...
</ServiceManifestImport>
```

Doporučujeme šifrovat heslo úložiště pomocí certifikátu šifrování, který je nasazený na všechny uzly clusteru. Když Service Fabric nasadí balíček služby do clusteru, certifikát šifrování se použije k dešifrování šifrovaného textu.  Rutina Invoke-ServiceFabricEncryptText se používá k vytvoření šifrovaného textu pro heslo, který se přidá do souboru ApplicationManifest.xml.

Následující skript vytvoří nový certifikát podepsaný svým držitelem a vyexportuje ho do souboru PFX.  Certifikát se naimportuje do existujícího trezoru klíčů a potom nasadí do clusteru Service Fabric.

```powershell
# Variables.
$certpwd = ConvertTo-SecureString -String "Pa$$word321!" -Force -AsPlainText
$filepath = "C:\MyCertificates\dataenciphermentcert.pfx"
$subjectname = "dataencipherment"
$vaultname = "mykeyvault"
$certificateName = "dataenciphermentcert"
$groupname="myclustergroup"
$clustername = "mycluster"

$subscriptionId = "subscription ID"

Login-AzureRmAccount

Select-AzureRmSubscription -SubscriptionId $subscriptionId

# Create a self signed cert, export to PFX file.
New-SelfSignedCertificate -Type DocumentEncryptionCert -KeyUsage DataEncipherment -Subject $subjectname -Provider 'Microsoft Enhanced Cryptographic Provider v1.0' `
| Export-PfxCertificate -FilePath $filepath -Password $certpwd

# Import the certificate to an existing key vault.  The key vault must be enabled for deployment.
$cer = Import-AzureKeyVaultCertificate -VaultName $vaultName -Name $certificateName -FilePath $filepath -Password $certpwd

Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName -ResourceGroupName $groupname -EnabledForDeployment

# Add the certificate to all the VMs in the cluster.
Add-AzureRmServiceFabricApplicationCertificate -ResourceGroupName $groupname -Name $clustername -SecretIdentifier $cer.SecretId
```
K zašifrování hesla použijte rutinu [Invoke-ServiceFabricEncryptText](/powershell/module/servicefabric/Invoke-ServiceFabricEncryptText?view=azureservicefabricps).

```powershell
$text = "=P==/==/=8=/=+u4lyOB=+=nWzEeRfF="
Invoke-ServiceFabricEncryptText -CertStore -CertThumbprint $cer.Thumbprint -Text $text -StoreLocation Local -StoreName My
```

Heslo nahraďte šifrovaným textem, který vrátila rutina [Invoke-ServiceFabricEncryptText](/powershell/module/servicefabric/Invoke-ServiceFabricEncryptText?view=azureservicefabricps), a `PasswordEncrypted` nastavte na hodnotu true.

```xml
<ServiceManifestImport>
    ...
    <Policies>
        <ContainerHostPolicies CodePackageRef="Code">
            <RepositoryCredentials AccountName="myregistry" Password="MIIB6QYJKoZIhvcNAQcDoIIB2jCCAdYCAQAxggFRMIIBTQIBADA1MCExHzAdBgNVBAMMFnJ5YW53aWRhdGFlbmNpcGhlcm1lbnQCEFfyjOX/17S6RIoSjA6UZ1QwDQYJKoZIhvcNAQEHMAAEg
gEAS7oqxvoz8i6+8zULhDzFpBpOTLU+c2mhBdqXpkLwVfcmWUNA82rEWG57Vl1jZXe7J9BkW9ly4xhU8BbARkZHLEuKqg0saTrTHsMBQ6KMQDotSdU8m8Y2BR5Y100wRjvVx3y5+iNYuy/JmM
gSrNyyMQ/45HfMuVb5B4rwnuP8PAkXNT9VLbPeqAfxsMkYg+vGCDEtd8m+bX/7Xgp/kfwxymOuUCrq/YmSwe9QTG3pBri7Hq1K3zEpX4FH/7W2Zb4o3fBAQ+FuxH4nFjFNoYG29inL0bKEcTX
yNZNKrvhdM3n1Uk/8W2Hr62FQ33HgeFR1yxQjLsUu800PrYcR5tLfyTB8BgkqhkiG9w0BBwEwHQYJYIZIAWUDBAEqBBBybgM5NUV8BeetUbMR8mJhgFBrVSUsnp9B8RyebmtgU36dZiSObDsI
NtTvlzhk11LIlae/5kjPv95r3lw6DHmV4kXLwiCNlcWPYIWBGIuspwyG+28EWSrHmN7Dt2WqEWqeNQ==" PasswordEncrypted="true"/>
            <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
        </ContainerHostPolicies>
    </Policies>
    ...
</ServiceManifestImport>
```

## <a name="configure-isolation-mode"></a>Konfigurace režimu izolace
Systém Windows podporuje pro kontejnery dva režimy izolace: procesy a Hyper-V. V režimu izolace procesů všechny kontejnery spuštěné na stejném hostitelském počítači sdílejí jádro s hostitelem. V režimu izolace Hyper-V se jádra pro jednotlivé kontejnery Hyper-V a hostitele kontejneru izolují. Režim izolace určuje element `ContainerHostPolicies` v souboru manifestu aplikace. Je možné zadat tyto režimy izolace: `process`, `hyperv` a `default`. Výchozím režimem izolace pro hostitele se systémem Windows Server je režim `process`. Výchozím režimem u hostitelů se systémem Windows 10 je `hyperv`. Následující fragment kódu ukazuje, jakým způsobem je režim izolace určený v souboru manifestu aplikace.

```xml
<ContainerHostPolicies CodePackageRef="Code" Isolation="hyperv">
```
   > [!NOTE]
   > Režim izolace hyperv je k dispozici ve skladových položkách Azure Ev3 a Dv3 s podporou vnořené virtualizace. 
   >
   >

## <a name="configure-resource-governance"></a>Konfigurace zásad správného řízení prostředků
[Zásady správného řízení prostředků](service-fabric-resource-governance.md) omezují prostředky, které kontejner může použít na hostiteli. Element `ResourceGovernancePolicy`, který je zadaný v manifestu aplikace, slouží k deklaraci omezení prostředků pro balíček kódu služby. Omezení prostředků je možné nastavit pro tyto prostředky: Memory, MemorySwap, CpuShares (relativní váha CPU), MemoryReservationInMB, BlkioWeight (relativní váha BlockIO).  V tomto příkladu balíček služby Guest1Pkg získá jedno jádro na uzlech clusteru, kde je umístěný.  Omezení paměti jsou absolutní, takže balíček kódu je omezený na 1024 MB paměti (a má tuto paměť softwarově vyhrazenou). Balíčky kódu (kontejnery a procesy) nejsou schopné přidělit víc paměti, než je toto omezení, a případný pokus o takové přidělení má za následek výjimku z důvodu nedostatku paměti. Aby vynucení omezení prostředků fungovala, musí být omezení paměti zadaná pro všechny balíčky kódu v rámci balíčku služby.

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="Guest1Pkg" ServiceManifestVersion="1.0.0" />
  <Policies>
    <ServicePackageResourceGovernancePolicy CpuCores="1"/>
    <ResourceGovernancePolicy CodePackageRef="Code" MemoryInMB="1024"  />
  </Policies>
</ServiceManifestImport>
```
## <a name="configure-docker-healthcheck"></a>Konfigurace dockeru HEALTHCHECK 

Počínaje v6.1 Service Fabric automaticky integruje události [dockeru HEALTHCHECK](https://docs.docker.com/engine/reference/builder/#healthcheck) do sestavy stavu systému. To znamená, že pokud váš kontejner má **HEALTHCHECK** povolený, Service Fabric oznámí stav vždy, když se změní stav kontejneru (nahlášený Dockerem). Pokud *health_status* je *healthy*, v [Service Fabric Exploreru](service-fabric-visualizing-your-cluster.md) se zobrazí sestava stavu **OK**. Pokud *health_status* je *unhealthy*, zobrazí se **UPOZORNĚNÍ**. Pokyn **HEALTHCHECK** odkazující na aktuální kontrolu, která se provede pro monitorování stavu kontejneru, musí být uvedený v souboru **dockerfile** použitém při generování image kontejneru. 

![HealthCheckHealthy][3]

![HealthCheckUnealthyApp][4]

![HealthCheckUnhealthyDsp][5]

Chování **HEALTHCHECK** pro jednotlivé kontejnery můžete nakonfigurovat zadáním možností **HealthConfig** jako součásti **ContainerHostPolicies** v manifestu aplikace.

```xml
<ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="ContainerServicePkg" ServiceManifestVersion="2.0.0" />
    <Policies>
      <ContainerHostPolicies CodePackageRef="Code">
        <HealthConfig IncludeDockerHealthStatusInSystemHealthReport="true" RestartContainerOnUnhealthyDockerHealthStatus="false" />
      </ContainerHostPolicies>
    </Policies>
</ServiceManifestImport>
```
Ve výchozím nastavení se *IncludeDockerHealthStatusInSystemHealthReport* nastaví na **true** a *RestartContainerOnUnhealthyDockerHealthStatus* se nastaví na **false**. Pokud je pro *RestartContainerOnUnhealthyDockerHealthStatus* nastavená hodnota **true**, kontejner, který je opakovaně nahlášený ve špatném stavu, se restartuje (potenciálně na jiných uzlech).

Pokud chcete zakázat integraci **HEALTHCHECK** pro celý cluster Service Fabric, musíte nastavit [EnableDockerHealthCheckIntegration](service-fabric-cluster-fabric-settings.md) na **false**.

## <a name="deploy-the-container-application"></a>Nasazení aplikace typu kontejner
Uložte všechny provedené změny a sestavte aplikaci. Pokud chcete aplikaci publikovat, klikněte pravým tlačítkem na **MyFirstContainer** v Průzkumníku řešení a vyberte **Publikovat**.

Do pole **Koncový bod připojení** zadejte koncový bod správy pro příslušný cluster.  Příklad: containercluster.westus2.cloudapp.azure.com:19000. Koncový bod připojení ke klientu najdete v okně Přehled pro váš cluster na webu [Azure Portal](https://portal.azure.com).

Klikněte na **Publikovat**.

[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) je webový nástroj pro kontrolu a správu aplikací a uzlů v clusteru Service Fabric. Otevřete prohlížeč, přejděte na adresu http://containercluster.westus2.cloudapp.azure.com:19080/Explorer/ a sledujte nasazení aplikace.  Aplikace se nasadí, ale bude v chybovém stavu, dokud se image nestáhne na uzlech clusteru (což v závislosti na velikosti image může nějakou dobu trvat): ![Chyba][1]

Aplikace je připravena, když je ve stavu ```Ready```: ![Připraveno][2]

Otevřete prohlížeč a přejděte na adresu http://containercluster.westus2.cloudapp.azure.com:8081. V prohlížeči by se měl zobrazit nadpis „Hello World!“.

## <a name="clean-up"></a>Vyčištění
Za spuštěný cluster se vám stále účtují poplatky, proto zvažte [odstranění clusteru](service-fabric-cluster-delete.md).  [Party clustery](https://try.servicefabric.azure.com/) se automaticky odstraní po několika hodinách.

Po nahrání image do registru kontejneru můžete odstranit místní image z vývojového počítače:

```
docker rmi helloworldapp
docker rmi myregistry.azurecr.io/samples/helloworldapp
```

## <a name="specify-os-build-version-specific-container-images"></a>Zadání imagí kontejneru pro konkrétní verze sestavení operačního systému 

Kontejnery Windows Serveru (režim izolace procesů) nemusí být kompatibilní s novějšími verzemi operačního systému. Například kontejnery Windows Serveru sestavené s využitím Windows Serveru 2016 nefungují na Windows Serveru verze 1709. Proto se může stát, že pokud jsou uzly clusteru aktualizované na nejnovější verzi, služby kontejneru sestavené s využitím dřívějších verzí operačního systému můžou selhat. Aby bylo možné se této situaci při použití modulu runtime verze 6.1 a novější vyhnout, Service Fabric podporuje zadávání několika imagí operačního systému pro jeden kontejner a jejich označení pomocí verzí sestavení operačního systému (získané spuštěním `winver` v příkazovém řádku Windows).  Doporučuje se nejdřív aktualizovat manifesty aplikací a zadat přepsání image pro jednotlivé verze operačního systému a teprve potom aktualizovat operační systém na uzlech. Následující fragment kódu ukazuje, jak v manifestu aplikace **ApplicationManifest.xml** zadat několik imagí kontejneru:


```xml
<ContainerHostPolicies> 
         <ImageOverrides> 
               <Image Name="myregistry.azurecr.io/samples/helloworldapp1701" Os="14393" /> 
               <Image Name="myregistry.azurecr.io/samples/helloworldapp1709" Os="16299" /> 
         </ImageOverrides> 
     </ContainerHostPolicies> 
```
Verze sestavení pro Windows Server 2016 je 14393 a verze sestavení pro Windows Server verze 1709 je 16299. Manifest služby i nadále určuje jenom jednu image pro každou službu kontejneru, jak ukazuje následující kód:

```xml
<ContainerHost>
    <ImageName>myregistry.azurecr.io/samples/helloworldapp</ImageName> 
</ContainerHost>
```

   > [!NOTE]
   > Funkce označování verze sestavení operačního systému jsou dostupné jenom pro Service Fabric ve Windows.
   >

Pokud základním operačním systémem virtuálního počítače je sestavení 16299 (verze 1709), Service Fabric vybere image kontejneru, která odpovídá této verzi Windows Serveru.  Pokud manifest aplikace obsahuje kromě označených imagí kontejneru také neoznačenou image, Service Fabric bude předpokládat, že tato neoznačená image funguje napříč verzemi. Doporučuje se image kontejneru explicitně označovat.


## <a name="complete-example-service-fabric-application-and-service-manifests"></a>Kompletní příklad manifestů služby a aplikace Service Fabric
Tady jsou kompletní manifesty aplikace a služby použité v tomto článku.

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
    <EnvironmentVariables>
      <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
      <EnvironmentVariable Name="BackendServiceName" Value=""/>
    </EnvironmentVariables>

  </CodePackage>

  <!-- Config package is the contents of the Config directoy under PackageRoot that contains an
       independently-updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to
           listen. Please note that if your service is partitioned, this port is shared with
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="Guest1TypeEndpoint" UriScheme="http" Port="8081" Protocol="http"/>
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
    <EnvironmentOverrides CodePackageRef="FrontendService.Code">
      <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
    </EnvironmentOverrides>
    <ConfigOverrides />
    <Policies>
      <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="MIIB6QYJKoZIhvcNAQcDoIIB2jCCAdYCAQAxggFRMIIBTQIBADA1MCExHzAdBgNVBAMMFnJ5YW53aWRhdGFlbmNpcGhlcm1lbnQCEFfyjOX/17S6RIoSjA6UZ1QwDQYJKoZIhvcNAQEHMAAEg
gEAS7oqxvoz8i6+8zULhDzFpBpOTLU+c2mhBdqXpkLwVfcmWUNA82rEWG57Vl1jZXe7J9BkW9ly4xhU8BbARkZHLEuKqg0saTrTHsMBQ6KMQDotSdU8m8Y2BR5Y100wRjvVx3y5+iNYuy/JmM
gSrNyyMQ/45HfMuVb5B4rwnuP8PAkXNT9VLbPeqAfxsMkYg+vGCDEtd8m+bX/7Xgp/kfwxymOuUCrq/YmSwe9QTG3pBri7Hq1K3zEpX4FH/7W2Zb4o3fBAQ+FuxH4nFjFNoYG29inL0bKEcTX
yNZNKrvhdM3n1Uk/8W2Hr62FQ33HgeFR1yxQjLsUu800PrYcR5tLfyTB8BgkqhkiG9w0BBwEwHQYJYIZIAWUDBAEqBBBybgM5NUV8BeetUbMR8mJhgFBrVSUsnp9B8RyebmtgU36dZiSObDsI
NtTvlzhk11LIlae/5kjPv95r3lw6DHmV4kXLwiCNlcWPYIWBGIuspwyG+28EWSrHmN7Dt2WqEWqeNQ==" PasswordEncrypted="true"/>
        <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
      </ContainerHostPolicies>
      <ServicePackageResourceGovernancePolicy CpuCores="1"/>
      <ResourceGovernancePolicy CodePackageRef="Code" MemoryInMB="1024"  />
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

## <a name="configure-time-interval-before-container-is-force-terminated"></a>Konfigurace časového intervalu před vynuceným ukončením kontejneru

Můžete nakonfigurovat časový interval, který určuje, jak dlouho modul runtime počká před odebráním kontejneru po zahájení odstraňování služby (nebo jejího přesunu do jiného uzlu). Konfigurací časového intervalu se do kontejneru odešle příkaz `docker stop <time in seconds>`.   Další podrobnosti najdete v dokumentaci k příkazu [docker stop](https://docs.docker.com/engine/reference/commandline/stop/). Časový interval pro čekání se zadává v části `Hosting`. Následující fragment manifestu clusteru ukazuje nastavení intervalu čekání:

```json
{
        "name": "Hosting",
        "parameters": [
          {
                "name": "ContainerDeactivationTimeout",
                "value" : "10"
          },
          ...
        ]
}
```
Výchozí časový interval je nastavený na 10 sekund. Vzhledem k tomu, že je tato konfigurace dynamická, časový limit aktualizuje v clusteru upgrade pouze konfigurace. 


## <a name="configure-the-runtime-to-remove-unused-container-images"></a>Konfigurace modulu runtime pro odebrání nepoužívaných imagí kontejneru

Cluster Service Fabric můžete nakonfigurovat tak, aby z uzlu odebral nepoužívané image kontejneru. Tato konfigurace umožňuje znovu získat místo na disku v případě, že je na uzlu příliš mnoho imagí kontejneru.  Pokud chcete tuto funkci povolit, aktualizujte část `Hosting` v manifestu clusteru, jak je znázorněno v následujícím fragmentu kódu: 


```json
{
        "name": "Hosting",
        "parameters": [
          {
                "name": "PruneContainerImages",
                "value": "True"
          },
          {
                "name": "ContainerImagesToSkip",
                "value": "microsoft/windowsservercore|microsoft/nanoserver|microsoft/dotnet-frameworku|..."
          }
          ...
          }
        ]
} 
```

Image, které se nesmí odstranit, můžete zadat v rámci parametru `ContainerImagesToSkip`. 


## <a name="configure-container-image-download-time"></a>Konfigurace doby stahování image kontejneru

Ve výchozím nastavení modul runtime Service Fabric pro stažení a extrakci imagí kontejneru přidělí 20 minut. Pro většinu imagí kontejnerů to stačí. U větších imagí nebo při pomalém síťovém připojení může být potřeba prodloužit čas, po který se čeká, než dojde ke zrušení stahování a extrakce imagí. Můžete k tomu použít atribut **ContainerImageDownloadTimeout** v části **Hosting** manifestu clusteru, jak ukazuje následující fragment kódu:

```json
{
"name": "Hosting",
        "parameters": [
          {
              "name": " ContainerImageDownloadTimeout ",
              "value": "1200"
          }
]
}
```


## <a name="set-container-retention-policy"></a>Nastavení zásad uchovávání informací kontejneru

Jako pomoc s diagnostikou selhání spuštění kontejneru Service Fabric (verze 6.1 nebo vyšší) podporuje zachování kontejnerů, které se ukončily nebo které se nepovedlo spustit. Tuto zásadu je možné nastavit v souboru **ApplicationManifest.xml**, jak ukazuje následující fragment kódu:

```xml
 <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="process" ContainersRetentionCount="2"  RunInteractive="true"> 
```

Nastavení **ContainersRetentionCount** určuje počet kontejnerů, které se při svém selhání zachovají. Pokud je zadaná hodnota záporná, zachovají se všechny kontejnery, které selhaly. Když atribut **ContainersRetentionCount** není zadaný, nezachovají se žádné kontejnery. Atribut **ContainersRetentionCount** také podporuje parametry aplikace, takže uživatelé mohou zadat různé hodnoty pro testovací a produkční clustery. Při použití této funkce se doporučuje použít omezení umístění a cílit službu kontejneru na konkrétní uzel. Zabrání se tak přesunu služby kontejneru na jiné uzly. Všechny kontejnery zachované pomocí této funkce je nutné ručně odebrat.


## <a name="next-steps"></a>Další kroky
* Další informace o spouštění [kontejnerů v Service Fabric](service-fabric-containers-overview.md).
* Přečtěte si kurz [Nasazení aplikace .NET v kontejneru](service-fabric-host-app-in-a-container.md).
* Informace o [životním cyklu aplikace](service-fabric-application-lifecycle.md) Service Fabric.
* Prohlédněte si [ukázky kódu kontejneru Service Fabric](https://github.com/Azure-Samples/service-fabric-containers) na GitHubu.

[1]: ./media/service-fabric-get-started-containers/MyFirstContainerError.png
[2]: ./media/service-fabric-get-started-containers/MyFirstContainerReady.png
[3]: ./media/service-fabric-get-started-containers/HealthCheckHealthy.png
[4]: ./media/service-fabric-get-started-containers/HealthCheckUnhealthy_App.png
[5]: ./media/service-fabric-get-started-containers/HealthCheckUnhealthy_Dsp.png
