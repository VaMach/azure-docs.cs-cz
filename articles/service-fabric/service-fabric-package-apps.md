---
title: "Balíček služby Azure Fabric app | Microsoft Docs"
description: "Jak balíčku aplikace Service Fabric před nasazením do clusteru."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: mani-ramaswamy
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: ryanwi
ms.openlocfilehash: 486a27d7ca576c8fe1552c02eb24ece6b8bb2ba8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="package-an-application"></a>Balíček aplikace
Tento článek popisuje, jak připravit pro nasazení a balíček aplikace Service Fabric.

## <a name="package-layout"></a>Balíček rozložení
Manifest aplikace, jeden nebo více manifesty služby a další potřebný balíček soubory musí být uspořádány v konkrétní rozložení pro nasazení do clusteru Service Fabric. Manifesty příklad v tomto článku by musela být uspořádány do následující adresářovou strukturu:

```
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat
```

Složky jsou pojmenované tak, aby odpovídala **název** atributy každý odpovídající element. Například, pokud service manifest obsažené dvě kód balíčků s názvy **MyCodeA** a **MyCodeB**, pak dvě složky se stejnými názvy by obsahovat potřebné binární soubory pro každý balíček kódu.

## <a name="use-setupentrypoint"></a>Použití SetupEntryPoint
Typické scénáře použití **SetupEntryPoint** když budete muset spustit spustitelný soubor před spuštěním služby nebo je třeba provést operace s vyššími oprávněními. Například:

* Nastavení a inicializace proměnných prostředí, které potřebuje spustitelný soubor služby. Není omezeno na napsané pomocí Service Fabric programovací modely pouze spustitelné soubory. Například npm.exe musí některé proměnné prostředí nakonfigurované pro nasazení aplikace node.js.
* Nastavení řízení přístupu nainstalováním certifikáty zabezpečení.

Další informace o tom, jak nakonfigurovat **SetupEntryPoint**, najdete v části [nakonfigurovat zásady pro bod služby instalační položka](service-fabric-application-runas-security.md)

<a id="Package-App"></a>
## <a name="configure"></a>Konfigurace
### <a name="build-a-package-by-using-visual-studio"></a>Vytvoření balíčku pomocí sady Visual Studio
Pokud chcete vytvořit aplikaci používáte Visual Studio 2015, můžete příkaz balíček automaticky vytvořit balíček, který odpovídá rozložení popsané výše.

Chcete-li vytvořit balíček, klikněte pravým tlačítkem na projekt aplikace v Průzkumníku řešení a vyberte příkaz balíček, jak je uvedeno níže:

![Zabalení aplikace pomocí sady Visual Studio][vs-package-command]

Po dokončení balení můžete najít umístění balíčku **výstup** okno. Krok balení probíhá automaticky při nasazení nebo ladění aplikace v sadě Visual Studio.

### <a name="build-a-package-by-command-line"></a>Vytvoření balíčku pomocí příkazového řádku
Je také možné programově zabalit aplikace pomocí `msbuild.exe`. Pod pokličkou Visual Studio je spuštěna ho tak, aby výstup stejné.

```shell
D:\Temp> msbuild HelloWorld.sfproj /t:Package
```

## <a name="test-the-package"></a>Testování balíčku
Struktura balíček místně pomocí prostředí PowerShell můžete ověřit pomocí [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) příkaz.
Tento příkaz kontroluje manifest analýza problémů a ověřte všechny odkazy. Příkaz pouze ověří správnost strukturální adresářů a souborů v balíčku.
Není ověřte žádný obsah balíčku kódu nebo data mimo kontrola, zda jsou přítomné všechny potřebné soubory.

```
PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
False
Test-ServiceFabricApplicationPackage : The EntryPoint MySetup.bat is not found.
FileName: C:\Users\servicefabric\AppData\Local\Temp\TestApplicationPackage_7195781181\nrri205a.e2h\MyApplicationType\MyServiceManifest\ServiceManifest.xml
```

Tato chyba ukazuje, že *MySetup.bat* souboru, kterou se odkazuje v service manifest **SetupEntryPoint** chybí balíček kódu. Po přidání souboru chybí předá ověření aplikace:

```
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │       MySetup.bat
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat

PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
True
PS D:\temp>
```

Pokud má vaše aplikace [parametry aplikace](service-fabric-manage-multiple-environment-app-configuration.md) definovat, můžete předat je do [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) pro správné ověření.

Pokud znáte clusteru, kde bude aplikace nasazena, je vhodné předáte v `ImageStoreConnectionString` parametr. V takovém případě balíček také ověřovat na předchozí verze této aplikace, které jsou již spuštěny v clusteru. Například ověření může zjistit, zda balíček se stejnou verzí, ale už nasazená jiný obsah.  

Jakmile se aplikace správně zabalený a ověřením úspěšně projde, vyhodnocení na základě velikosti a počtu souborů, které je podle potřeby komprese.

## <a name="compress-a-package"></a>Komprimovat balíčku
Pokud se balíček je velký nebo má mnoho souborů, můžete je Komprimovat pro rychlejší nasazení. Komprese snižuje počet souborů a velikost balíčku.
Pro komprimovaný aplikace balíček [nahrávání balíčku aplikace](service-fabric-deploy-remove-applications.md#upload-the-application-package) trvat delší dobu, ve srovnání s odesílání dekomprimaci balíčku (speciálně. Pokud se započítá komprese čas), ale [registrace](service-fabric-deploy-remove-applications.md#register-the-application-package) a [zrušením registrace typu aplikace](service-fabric-deploy-remove-applications.md#unregister-an-application-type) je rychlejší balíčku komprimované aplikace.

Tento mechanismus nasazení je stejný pro komprimované a nekomprimované balíčky. Pokud je komprimovaný balíček, je jako takový uložený v úložišti clusteru bitové kopie a je nekomprimovaným na uzlu před spuštěním aplikace.
Komprese nahradí platný balíček Service Fabric komprimované verze. Složku musíte povolit oprávnění k zápisu. Komprese systémem již zkomprimovaného balíčku vypočítá žádné změny.

Balíček lze komprimovat spuštěním příkazu prostředí Powershell [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) s `CompressPackage` přepínače. Můžete dekomprimovat balíček se stejným příkaz `UncompressPackage` přepínače.

Následující příkaz komprimaci balíček bez kopírování do úložiště bitové kopie. Můžete zkopírovat zkomprimovaného balíčku na jeden nebo více clusterů Service Fabric, podle potřeby, pomocí [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) bez `SkipCopy` příznak.
Nyní zahrnuje komprimované soubory pro balíček `code`, `config`, a `data` balíčky. Manifest aplikace a služby manifestech nejsou ZIP, protože jsou potřebné pro mnoho interní operace (jako je balíček sdílení, aplikace zadejte název a verze extrakce pro určité ověření).
Pomocí formátu ZIP manifesty by proveďte, tyto operace neefektivní.

```
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │       MySetup.bat
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat
PS D:\temp> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath .\MyApplicationType -CompressPackage -SkipCopy

PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
       ServiceManifest.xml
       MyCode.zip
       MyConfig.zip
       MyData.zip

```

Alternativně můžete zkomprimovat a zkopírujte balíček s [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) v jednom kroku.
Pokud balíček je rozsáhlý, zadejte dostatečně vysoký vypršení časového limitu umožňující čas pro balíček komprese a nahrávání do clusteru.
```
PS D:\temp> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath .\MyApplicationType -ApplicationPackagePathInImageStore MyApplicationType -ImageStoreConnectionString fabric:ImageStore -CompressPackage -TimeoutSec 5400
```

Interně Service Fabric vypočítá kontrolní součty pro balíčky aplikací pro ověření. Při použití komprese, kontrolní součty se vypočítávají v komprimované verze jednotlivých balíčků.
Pokud jste zkopírovali nekomprimované verzi balíčku aplikace a chcete používat kompresi u stejného balíčku, musíte změnit verze `code`, `config`, a `data` balíčků, aby se zabránilo neshoda kontrolního součtu. Pokud jsou balíčky beze změny, místo změníte verzi, můžete použít [rozdílové zřizování](service-fabric-application-upgrade-advanced.md). Tato možnost, nebudou obsahovat beze změny balíček místo něj odkazovat z manifestu služby.

Podobně pokud jste nahráli komprimované verze balíčku a chcete použít dekomprimaci balíčku, musíte aktualizovat verze, aby se zabránilo neshoda kontrolního součtu.

Balíček je nyní zabalené správně, ověřit a komprimované (v případě potřeby), tak, aby byl připraven na [nasazení](service-fabric-deploy-remove-applications.md) na jeden nebo více clusterů Service Fabric.

### <a name="compress-packages-when-deploying-using-visual-studio"></a>Komprimovat balíčky při nasazení pomocí sady Visual Studio
Můžete určit, aby chcete komprimovat balíčků na nasazení, přidáním Visual Studio `CopyPackageParameters` elementu, který chcete profil publikování a nastavte `CompressPackage` atribut `true`.

``` xml
    <PublishProfile xmlns="http://schemas.microsoft.com/2015/05/fabrictools">
        <ClusterConnectionParameters ConnectionEndpoint="mycluster.westus.cloudapp.azure.com" />
        <ApplicationParameterFile Path="..\ApplicationParameters\Cloud.xml" />
        <CopyPackageParameters CompressPackage="true"/>
    </PublishProfile>
```

## <a name="next-steps"></a>Další kroky
[Nasazení a odebírat aplikace] [ 10] popisuje, jak pomocí prostředí PowerShell ke správě instance aplikace

[Správa aplikací parametry pro prostředí s více] [ 11] popisuje postup konfigurace parametrů a proměnných prostředí pro instance jinou aplikaci.

[Konfigurovat zásady zabezpečení pro vaši aplikaci] [ 12] popisuje, jak ke spouštění služeb v rámci zásad zabezpečení, které omezují přístup.

<!--Image references-->
[vs-package-command]: ./media/service-fabric-package-apps/vs-package-command.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md
