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
ms.date: 06/29/2017
ms.author: ryanwi
ms.translationtype: HT
ms.sourcegitcommit: 9afd12380926d4e16b7384ff07d229735ca94aaa
ms.openlocfilehash: 254f38a600ea4026120bc411368eeb01310e56b2
ms.contentlocale: cs-cz
ms.lasthandoff: 07/15/2017

---
# <a name="create-your-first-java-service-fabric-reliable-actors-application-on-linux"></a>Vytvoření první aplikace Service Fabric Reliable Actors v Javě v Linuxu

Tento rychlý start vám pomůže během několika minut vytvořit první aplikaci Azure Service Fabric v Javě v linuxovém vývojovém prostředí.  Až budete hotovi, budete mít jednoduchou jednoúčelovou aplikaci v Javě spuštěnou v místním vývojovém clusteru.  

## <a name="prerequisites"></a>Požadavky
Než začnete, nainstalujte sadu Service SDK a Azure CLI a nastavte vývojový cluster ve svém [linuxovém vývojovém prostředí](service-fabric-get-started-linux.md). Pokud používáte Mac OS X, můžete k [nastavení linuxového vývojového prostředí ve virtuálním počítači použít Vagrant](service-fabric-get-started-mac.md).

Budete také pro nasazení aplikace chtít nakonfigurovat [Azure CLI 2.0](service-fabric-azure-cli-2-0.md) (doporučeno) nebo [XPlat CLI](service-fabric-azure-cli.md).

## <a name="create-the-application"></a>Vytvoření aplikace
Aplikace Service Fabric obsahuje jednu nebo víc služeb, z nichž každá má určitou roli při poskytování funkcí aplikace. Service Fabric SDK pro Linux zahrnuje generátor [Yeoman](http://yeoman.io/), který vám usnadní vytvoření první služby a případná další rozšíření později.  Vytvořit, sestavit a nasadit aplikace Service Fabric v Javě můžete také pomocí modulu plug-in pro Eclipse. Viz [Vytvoření a nasazení první aplikace v Javě pomocí Eclipse](service-fabric-get-started-eclipse.md). Pro účely tohoto rychlého startu použijte Yeoman k vytvoření aplikace s jednou službou, která ukládá a získává hodnotu čítače.

1. V terminálu zadejte ``yo azuresfjava``.
2. Pojmenujte svoji aplikaci. 
3. Vyberte typ první služby a pojmenujte ji. Pro účely tohoto kurzu zvolte službu Reliable Actor. Další informace o ostatních typech služeb najdete v tématu [Přehled programovacího modelu Service Fabric](service-fabric-choose-framework.md).
   ![Generátor Service Fabric Yeoman pro Javu][sf-yeoman]

## <a name="build-the-application"></a>Sestavení aplikace
Šablony Service Fabric Yeoman zahrnují skript sestavení pro [Gradle](https://gradle.org/), který můžete použít k sestavení aplikace z terminálu. Pokud chcete sestavit a zabalit aplikaci, spusťte následující:

  ```bash
  cd myapp
  gradle
  ```

## <a name="deploy-the-application"></a>Nasazení aplikace
Jakmile je aplikace sestavená, můžete ji nasadit do místního clusteru.

### <a name="using-xplat-cli"></a>Použití XPlat CLI

1. Připojte se k místnímu clusteru služby Service Fabric.

    ```bash
    azure servicefabric cluster connect
    ```

2. Spuštěním instalačního skriptu, který je součástí šablony, zkopírujte balíček aplikace do úložiště imagí clusteru, zaregistrujte typ aplikace a vytvořte její instanci.

    ```bash
    ./install.sh
    ```

### <a name="using-azure-cli-20"></a>Použití Azure CLI 2.0

Nasazení sestavené aplikace je stejné jako u všech ostatních aplikací Service Fabric. Podrobné pokyny najdete v dokumentaci s popisem [správy aplikace Service Fabric pomocí Azure CLI](service-fabric-application-lifecycle-azure-cli-2-0.md).

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

## <a name="next-steps"></a>Další kroky
* [Vytvoření první aplikace Service Fabric v Javě v Linuxu pomocí Eclipse](service-fabric-get-started-eclipse.md)
* [Další informace o Reliable Actors](service-fabric-reliable-actors-introduction.md)
* [Komunikace s clustery Service Fabric pomocí Azure CLI](service-fabric-azure-cli.md)
* [Řešení potíží s nasazením](service-fabric-azure-cli.md#troubleshooting)
* Informace o [možnostech podpory pro Service Fabric](service-fabric-support.md)

## <a name="related-articles"></a>Související články

* [Začínáme s platformou Service Fabric a Azure CLI 2.0](service-fabric-azure-cli-2-0.md)
* [Začínáme se Service Fabric XPlat CLI](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-yeoman.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-java/sfx-primary.png
[sf-eclipse-templates]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-eclipse-templates.png

