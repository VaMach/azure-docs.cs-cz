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
ms.translationtype: HT
ms.sourcegitcommit: 54454e98a2c37736407bdac953fdfe74e9e24d37
ms.openlocfilehash: da6a8b4824d7215eb1db131680856ac04003f5aa
ms.contentlocale: cs-cz
ms.lasthandoff: 07/13/2017

---
# <a name="prepare-your-development-environment-on-linux"></a>Příprava vývojového prostředí v Linuxu
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md)
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
>
>  

Pokud chcete sestavovat a spouštět [aplikace Azure Service Fabric](service-fabric-application-model.md) na vývojovém počítači s Linuxem, musíte nainstalovat modul runtime a běžnou sadu SDK. Můžete také nainstalovat volitelné sady SDK pro Javu a .NET Core.

## <a name="prerequisites"></a>Požadavky

Pro vývoj jsou podporovány tyto verze operačních systémů:

* Ubuntu 16.04 (`Xenial Xerus`)

## <a name="update-your-apt-sources"></a>Aktualizace zdrojů APT
Pokud chcete nainstalovat sadu SDK a přidružený balíček modulu runtime pomocí nástroje pro příkazový řádek apt-get, musíte nejprve aktualizovat zdroje APT (Advanced Packaging Tool).

1. Otevřete terminál.
2. Přidejte do seznamu zdrojů úložiště Service Fabric.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/servicefabric/ trusty main" > /etc/apt/sources.list.d/servicefabric.list'
    ```

3. Přidejte do seznamu zdrojů úložiště `dotnet`.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
    ```

4. Přidejte do své klíčenky APT nový klíč GPG (Gnu Privacy Guard neboli GnuPG).

    ```bash
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
    ```

5. Přidejte do své klíčenky APT oficiální klíč GPG Dockeru.

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

## <a name="install-and-set-up-the-sdk-for-containers-and-guest-executables"></a>Instalace a nastavení sady SDK pro kontejnery a spustitelné soubory hosta

Po aktualizaci vašich zdrojů můžete nainstalovat sadu SDK.

1. Nainstalujte balíček sady Service Fabric SDK, potvrďte instalaci a vyjádřete souhlas s licenční smlouvou.

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

Po nainstalování balíčku běžné sady SDK byste měli být schopni vytvářet aplikace se službami spustitelného souboru hosta nebo kontejneru spuštěním příkazu `yo azuresfguest` nebo `yo azuresfcontainer`. Může být potřeba nastavit vaši proměnnou prostředí $NODE_PATH na umístění modulů uzlu. 


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

## <a name="set-up-the-xplat-service-fabric-cli"></a>Nastavení rozhraní příkazového řádku Service Fabric XPlat
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

### <a name="set-up-azure-cli-20"></a>Nastavení Azure CLI 2.0

Jako alternativu rozhraní XPlat CLI nyní rozhraní Azure CLI zahrnuje příkazový modul Service Fabric.

Další informace o instalaci Azure CLI 2.0 a používání příkazů Service Fabric najdete v tématu [Začínáme s platformou Service Fabric a Azure CLI 2.0](service-fabric-azure-cli-2-0.md).

## <a name="set-up-a-local-cluster"></a>Nastavení místního clusteru
Pokud je instalace úspěšná, měli byste být schopni spustit místní cluster.

1. Spusťte instalační skript clusteru.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```

2. Otevřete webový prohlížeč a přejděte do [Service Fabric Exploreru](http://localhost:19080/Explorer). Pokud se cluster spustil, měl by se zobrazit řídicí panel Service Fabric Exploreru.

    ![Service Fabric Explorer v Linuxu][sfx-linux]

V tuto chvíli můžete nasadit předem sestavené balíčky aplikací Service Fabric nebo nové balíčky založené na kontejnerech nebo spustitelných souborech hostů. Pokud chcete sestavit nové služby pomocí sad Java SDK nebo .NET Core SDK, postupujte podle pokynů k instalaci uvedených v dalších částech.


> [!NOTE]
> Samostatné clustery nejsou podporované v systému Linux. Verze Preview podporuje pouze clustery typu One-box a clustery Azure s více počítači s Linuxem.
>

## <a name="install-the-java-sdk-optional-if-you-want-to-use-the-java-programming-models"></a>Instalace sady Java SDK (volitelné, pokud chcete použít programovací modely jazyka Java)
Sada Java SDK poskytuje knihovny a šablony potřebné k sestavení služeb Service Fabric pomocí Javy.

1. Nainstalujte balíček Java SDK.

    ```bash
    sudo apt-get install servicefabricsdkjava
    ```

2. Spusťte instalační skript sady SDK.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/java/sdkjavasetup.sh
    ```

## <a name="install-the-eclipse-neon-plug-in-optional"></a>Instalace modulu plug-in Eclipse Neon (volitelné)

Modul plug-in Eclipse pro Service Fabric můžete nainstalovat z **integrovaného vývojového prostředí Eclipse pro vývojáře v jazyce Java**. Mimo aplikací Service Fabric v Javě můžete k vytvoření aplikací spustitelných souborů hosta a aplikací kontejneru Service Fabric použít Eclipse.

> [!NOTE]
> Sada Java SDK je předpokladem používání modulu plug-in Eclipse, i když ho používáte jenom pro aplikace spustitelných souborů hosta a kontejnerů.
>

1. V prostředí Eclipse se ujistěte, že máte nainstalované nejnovější prostředí Eclipse Neon a nejnovější verzi Buildship (1.0.17 nebo novější). Verze nainstalovaných komponent můžete zkontrolovat tak, že vyberete **Help** (Nápověda) > **Installation Details** (Podrobnosti o instalaci). Buildship můžete aktualizovat pomocí pokynů v článku [Eclipse Buildship: Eclipse Plug-ins for Gradle][buildship-update] (Eclipse Buildship: Moduly plug-in Eclipse pro Gradle).

2. Pokud chcete nainstalovat modul plug-in Service Fabric, vyberte **Help** (Nápověda) > **Install New Software** (Instalace nového softwaru).

3. V poli **Work with** (Pracovat s) zadejte **http://dl.microsoft.com/eclipse**.

4. Klikněte na **Přidat**.

    ![Stránka Available Software (Dostupný software)][sf-eclipse-plugin]

5. Vyberte modul plug-in **ServiceFabric** a potom klikněte na **Next** (Další).

6. Dokončete instalaci a potom vyjádřete souhlas s Licenční smlouvou s koncovým uživatelem.

Pokud už máte modul plug-in Service Fabric Eclipse nainstalovaný, ověřte, že používáte nejnovější verzi. Kontrolu můžete provést výběrem **Help** (Nápověda) > **Installation Details** (Podrobnosti o instalaci) a vyhledáním Service Fabric v seznamu nainstalovaných modulů plug-in. Pokud je k dispozici novější verze, vyberte **Update** (Aktualizovat).

Další informace najdete v tématu [Modul plug-in Service Fabric pro vývoj aplikací v Eclipse Javě](service-fabric-get-started-eclipse.md).


## <a name="install-the-net-core-sdk-optional-if-you-want-to-use-the-net-core-programming-models"></a>Instalace sady .NET Core SDK (volitelné, pokud chcete použít programovací modely .NET Core)
Sada .NET Core SDK poskytuje knihovny a šablony potřebné k sestavení služeb Service Fabric pomocí .NET Core.

1. Nainstalujte balíček .NET Core SDK.

   ```bash
   sudo apt-get install servicefabricsdkcsharp
   ```

2. Spusťte instalační skript sady SDK.

   ```bash
   sudo /opt/microsoft/sdk/servicefabric/csharp/sdkcsharpsetup.sh
   ```

## <a name="update-the-sdk-and-runtime"></a>Aktualizace sady SDK a modulu runtime

Pokud chcete aktualizovat sadu SDK a modul runtime na nejnovější verze, spusťte následující příkazy (odeberte sady SDK, které nechcete použít):

```bash
sudo apt-get update
sudo apt-get install servicefabric servicefabricsdkcommon servicefabricsdkcsharp servicefabricsdkjava
```


> [!NOTE]
> Aktualizace balíčků může způsobit zastavení místního vývojového clusteru. Po provedení upgradu restartujte místní cluster podle pokynů na této stránce.

## <a name="next-steps"></a>Další kroky
* [Vytvoření a nasazení první aplikace Service Fabric v Javě v Linuxu pomocí Yeomana](service-fabric-create-your-first-linux-application-with-java.md)
* [Vytvoření a nasazení první aplikace Service Fabric v Javě v Linuxu pomocí modulu plug-in Service Fabric pro Eclipse](service-fabric-get-started-eclipse.md)
* [Vytvoření první aplikace v CSharp v Linuxu](service-fabric-create-your-first-linux-application-with-csharp.md)
* [Příprava vývojového prostředí v OSX](service-fabric-get-started-mac.md)
* [Správa aplikací Service Fabric pomocí XPlat CLI](service-fabric-azure-cli.md)
* [Rozdíly Service Fabric pro Windows a Linux](service-fabric-linux-windows-differences.md)

## <a name="related-articles"></a>Související články

* [Začínáme s platformou Service Fabric a Azure CLI 2.0](service-fabric-azure-cli-2-0.md)
* [Začínáme se Service Fabric XPlat CLI](service-fabric-azure-cli.md)

<!-- Links -->

[azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[install-node]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png

