---
title: "Vytvoření první aplikace mikroslužeb Azure v Linuxu pomocí jazyka C# | Dokumentace Microsoftu"
description: "Vytvoření a nasazení aplikace Service Fabric pomocí jazyka C##"
services: service-fabric
documentationcenter: csharp
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: 5a96d21d-fa4a-4dc2-abe8-a830a3482fb1
ms.service: service-fabric
ms.devlang: csharp
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/04/2016
ms.author: subramar
translationtype: Human Translation
ms.sourcegitcommit: 7033955fa9c18b2fa1a28d488ad5268d598de287
ms.openlocfilehash: c810f3e86ba582943e88f3085f6d9cff2496031c


---
# <a name="create-your-first-azure-service-fabric-application"></a>Vytvoření první aplikace Azure Service Fabric
> [!div class="op_single_selector"]
> * [C# – Windows](service-fabric-create-your-first-application-in-visual-studio.md)
> * [Java – Linux](service-fabric-create-your-first-linux-application-with-java.md)
> * [C# – Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
> 
> 

Service Fabric poskytuje sady SDK pro vytváření služeb v Linuxu pomocí .NET Core a Javy. V tomto kurzu si projdeme postup vytvoření aplikace pro Linux a vytvoření služby pomocí jazyka C# (.NET Core).

## <a name="prerequisites"></a>Požadavky
Než začnete, ujistěte se, že máte [v Linuxu nastavené vývojové prostředí](service-fabric-get-started-linux.md). Pokud používáte Mac OS X, můžete k [nastavení linuxového prostředí ve virtuálním počítači použít Vagrant](service-fabric-get-started-mac.md).

## <a name="create-the-application"></a>Vytvoření aplikace
Aplikace Service Fabric může obsahovat jednu nebo víc služeb, z nichž každá má určitou roli při poskytování funkcí aplikace. Service Fabric SDK pro Linux zahrnuje generátor [Yeoman](http://yeoman.io/), který vám usnadní vytvoření první služby a případná další rozšíření později. Pomocí generátoru Yeoman vytvoříme aplikaci s jedinou službou.

1. V terminálu zadejte následující příkaz, který zahájí sestavování základní kostry aplikace: `yo azuresfcsharp`
2. Pojmenujte svoji aplikaci.
3. Vyberte typ první služby a pojmenujte ji. Pro účely tohoto kurzu zvolíme službu Reliable Actor.
   
   ![Generátor Service Fabric Yeoman pro jazyk C##][sf-yeoman]

> [!NOTE]
> Další informace o možnostech najdete v tématu [Přehled programovacího modelu Service Fabric](service-fabric-choose-framework.md).
> 
> 

## <a name="build-the-application"></a>Sestavení aplikace
Šablony generátoru Service Fabric Yeoman zahrnují skript sestavení, který můžete použít k sestavení aplikace z terminálu (po přejití do složky aplikace).

  ```sh
 cd myapp 
 ./build.sh 
  ```

## <a name="deploy-the-application"></a>Nasazení aplikace
Jakmile je aplikace sestavená, můžete ji nasadit do místního clusteru pomocí rozhraní příkazového řádku Azure.

1. Připojte se k místnímu clusteru služby Service Fabric.
   
    ```sh
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
3. Klikněte na uzel, který jste našli v předchozím kroku, a potom v nabídce Akce vyberte **Deaktivovat (restartovat)**. Tato akce restartuje jeden uzel v místním clusteru a vynutí převzetí služeb při selhání jednou ze sekundárních replik spuštěných v jiném uzlu. Při provádění této akce věnujte pozornost výstupu z klienta testování a všimněte si, že se čítač bez ohledu na převzetí služeb při selhání pořád postupně zvyšuje.

## <a name="adding-more-services-to-an-existing-application"></a>Přidání více služeb do stávající aplikace

Pokud chcete přidat další službu do aplikace již vytvořené pomocí `yo`, proveďte následující kroky: 
1. Změňte adresář na kořenovou složku stávající aplikace.  Například `cd ~/YeomanSamples/MyApplication`, pokud `MyApplication` je aplikace vytvořená pomocí Yeomanu.
2. Spusťte `yo azuresfcsharp:AddService`.

## <a name="next-steps"></a>Další kroky
* [Další informace o Reliable Actors](service-fabric-reliable-actors-introduction.md)
* [Komunikace s clustery Service Fabric pomocí rozhraní příkazového řádku Azure](service-fabric-azure-cli.md)
* Informace o [možnostech podpory pro Service Fabric](service-fabric-support.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-csharp/yeoman-csharp.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-csharp/sfx-primary.png



<!--HONumber=Jan17_HO4-->


