<properties
   pageTitle="Nastavení vývojového prostředí v Linuxu | Microsoft Azure"
   description="Nainstalujte modul runtime a sadu SDK a vytvořte místní vývojový cluster v Linuxu. Po dokončení této instalace a nastavení budete moci sestavovat aplikace."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/26/2016"
   ms.author="seanmck"/>


# Příprava vývojového prostředí v Linuxu


> [AZURE.SELECTOR]
-[ Windows](service-fabric-get-started.md)
- [Linux](service-fabric-get-started-linux.md)
- [OSX](service-fabric-get-started-mac.md)

 Pokud chcete sestavovat a spouštět [aplikace Azure Service Fabric](service-fabric-application-model.md) na vývojovém počítači s Linuxem, musíte nainstalovat modul runtime a běžnou sadu SDK. Můžete také nainstalovat volitelné sady SDK pro Javu a .NET Core.

## Požadavky
### Podporované verze operačních systémů
Pro vývoj jsou podporovány tyto verze operačních systémů:

- Ubuntu 16.04 (Xenial Xerus)

## Aktualizace zdrojů apt

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

## Instalace a nastavení sady SDK

Jakmile jsou vaše zdroje aktualizované, můžete nainstalovat sadu SDK.

1. Nainstalujte balíček Service Fabric SDK. Zobrazí se výzva k potvrzení instalace a vyjádření souhlasu s licenční smlouvou.

    ```bash
    sudo apt-get install servicefabricsdkcommon
    ```

2. Spusťte instalační skript sady SDK.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/sdkcommonsetup.sh
    ```

## Nastavení rozhraní příkazového řádku Azure pro více platforem

[Rozhraní příkazového řádku Azure pro více platforem][ azure-xplat-cli-github] obsahuje příkazy pro komunikaci s entitami služby Service Fabric, včetně clusterů a aplikací. Je založené na Node.js, takže [se ujistěte, že máte Node nainstalované][install-node] před tím, než budete pokračovat v pokynech níže.

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

## Nastavení místního clusteru

Pokud se všechno úspěšně nainstalovalo, měli byste být schopni spustit místní cluster.

1. Spusťte instalační skript clusteru.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```

2. Otevřete webový prohlížeč a přejděte na adresu http://localhost:19080/Explorer. Pokud se cluster spustil, měl by se zobrazit řídicí panel Service Fabric Exploreru.

    ![Service Fabric Explorer v Linuxu][sfx-linux]

V tuto chvíli můžete nasadit předem sestavené balíčky aplikací Service Fabric nebo nové balíčky založené na kontejnerech nebo spustitelných souborech hostů. Chcete-li sestavit nové služby pomocí sad Java SDK nebo .NET Core SDK, postupujte podle níže uvedených pokynů k instalaci.

## Instalace sady Java SDK a modulu plug-in Eclipse Neon (volitelné)

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

## Instalace sady .NET Core SDK (volitelné)

Sada .NET Core SDK poskytuje knihovny a šablony potřebné k sestavení služeb Service Fabric pomocí .NET Core pro více platforem.

1. Nainstalujte balíček .NET Core SDK.

    ```bash
    sudo apt-get install servicefabricsdkcsharp
    ```

2. Spusťte instalační skript sady SDK.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/csharp/sdkcsharpsetup.sh
    ```

## Další kroky

- [Vytvoření první aplikace v Javě v Linuxu](service-fabric-create-your-first-linux-application-with-java.md)

- [Příprava vývojového prostředí v OSX](service-fabric-get-started-mac.md)


<!-- Links -->

[ azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[install-node]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png



<!--HONumber=Sep16_HO4-->


