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
ms.date: 8/23/2017
ms.author: subramar
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: 4f51030446d2d2a5a11018b1fce7d7e9193f3dfc
ms.contentlocale: cs-cz
ms.lasthandoff: 08/24/2017

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
    sudo sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/servicefabric/ xenial main" > /etc/apt/sources.list.d/servicefabric.list'
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
    sudo apt-get install curl
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

## <a name="install-and-set-up-the-sdk-for-local-cluster-setup"></a>Instalace a nastavení sady SDK pro nastavení místního clusteru

Po aktualizaci vašich zdrojů můžete nainstalovat sadu SDK. Nainstalujte balíček sady Service Fabric SDK, potvrďte instalaci a vyjádřete souhlas s licenční smlouvou.

```bash
sudo apt-get install servicefabricsdkcommon
```

>   [!TIP]
>   Následující příkazy automatizují přijetí licence pro balíčky Service Fabric:
>   ```bash
>   echo "servicefabric servicefabric/accepted-eula-v1 select true" | sudo debconf-set-selections
>   echo "servicefabricsdkcommon servicefabricsdkcommon/accepted-eula-v1 select true" | sudo debconf-set-selections
>   ```

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

## <a name="set-up-the-service-fabric-cli"></a>Nastavení rozhraní příkazového řádku Service Fabric

[Rozhraní příkazového řádku Service Fabric](service-fabric-cli.md) obsahuje příkazy pro komunikaci s entitami služby Service Fabric, včetně clusterů a aplikací. Je založeno na Pythonu, takže je nutné mít nainstalovaný python a pip předtím, než budete pokračovat následujícím příkazem:

```bash
pip install sfctl
```

## <a name="install-and-set-up-the-generators-for-containers-and-guest-executables"></a>Instalace a nastavení generátorů pro kontejnery a spustitelné soubory hosta
Service Fabric nabízí nástroje pro generování uživatelského rozhraní, které vám pomůžou vytvořit aplikace Service Fabric z terminálu pomocí generátoru šablon Yeoman. Postupujte podle následujících kroků, abyste zkontrolovali, že máte na svém počítači funkční generátor šablon Service Fabric yeoman.

1. Instalace nodejs a NPM na počítači

  ```bash
  sudo apt-get install npm
  sudo apt install nodejs-legacy
  ```
2. Instalace generátoru šablon [Yeoman](http://yeoman.io/) na počítač z NPM

  ```bash
  sudo npm install -g yo
  ```
3. Instalace generátoru pro kontejnery a spustitelné soubory hosta Service Fabric Yeo z NPM

  ```bash
  sudo npm install -g generator-azuresfcontainer  # for Service Fabric container application
  sudo npm install -g generator-azuresfguest      # for Service Fabric guest executable application
  ```

Po nainstalování uvedených generátorů byste měli být schopni vytvářet aplikace se službami spustitelného souboru hosta nebo kontejneru spuštěním příkazu `yo azuresfguest` nebo `yo azuresfcontainer`.

## <a name="install-the-necessary-java-artifacts-optional-if-you-want-to-use-the-java-programming-models"></a>Instalace potřebných artefaktů Java (volitelné, pokud chcete použít programovací modely jazyka Java)

Chcete-li sestavovat služby Service Fabric pomocí Javy, zkontrolujte, že máte nainstalovanou sadu JDK 1.8 a Gradle, které se používají pro spouštění úloh sestavení. Následující fragment kódu nainstaluje otevřenou sadu JDK 1.8 společně s Gradlem. Knihovny Service Fabric Java se berou z Mavenu.

  ```bash
  sudo apt-get install openjdk-8-jdk-headless
  sudo apt-get install gradle
  ```

## <a name="install-the-eclipse-neon-plug-in-optional"></a>Instalace modulu plug-in Eclipse Neon (volitelné)

Modul plug-in Eclipse pro Service Fabric můžete nainstalovat z **integrovaného vývojového prostředí Eclipse pro vývojáře v jazyce Java**. Mimo aplikací Service Fabric v Javě můžete k vytvoření aplikací spustitelných souborů hosta a aplikací kontejneru Service Fabric použít Eclipse.

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
Sada .NET Core SDK poskytuje knihovny a šablony potřebné k sestavení služeb Service Fabric pomocí .NET Core. Nainstalujte balíček .NET Core SDK spuštěním následujícího kódu:

   ```bash
   sudo apt-get install servicefabricsdkcsharp
   ```

## <a name="update-the-sdk-and-runtime"></a>Aktualizace sady SDK a modulu runtime

Pokud chcete aktualizovat sadu SDK a modul runtime na nejnovější verze, spusťte následující příkazy (odeberte sady SDK, které nechcete použít):

```bash
sudo apt-get update
sudo apt-get install servicefabric servicefabricsdkcommon servicefabricsdkcsharp
```
Pokud chcete aktualizovat binární soubory sady Java SDK z Mavenu, je potřeba aktualizovat podrobnosti o verzi příslušného binárního souboru v souboru ``build.gradle`` tak, aby odkazovaly na nejnovější verzi. Abyste přesně zjistili, kde potřebujete aktualizovat verzi, můžete se podívat na jakýkoli soubor ``build.gradle`` v úvodních příkladech Service Fabric, které jsou uvedené [tady](https://github.com/Azure-Samples/service-fabric-java-getting-started).

> [!NOTE]
> Aktualizace balíčků může způsobit zastavení místního vývojového clusteru. Po provedení upgradu restartujte místní cluster podle pokynů na této stránce.

## <a name="next-steps"></a>Další kroky

* [Vytvoření a nasazení první aplikace Service Fabric v Javě v Linuxu pomocí Yeomana](service-fabric-create-your-first-linux-application-with-java.md)
* [Vytvoření a nasazení první aplikace Service Fabric v Javě v Linuxu pomocí modulu plug-in Service Fabric pro Eclipse](service-fabric-get-started-eclipse.md)
* [Vytvoření první aplikace v CSharp v Linuxu](service-fabric-create-your-first-linux-application-with-csharp.md)
* [Příprava vývojového prostředí v OSX](service-fabric-get-started-mac.md)
* [Správa aplikací pomocí Service Fabric CLI](service-fabric-application-lifecycle-sfctl.md)
* [Rozdíly Service Fabric pro Windows a Linux](service-fabric-linux-windows-differences.md)
* [Začínáme s rozhraním příkazového řádku Service Fabric](service-fabric-cli.md)

<!-- Links -->

[azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[install-node]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png

