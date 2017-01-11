---
title: "Vytvoření první aplikace Service Fabric v Linuxu pomocí Javy | Dokumentace Microsoftu"
description: "Vytvoření a nasazení aplikace Service Fabric pomocí Javy"
services: service-fabric
documentationcenter: java
author: seanmck
manager: timlt
editor: 
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/04/2016
ms.author: seanmck
translationtype: Human Translation
ms.sourcegitcommit: 6d8f489ac053db4898741671df73b6abfabeb0dd
ms.openlocfilehash: 05361e08b93c93491111661b5fe997ebf5053d16


---
# <a name="create-your-first-azure-service-fabric-application"></a>Vytvoření první aplikace Azure Service Fabric
> [!div class="op_single_selector"]
> * [C# – Windows](service-fabric-create-your-first-application-in-visual-studio.md)
> * [Java – Linux](service-fabric-create-your-first-linux-application-with-java.md)
> * [C# – Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
> 
> 

Service Fabric poskytuje sady SDK pro vytváření služeb v Linuxu pomocí .NET Core a Javy. V tomto kurzu vytvoříme aplikaci pro Linux a sestavíme službu pomocí Javy.  Následující video Microsoft Virtual Academy vás také provede procesem vytváření aplikace Java v Linuxu:  
<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=DOX8K86yC_206218965">  
<img src="./media/service-fabric-create-your-first-linux-application-with-java/LinuxVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="prerequisites"></a>Požadavky
Než začnete, ujistěte se, že máte [v Linuxu nastavené vývojové prostředí](service-fabric-get-started-linux.md). Pokud používáte Mac OS X, můžete k [nastavení linuxového prostředí ve virtuálním počítači použít Vagrant](service-fabric-get-started-mac.md).

## <a name="create-the-application"></a>Vytvoření aplikace
Aplikace Service Fabric může obsahovat jednu nebo víc služeb, z nichž každá má určitou roli při poskytování funkcí aplikace. Service Fabric SDK pro Linux zahrnuje generátor [Yeoman](http://yeoman.io/), který vám usnadní vytvoření první služby a případná další rozšíření později. Pomocí generátoru Yeoman vytvoříme aplikaci s jedinou službou.

1. V terminálu zadejte **yo azuresfjava**.
2. Pojmenujte svoji aplikaci.
3. Vyberte typ první služby a pojmenujte ji. Pro účely tohoto kurzu zvolíme službu Reliable Actor.
   
   ![Generátor Service Fabric Yeoman pro Javu][sf-yeoman]

> [!NOTE]
> Další informace o možnostech najdete v tématu [Přehled programovacího modelu Service Fabric](service-fabric-choose-framework.md).
> 
> 

## <a name="build-the-application"></a>Sestavení aplikace
Šablona Service Fabric Yeoman zahrnuje skript sestavení pro [Gradle](https://gradle.org/), který můžete použít k sestavení aplikace u terminálu.

  ```bash
  cd myapp
  gradle
  ```

## <a name="deploy-the-application"></a>Nasazení aplikace
Jakmile je aplikace sestavená, můžete ji nasadit do místního clusteru pomocí rozhraní příkazového řádku Azure.

1. Připojte se k místnímu clusteru služby Service Fabric.
   
    ```bash
    azure servicefabric cluster connect
    ```
2. Pomocí instalačního skriptu, který je součástí šablony, zkopírujte balíček aplikace do úložiště imagí clusteru, zaregistrujte typ aplikace a vytvořte její instanci.
   
    ```bash
    ./install.sh
    ```
3. Otevřete prohlížeč a přejdete k Service Fabric Exploreru na adrese http://localhost:19080/Explorer (pokud používáte Vagrant v Mac OS X, místo localhost použijte privátní IP adresu virtuálního počítače).
4. Rozbalte uzel Aplikace a všimněte si, že už obsahuje položku pro váš typ aplikace a další položku pro první instanci tohoto typu.

## <a name="start-the-test-client-and-perform-a-failover"></a>Spuštění klienta testování a převzetí služeb při selhání
Projekty Actor samy o sobě nedělají nic. Vyžadují, aby jim jiná služba nebo klient posílali zprávy. Šablona actor zahrnuje jednoduchý testovací skript, který můžete použít k interakci se službou actor.

1. Spusťte skript pomocí pomocného sledovacího programu a prohlédněte si výstup služby actor.
   
    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```
2. V Service Fabric Exploreru vyhledejte uzel, který je hostitelem primární repliky pro službu actor. Na snímku níže je to uzel 3.
   
    ![Vyhledání primární repliky v Service Fabric Exploreru][sfx-primary]
3. Klikněte na uzel, který jste našli v předchozím kroku, a potom v nabídce Akce vyberte **Deaktivovat (restartovat)**. Restartuje se tak jeden z pěti uzlů v místním clusteru a vynutí převzetí služeb při selhání jednou ze sekundárních replik spuštěných v jiném uzlu. Věnujte přitom pozornost výstupu z klienta testování a všimněte si, že se čítač bez ohledu na převzetí služeb při selhání pořád postupně zvyšuje.

## <a name="build-and-deploy-an-application-with-the-eclipse-neon-plugin"></a>Sestavení a nasazení aplikace pomocí modulu plug-in Eclipse Neon
Pokud jste nainstalovali [modul plug-in Service Fabric](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-get-started-linux#install-the-java-sdk-and-eclipse-neon-plugin-optional) pro Eclipse Neon, můžete ho použít k vytvoření, sestavení a nasazení aplikací Service Fabric vytvořených v jazyce Java.  Při instalaci Eclipse vyberte **Eclipse IDE pro vývojáře Java**.

### <a name="create-the-application"></a>Vytvoření aplikace
Modul plug-in Service Fabric je dostupný prostřednictvím rozšíření Eclipse.

1. V Eclipse vyberte **Soubor > Další > Service Fabric**. Zobrazí se řada možností, mimo jiné Objekty actor a Kontejnery.
   
    ![Šablony Service Fabric v Eclipse][sf-eclipse-templates]
2. V tomto případě zvolte Bezstavová služba.
3. Budete vyzváni k potvrzení použití perspektivy služby Service Fabric, která optimalizuje Eclipse pro použití s projekty Service Fabric. Zvolte Ano.

### <a name="deploy-the-application"></a>Nasazení aplikace
Šablony Service Fabric zahrnují sadu úloh Gradlu pro sestavování a nasazování aplikací, které můžete aktivovat prostřednictvím Eclipse. 

1. Zvolte **Run > Run Configurations** (Spustit > Konfigurace spuštění).
2. Zadejte **local** (místní) nebo **cloud** (v cloudu). Výchozí nastavení je **local** (místní). Pro nasazení na vzdálený cluster vyberte **cloud** (v cloudu).
3. Ujistěte se, že jsou v publikačních profilech vyplněné správné informace a podle potřeby upravte soubor `local.json` nebo `cloud.json`.
4. Klikněte na **Run** (Spustit).

Vaše aplikace se během chvilky sestaví a nasadí. Ke sledování jejího stavu můžete využít Service Fabric Explorer.

## <a name="adding-more-services-to-an-existing-application"></a>Přidání více služeb do stávající aplikace

Pokud chcete přidat další službu do aplikace již vytvořené pomocí `yo`, proveďte následující kroky: 
1. Změňte adresář na kořenovou složku stávající aplikace.  Například `cd ~/YeomanSamples/MyApplication`, pokud `MyApplication` je aplikace vytvořená pomocí Yeomanu.
2. Spusťte `yo azuresfjava:AddService`.


## <a name="next-steps"></a>Další kroky
* [Další informace o Reliable Actors](service-fabric-reliable-actors-introduction.md)
* [Komunikace s clustery Service Fabric pomocí rozhraní příkazového řádku Azure](service-fabric-azure-cli.md)
* Informace o [možnostech podpory pro Service Fabric](service-fabric-support.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-yeoman.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-java/sfx-primary.png
[sf-eclipse-templates]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-eclipse-templates.png



<!--HONumber=Dec16_HO2-->


