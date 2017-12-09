---
title: "Azure IoT Edge Glosář termínů | Microsoft Docs"
description: "Principy – Glosář běžných termínů týkající se Azure IoT okraj."
services: iot-hub
documentationcenter: .net
author: menchi
manager: bzurcher
editor: 
ms.assetid: 16ef29ea-a185-48c3-ba13-329325dc6716
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/06/2017
ms.author: menchi
ms.openlocfilehash: 420a0101565dedf7f2d4b4c8359e35a579aaa4bc
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/09/2017
---
# <a name="glossary-of-iot-edge-terms"></a>Glosář termínů IoT Edge
V tomto článku jsou uvedeny některé běžné termínů používaných v článcích IoT okraj.

## <a name="iot-edge"></a>IoT Edge
Azure IoT Edge umožňuje řízené cloudové nasazení služby Azure a kódu pro konkrétní řešení pro místní zařízení. IoT hraniční zařízení můžete shromažďovat data z jiných zařízení a provádět výpočty a analýzy, předtím, než odešle data do cloudu. Další informace najdete v tématu [Azure IoT Edge](https://docs.microsoft.com/en-us/azure/iot-edge/).

## <a name="iot-edge-agent"></a>Agent IoT Edge
Součást zodpovědná za nasazení a monitorování modulů IoT Edge runtime.

## <a name="iot-edge-device"></a>Zařízení IoT Edge
IoT hraniční zařízení mít IoT Edge nainstalován modul runtime a jsou označeny jako "IoT hraniční zařízení" v podrobností o zařízení. Zjistěte, jak [nasazení Azure IoT Edge v simulovaném zařízení v systému Linux – Náhled](https://docs.microsoft.com/en-us/azure/iot-edge/tutorial-simulate-device-linux).

## <a name="iot-edge-deployment"></a>Nasazení IoT Edge
Nasazení služby IoT Edge nakonfiguruje cíl sadu IoT hraniční zařízení pro spuštění sady IoT Edge moduly. Každé nasazení nepřetržitě zajišťuje, aby všechna zařízení, které odpovídají jeho cílovou podmínku běží Zadaná sada modulů, i když nové zařízení se vytvoří nebo jsou upraveny tak, aby odpovídaly cílovou podmínku. Každé zařízení IoT Edge pouze obdrží nejvyšší prioritou nasazení jejichž cílovou podmínku splňuje. Další informace o [IoT Edge nasazení](https://docs.microsoft.com/en-us/azure/iot-edge/module-deployment-monitoring).

## <a name="iot-edge-deployment-manifest"></a>Okraj IoT – manifest nasazení
Dokument Json obsahující informace, která se mají zkopírovat jeden nebo více IoT hraniční zařízení modulu twin(s) nasadit sadu moduly, trasy a přidružené modulu požadované vlastnosti.

## <a name="iot-edge-gateway-device"></a>Zařízení IoT hraniční brány
IoT hraniční zařízení s podřízené zařízení. Podřízené zařízení může být buď IoT Edge, nebo není IoT hraniční zařízení.

## <a name="iot-edge-hub"></a>Centra IoT Edge
Součást zodpovědná za modulu do modulu komunikace (směrem k IoT Hub) nadřazené a podřízené (mimo IoT Hub) IoT Edge runtime komunikace. 

## <a name="iot-edge-leaf-device"></a>Zařízení IoT okraj listu
IoT hraniční zařízení s žádné podřízené zařízení. 

## <a name="iot-edge-module"></a>Modul IoT Edge
Modul IoT Edge je kontejner Docker, kterou můžete nasadit do zařízení IoT okraj. Provádí konkrétní úlohy, jako je příjem zpráv ze zařízení, transformace zprávu nebo odesílání zprávy do služby IoT hub. Ho komunikuje s z ostatních modulů a odesílání dat do modulu runtime IoT okraj. [Pochopení požadavků a nástrojů pro vývoj modulů IoT Edge](https://docs.microsoft.com/en-us/azure/iot-edge/module-development).

## <a name="iot-edge-module-identity"></a>Okraj IoT modulu identity
Záznam v registru identit služby IoT Hub modul s podrobnostmi o pověření existence a zabezpečení modul používaný k ověření s hraniční rozbočovače nebo IoT Hub.

## <a name="iot-edge-module-image"></a>Obrázek modulu IoT Edge
Docker obrázek, který je použit modulem runtime IoT Edge se vytvořit instanci modulu instancí.

## <a name="iot-edge-module-twin"></a>Okraj IoT modulu twin
Ve službě IoT Hub, která ukládá informace o stavu pro modul instance jako trvalý dokumentu Json. 

## <a name="iot-edge-runtime"></a>Modul runtime IoT Edge
Modul runtime IoT hraniční zahrnuje vše, co Microsoft distribuuje být nainstalovány na IoT hraniční zařízení. Zahrnutý Edge agenta, rozbočovače okraj a okraj CTL nástroj.

## <a name="iot-edge-set-modules-to-a-single-device"></a>Nastavit hraniční IoT modulů na jedno zařízení
Operace, která zkopíruje obsah manifestu IoT Edge na jednom zařízení, dvojici modulu. Základní rozhraní API je obecný použít konfiguraci, což trvá jednoduše manifest IoT okraj jako vstup.

## <a name="priority"></a>Priorita
Při dvou nasazeních IoT Edge cíle do stejného zařízení, získá použít nasazení s vyšší prioritou. Pokud má dvě nasazení se stejnou prioritou, získá použít nasazení s novější datum vytvoření. Další informace o [s prioritou](https://docs.microsoft.com/en-us/azure/iot-edge/module-deployment-monitoring#priority).

## <a name="target-condition"></a>Cílovou podmínku
V nasazení IoT okraj, je cílovou podmínku žádné Boolean podmínka značky dvojčata zařízení vyberte zařízení cíl nasazení, například "tag.environment = prod". Cílovou podmínku nepřetržitě vyhodnotí zahrnout všechny nová zařízení, které splňují požadavky nebo odeberte zařízení, které už provést. Další informace o [cíle stavu](https://docs.microsoft.com/en-us/azure/iot-edge/module-deployment-monitoring#target-condition)


## <a name="next-steps"></a>Další kroky

* V tématu [IoT Hub Glosář](#iot-hub-devguide-glossary.md)
* Další hraniční IoT [vývoj modulu](https://docs.microsoft.com/en-us/azure/iot-edge/iot-edge-modules)
* Další hraniční IoT [nasazení](https://docs.microsoft.com/en-us/azure/iot-edge/module-deployment-monitoring)
