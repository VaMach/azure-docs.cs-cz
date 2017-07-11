---
title: "Nastavení vývojového prostředí v Linuxu | Dokumentace Microsoftu"
description: "Nainstalujte modul runtime a sadu SDK a vytvořte místní vývojový cluster v Linuxu. Po dokončení této instalace a nastavení budete moci sestavovat aplikace."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 6/28/2017
ms.author: subramar
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: 5840932d79ef9fd99a94eb6ae4e587b0e616065e
ms.contentlocale: cs-cz
ms.lasthandoff: 07/01/2017


---
<a id="prepare-your-development-environment-on-linux" class="xliff"></a>

# Příprava vývojového prostředí v Linuxu
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md)
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
>
>  

 Pokud chcete sestavovat a spouštět [aplikace Azure Service Fabric](service-fabric-application-model.md) na vývojovém počítači s Linuxem, musíte nainstalovat modul runtime a běžnou sadu SDK. Můžete také nainstalovat volitelné sady SDK pro Javu a .NET Core.

<a id="prerequisites" class="xliff"></a>

## Požadavky

<a id="supported-operating-system-versions" class="xliff"></a>

### Podporované verze operačních systémů
Pro vývoj jsou podporovány tyto verze operačních systémů:

* Ubuntu 16.04 (`Xenial Xerus`)

<a id="update-your-apt-sources" class="xliff"></a>

## Aktualizace zdrojů apt
Chcete-li nainstalovat sadu SDK a přidružený balíček modulu runtime pomocí apt-get, musíte nejprve aktualizovat zdroje apt.

1. Otevřete terminál.
2. Přidejte do seznamu zdrojů úložiště Service Fabric.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/servicefabric/ trusty main" > /etc/apt/sources.list.d/servicefabric.list'
    ```

3. Přidejte do seznamu zdrojů úložiště `dotnet`.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
    ```

4. Přidejte nový klíč GPG do své klíčenky apt.

    ```bash
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
    ```

5. Přidejte do své klíčenky apt oficiální klíč GPG Dockeru.

    ```bash
    sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    ```

6. Nastavte úložiště Dockeru.

    ```bash
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    ```

7. Obnovte seznamy balíčků na základě nově přidaných úložišť.

    ```bash
    sudo apt-get update
    ```

<a id="install-and-set-up-the-sdk-for-containers-and-guest-executables" class="xliff"></a>

## Instalace a nastavení sady SDK pro kontejnery a spustitelné soubory hosta

Jakmile jsou vaše zdroje aktualizované, můžete nainstalovat sadu SDK.

1. Nainstalujte balíček Service Fabric SDK. Zobrazí se výzva k potvrzení instalace a vyjádření souhlasu s licenční smlouvou.

    ```bash
    sudo apt-get install servicefabricsdkcommon
    ```

    >   [!TIP]
    >   Následující příkazy automatizují přijetí licence pro balíčky Service Fabric:
    >   ```bash
    >   echo "servicefabric servicefabric/accepted-eula-v1 select true" | debconf-set-selections
    >   echo "servicefabricsdkcommon servicefabricsdkcommon/accepted-eula-v1 select true" | debconf-set-selections
    >   ```
    
2. Spusťte instalační skript sady SDK.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/sdkcommonsetup.sh
    ```

Po spuštění kroků instalace balíčku běžné sady SDK by mělo být vytváření aplikací se službami spustitelného souboru hosta nebo kontejneru možné spuštěním příkazu `yo azuresfguest` nebo `yo azuresfcontainer`. Může být potřeba nastavit vaši proměnnou prostředí **$NODE_PATH** na umístění, kde jsou umístěné na moduly uzlu. 


```bash
    export NODE_PATH=$NODE_PATH:$HOME/.node/lib/node_modules 
```

Pokud používáte prostředí jako uživatel root, může být potřeba nastavit proměnnou pomocí následujícího příkazu:

```bash
    export NODE_PATH=$NODE_PATH:/root/.node/lib/node_modules 
```


> [!TIP]
> Můžete chtít přidat tyto příkazy do souboru ~/.bashrc, aby nebylo potřeba nastavovat proměnnou prostředí při každém přihlášení.
>

<a id="setup-the-xplat-service-fabric-cli" class="xliff"></a>

## Nastavení rozhraní příkazového řádku Service Fabric XPlat
[Rozhraní příkazového řádku XPlat][azure-xplat-cli-github] obsahuje příkazy pro komunikaci s entitami služby Service Fabric, včetně clusterů a aplikací. Je založené na Node.js, takže [se ujistěte, že máte Node nainstalované][install-node] před tím, než budete pokračovat v následujících pokynech:

1. Naklonujte úložiště GitHub do svého vývojového počítače.

    ```bash
    git clone https://github.com/Azure/azure-xplat-cli.git
    ```

2. Přepněte do naklonovaného úložiště a nainstalujte závislosti rozhraní příkazového řádku pomocí Node Package Manageru (npm).

    ```bash
    cd azure-xplat-cli
    npm install
    ```

3. Vytvořte symlink ze složky `bin/azure` naklonovaného úložiště do složky `/usr/bin/azure`.

    ```bash
    sudo ln -s $(pwd)/bin/azure /usr/bin/azure
    ```

4. Nakonec povolte automatické dokončování příkazů služby Service Fabric.

    ```bash
    azure --completion >> ~/azure.completion.sh
    echo 'source ~/azure.completion.sh' >> ~/.bash_profile
    source ~/azure.completion.sh
    ```

<a id="setup-azure-cli-20" class="xliff"></a>

### Nastavení Azure CLI 2.0

Jako alternativu rozhraní XPlat CLI nyní rozhraní Azure CLI zahrnuje příkazový modul Service Fabric.

Další informace o instalaci Azure CLI 2.0 a použití příkazů Service Fabric najdete v [úvodní dokumentaci](service-fabric-azure-cli-2-0.md).

<a id="set-up-a-local-cluster" class="xliff"></a>

## Nastavení místního clusteru
Pokud se všechno úspěšně nainstalovalo, měli byste být schopni spustit místní cluster.

1. Spusťte instalační skript clusteru.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```

2. Otevřete webový prohlížeč a přejděte na adresu http://localhost:19080/Explorer. Pokud se cluster spustil, měl by se zobrazit řídicí panel Service Fabric Exploreru.

    ![Service Fabric Explorer v Linuxu][sfx-linux]

V tuto chvíli můžete nasadit předem sestavené balíčky aplikací Service Fabric nebo nové balíčky založené na kontejnerech nebo spustitelných souborech hostů. Pokud chcete sestavit nové služby pomocí sad Java SDK nebo .NET Core SDK, postupujte podle pokynů k instalaci uvedených v dalších částech.


> [!NOTE]
> Samostatné clustery se v Linuxu nepodporují – ve verzi Preview se podporují pouze clustery s jedním počítačem a clustery s více počítači Azure s Linuxem.
>

<a id="install-the-java-sdk-optional-if-you-wish-to-use-the-java-programming-models" class="xliff"></a>

## Instalace sady Java SDK (volitelné, pokud chcete použít programovací modely jazyka Java)
Sada Java SDK poskytuje knihovny a šablony potřebné k sestavení služeb Service Fabric pomocí Javy.

1. Nainstalujte balíček Java SDK.

    ```bash
    sudo apt-get install servicefabricsdkjava
    ```

2. Spusťte instalační skript sady SDK.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/java/sdkjavasetup.sh
    ```

<a id="install-the-eclipse-neon-plugin-optional" class="xliff"></a>

## Instalace modulu plug-in Eclipse Neon (volitelné)

Modul plug-in Eclipse pro Service Fabric můžete nainstalovat z **Eclipse IDE pro vývojáře Java**. Mimo aplikací Service Fabric v Javě můžete k vytvoření aplikací spustitelných souborů hosta a aplikací kontejneru Service Fabric použít Eclipse.

> [!NOTE]
> Sada Java SDK je předpokladem používání modulu plug-in Eclipse, i když ho používáte jenom pro aplikace spustitelných souborů hosta a kontejnerů.
>

1. V prostředí Eclipse se ujistěte, že máte nainstalované nejnovější prostředí Eclipse **Neon** a nejnovější verzi Buildship (1.0.17 nebo novější). Verze nainstalovaných komponent můžete zkontrolovat tak, že zvolíte **Nápověda > Podrobnosti o instalaci**. Buildship můžete aktualizovat pomocí pokynů [zde][buildship-update].
2. Chcete-li nainstalovat modul plug-in Service Fabric, zvolte **Nápověda > Instalace nového softwaru**.
3. Do textového pole Work with (Pracovat s) zadejte http://dl.microsoft.com/eclipse.
4. Klikněte na Přidat.

    ![Modul plug-in Eclipse][sf-eclipse-plugin]

5. Zvolte modul plug-in služby Service Fabric a klikněte na **Další**.
6. Pokračujte v instalaci a přijměte licenční smlouvu pro koncového zákazníka.

Pokud už máte modul plug-in Service Fabric Eclipse nainstalovaný, ověřte, že používáte nejnovější verzi. Kontrolu můžete provést výběrem ``Help => Installation Details`` a vyhledáním Service Fabric v seznamu nainstalovaných modulů plug-in. Pokud je k dispozici novější verze, vyberte aktualizaci.

Další informace najdete v tématu [Service Fabric: Začínáme s Eclipsem](service-fabric-get-started-eclipse.md).


<a id="install-the-net-core-sdk-optional-if-you-wish-to-use-the-net-core-programming-models" class="xliff"></a>

## Instalace sady .NET Core SDK (volitelné, pokud chcete použít programovací modely .NET Core)
Sada .NET Core SDK poskytuje knihovny a šablony potřebné k sestavení služeb Service Fabric pomocí .NET Core.

1. Nainstalujte balíček .NET Core SDK.

   ```bash
   sudo apt-get install servicefabricsdkcsharp
   ```

2. Spusťte instalační skript sady SDK.

   ```bash
   sudo /opt/microsoft/sdk/servicefabric/csharp/sdkcsharpsetup.sh
   ```

<a id="updating-the-sdk-and-runtime" class="xliff"></a>

## Aktualizace sady SDK a modulu runtime

Pokud chcete aktualizovat sadu SDK a modul runtime na nejnovější verze, spusťte následující příkazy (odeberte sady SDK, které nechcete použít):

```bash
sudo apt-get update
sudo apt-get install servicefabric servicefabricsdkcommon servicefabricsdkcsharp servicefabricsdkjava
```


> [!NOTE]
> Aktualizace balíčků může způsobit zastavení místního vývojového clusteru. Po provedení upgradu restartujte místní cluster podle pokynů na této stránce.

<a id="next-steps" class="xliff"></a>

## Další kroky
* [Vytvoření a nasazení první aplikace Service Fabric v Javě v Linuxu pomocí Yeomana](service-fabric-create-your-first-linux-application-with-java.md)
* [Vytvoření a nasazení první aplikace Service Fabric v Javě v Linuxu pomocí modulu plug-in Service Fabric pro Eclipse](service-fabric-get-started-eclipse.md)
* [Vytvoření první aplikace v CSharp v Linuxu](service-fabric-create-your-first-linux-application-with-csharp.md)
* [Příprava vývojového prostředí v OSX](service-fabric-get-started-mac.md)
* [Správa aplikací Service Fabric pomocí XPlat CLI](service-fabric-azure-cli.md)
* [Rozdíly Service Fabric pro Windows a Linux](service-fabric-linux-windows-differences.md)

<a id="related-articles" class="xliff"></a>

## Související články

* [Začínáme s platformou Service Fabric a Azure CLI 2.0](service-fabric-azure-cli-2-0.md)
* [Začínáme se Service Fabric XPlat CLI](service-fabric-azure-cli.md)

<!-- Links -->

[azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[install-node]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png

