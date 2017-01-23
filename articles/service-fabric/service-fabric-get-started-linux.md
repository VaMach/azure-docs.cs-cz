---
title: "Nastavení vývojového prostředí v Linuxu | Dokumentace Microsoftu"
description: "Nainstalujte modul runtime a sadu SDK a vytvořte místní vývojový cluster v Linuxu. Po dokončení této instalace a nastavení budete moci sestavovat aplikace."
services: service-fabric
documentationcenter: .net
author: seanmck
manager: timlt
editor: 
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/26/2016
ms.author: seanmck
translationtype: Human Translation
ms.sourcegitcommit: 206b958b4c266b9977a9beb8ffb6a0576f068a9a
ms.openlocfilehash: cc4fbb67baf14f4a104a5de6dbf11ad195a42d15


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

### <a name="supported-operating-system-versions"></a>Podporované verze operačních systémů
Pro vývoj jsou podporovány tyto verze operačních systémů:

* Ubuntu 16.04 („Xenial Xerus“)

## <a name="update-your-apt-sources"></a>Aktualizace zdrojů apt
Chcete-li nainstalovat sadu SDK a přidružený balíček modulu runtime pomocí apt-get, musíte nejprve aktualizovat zdroje apt.

1. Otevřete terminál.
2. Přidejte do seznamu zdrojů úložiště Service Fabric.
   
    ```bash
    sudo sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/servicefabric/ trusty main" > /etc/apt/sources.list.d/servicefabric.list'
    ```
3. Přidejte nový klíč GPG do své klíčenky apt.
   
    ```bash
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    ```
4. Obnovte seznamy balíčků na základě nově přidaných úložišť.
   
    ```bash
    sudo apt-get update
    ```

## <a name="install-and-set-up-the-sdk"></a>Instalace a nastavení sady SDK
Jakmile jsou vaše zdroje aktualizované, můžete nainstalovat sadu SDK.

1. Nainstalujte balíček Service Fabric SDK. Zobrazí se výzva k potvrzení instalace a vyjádření souhlasu s licenční smlouvou.
   
    ```bash
    sudo apt-get install servicefabricsdkcommon
    ```
2. Spusťte instalační skript sady SDK.
   
    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/sdkcommonsetup.sh
    ```


## <a name="set-up-the-azure-cross-platform-cli"></a>Nastavení rozhraní příkazového řádku Azure pro více platforem
[Rozhraní příkazového řádku Azure pro více platforem][azure-xplat-cli-github] obsahuje příkazy pro komunikaci s entitami služby Service Fabric, včetně clusterů a aplikací. Je založené na Node.js, takže [se ujistěte, že máte Node nainstalované][install-node] před tím, než budete pokračovat v následujících pokynech:

1. Naklonujte úložiště GitHub do svého vývojového počítače.
   
    ```bash
    git clone https://github.com/Azure/azure-xplat-cli.git
    ```
2. Přepněte do naklonovaného úložiště a nainstalujte závislosti rozhraní příkazového řádku pomocí Node Package Manageru (npm).
   
    ```bash
    cd azure-xplat-cli
    npm install
    ```
3. Vytvořte symlink ze složky bin/azure naklonovaného úložiště do složky /usr/bin/azure. Tím se přidá k vaší cestě a příkazy budou dostupné z libovolného adresáře.
   
    ```bash
    sudo ln -s $(pwd)/bin/azure /usr/bin/azure
    ```
4. Nakonec povolte automatické dokončování příkazů služby Service Fabric.
   
    ```bash
    azure --completion >> ~/azure.completion.sh
    echo 'source ~/azure.completion.sh' >> ~/.bash_profile
    source ~/azure.completion.sh
    ```

## <a name="set-up-a-local-cluster"></a>Nastavení místního clusteru
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
> 

## <a name="install-the-java-sdk-and-eclipse-neon-plugin-optional"></a>Instalace sady Java SDK a modulu plug-in Eclipse Neon (volitelné)
Sada Java SDK poskytuje knihovny a šablony potřebné k sestavení služeb Service Fabric pomocí Javy.

1. Nainstalujte balíček Java SDK.
   
    ```bash
    sudo apt-get install servicefabricsdkjava
    ```
2. Spusťte instalační skript sady SDK.
   
    ```bash
    sudo /opt/microsoft/sdk/servicefabric/java/sdkjavasetup.sh
    ```

Modul plug-in Eclipse pro Service Fabric můžete nainstalovat z integrovaného vývojového prostředí Eclipse Neon.

1. V nástroji Eclipse se ujistěte, že máte nainstalovanou verzi Buildship 1.0.17 nebo novější. Verze nainstalovaných komponent můžete zkontrolovat tak, že zvolíte **Nápověda > Podrobnosti o instalaci**. Buildship můžete aktualizovat pomocí pokynů [zde][buildship-update].
2. Chcete-li nainstalovat modul plug-in Service Fabric, zvolte **Nápověda > Instalace nového softwaru**.
3. Do textového pole „Pracovat s“ zadejte: http://dl.windowsazure.com/eclipse/servicefabric.
4. Klikněte na Přidat.
   
    ![Modul plug-in Eclipse][sf-eclipse-plugin]
5. Zvolte modul plug-in služby Service Fabric a klikněte na Další.
6. Pokračujte v instalaci a přijměte licenční smlouvu pro koncového zákazníka.

## <a name="install-the-net-core-sdk-optional"></a>Instalace sady .NET Core SDK (volitelné)
Sada .NET Core SDK poskytuje knihovny a šablony potřebné k sestavení služeb Service Fabric pomocí .NET Core pro více platforem.

1. Nainstalujte balíček .NET Core SDK.
   
   ```bash
   sudo apt-get install servicefabricsdkcsharp
   ```

2. Spusťte instalační skript sady SDK.
   
   ```bash
   sudo /opt/microsoft/sdk/servicefabric/csharp/sdkcsharpsetup.sh
   ```

## <a name="updating-the-sdk-and-runtime"></a>Aktualizace sady SDK a modulu runtime

Pokud chcete aktualizovat sadu SDK a modul runtime na nejnovější verze, spusťte následující příkazy (ze seznamu odeberte sady SDK, které nechcete aktualizovat nebo nainstalovat):

   ```bash
   sudo apt-get update
   sudo apt-get install servicefabric, servicefabricsdkcommon, servicefabricsdkcsharp, servicefabricsdkjava
   ```

Pokud chcete aktualizovat rozhraní příkazového řádku, přejděte do adresáře, kam jste naklonovali rozhraní příkazového řádku a proveďte aktualizaci spuštěním příkazu `git pull`. 

## <a name="next-steps"></a>Další kroky
* [Vytvoření první aplikace v Javě v Linuxu](service-fabric-create-your-first-linux-application-with-java.md)
* [Vytvoření první aplikace v CSharp v Linuxu](service-fabric-create-your-first-linux-application-with-csharp.md)
* [Příprava vývojového prostředí v OSX](service-fabric-get-started-mac.md)
* [Správa aplikací Service Fabric pomocí Azure CLI](service-fabric-azure-cli.md)

<!-- Links -->

[azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[install-node]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png



<!--HONumber=Jan17_HO1-->


