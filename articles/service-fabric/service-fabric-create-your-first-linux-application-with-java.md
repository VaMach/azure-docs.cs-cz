---
title: Vytvoření první aplikace Service Fabric v Linuxu pomocí Javy | Microsoft Docs
description: Vytvoření a nasazení aplikace Service Fabric pomocí Javy
services: service-fabric
documentationcenter: java
author: seanmck
manager: timlt
editor: ''

ms.service: service-fabric
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/04/2016
ms.author: seanmck

---
# Vytvoření první aplikace Azure Service Fabric
> [!div class="op_single_selector"]
> * [C# – Windows](service-fabric-create-your-first-application-in-visual-studio.md)
> * [Java – Linux](service-fabric-create-your-first-linux-application-with-java.md)
> * [C# – Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
> 
> 

Service Fabric poskytuje sady SDK pro vytváření služeb v Linuxu pomocí .NET Core a Javy. V tomto kurzu si projdeme postup vytvoření aplikace pro Linux a vytvoření služby pomocí Javy.

## Požadavky
Než začnete, ujistěte se, že máte [v Linuxu nastavené vývojové prostředí](service-fabric-get-started-linux.md). Pokud používáte Mac OS X, můžete k [nastavení linuxového prostředí ve virtuálním počítači použít Vagrant](service-fabric-get-started-mac.md).

## Vytvoření aplikace
Aplikace Service Fabric může obsahovat jednu nebo víc služeb, z nichž každá má určitou roli při poskytování funkcí aplikace. Service Fabric SDK pro Linux zahrnuje generátor [Yeoman](http://yeoman.io/), který vám usnadní vytvoření první služby a případná další rozšíření později. Pomocí generátoru Yeoman teď vytvoříme novou aplikaci s jedinou službou.

1. V terminálu zadejte **yo azuresfjava**.
2. Pojmenujte svoji aplikaci.
3. Vyberte typ první služby a pojmenujte ji. Pro účely tohoto kurzu zvolíme službu Reliable Actors.
   
   ![Generátor Service Fabric Yeoman pro Javu][sf-yeoman]

> [!NOTE]
> Další informace o možnostech najdete v tématu [Přehled programovacího modelu Service Fabric](service-fabric-choose-framework.md).
> 
> 

## Sestavení aplikace
Šablona Service Fabric Yeoman zahrnuje skript sestavení pro [Gradle](https://gradle.org/), který můžete použít k sestavení aplikace u terminálu.

  ```bash
  cd myapp
  gradle
  ```

## Nasazení aplikace
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

## Spuštění klienta testování a převzetí služeb při selhání
Projekty Actor samy o sobě nedělají nic. Vyžadují, aby jim jiná služba nebo klient posílali zprávy. Šablona actor zahrnuje jednoduchý testovací skript, který můžete použít k interakci se službou actor.

1. Spusťte skript pomocí pomocného sledovacího programu a prohlédněte si výstup služby actor.
   
    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```
2. V Service Fabric Exploreru vyhledejte uzel, který je hostitelem primární repliky pro službu actor. Na snímku níže je to uzel 3.
   
    ![Vyhledání primární repliky v Service Fabric Exploreru][sfx-primary]
3. Klikněte na uzel, který jste našli v předchozím kroku, a potom v nabídce Akce vyberte **Deaktivovat (restartovat)**. Restartuje se tak jeden z pěti uzlů v místním clusteru a vynutí převzetí služeb při selhání jednou ze sekundárních replik spuštěných v jiném uzlu. Věnujte přitom pozornost výstupu z klienta testování a všimněte si, že se čítač bez ohledu na převzetí služeb při selhání pořád postupně zvyšuje.

## Sestavení a nasazení aplikace pomocí modulu plug-in Eclipse Neon
Pokud jste nainstalovali modul plug-in služby pro Eclipse Neon, můžete ho použít k vytvoření, sestavení a nasazení aplikací Service Fabric vytvořených v jazyce Java.  Při instalaci Eclipse vyberte **Eclipse IDE pro vývojáře Java**.

### Vytvoření aplikace
Modul plug-in Service Fabric je dostupný prostřednictvím rozšíření Eclipse.

1. V Eclipse vyberte **Soubor > Další > Service Fabric**. Zobrazí se řada možností, mimo jiné Objekty actor a Kontejnery.
   
    ![Šablony Service Fabric v Eclipse][sf-eclipse-templates]
2. V tomto případě zvolte Bezstavová služba.
3. Budete vyzváni k potvrzení použití perspektivy služby Service Fabric, která optimalizuje Eclipse pro použití s projekty Service Fabric. Zvolte Ano.

### Nasazení aplikace
Šablony Service Fabric zahrnují sadu úloh Gradlu pro sestavování a nasazování aplikací, které můžete aktivovat prostřednictvím Eclipse.

1. Zvolte **Run > Run Configurations** (Spustit > Konfigurace spuštění).
2. Rozbalte **Gradle Project** (Projekt Gradlu) a zvolte **ServiceFabricDeployer**.
3. Klikněte na **Run** (Spustit).

Vaše aplikace se během chvilky sestaví a nasadí. Ke sledování jejího stavu můžete využít Service Fabric Explorer.

## Další kroky
* [Další informace o Reliable Actors](service-fabric-reliable-actors-introduction.md)
* [Komunikace s clustery Service Fabric pomocí Azure CLI](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-yeoman.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-java/sfx-primary.png
[sf-eclipse-templates]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-eclipse-templates.png



<!--HONumber=Oct16_HO3-->


