---
title: "Řešení potíží s chybami Docker klienta v systému Windows pomocí sady Visual Studio | Microsoft Docs"
description: "Řešení problémů, ke které dojde při použití sady Visual Studio k vytvoření a nasazení webové aplikace do Docker v systému Windows pomocí sady Visual Studio."
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: 
ms.assetid: 346f70b9-7b52-4688-a8e8-8f53869618d3
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: 89fa04a1107b6abb49aefd68066443717ac9b731
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-visual-studio-docker-development"></a>Řešení potíží s Visual Studio Docker vývoj

Při práci s nástroji Visual Studio Tools pro Docker Preview, se může vyskytnout některé problémy, kvůli povaha ve verzi preview.
Toto jsou některé běžné problémy a řešení.  

## <a name="visual-studio-2017-rc"></a>Visual Studio 2017 RC

### <a name="linux-containers"></a>**Kontejnery Linux**

####  <a name="build-errors-occur-when-debugging-a-net-core-web-or-console-application"></a>Sestavení dojít k chybám při ladění aplikace web nebo konzoly .NET Core  

To může souviset s není sdílení jednotky, které se nachází projektu s Docker pro systém Windows.  Můžete obdržet chybu takto:

```
The "PrepareForLaunch" task failed unexpectedly.
Microsoft.DotNet.Docker.CommandLineClientException: Creating network "webapplication13628050196_default" with the default driver
Building webapplication1
Creating webapplication13628050196_webapplication1_1
ERROR: for webapplication1  Cannot create container for service webapplication1: C: drive is not shared. Please share it in Docker for Windows Settings
```
K vyřešení tohoto problému:

1. Klikněte pravým tlačítkem na **Docker pro systém Windows** v oznamovací oblasti a potom vyberte **nastavení**.  
2. Vyberte **sdílené disky** a sdílet jednotku, kde se nachází projektu.

### <a name="windows-containers"></a>**Kontejnery Windows**

Následující problémy jsou specifické pro ladění rozhraní .NET Framework web a konzoly aplikací v kontejnerech systému Windows.

#### <a name="prerequisites"></a>Požadavky

1. Visual Studio 2017 RC (nebo novější) s .NET Core a Docker Preview musí být nainstalován zatížení.
2. Windows 10 Anniversary aktualizaci s aby Windows Update nejnovější opravy. Konkrétně [KB3194798](https://support.microsoft.com/en-us/help/3194798/cumulative-update-for-windows-10-version-1607-and-windows-server-2016-october-11,-2016) musí být nainstalován. 
3. [Docker pro systém Windows](https://docs.docker.com/docker-for-windows/) (sestavení 1.13.0 nebo novější) musí být nainstalován.
4. **Přepnout na Windows kontejnery** musí být vybrán. V oznamovací oblasti, klikněte na tlačítko **Docker pro systém Windows**a potom vyberte **přepnout do kontejnerů Windows**. Po restartování počítače, ujistěte se, že toto nastavení je zachován.

#### <a name="console-output-does-not-appear-in-visual-studios-output-window-while-debugging-a-console-application"></a>Výstup konzoly se nezobrazí v okně výstupu sady Visual Studio při ladění aplikace konzoly

Jedná se o známý problém s ladicím programu sady Visual Studio (msvsmon.exe), který není aktuálně určený pro tento scénář. Podpora pro tento scénář mohou být zahrnuta v budoucí verzi. Pokud chcete zobrazit výstup z konzolové aplikace v sadě Visual Studio, použijte **Docker: spuštění projektu**, což je totéž jako **spustit bez ladění**.

#### <a name="debugging-web-applications-with-the-release-configuration-fails-with-403-forbidden-error"></a>Ladění webových aplikací s se verze konfigurace nezdaří s chybou (403) zakázáno

Chcete-li tento problém obejít, otevřete web.release.config v řešení a komentář nebo odstranit následující řádky:

```
<compilation xdt:Transform="RemoveAttributes(debug)" />
```

## <a name="visual-studio-2015"></a>Visual Studio 2015

### <a name="linux-containers"></a>**Kontejnery Linux**

#### <a name="unable-to-validate-volume-mapping"></a>Nelze ověřit mapování svazku
Svazek mapování je potřeba zdrojový kód a binární soubory aplikace nasdílet složky aplikace v kontejneru.  Mapování konkrétní svazku jsou obsaženy v rámci docker compose.dev.debug.yml a docker compose.dev.release.yml. Jak se změnily soubory na hostitelském počítači, kontejnery podle těchto změn v podobné struktura složek.

Chcete-li povolit mapování svazku:

1. Klikněte na tlačítko **Moby** v oznamovací oblasti a vyberte **nastavení**.
2. Vyberte **sdílené disky**.
3. Vyberte disk, který je hostitelem projektu a jednotky, kde % USERPROFILE % umístěna.
4. Klikněte na tlačítko **Použít**.

Chcete-li otestovat, pokud je svazek mapování funkce, znovu sestavit a vyberte F5 z Visual Studia po jedné nebo více jednotek byly sdílené, nebo z příkazového řádku spusťte následující.

> [!NOTE]
> Tento příklad předpokládá, že vaši uživatelé složka je umístěna na jednotce C a zda je sdílen.
> Upraveno v případě potřeby, pokud jste sdíleli jinou jednotku.

```
docker run -it -v /c/Users/Public:/wormhole busybox
```

Spusťte následující kód v kontejneru systému Linux.

```
/ # ls
```

Měli byste vidět ve složce Uživatelé nebo veřejné výpis adresáře. Pokud jsou zobrazeny žádné soubory a složky /c/Users/Public není prázdný, mapování svazek není správně nakonfigurována.

```
bin       etc       proc      sys       usr       wormhole
dev       home      root      tmp       var
```

Přejděte do adresáře díry na Zobrazit obsah `/c/Users/Public` directory:

```
/ # cd wormhole/
/wormhole # ls
AccountPictures  Downloads        Music            Videos
Desktop          Host             NuGet.Config     desktop.ini
Documents        Libraries        Pictures
/wormhole #
```

> [!NOTE]
> Při práci s virtuálními počítači Linux, systém souborů kontejneru je malá a velká písmena.

## <a name="build-prepareforbuild-task-failed-unexpectedly"></a>Sestavení: Úlohy "PrepareForBuild" se nezdařilo neočekávaně

Microsoft.DotNet.Docker.CommandLine.ClientException: Došlo k chybě při pokusu o připojení.

Ověřte, zda je spuštěna výchozího Docker hostitele. Otevřete příkazový řádek a spusťte:

```
docker info
```

Pokud tento příkaz vrátí chybu, pak se pokusíte spustit **Docker pro systém Windows** aplikace na ploše. Pokud aplikace na ploše se běží, pak **Moby** by měly jít vidět v oznamovací oblasti. Klikněte pravým tlačítkem na **Moby** a otevřete **nastavení**. Klikněte na tlačítko **resetovat**a pak restartujte Docker.

## <a name="an-error-dialog-occurs-when-attempting-to-add-docker-support-or-debug-f5-an-aspnet-core-application-in-a-container"></a>Dialogové okno chyby dojde při pokusu o přidání podpory Docker nebo ladění aplikace ASP.NET Core v kontejneru (F5)

Po odinstalaci a instalace rozšíření, může být poškozen mezipaměti Managed Extensibility Framework (MEF) v sadě Visual Studio. Pokud k tomu dojde, může to způsobit různé chybové zprávy při se přidání podpory Docker nebo pokusu o spuštění nebo ladění aplikace ASP.NET Core (F5). To dočasně vyřešit použijte následující postup k odstranění a znovu vygenerovat mezipaměť MEF.

1. Zavřete všechny instance sady Visual Studio.
1. Otevřete % USERPROFILE%\AppData\Local\Microsoft\VisualStudio\14.0\.
1. Odstraňte následující složky:
     ```
       ComponentModelCache
       Extensions
       MEFCacheBackup
    ```
1. Otevřete sadu Visual Studio.
1. Tento scénář pokus opakujte.
