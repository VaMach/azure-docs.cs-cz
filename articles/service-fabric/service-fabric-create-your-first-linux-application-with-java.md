---
title: "Vytvoření aplikace Azure Service Fabric Reliable Actors v Javě v Linuxu | Dokumentace Microsoftu"
description: "Zjistěte, jak za pět minut vytvořit a nasadit aplikaci Service Fabric Reliable Actors v Javě."
services: service-fabric
documentationcenter: java
author: rwike77
manager: timlt
editor: 
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/23/2017
ms.author: ryanwi
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: baf948587ede31fe3d5b4f6f0981269b4cfe4d3d
ms.contentlocale: cs-cz
ms.lasthandoff: 08/24/2017

---
# <a name="create-your-first-java-service-fabric-reliable-actors-application-on-linux"></a>Vytvoření první aplikace Service Fabric Reliable Actors v Javě v Linuxu
> [!div class="op_single_selector"]
> * [C# – Windows](service-fabric-create-your-first-application-in-visual-studio.md)
> * [Java – Linux](service-fabric-create-your-first-linux-application-with-java.md)
> * [C# – Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
>
>

Tento rychlý start vám pomůže během několika minut vytvořit první aplikaci Azure Service Fabric v Javě v linuxovém vývojovém prostředí.  Až budete hotovi, budete mít jednoduchou jednoúčelovou aplikaci v Javě spuštěnou v místním vývojovém clusteru.  

## <a name="prerequisites"></a>Požadavky
Než začnete, nainstalujte sadu Service SDK a Service Fabric CLI a nastavte vývojový cluster ve svém [linuxovém vývojovém prostředí](service-fabric-get-started-linux.md). Pokud používáte Mac OS X, můžete k [nastavení linuxového vývojového prostředí ve virtuálním počítači použít Vagrant](service-fabric-get-started-mac.md).

Budete také chtít nainstalovat [Service Fabric CLI](service-fabric-cli.md).

### <a name="install-and-set-up-the-generators-for-java"></a>Instalace a nastavení generátorů pro Javu
Service Fabric nabízí nástroje pro generování uživatelského rozhraní, které vám pomůžou vytvořit aplikaci Service Fabric Java z terminálu pomocí generátoru šablon Yeoman. Postupujte podle následujících kroků, abyste zkontrolovali, že máte na svém počítači funkční generátor šablon Service Fabric yeoman pro Javu.
1. Instalace nodejs a NPM na počítači

  ```bash
  sudo apt-get install npm
  sudo apt install nodejs-legacy
  ```
2. Instalace generátoru šablon [Yeoman](http://yeoman.io/) na počítač z NPM

  ```bash
  sudo npm install -g yo
  ```
3. Instalace generátoru aplikací Service Fabric Yeo Java z NPM

  ```bash
  sudo npm install -g generator-azuresfjava
  ```

## <a name="create-the-application"></a>Vytvoření aplikace
Aplikace Service Fabric obsahuje jednu nebo víc služeb, z nichž každá má určitou roli při poskytování funkcí aplikace. Generátor, který jste nainstalovali v poslední části, vám usnadní vytvoření první služby a případná další rozšíření později.  Vytvořit, sestavit a nasadit aplikace Service Fabric v Javě můžete také pomocí modulu plug-in pro Eclipse. Viz [Vytvoření a nasazení první aplikace v Javě pomocí Eclipse](service-fabric-get-started-eclipse.md). Pro účely tohoto rychlého startu použijte Yeoman k vytvoření aplikace s jednou službou, která ukládá a získává hodnotu čítače.

1. V terminálu zadejte ``yo azuresfjava``.
2. Pojmenujte svoji aplikaci.
3. Vyberte typ první služby a pojmenujte ji. Pro účely tohoto kurzu zvolte službu Reliable Actor. Další informace o ostatních typech služeb najdete v tématu [Přehled programovacího modelu Service Fabric](service-fabric-choose-framework.md).
   ![Generátor Service Fabric Yeoman pro Javu][sf-yeoman]

## <a name="build-the-application"></a>Sestavení aplikace
Šablony Service Fabric Yeoman zahrnují skript sestavení pro [Gradle](https://gradle.org/), který můžete použít k sestavení aplikace z terminálu.
Závislosti Service Fabric Java se získávají z Mavenu. Chcete-li sestavovat aplikace Service Fabric Java a pracovat s nimi, musíte zajistit, že máte nainstalovanou sadu JDK a Gradle. Pokud ještě nejsou instalované, můžete nainstalovat sadu JDK(openjdk-8-jdk) a Gradle spuštěním následujícího kódu:

  ```bash
  sudo apt-get install openjdk-8-jdk-headless
  sudo apt-get install gradle
  ```

Pokud chcete sestavit a zabalit aplikaci, spusťte následující:

  ```bash
  cd myapp
  gradle
  ```

## <a name="deploy-the-application"></a>Nasazení aplikace
Jakmile je aplikace sestavená, můžete ji nasadit do místního clusteru.

1. Připojte se k místnímu clusteru služby Service Fabric.

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```

2. Spuštěním instalačního skriptu, který je součástí šablony, zkopírujte balíček aplikace do úložiště imagí clusteru, zaregistrujte typ aplikace a vytvořte její instanci.

    ```bash
    ./install.sh
    ```

Nasazení sestavené aplikace je stejné jako u všech ostatních aplikací Service Fabric. Podrobné pokyny najdete v dokumentaci s popisem [správy aplikace Service Fabric pomocí Service Fabric CLI](service-fabric-application-lifecycle-sfctl.md).

Parametry těchto příkazů najdete v generovaných manifestech uvnitř balíčku aplikace.

Jakmile je aplikace nasazená, otevřete prohlížeč a přejděte k nástroji [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) na adrese [http://localhost:19080/Explorer](http://localhost:19080/Explorer).
Pak rozbalte uzel **Aplikace** a všimněte si, že už obsahuje položku pro váš typ aplikace a další položku pro první instanci tohoto typu.

## <a name="start-the-test-client-and-perform-a-failover"></a>Spuštění klienta testování a převzetí služeb při selhání
Samotné objekty actor nic nedělají – vyžadují, aby jim jiná služba nebo klient posílali zprávy. Šablona actor zahrnuje jednoduchý testovací skript, který můžete použít k interakci se službou actor.

1. Spusťte skript pomocí pomocného sledovacího programu a prohlédněte si výstup služby actor.  Testovací skript volá metodu `setCountAsync()` objektu actor pro zvýšení čítače a metodu `getCountAsync()` objektu actor pro získání nové hodnoty čítače, kterou zobrazí v konzole.

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```

2. V nástroji Service Fabric Explorer vyhledejte uzel, který je hostitelem primární repliky pro službu objektu actor. Na snímku níže je to uzel 3. Primární replika služby zpracovává operace čtení a zápisu.  Změny stavu služby se následně replikují do sekundárních replik, které jsou na následujícím snímku obrazovku spuštěné na uzlech 0 a 1.

    ![Vyhledání primární repliky v Service Fabric Exploreru][sfx-primary]

3. V části **Uzly** klikněte na uzel, který jste našli v předchozím kroku, a pak v nabídce Akce vyberte **Deaktivovat (restartovat)**. Tato akce restartuje uzel spuštěný v primární replice služby a vynutí převzetí služeb při selhání jednou ze sekundárních replik spuštěných na jiném uzlu.  Úroveň této sekundární repliky se zvýší na primární, v jiném uzlu se vytvoří jiná sekundární replika a primární replika začne přijímat operace čtení a zápisu. Při restartování uzlu sledujte výstup z testovacího klienta a všimněte si, že se čítač bez ohledu na převzetí služeb při selhání pořád postupně zvyšuje.

## <a name="remove-the-application"></a>Odebrání aplikace
Pomocí odinstalačního skriptu, který je součástí šablony, odstraňte instanci aplikace, zrušte registraci balíčku aplikace a odeberete balíček aplikace z úložiště imagí clusteru.

```bash
./uninstall.sh
```

V nástroji Service Fabric Explorer uvidíte, že se aplikace a typ aplikace už nezobrazují v uzlu **Aplikace**.

## <a name="service-fabric-java-libraries-on-maven"></a>Knihovny Service Fabric Java v Mavenu
Hostitelem knihoven Service Fabric Java je Maven. Můžete přidat závislosti do souborů ``pom.xml`` nebo ``build.gradle`` vašich projektů, aby se používali knihovny Service Fabric Java z úložiště **mavenCentral**.

### <a name="actors"></a>Objekty actor

Podpora Service Fabric Reliable Actor pro vaši aplikaci.

  ```XML
  <dependency>
      <groupId>com.microsoft.servicefabric</groupId>
      <artifactId>sf-actors-preview</artifactId>
      <version>0.10.0</version>
  </dependency>
  ```

  ```gradle
  repositories {
      mavenCentral()
  }
  dependencies {
      compile 'com.microsoft.servicefabric:sf-actors-preview:0.10.0'
  }
  ```

### <a name="services"></a>Služby

Podpora Service Fabric Stateless Service pro vaši aplikaci.

  ```XML
  <dependency>
      <groupId>com.microsoft.servicefabric</groupId>
      <artifactId>sf-services-preview</artifactId>
      <version>0.10.0</version>
  </dependency>
  ```

  ```gradle
  repositories {
      mavenCentral()
  }
  dependencies {
      compile 'com.microsoft.servicefabric:sf-services-preview:0.10.0'
  }
  ```

### <a name="others"></a>Ostatní
#### <a name="transport"></a>Přenos

Podpora přenosové vrstvy pro aplikace Service Fabric Java. Pokud neprogramujete na úrovni přenosové vrstvy, nemusíte tuto závislost do aplikace Reliable Actor nebo aplikace služby explicitně přidávat.

  ```XML
  <dependency>
      <groupId>com.microsoft.servicefabric</groupId>
      <artifactId>sf-transport-preview</artifactId>
      <version>0.10.0</version>
  </dependency>
  ```

  ```gradle
  repositories {
      mavenCentral()
  }
  dependencies {
      compile 'com.microsoft.servicefabric:sf-transport-preview:0.10.0'
  }
  ```

#### <a name="fabric-support"></a>Podpora prostředků infrastruktury

Podpora na úrovni systému pro Service Fabric, která komunikuje s modulem runtime nativním pro Service Fabric. Tuto závislost nemusíte do aplikace Reliable Actor nebo aplikace služby explicitně přidávat. Načte se z Mavenu automaticky, jakmile zahrnete ostatní závislosti uvedené výše.

  ```XML
  <dependency>
      <groupId>com.microsoft.servicefabric</groupId>
      <artifactId>sf-preview</artifactId>
      <version>0.10.0</version>
  </dependency>
  ```

  ```gradle
  repositories {
      mavenCentral()
  }
  dependencies {
      compile 'com.microsoft.servicefabric:sf-preview:0.10.0'
  }
  ```

## <a name="migrating-old-service-fabric-java-applications-to-be-used-with-maven"></a>Migrace starých aplikací Service Fabric Java pro použití s Mavenem
Nedávno jsme přesunuli knihovny Service Fabric Java ze sady Service Fabric Java SDK do úložiště Maven. Zatímco nové aplikace, které vygenerujete pomocí Yeomanu nebo Eclipse, vygenerují nejnovější aktualizované projekty (které budou moct pracovat s Mavenem), můžete vaše stávající bezstavové aplikace nebo aplikace objektu actor v Javě pro Service Fabric, které dřív používali sadu Service Fabric Java SDK, aktualizovat tak, aby používaly závislosti Service Fabric Java z Mavenu. Abyste zajistili fungování starších aplikací s Mavenem, postupujte podle kroků uvedených [tady](service-fabric-migrate-old-javaapp-to-use-maven.md).

## <a name="next-steps"></a>Další kroky

* [Vytvoření první aplikace Service Fabric v Javě v Linuxu pomocí Eclipse](service-fabric-get-started-eclipse.md)
* [Další informace o Reliable Actors](service-fabric-reliable-actors-introduction.md)
* [Komunikace s clustery Service Fabric pomocí rozhraní příkazového řádku Service Fabric](service-fabric-cli.md)
* Informace o [možnostech podpory pro Service Fabric](service-fabric-support.md)
* [Začínáme se Service Fabric CLI](service-fabric-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-yeoman.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-java/sfx-primary.png
[sf-eclipse-templates]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-eclipse-templates.png

