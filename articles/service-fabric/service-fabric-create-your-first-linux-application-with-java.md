---
title: "Vytvoření první aplikace mikroslužeb Azure v Linuxu pomocí Javy | Dokumentace Microsoftu"
description: "Vytvoření a nasazení aplikace Service Fabric pomocí Javy"
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
ms.date: 06/02/2017
ms.author: ryanwi
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: e229602b4bfa72977c9b15e854d796ed09fa55d2
ms.contentlocale: cs-cz
ms.lasthandoff: 07/01/2017


---
<a id="create-your-first-service-fabric-java-application-on-linux" class="xliff"></a>

# Vytvoření první aplikace Service Fabric v Javě v Linuxu
> [!div class="op_single_selector"]
> * [C# – Windows](service-fabric-create-your-first-application-in-visual-studio.md)
> * [Java – Linux](service-fabric-create-your-first-linux-application-with-java.md)
> * [C# – Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
>
>

Tento rychlý start vám pomůže během několika minut vytvořit první aplikaci Azure Service Fabric v Javě v linuxovém vývojovém prostředí.  Až budete hotovi, budete mít jednoduchou jednoúčelovou aplikaci v Javě spuštěnou v místním vývojovém clusteru.  

<a id="prerequisites" class="xliff"></a>

## Požadavky
Než začnete, nainstalujte sadu Service SDK a Azure CLI a nastavte vývojový cluster ve svém [linuxovém vývojovém prostředí](service-fabric-get-started-linux.md). Pokud používáte Mac OS X, můžete k [nastavení linuxového vývojového prostředí ve virtuálním počítači použít Vagrant](service-fabric-get-started-mac.md).

Budete také pro nasazení aplikace chtít nakonfigurovat [Azure CLI 2.0](service-fabric-azure-cli-2-0.md) (doporučeno) nebo [XPlat CLI](service-fabric-azure-cli.md).

<a id="create-the-application" class="xliff"></a>

## Vytvoření aplikace
Aplikace Service Fabric obsahuje jednu nebo víc služeb, z nichž každá má určitou roli při poskytování funkcí aplikace. Service Fabric SDK pro Linux zahrnuje generátor [Yeoman](http://yeoman.io/), který vám usnadní vytvoření první služby a případná další rozšíření později.  Vytvořit, sestavit a nasadit aplikace Service Fabric v Javě můžete také pomocí modulu plug-in pro Eclipse. Viz [Vytvoření a nasazení první aplikace v Javě pomocí Eclipse](service-fabric-get-started-eclipse.md). Pro účely tohoto rychlého startu použijte Yeoman k vytvoření aplikace s jednou službou, která ukládá a získává hodnotu čítače.

1. V terminálu zadejte ``yo azuresfjava``.
2. Pojmenujte svoji aplikaci. 
3. Vyberte typ první služby a pojmenujte ji. Pro účely tohoto kurzu zvolte službu Reliable Actor. Další informace o ostatních typech služeb najdete v tématu [Přehled programovacího modelu Service Fabric](service-fabric-choose-framework.md).
   ![Generátor Service Fabric Yeoman pro Javu][sf-yeoman]

<a id="build-the-application" class="xliff"></a>

## Sestavení aplikace
Šablona Service Fabric Yeoman zahrnuje skript sestavení pro [Gradle](https://gradle.org/), který můžete použít k sestavení aplikace u terminálu. Pokud chcete sestavit a zabalit aplikaci, spusťte následující:

  ```bash
  cd myapp
  gradle
  ```

<a id="deploy-the-application" class="xliff"></a>

## Nasazení aplikace
Jakmile je aplikace sestavená, můžete ji nasadit do místního clusteru.

<a id="using-xplat-cli" class="xliff"></a>

### Použití XPlat CLI

1. Připojte se k místnímu clusteru služby Service Fabric.

    ```bash
    azure servicefabric cluster connect
    ```

2. Spuštěním instalačního skriptu, který je součástí šablony, zkopírujte balíček aplikace do úložiště imagí clusteru, zaregistrujte typ aplikace a vytvořte její instanci.

    ```bash
    ./install.sh
    ```

<a id="using-azure-cli-20" class="xliff"></a>

### Použití Azure CLI 2.0

Nasazení sestavené aplikace je stejné jako u všech ostatních aplikací Service Fabric. Podrobné pokyny najdete v dokumentaci s popisem [správy aplikace Service Fabric pomocí Azure CLI](service-fabric-application-lifecycle-azure-cli-2-0.md).

Parametry těchto příkazů najdete v generovaných manifestech uvnitř balíčku aplikace.

Jakmile je aplikace nasazená, otevřete prohlížeč a přejděte k nástroji [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) na adrese [http://localhost:19080/Explorer](http://localhost:19080/Explorer).
Pak rozbalte uzel **Aplikace** a všimněte si, že už obsahuje položku pro váš typ aplikace a další položku pro první instanci tohoto typu.

<a id="start-the-test-client-and-perform-a-failover" class="xliff"></a>

## Spuštění klienta testování a převzetí služeb při selhání
Samotné objekty actor nic nedělají – vyžadují, aby jim jiná služba nebo klient posílali zprávy. Šablona actor zahrnuje jednoduchý testovací skript, který můžete použít k interakci se službou actor.

1. Spusťte skript pomocí pomocného sledovacího programu a prohlédněte si výstup služby actor.  Testovací skript volá metodu `setCountAsync()` objektu actor pro zvýšení čítače a metodu `getCountAsync()` objektu actor pro získání nové hodnoty čítače, kterou zobrazí v konzole.

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```

2. V nástroji Service Fabric Explorer vyhledejte uzel, který je hostitelem primární repliky pro službu objektu actor. Na snímku níže je to uzel 3. Primární replika služby zpracovává operace čtení a zápisu.  Změny stavu služby se následně replikují do sekundárních replik, které jsou na následujícím snímku obrazovku spuštěné na uzlech 0 a 1.

    ![Vyhledání primární repliky v Service Fabric Exploreru][sfx-primary]

3. V části **Uzly** klikněte na uzel, který jste našli v předchozím kroku, a pak v nabídce Akce vyberte **Deaktivovat (restartovat)**. Tato akce restartuje uzel spuštěný v primární replice služby a vynutí převzetí služeb při selhání jednou ze sekundárních replik spuštěných na jiném uzlu.  Úroveň této sekundární repliky se zvýší na primární, v jiném uzlu se vytvoří jiná sekundární replika a primární replika začne přijímat operace čtení a zápisu. Při restartování uzlu sledujte výstup z testovacího klienta a všimněte si, že se čítač bez ohledu na převzetí služeb při selhání pořád postupně zvyšuje.

<a id="add-another-service-to-the-application" class="xliff"></a>

## Přidání další služby do aplikace
Pokud chcete do existující aplikace přidat další službu pomocí `yo`, proveďte následující kroky:
1. Změňte adresář na kořenovou složku stávající aplikace.  Například `cd ~/YeomanSamples/MyApplication`, pokud `MyApplication` je aplikace vytvořená pomocí Yeomanu.
2. Spusťte `yo azuresfjava:AddService`.
3. Sestavte a nasaďte aplikaci stejně jako v předchozích krocích.

<a id="remove-the-application" class="xliff"></a>

## Odebrání aplikace
Pomocí odinstalačního skriptu, který je součástí šablony, odstraňte instanci aplikace, zrušte registraci balíčku aplikace a odeberete balíček aplikace z úložiště imagí clusteru.

```bash
./uninstall.sh
```

V nástroji Service Fabric Explorer uvidíte, že se aplikace a typ aplikace už nezobrazují v uzlu **Aplikace**.

<a id="next-steps" class="xliff"></a>

## Další kroky
* [Vytvoření první aplikace Service Fabric v Javě v Linuxu pomocí Eclipse](service-fabric-get-started-eclipse.md)
* [Další informace o Reliable Actors](service-fabric-reliable-actors-introduction.md)
* [Komunikace s clustery Service Fabric pomocí Azure CLI](service-fabric-azure-cli.md)
* [Řešení potíží s nasazením](service-fabric-azure-cli.md#troubleshooting)
* Informace o [možnostech podpory pro Service Fabric](service-fabric-support.md)

<a id="related-articles" class="xliff"></a>

## Související články

* [Začínáme s platformou Service Fabric a Azure CLI 2.0](service-fabric-azure-cli-2-0.md)
* [Začínáme se Service Fabric XPlat CLI](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-yeoman.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-java/sfx-primary.png
[sf-eclipse-templates]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-eclipse-templates.png

