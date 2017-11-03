---
title: "Začínáme s Azure IoT okraj (Windows) | Microsoft Docs"
description: "Postup vytvoření Azure IoT vstupní brána na počítači s Windows a další informace o klíčových konceptů služby Azure IoT okraj jako jsou moduly a konfigurační soubory JSON."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 9aff3724-5e4e-40ec-b95a-d00df4f590c5
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/29/2017
ms.author: andbuc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 262963250921ceb6f1a2fed0f68c36a9f052b47b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="explore-azure-iot-edge-architecture-on-windows"></a>Prozkoumejte Azure IoT Edge architektura v systému Windows

[!INCLUDE [iot-hub-iot-edge-getstarted-selector](../../includes/iot-hub-iot-edge-getstarted-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-windows](../../includes/iot-hub-iot-edge-install-build-windows.md)]

## <a name="run-the-sample"></a>Spustit ukázku

**Build.cmd** skript generuje jeho výstup v **sestavení** složky ve vaší místní kopii **iot hranou** úložiště. Tento výstup zahrnuje mnoho souborů, ale tato ukázka se zaměřuje na tři:
- Dva moduly hraniční IoT: **sestavení\\moduly\\protokolovač\\ladění\\logger.dll** a **sestavení\\moduly\\hello_world\\ Ladění\\hello\_world.dll**. 
- Spustitelný soubor: **sestavení\\ukázky\\hello\_world\\ladění\\hello\_world\_sample.exe**. Tento proces používá konfigurační soubor JSON jako argument příkazového řádku.

Čtvrtý soubor, který používáte v této ukázce není ve složce sestavení, ale byl zahrnut při klonovat úložiště hranou iot:
- Konfigurační soubor JSON: **ukázky\\hello\_world\\src\\hello\_world\_win.json**. Tento soubor obsahuje cesty na dva moduly. Také deklaruje, kde logger.dll zapíše výstup do. Výchozí hodnota je **log.txt** v aktuální pracovní adresář. 

   >[!NOTE]
   >Pokud přesunete ukázka moduly, nebo přidejte vlastní pro testování, aktualizovat **module.path** hodnoty v konfiguračním souboru tak, aby odpovídaly. Jsou modulu cesty relativní k adresáři kde **hello\_world\_sample.exe** nachází. 

Pokud chcete spustit ukázku, postupujte takto:

1. Přejděte na **sestavení** složku v kořenovém místní kopii **iot hranou** úložiště.

1. Spusťte následující příkaz:

   ```cmd
   samples\hello_world\Debug\hello_world_sample.exe ..\samples\hello_world\src\hello_world_win.json
   ```

1. Tento výstup znamená, že ukázce je spuštěna úspěšně:

   ```cmd
   gateway successfully created from JSON
   gateway shall run until ENTER is pressed
   ```
  
1. Stiskněte **Enter** klíč se zastavit proces.


[!INCLUDE [iot-hub-iot-edge-getstarted-code](../../includes/iot-hub-iot-edge-getstarted-code.md)]
