---
title: "Nastavení vývojového prostředí v Mac OS X pro práci s Azure Service Fabric | Dokumentace Microsoftu"
description: "Nainstalujte modul runtime, sadu SDK a nástroje a vytvořte místní vývojový cluster. Po dokončení této instalace a nastavení budete moci sestavovat aplikace v Mac OS X."
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: 
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/31/2017
ms.author: saysa
ms.openlocfilehash: cf67c377cd5c17f7b540fb23af48a333a9cddf69
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2017
---
# <a name="set-up-your-development-environment-on-mac-os-x"></a>Nastavení vývojového prostředí v Mac OS X
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md)
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
>
>  

Pomocí Mac OS X můžete sestavit aplikace Service Fabric, které poběží na clusterech s Linuxem. Tento článek popisuje nastavení počítače Mac pro vývoj.

## <a name="prerequisites"></a>Požadavky
Service Fabric nefunguje v OS X nativně. Pro spuštění místního clusteru Service Fabric poskytujeme předkonfigurovanou image kontejneru Dockeru. Než začnete, budete potřebovat:

* Minimálně 4 GB RAM
* Nejnovější verzi [Dockeru](https://www.docker.com/)
* Přístup k [imagi](https://hub.docker.com/r/servicefabricoss/service-fabric-onebox/) kontejneru Dockeru pro Service Fabric One-box

>[!TIP]
> * Docker můžete na svém počítači Mac nainstalovat podle postupu uvedeného v oficiální [dokumentaci](https://docs.docker.com/docker-for-mac/install/#what-to-know-before-you-install) k Dockeru. 
> * Po dokončení instalace ověřte, že proběhla úspěšně, podle postupu uvedeného [tady](https://docs.docker.com/docker-for-mac/#check-versions-of-docker-engine-compose-and-machine).


## <a name="create-a-local-container-and-setup-service-fabric"></a>Vytvoření místního kontejneru a nastavení Service Fabric
Pokud chcete nastavit místní kontejner Dockeru a mít v něm spuštěný cluster Service Fabric, proveďte následující kroky:

1. Stáhněte si image z úložiště Docker Hub:

    ```bash
    docker pull servicefabricoss/service-fabric-onebox
    ```

2. Spusťte instanci kontejneru Service Fabric One-box s touto imagí:

    ```bash
    docker run -itd -p 19080:19080 --name sfonebox servicefabricoss/service-fabric-onebox
    ```
    >[!TIP]
    >Pokud zadáte název instance kontejneru, můžete s ní pracovat srozumitelněji. 

3. Přihlaste se ke kontejneru Dockeru v interaktivním režimu SSH:

    ```bash
    docker exec -it sfonebox bash
    ```

4. Spusťte instalační skript, který načte požadované závislosti a pak v kontejneru spustí cluster.

    ```bash
    ./setup.sh     # Fetches and installs the dependencies required for Service Fabric to run
    ./run.sh       # Starts the local cluster
    ```

5. Po úspěšném dokončení kroku 4 můžete na svém počítači Mac přejít na adresu ``http://localhoist:19080``, kde by se měl zobrazit Service Fabric Explorer.


## <a name="set-up-the-service-fabric-cli-sfctl-on-your-mac"></a>Nastavení Service Fabric CLI (sfctl) na počítači Mac

Pokud chcete nainstalovat Service Fabric CLI (`sfctl`) na svém počítači Mac, postupujte podle pokynů v tématu [Service Fabric CLI](service-fabric-cli.md#cli-mac).
Příkazy rozhraní příkazového řádku pro komunikaci s entitami Service Fabric, včetně clusterů, aplikací a služeb.

## <a name="create-application-on-your-mac-using-yeoman"></a>Vytvoření aplikace na počítači Mac pomocí Yeomana

Service Fabric nabízí nástroje pro generování uživatelského rozhraní, které vám pomůžou vytvořit aplikaci Service Fabric z terminálu pomocí generátoru šablon Yeoman. Postupujte podle následujících kroků, abyste zkontrolovali, že máte na svém počítači funkční generátor šablon Service Fabric Yeoman.

1. Na počítači Mac musí být nainstalováno Node.js a NPM. Pokud ne, můžete nainstalovat Node.js a NPM pomocí Homebrew následujícím způsobem. Pokud chcete zkontrolovat verze Node.js a NPM nainstalované na počítači Mac, můžete použít možnost ``-v``.

    ```bash
    brew install node
    node -v
    npm -v
    ```
2. Nainstalujte na svém počítači generátor šablon [Yeoman](http://yeoman.io/) z NPM.

    ```bash
    npm install -g yo
    ```
3. Nainstalujte generátor Yeoman, který chcete použít, podle kroků v úvodní [dokumentaci](service-fabric-get-started-linux.md). Pokud chcete vytvářet aplikace Service Fabric pomocí Yeomana, postupujte podle těchto kroků:

    ```bash
    npm install -g generator-azuresfjava       # for Service Fabric Java Applications
    npm install -g generator-azuresfguest      # for Service Fabric Guest executables
    npm install -g generator-azuresfcontainer  # for Service Fabric Container Applications
    ```
4. K sestavení aplikace Service Fabric Java na hostitelském počítači Mac budete potřebovat nainstalovanou sadu JDK 1.8 a Gradle. Pokud tam ještě nejsou, můžete je nainstalovat pomocí [HomeBrew](https://brew.sh/). 

    ```bash
    brew update
    brew cask install java
    brew install gradle
    ```

## <a name="deploy-application-on-your-mac-from-terminal"></a>Nasazení aplikace na počítači Mac z terminálu

Po vytvoření a sestavení aplikace Service Fabric ji můžete nasadit pomocí [Service Fabric CLI](service-fabric-cli.md#cli-mac), a to následujícím postupem:

1. Připojte se ke clusteru Service Fabric spuštěnému uvnitř instance kontejneru na vašem počítači Mac.

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```

2. Přejděte do adresáře vašeho projektu a spusťte instalační skript.

    ```bash
    cd MyProject
    bash install.sh
    ```

## <a name="set-up-net-core-20-development"></a>Nastavení pro vývoj v .NET Core 2.0

Pokud chcete začít [vytvářet aplikace Service Fabric v jazyce C#](service-fabric-create-your-first-linux-application-with-csharp.md), nainstalujte sadu [.NET Core 2.0 SDK pro Mac](https://www.microsoft.com/net/core#macos). Balíčky pro aplikace Service Fabric v .NET Core 2.0 jsou hostované na NuGet.org a aktuálně ve verzi Preview.

## <a name="install-the-service-fabric-plugin-for-eclipse-neon-on-your-mac"></a>Instalace modulu plug-in Service Fabric pro Eclipse Neon na počítači Mac

Service Fabric poskytuje modul plug-in pro **integrovaná vývojové prostředí Eclipse Neon pro Javu**, který může zjednodušit proces vytváření, sestavování a nasazování služeb v Javě. Můžete provést kroky instalace uvedené v této obecné [dokumentaci](service-fabric-get-started-eclipse.md#install-or-update-the-service-fabric-plug-in-in-eclipse-neon) týkající se instalace nebo aktualizace modulu plug-in Service Fabric Eclipse na nejnovější verzi.

Všechny další kroky uvedené v [dokumentaci k Service Fabric Eclipse](service-fabric-get-started-eclipse.md) pro sestavení aplikace, přidání služby jako aplikace, instalace/odinstalace aplikace atd. budou použitelné i tady.

Kromě výše uvedených kroků musíte pro zajištění spolupráce modulu plug-in Service Fabric Eclipse s kontejnerem Dockeru na počítači Mac vytvořit instanci kontejneru s cestou sdílenou s vaším hostitelem, a to následujícím způsobem:
```bash
docker run -itd -p 19080:19080 -v /Users/sayantan/work/workspaces/mySFWorkspace:/tmp/mySFWorkspace --name sfonebox servicefabricoss/service-fabric-onebox
```
kde ``/Users/sayantan/work/workspaces/mySFWorkspace`` je absolutní cesta pracovního prostoru na počítači Mac a ``/tmp/mySFWorkspace`` je cesta uvnitř kontejneru, na kterou se namapuje.

> [!NOTE]
>1. Pokud se název nebo cesta vašeho pracovního prostoru liší, aktualizujte je odpovídajícím způsobem ve výše uvedeném příkazu ``docker run``.
>2. Pokud spouštíte kontejner s jiným názvem než ``sfonebox``, aktualizujte název v souboru ``testclient.sh`` ve vaší aplikaci Service Fabric Actor v Javě.

## <a name="next-steps"></a>Další kroky
<!-- Links -->
* [Vytvoření a nasazení první aplikace Service Fabric v Javě v Linuxu pomocí Yeomana](service-fabric-create-your-first-linux-application-with-java.md)
* [Vytvoření a nasazení první aplikace Service Fabric v Javě v Linuxu pomocí modulu plug-in Service Fabric pro Eclipse](service-fabric-get-started-eclipse.md)
* [Vytvoření clusteru služby Service Fabric na webu Azure Portal](service-fabric-cluster-creation-via-portal.md)
* [Vytvoření clusteru služby Service Fabric pomocí Azure Resource Manageru](service-fabric-cluster-creation-via-arm.md)
* [Pochopení aplikačního modelu služby Service Fabric](service-fabric-application-model.md)
* [Správa aplikací pomocí Service Fabric CLI](service-fabric-application-lifecycle-sfctl.md)

<!-- Images -->
[cluster-setup-script]: ./media/service-fabric-get-started-mac/cluster-setup-mac.png
[sfx-mac]: ./media/service-fabric-get-started-mac/sfx-mac.png
[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship
