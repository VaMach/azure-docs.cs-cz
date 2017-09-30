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
ms.date: 08/21/2017
ms.author: saysa
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 28424d139499b797b09664f73657a7f73361e3bc
ms.contentlocale: cs-cz
ms.lasthandoff: 09/25/2017

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
Service Fabric nefunguje v OS X nativně. Pro spuštění místního clusteru služby Service Fabric poskytujeme předem nakonfigurovaný virtuální počítač se systémem Ubuntu pomocí aplikací Vagrant a VirtualBox. Než začnete, budete potřebovat:

* [Vagrant (verzi 1.8.4 nebo novější)](http://www.vagrantup.com/downloads.html)
* [VirtualBox](http://www.virtualbox.org/wiki/Downloads)

>[!NOTE]
> Musíte pro Vagrant a VirtualBox používat vzájemně podporované verze. Vagrant se může chovat chybně v nepodporované verzi VirtualBox.
>

## <a name="create-the-local-vm"></a>Vytvoření místního virtuálního počítače
Chcete-li vytvořit místní virtuální počítač s clusterem Service Fabric s 5 uzly, postupujte následovně:

1. Klonování úložiště `Vagrantfile`

    ```bash
    git clone https://github.com/azure/service-fabric-linux-vagrant-onebox.git
    ```
    Tento postup přinese soubor `Vagrantfile` obsahující konfiguraci virtuálního počítače společně s umístěním virtuálního počítače, ze kterého je stažen.  Tento soubor odkazuje na skladovou image Ubuntu. 

2. Přejděte do místního klonu úložiště.

    ```bash
    cd service-fabric-linux-vagrant-onebox
    ```
3. (Volitelné) Upravte výchozí nastavení virtuálního počítače.

    Ve výchozím nastavení je místní virtuální počítač nakonfigurován takto:

   * 3 GB přidělené paměti
   * Privátní hostitelská síť nakonfigurovaná na IP adresu 192.168.50.50, která umožňuje průchod přenosu dat z hostitelského počítače Mac

     V souboru `Vagrantfile` můžete změnit kterékoli z těchto nastavení nebo k virtuálnímu počítači přidat další konfiguraci. Úplný seznam možností konfigurace najdete v [dokumentaci k Vagrant](http://www.vagrantup.com/docs).
4. Vytvořte virtuální počítač.

    ```bash
    vagrant up
    ```


5. Přihlaste se k virtuálnímu počítači a nainstalujte sadu Service Fabric SDK.

    ```bash
    vagrant ssh
    ```
   
   Nainstalujte sadu SDK způsobem popsaným v tématu věnovaném [instalaci sady SDK](service-fabric-get-started-linux.md).  Společně s rozhraním příkazového řádku sfctl je poskytován níže uvedený skript pro usnadnění instalace modulu runtime Service Fabric a běžné sady SDK Service Fabric. Spuštěním skriptu se předpokládá, že jste si přečetli o licencích pro veškerý instalovaný software a souhlasíte s nimi.

    ```bash
    sudo curl -s https://raw.githubusercontent.com/Azure/service-fabric-scripts-and-templates/master/scripts/SetupServiceFabric/SetupServiceFabric.sh | sudo bash
    ```

5.  Spusťte cluster Service Fabric.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```

    >[!TIP]
    > Pokud stažení virtuálního počítače trvá dlouhou dobu, můžete ho stáhnout pomocí nástrojů wget nebo curl nebo prostřednictvím prohlížeče tak, že přejdete na odkaz určený položkou **config.vm.box_url** v souboru `Vagrantfile`. Po stažení do místního počítače upravte soubor `Vagrantfile` tak, aby odkazoval na místní cestu, kam jste image stáhli. Pokud jste například stáhli image do adresáře /home/users/test/azureservicefabric.tp8.box, nastavte položku **config.vm.box_url** na tuto cestu.
    >

5. Otestujte správné nastavení clusteru tak, že přejdete do Service Fabric Exploreru na adrese http://192.168.50.50:19080/Explorer (za předpokladu, že jste ponechali výchozí IP adresu privátní sítě).

    ![Service Fabric Explorer zobrazený z hostitelského počítače Mac][sfx-mac]


## <a name="create-application-on-mac-using-yeoman"></a>Vytvoření aplikace na počítači Mac pomocí Yeomanu
Service Fabric nabízí nástroje pro generování uživatelského rozhraní, které vám pomůžou vytvořit aplikaci Service Fabric z terminálu pomocí generátoru šablon Yeoman. Postupujte podle následujících kroků, abyste zkontrolovali, že máte na svém počítači funkční generátor šablon Service Fabric yeoman.

1. V počítači Mac musí být instalováno Node.js a NPM. Pokud ne, můžete nainstalovat Node.js a NPM pomocí Homebrew následujícím postupem. Pokud chcete zkontrolovat verze Node.js a NPM nainstalované na počítači Mac, můžete použít možnost ``-v``.

  ```bash
  brew install node
  node -v
  npm -v
  ```
2. Instalace generátoru šablon [Yeoman](http://yeoman.io/) na počítač z NPM

  ```bash
  npm install -g yo
  ```
3. Nainstalujte generátor Yeoman, který chcete použít, podle kroků v úvodní [dokumentaci](service-fabric-get-started-linux.md). Pokud chcete vytvářet aplikace Service Fabric pomocí Yeomanu, postupujte podle těchto kroků:

  ```bash
  npm install -g generator-azuresfjava       # for Service Fabric Java Applications
  npm install -g generator-azuresfguest      # for Service Fabric Guest executables
  npm install -g generator-azuresfcontainer  # for Service Fabric Container Applications
  ```
4. K vytvoření aplikace Service Fabric Java na počítači Mac budete potřebovat nainstalovanou sadu JDK 1.8 a Gradle.

## <a name="set-up-net-core-20-development"></a>Nastavení pro vývoj v .NET Core 2.0

Pokud chcete začít [vytvářet aplikace Service Fabric v jazyce C#](service-fabric-create-your-first-linux-application-with-csharp.md), nainstalujte sadu [.NET Core 2.0 SDK pro Mac](https://www.microsoft.com/net/core#macos). Balíčky pro aplikace Service Fabric v .NET Core 2.0 jsou hostované na NuGet.org a aktuálně ve verzi Preview.


## <a name="install-the-service-fabric-plugin-for-eclipse-neon"></a>Instalace modulu plug-in Service Fabric pro Eclipse Neon

Service Fabric poskytuje modul plug-in pro **integrovaná vývojové prostředí Eclipse Neon pro Javu**, který může zjednodušit proces vytváření, sestavování a nasazování služeb v Javě. Můžete provést kroky instalace uvedené v této obecné [dokumentaci](service-fabric-get-started-eclipse.md#install-or-update-the-service-fabric-plug-in-in-eclipse-neon) týkající se instalace nebo aktualizace modulu plug-in Service Fabric Eclipse.

>[!TIP]
> Standardně podporujeme výchozí adresu IP, jak je uvedeno v souboru ``Vagrantfile`` a v souboru ``Local.json`` generované aplikace. V případě, že provedete změnu a nasadíte Vagrant s jinou IP adresou, aktualizujte také odpovídající IP adresu v souboru ``Local.json`` vaší aplikace.

## <a name="next-steps"></a>Další kroky
<!-- Links -->
* [Vytvoření a nasazení první aplikace Service Fabric v Javě v Linuxu pomocí Yeomana](service-fabric-create-your-first-linux-application-with-java.md)
* [Vytvoření a nasazení první aplikace Service Fabric v Javě v Linuxu pomocí modulu plug-in Service Fabric pro Eclipse](service-fabric-get-started-eclipse.md)
* [Vytvoření clusteru služby Service Fabric na webu Azure Portal](service-fabric-cluster-creation-via-portal.md)
* [Vytvoření clusteru služby Service Fabric pomocí Azure Resource Manageru](service-fabric-cluster-creation-via-arm.md)
* [Pochopení aplikačního modelu služby Service Fabric](service-fabric-application-model.md)

<!-- Images -->
[cluster-setup-script]: ./media/service-fabric-get-started-mac/cluster-setup-mac.png
[sfx-mac]: ./media/service-fabric-get-started-mac/sfx-mac.png
[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

