---
title: "Pochopení ceny služby Azure IoT Hub | Microsoft Docs"
description: "Příručka vývojáře - informace o jak měření a ceny funguje službou IoT Hub, včetně šlo příklady."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 1ac90923-1edf-4134-bbd4-77fee9b68d24
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/29/2017
ms.author: elioda
ms.openlocfilehash: 05006a78cc7d82bc048ec5706465f7140eb40e94
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-iot-hub-pricing-information"></a>Informace o cenách Azure IoT Hub

[Azure IoT Hub ceny] [ lnk-pricing] poskytuje obecné informace o různých SKU a ceny pro centra IoT. Tento článek obsahuje další podrobnosti o tom, jak jsou různé funkce IoT Hub měřené jako zprávy službou IoT Hub.

## <a name="charges-per-operation"></a>Poplatky za operace

| Operace | Informace o fakturaci | 
| --------- | ------------------- |
| Operace registru identit <br/> (vytvořit, získat, seznam, aktualizace, odstranění) | Není účtován. |
| Zprávy typu zařízení-cloud | Úspěšně odeslané zprávy budou účtovat v bloky 4 KB na příjem příchozích dat do služby IoT Hub, například, že zprávu 6KB je účtován 2 zprávy. |
| Zprávy typu cloud zařízení | Budou se účtovat úspěšně odeslaných zpráv v 4 KB bloků, například zprávu 6 KB je účtován 2 zprávy. |
| Nahrávání souborů | Přenos souborů do úložiště Azure není – měření podle objemu službou IoT Hub. Zprávy pro zahájení a ukončení přenosu souborů budou účtovat podle messaged měřené v přírůstcích po 4 KB. Například soubor 10 MB je účtován dvě zprávy kromě náklady na úložiště Azure. |
| Přímé metody | Metoda úspěšné požadavky budou účtovat v bloky 4 KB, odpovědi s neprázdný subjekty účtované v 4 KB jako další zprávy. Požadavky na odpojený zařízení budou účtovat jako zprávy v bloky 4 KB. Například metoda subjektu 6 KB, jejímž výsledkem odpověď se žádný text ze zařízení, účtován jako dvě zprávy. Metoda s 6 KB textu, jejímž výsledkem 1 KB odpověď od zařízení, je účtován jako dvě zprávy pro požadavek plus další zprávu pro odpověď. |
| Čtení dvojčat zařízení | Dvojče zařízení čte ze zařízení a z řešení zpět end účtujeme jako zprávy v bloky o velikosti 512 bajtů. Například čtení twin 6 KB zařízení je účtován jako 12 zprávy. |
| Aktualizace dvojče zařízení (značky a vlastnosti) | Aktualizace zařízení twin ze zařízení a z back-end řešení budou účtovat jako zprávy v bloky o velikosti 512 bajtů. Například čtení twin 6 KB zařízení je účtován jako 12 zprávy. |
| Dotazy twin zařízení | Dotazy budou účtovat jako zprávy v závislosti na velikosti výsledek v bloky o velikosti 512 bajtů. |
| Operace úloh <br/> (vytvoření, aktualizace, výpis, odstranění) | Není účtován. |
| Operace úlohy na zařízení | Budou se účtovat normální úlohy operací (např. aktualizace twin zařízení a metody). Úlohu výsledkem volání metody 1000 s požadavky 1 KB a prázdný text odpovědi je pro instanci účtovat 1000 zprávy. |

> [!NOTE]
> Všech velikostí se vypočítávají zvažování velikost datové části v bajtech (protokol rámcovacích bude ignorován). V případě zprávy (která mají vlastnosti a text) velikost je počítaný způsobem, bez ohledu na protokol, jak je popsáno v [IoT Hub – Příručka vývojáře pro zasílání zpráv][lnk-message-size].

## <a name="example-1"></a>Příklad #1

Zařízení odesílá do služby IoT Hub, který je pak přečte Azure Stream Analytics jednu zprávu typu zařízení cloud 1 KB za minutu. Back-end řešení volá metodu (s odebranou datovou částí 512 bajtů) na zařízení každých 10 minut k aktivaci určité akce. Zařízení reaguje na metodu s tím výsledkem 200 bajtů.

Zařízení spotřebovává 1 zpráva * = 60 minut * 24 hodin 1440 zprávy na každý den pro zprávy typu zařízení cloud a 2 požadavku a odpovědi * 6krát za hodinu * 24 hodin = 288 zprávy pro metody pro celkový počet zpráv. 1728 za den.

## <a name="example-2"></a>Příklad #2

Zařízení odesílá jednu zprávu 100 KB zařízení cloud každou hodinu. Aktualizuje také jeho dvojče zařízení s datových částí 1 KB každé 4 hodiny. Řešení zpět ukončení, jednou za den, čte dvojče zařízení 14 KB a aktualizuje se 512 bajtů datové části, chcete-li změnit konfiguraci.

Zařízení spotřebovává 25 zprávy (100KB nebo 4KB) * 24 hodin pro zprávy typu zařízení cloud, plus 2 zprávy (1KB/0,5 KB) * 6krát na každý den pro aktualizace zařízení twin, celkem 612 zpráv za den.
Back-end řešení využívá 28 zprávy (14KB/0,5 KB) ke čtení dvojče zařízení plus 1 zpráva aktualizace, celkem 29 zprávy.

Celkem zařízení a back-end řešení využívat 641 zpráv za den.


[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[lnk-message-size]: iot-hub-devguide-messages-construct.md
