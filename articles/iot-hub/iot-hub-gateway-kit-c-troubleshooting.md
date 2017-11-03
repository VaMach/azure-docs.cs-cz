---
title: "SensorTag zařízení & brány Azure IoT – řešení potíží s | Microsoft Docs"
description: "Řešení potíží s stránky pro bránu Intel NUC"
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "problémy s IOT, internet věcí problémů"
ROBOTS: NOINDEX
ms.assetid: 5f742c38-0e33-465a-9a0d-1e41e8d17187
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 7e80951de55ade6b5140608dcff8ebb064f942ca
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshooting"></a>Řešení potíží

## <a name="hardware-issues"></a>Problémy s hardwarem

### <a name="ti-sensortag-cannot-be-connected"></a>Nelze se připojit TI SensorTag

Chcete-li vyřešit potíže s připojením k SensorTag, použijte [SensorTag aplikace](http://processors.wiki.ti.com/index.php/SensorTag_User_Guide#SensorTag_App_user_guide).

### <a name="have-an-issue-with-intel-nuc"></a>Jít o problém s Intel NUC

Odstraňování problémů, spouštění, najdete v tématu [řešení potíží s žádný spouštěcí na Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/000005845.html).

Odstraňování problémů, operační systém, najdete v tématu [řešení potíží s operačního systému na Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/000006018.html).

Řešení potíží s další problémy, najdete v tématu [Blink kódy a zvukový signál kódy pro Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/intel-nuc-boards/000005854.html).

## <a name="nodejs-package-issues"></a>Problémy balíčku Node.js

### <a name="no-response-during-gulp-tasks"></a>Žádná odpověď během gulp úlohy

Pokud narazíte na problémy ve spuštěné úkoly gulp, můžete přidat `--verbose` možnost pro ladění. Zkuste ukončit aktuální úlohy gulp pomocí `Ctrl + C`a potom spusťte následující příkaz v okně konzoly zobrazíte zprávy ladění. Může se zobrazit podrobné chybové zprávy ve výstupu konzoly.

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a>Problémy zjišťování zařízení

Pomoc při řešení běžných potíží s `discover-sensortag` příkazu, zkontrolujte [stránce wikiwebu](https://wiki.archlinux.org/index.php/bluetooth#Bluetoothctl).

### <a name="npm-issues"></a>npm problémy

Došlo k pokusu o aktualizaci vašeho balíčku npm spuštěním následujícího příkazu:

```bash
npm install -g npm
```

Pokud problém přetrvává, nechte komentáře na konci tohoto článku nebo vytvořte potíže Githubu v našem [úložiště ukázkové](https://github.com/azure-samples/iot-hub-c-intel-nuc-gateway-getting-started).

## <a name="remote-debugging"></a>Vzdálené ladění
> Následující pokyny jsou určené pro ladění node.js skripty použité v tomto kurzu.
### <a name="run-the-sample-application-in-debug-mode"></a>Spuštění ukázkové aplikace v režimu ladění

Spuštění ukázkové aplikace v režimu ladění tak, že spustíte následující příkaz:

```bash
gulp run --debug
```

Když modul ladění je připraven, měli byste vidět `Debugger listening on port 5858` ve výstupu konzoly.

### <a name="configure-visual-studio-code-to-connect-to-the-remote-device"></a>Konfigurace Visual Studio Code pro připojení k vzdálené zařízení

1. Otevřete **ladění** panely na levé straně.
2. Klikněte na tlačítko se zeleným **spustit ladění** tlačítko (F5). Otevře se Visual Studio Code `launch.json` souboru.
3. Aktualizace `launch.json` soubor s následujícím obsahem. Nahraďte `[device hostname or IP address]` s skutečné zařízení IP adresu nebo název hostitele.

   ``` json
   {
     "version": "0.2.0",
     "configurations": [
        {
            "name": "Attach",
            "type": "node",
            "request": "attach",
            "port": 5858,
            "address": "[device hostname or IP address]",
            "restart": false,
            "sourceMaps": false,
            "outDir": null,
            "localRoot": "${workspaceRoot}",
            "remoteRoot": "~/ble_sample"
        }
     ]
   }
   ```

![Konfigurace vzdáleného ladění](./media/iot-hub-gateway-kit-lessons/troubleshooting/remote_debugging_configuration.png)

### <a name="attach-to-the-remote-application"></a>Připojení k vzdálené aplikaci

Klikněte na tlačítko se zeleným **spustit ladění** (F5) tlačítko Spustit ladění.

Čtení [JavaScript v produktu VS Code](https://code.visualstudio.com/docs/languages/javascript#_debugging) Další informace o ladicího programu.

![Ukázka zakázat ladění](./media/iot-hub-gateway-kit-lessons/troubleshooting/debugging_ble_sample.png)

## <a name="azure-cli-issues"></a>Azure CLI problémy

Rozhraní příkazového řádku Azure (Azure CLI) je buildu preview. K vyhledání řešení, můžete použít [průvodci instalaci Preview](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md).

Pokud narazíte na všechny chyby pomocí nástroje souboru [problém](https://github.com/Azure/azure-cli/issues) v **problémy** část úložiště GitHub.

Pomoc při řešení běžných potíží, najdete [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).

Pokud jsou splněny "Nelze najít na verzi, která splňuje požadavek", spusťte následující příkaz pro upgrade na nejnovější verzi pip.

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a>Problémy instalace Python

### <a name="legacy-installation-issues-macos"></a>Problémy instalace starší verze (macOS)

Když instalujete pip, oprávnění chyba se vyvolá, když starší balíčky jsou nainstalované s **su** oprávnění. K této situaci dochází, protože předchozí instalaci jazyka Python pomocí brew (macOS) není zcela odinstalována. Některé balíčky pip z předchozí instalace byly vytvořeny pomocí kořenového, což způsobí, že chyba oprávnění. Řešení je odebrat tyto balíčky nainstalované pomocí kořenového. Tuto úlohu dokončit pomocí následujících kroků:

1. Přejděte na `/usr/local/lib/python2.7/site-packages`.
2. Seznam balíčky vytvořené pomocí kořenového:`ls -l | grep root`
3. Odinstalaci balíčků z kroku 2:`sudo rm -rf {package name}`
4. Přeinstalujte Python.

## <a name="azure-iot-hub-issues"></a>Azure IoT Hub problémy

Pokud jste úspěšně zřídit služby Azure IoT hub pomocí Azure CLI, musíte nástroj pro správu zařízení, které se připojují ke službě IoT hub, zkuste následující nástroje.

### <a name="device-explorer"></a>Průzkumník zařízení

[Průzkumník zařízení](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) spouští na místní počítač se systémem Windows a připojí se ke službě IoT hub v Azure. Komunikuje s následující [koncové body centra IoT](https://azure.microsoft.com/en-us/documentation/articles/iot-hub-devguide/):

- Správa identit zařízení a zřizovat a spravovat zařízení zaregistrován u služby IoT hub.
- Zobrazí zařízení cloud, můžete monitorovat zprávy odeslané ze zařízení do služby IoT hub.
- Cloud zařízení odešlete, tak mohou zasílat zprávy do zařízení ze služby IoT hub.

Konfigurace připojovacího řetězce IoT hub v rámci tohoto nástroje můžete použít všechny jeho možnosti.

### <a name="iothub-explorer"></a>iothub-explorer

[iothub-explorer](https://github.com/Azure/iothub-explorer) je ukázkový nástroj víceplatformového rozhraní příkazového řádku ke správě klientů zařízení. Nástroj můžete použít ke správě zařízení v registru identit, sledování zpráv typu zařízení cloud a odesílat příkazy typu cloud zařízení.

Chcete-li nainstalovat nejnovější verzi (předprodejní) nástroj iothub Průzkumník, spusťte následující příkaz:

```bash
npm install -g iothub-explorer@latest
```

Chcete-li získat další nápovědu k všechny příkazy iothub-explorer a jejich parametrů, spusťte následující příkaz:

```bash
iothub-explorer help
```

### <a name="the-azure-portal"></a>Portál Azure

Úplné rozhraní příkazového řádku prostředí umožňuje vytvářet a spravovat všechny prostředky Azure. Můžete také použít [portál Azure](https://azure.microsoft.com/en-us/documentation/articles/azure-portal-overview/) pomoci zřizování, spravovat a ladit vašich prostředků Azure.

## <a name="azure-storage-issues"></a>Azure problémů s úložištěm

[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com/) je samostatná aplikace od společnosti Microsoft, který můžete použít pro práci s daty Azure Storage ve Windows, systému macOS a Linux. Pomocí tohoto nástroje můžete připojit k tabulku a zobrazit data v ní. Tento nástroj slouží k řešení potíží vašeho úložiště Azure.
