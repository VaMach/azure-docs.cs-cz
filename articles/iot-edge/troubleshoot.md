---
title: "Řešení Azure IoT Edge | Microsoft Docs"
description: "Řešení běžných potíží a další dovednosti při řešení potíží pro službu Azure IoT Edge"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 12/15/2017
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: cb998caf35a9a55ea737cc1a24fbce38aac8abc4
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/16/2017
---
# <a name="common-issues-and-resolutions-for-azure-iot-edge"></a>Běžné problémy a řešení Azure IoT Edge

Pokud máte problémy s Azure IoT Edge ve vašem prostředí, použijte tento článek jako vodítko pro odstraňování potíží a řešení. 

## <a name="standard-diagnostic-steps"></a>Standardní kroků diagnostiky 

Pokud narazíte na problém, další informace o stavu zařízení IoT hraniční kontrolou kontejneru protokoly a zprávy, které předat do a ze zařízení. Pomocí nástrojů pro příkazy a v této části můžete získat informace. 

* Podívejte se na protokoly kontejnery docker rozpoznat problémy. Začínat nasazené kontejnerů, potom si prohlédněte kontejnerů, které tvoří runtime hraniční IoT: Agent okraj a okraj rozbočovače. Protokoly Edge agenta obvykle poskytují informace na lifecylce každý kontejneru. Protokoly Edge rozbočovače poskytují informace o zasílání zpráv a směrování. 

   ```cmd
   docker logs <container name>
   ```

* Zobrazení zpráv průchodu přes hraniční rozbočovače a shromažďovat přehledy o aktualizace vlastností zařízení s podrobné protokoly z kontejnerů modulu runtime.

   ```cmd
   iotedgectl setup --runtime-log-level DEBUG
   ```

* Pokud máte problémy s připojením, zkontrolujte proměnných prostředí hraniční zařízení jako řetězec připojení vašeho zařízení:

   ```cmd
   docker exec edgeAgent printenv
   ```

Můžete také zkontrolovat zprávy odesílané mezi IoT Hub a IoT hraniční zařízení. Tyto zprávy zobrazit pomocí [Azure IoT Toolkit](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) rozšíření pro Visual Studio Code. Další informace najdete v části [užitečné nástroje při vývoji s Azure IoT](https://blogs.msdn.microsoft.com/iotdev/2017/09/01/handy-tool-when-you-develop-with-azure-iot/).

Po prozkoumání zprávy informace a protokolování, můžete také zkusit restartování runtime Azure IoT Edge:

   ```cmd
   iotedgectl restart
   ```

## <a name="edge-agent-stops-after-about-a-minute"></a>Hraniční Agent přestane po o několik minut

Edge agenta spustí a úspěšném spuštění pro o několik minut a pak zastaví. Protokoly znamenat, že Edge Agent se pokouší o připojení ke službě IoT Hub přes AMQP, a pak se pokusíte připojit pomocí protokolu AMQP přes protokol websocket přibližně 30 sekund později. Pokud to nepomůže, ukončí agenta okraj. 

Příklad Edge agenta protokoly:

```
2017-11-28 18:46:19 [INF] - Starting module management agent. 
2017-11-28 18:46:19 [INF] - Version - 1.0.7516610 (03c94f85d0833a861a43c669842f0817924911d5) 
2017-11-28 18:46:19 [INF] - Edge agent attempting to connect to IoT Hub via AMQP... 
2017-11-28 18:46:49 [INF] - Edge agent attempting to connect to IoT Hub via AMQP over WebSocket... 
```

### <a name="root-cause"></a>Příčiny
Síťové konfigurace v síti hostitele brání dosažení sítě hraniční agenta. Agent se pokusí o připojení prostřednictvím protokolu AMQP (port 5671) nejdřív. Pokud tato možnost selže, pokusí technologie websockets (port 443).

Modul runtime IoT Edge nastaví sítě pro každou z modulů komunikovat na. V systému Linux je tato síť síťový most? víc informací. V systému Windows používá adres (NAT) Tento problém je dnes běžné na zařízeních s Windows pomocí Windows kontejnery, které NAT sítě. 

### <a name="resolution"></a>Řešení
Zkontrolujte, zda je trasa k Internetu pro IP adresy přiřazené k této síti most/NAT. Byly případy, kdy konfigurace VPN na hostiteli přepsání IoT hraniční síti. 

## <a name="edge-hub-fails-to-start"></a>Hraniční rozbočovače nepodaří spustit

Hraniční rozbočovače nepodaří spustit a výtisků následující zpráva do protokolů: 

```
One or more errors occurred. 
(Docker API responded with status code=InternalServerError, response=
{\"message\":\"driver failed programming external connectivity on endpoint edgeHub (6a82e5e994bab5187939049684fb64efe07606d2bb8a4cc5655b2a9bad5f8c80): 
Error starting userland proxy: Bind for 0.0.0.0:443 failed: port is already allocated\"}\n) 
```

### <a name="root-cause"></a>Příčiny
Jiný proces na hostitelském počítači má vázán port 443. Mapuje rozbočovače Edge porty 5671 a 443 pro použití v scénáře brány. Toto mapování portů se nezdaří, pokud jiný proces je již vázána na tento port. 

### <a name="resolution"></a>Řešení
Najděte a zastavit proces, který používá port 443. Tento proces je obvykle webový server.

## <a name="edge-agent-cant-access-a-modules-image-403"></a>Hraniční Agent nemá přístup k bitové kopie modulu (403)
Kontejner se nepodaří spustit, a protokolu agenta Edge zobrazit Chyba 403. 

### <a name="root-cause"></a>Příčiny
Hraniční Agent nemá oprávnění pro přístup k bitové kopie modul. 

### <a name="resolution"></a>Řešení
Pokuste se o spuštění `iotedgectl login` příkaz znovu.

## <a name="next-steps"></a>Další kroky
Myslíte nalezen chyby v platformy IoT okraj? [Ohlásit problém](https://github.com/Azure/iot-edge/issues) tak, aby abychom mohli pokračovat ke zlepšení. 
