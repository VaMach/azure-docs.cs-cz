---
title: "Začínáme s Azure IoT okraj (Linux) | Microsoft Docs"
description: "Postup vytvoření brány na počítači s Linuxem a popis klíčových konceptů v Azure IoT Edge, jako jsou moduly a konfigurační soubory JSON."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: cf537bdd-2352-4bb1-96cd-a283fcd3d6cf
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/19/2017
ms.author: andbuc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fb65e3c34d2b2a14370792d8506c13c8c5fb522e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="explore-azure-iot-edge-architecture-on-linux"></a>Prozkoumání architektury Azure IoT Edge v Linuxu

[!INCLUDE [iot-hub-iot-edge-getstarted-selector](../../includes/iot-hub-iot-edge-getstarted-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-linux](../../includes/iot-hub-iot-edge-install-build-linux.md)]

## <a name="how-to-run-the-sample"></a>Spuštění ukázky

Skript **build.sh** generuje výstup ve složce **build** v místní kopii úložiště **iot-edge**. Tento výstup zahrnuje dvě IoT Edge moduly používané v této ukázce.

Skript sestavení umístí modul **liblogger.so** do složky **build/modules/logger/** a modul **libhello\_world.so** do složky **build/modules/hello_world/**. Použít tyto cesty pro **modulu cesta** hodnoty, jak je znázorněno v příkladu JSON nastavení souboru.

Hello\_world\_ukázka proces má cestu k konfigurační soubor JSON jako argument příkazového řádku. Následující ukázkový soubor JSON je součástí úložiště sady SDK v umístění **samples/hello\_world/src/hello\_world\_lin.json**. Tento konfigurační soubor funguje, jako je nezměníte skriptu buildu umístit IoT Edge moduly nebo ukázka spustitelné soubory v jiné než výchozí umístění.

> [!NOTE]
> Cesty k modulům jsou relativní vzhledem k aktuálnímu pracovnímu adresáři, ze kterého je spuštěn spustitelný soubor hello\_world\_sample, ne k adresáři, ve kterém je spustitelný soubor umístěn. Ukázkový konfigurační soubor JSON ve výchozím nastavení zapíše do aktuálního pracovního adresáře soubor log.txt.

```json
{
    "modules" :
    [
        {
            "name" : "logger",
            "loader": {
            "name": "native",
            "entrypoint": {
                "module.path": "./modules/logger/liblogger.so"
            }
            },
            "args" : {"filename":"log.txt"}
        },
        {
            "name" : "hello_world",
            "loader": {
            "name": "native",
            "entrypoint": {
                "module.path": "./modules/hello_world/libhello_world.so"
            }
            },
            "args" : null
        }
    ],
    "links":
    [
        {
            "source": "hello_world",
            "sink": "logger"
        }
    ]
}
```

1. Přejděte na **sestavení** složku v kořenovém místní kopii **iot hranou** úložiště.

1. Spusťte následující příkaz:

    ```sh
    ./samples/hello_world/hello_world_sample ../samples/hello_world/src/hello_world_lin.json
    ```

[!INCLUDE [iot-hub-iot-edge-getstarted-code](../../includes/iot-hub-iot-edge-getstarted-code.md)]

