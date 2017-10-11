---
title: "StorSimple hardwarové součásti a stav | Microsoft Docs"
description: "Naučte se monitorovat hardwarové součásti zařízení StorSimple pomocí služby StorSimple Manager."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 0d56a2ba-daf0-45ad-9610-8b8712dd5750
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2016
ms.author: alkohli
ms.openlocfilehash: 93df79c9d349c294b692148a19c9d881c4de4f7b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-manager-service-to-monitor-hardware-components-and-status"></a>Použít službu StorSimple Manager k monitorování hardwarové součásti a stav
## <a name="overview"></a>Přehled
Tento článek popisuje různé fyzické a logické součásti v zařízení StorSimple na místě. Také vysvětluje, jak monitorovat stav součásti zařízení pomocí **údržby** stránky ve službě StorSimple Manager. 

**Údržby** stránka zobrazuje stav hardwaru všechny součásti zařízení StorSimple. 

V části Seznam komponent pro 8100 existují tři části, které popisují:

* **Sdílené součásti** – tyto nejsou součástí řadiče, například diskové jednotky, skříň, PCM součásti a teploty PCM řádek napětí a řádek aktuální senzorů.
* **Součásti řadič 0** – komponenty, které jsou umístěné na řadič 0, jako je třeba řadič, SAS expander a konektor, senzory teploty řadiče a různých síťových rozhraní.
* **Součásti řadiči 1** – komponenty, které tvoří řadič 1, podobné těm, které jsou podrobně popsané pro řadič 0.

Zařízení s 8600 má další součásti, které odpovídají skříni rozšířené Bunch disky (EBOD). V seznamu součástí jsou pět oddíly. Z těchto existují tři části, které obsahují součásti ve skříni primární a jsou stejné jako ty, které jsou popsané pro 8100. Existují dvě další části EBOD skříň, které popisují:

* **Sdílené součásti skříň EBOD** – součásti nacházející se v EBOD skříň a PCM, které nejsou součástí EBOD řadiče.
* **Součásti EBOD řadič 0** – komponenty, které jsou umístěné na EBOD skříň 0, jako je například řadičem EBOD senzory teploty expander a konektor a řadič SAS.
* **EBOD řadič 1 součásti** – komponenty, které tvoří EBOD skříň 1, podobně jako tyto podrobné pro EBOD skříň 0.

> [!NOTE]
> **V části stav hardwaru není k dispozici na stránce údržby pro virtuální zařízení StorSimple (1100).**
> 
> 

## <a name="monitor-the-hardware-status"></a>Monitorování stavu hardwaru
Proveďte následující kroky k zobrazení stavu hardwaru součásti zařízení:

1. Přejděte na **zařízení**, vyberte konkrétní zařízení StorSimple. Přejděte do nabídky úrovni zařízení a potom klepněte na tlačítko **údržby**. 
2. Vyhledejte **stavu hardwaru** části a vyberte z dostupných komponentách (jak je popsáno výše). Jednoduše klikněte na šipku předcházející součást štítek, který chcete v seznamu rozbalit a zobrazit stav různých součástí zařízení. Najdete v článku [seznam podrobné komponent pro primární skříň](#component-list-for-primary-enclosure-of-storsimple-device) a [seznam podrobné komponent pro skříni EBOD](#component-list-for-ebod-enclosure-of-storsimple-device).
3. Použijte následující schéma kódování barva interpretace stav součásti:
   
   * **Zelená kontrola** – Denotes **stavu v pořádku** nebo **OK** součásti.
   * **Žlutý** – označuje komponentu v **upozornění** stavu.
   * **Červený vykřičník** – Denotes komponenty, která má **selhání** nebo **potřebuje pozornost** stavu.
   * **Bílé černým textem** – označuje komponenty, která není k dispozici.
4. Pokud narazíte na komponenty, která není součástí **stavu v pořádku** stavu, obraťte se na Microsoft Support. Pokud výstrahy jsou povolené na vašem zařízení, obdržíte e-mailové výstrahy. Pokud potřebujete nahradit komponentu selhání hardwaru, přečtěte si téma [StorSimple hardwarové součásti nahrazení](storsimple-hardware-component-replacement.md).

## <a name="component-list-for-primary-enclosure-of-storsimple-device"></a>Seznam komponent pro primární skříň zařízení StorSimple
Následující tabulka popisuje fyzické a logické součásti, které jsou obsažené v primární skříň z vašeho místního zařízení StorSimple.

| Komponenta | Modul | Typ | Umístění | Pole replaceable jednotka (FRU)? | Popis |
| --- | --- | --- | --- | --- | --- |
| Jednotka ve slotu [0-11] |Diskové jednotky |Fyzické |Shared |Ano |Jeden řádek se zobrazí u všech SSD a HDD disky ve skříni primární. |
| Senzor teploty vedlejším |Skříň |Fyzické |Shared |Ne |Měří teploty v rámci skříň. |
| Senzor teploty střední roviny |Skříň |Fyzické |Shared |Ne |Měří teplota rovinu střední. |
| Zvukové poplašné |Skříň |Fyzické |Shared |Ne |Určuje, zda je funkční subsystém zvukové poplašné v rámci skříň. |
| Skříň |Skříň |Fyzické |Shared |Ano |Indikuje přítomnost skříň. |
| Nastavení skříň |Skříň |Fyzické |Shared |Ne |Odkazuje na přední panel skříň. |
| Řádek napěťovými senzory |PCM |Fyzické |Shared |Ne |Řada řádku napěťovými senzory mít jejich stavu zobrazí, která určuje, zda je měřená napětí v toleranci. |
| Aktuální senzorů řádku |PCM |Fyzické |Shared |Ne |Řada senzorů aktuálního řádku mít jejich stavu zobrazí, která určuje, zda je aktuální měřená v rámci tolerance. |
| Senzory teploty v PCM |PCM |Fyzické |Shared |Ne |Řada senzory teploty, jako je například Inlet a senzory hotspotů mít jejich stavu zobrazí, která udává, zda teplota měřená v rámci tolerance. |
| Zdroj napájení [0-1] |PCM |Fyzické |Shared |Ano |U každého zdroje napájení ve dvou PCMs umístěný zadní zařízení se zobrazí jeden řádek. |
| Chladicí [0-1] |PCM |Fyzické |Shared |Ano |Pro každou čtyři chladicí ventilátory, které se nacházejí ve dvou PCMs se zobrazí jeden řádek. |
| Baterie [0-1] |PCM |Fyzické |Shared |Ano |Zobrazí jeden řádek pro každý stav baterie. zálohování modulů, které jsou v PCM nainstalován. |
| Metis |Není k dispozici |Logické |Shared |Není k dispozici |Zobrazuje stav baterie: jestli potřebují poplatků a se blíží ukončenou životností. |
| Cluster |Není k dispozici |Logické |Shared |Není k dispozici |Zobrazí stav clusteru, který se vytvoří mezi dvěma moduly integrovaného řadiče. |
| Uzel clusteru |Není k dispozici |Logické |Shared |Není k dispozici |Označuje stav řadiče v rámci clusteru. |
| Kvorum clusteru |Není k dispozici |Logické | |Není k dispozici |Indikuje přítomnost většina disku členství ve fondu úložiště pevný disk. |
| Datový prostor pevný disk |Není k dispozici |Logické |Shared |Není k dispozici |Prostor úložiště, který se používá pro data do fondu úložiště jednotku pevného disku (HDD). |
| Správy místa na HDD |Není k dispozici |Logické |Shared |Není k dispozici |Místa vyhrazeného ve fondu HDD úložiště pro úlohy správy. |
| Místo na pevný disk kvora |Není k dispozici |Logické |Shared |Není k dispozici |Místa vyhrazeného ve fondu úložiště pevného disku pro kvorum clusteru. |
| Nahrazení místa na HDD |Není k dispozici |Logické |Shared |Není k dispozici |Místa vyhrazeného ve fondu úložiště HDD určena k nahrazení řadiče. |
| Datový prostor SSD |Není k dispozici |Logické |Shared |Není k dispozici |Prostor úložiště pro data do fondu úložiště SSD jednotky (SSD Solid-State Drive). |
| Místo paměti NVRAM SSD |Není k dispozici |Logické |Shared |Není k dispozici |Prostor úložiště ve fondu úložiště SSD, který je vyhrazen pro logiku paměti NVRAM. |
| Pevný disk fondu úložiště |Není k dispozici |Logické |Shared |Není k dispozici |Zobrazuje stav fondu úložiště logické, který je vytvořený z zařízení pevné disky. |
| Fond úložiště SSD |Není k dispozici |Logické |Shared |Není k dispozici |Zobrazuje stav fondu úložiště logické, který je vytvořený z zařízení SSD. |
| Řadič [0-1] [stavu] |VSTUPNĚ-VÝSTUPNÍCH OPERACÍ |Fyzické |Řadiče |Ano |Zobrazí stav řadiče, a zda je v režimu aktivní nebo pohotovostní v rámci skříň. |
| Senzory teploty v kontroleru |VSTUPNĚ-VÝSTUPNÍCH OPERACÍ |Fyzické |Řadiče |Ne |Řada senzory teploty, například modul vstupně-výstupních operací, teploty procesoru, DIMM a PCIe senzorů mít jejich stavu zobrazí, který označuje, zda je teploty došlo v toleranci. |
| SAS expander |VSTUPNĚ-VÝSTUPNÍCH OPERACÍ |Fyzické |Řadiče |Ne |Označuje stav sériové připojené expander SCSI (SAS), který se používá k připojení k řadiči integrované úložiště. |
| Konektor SAS [0-1] |VSTUPNĚ-VÝSTUPNÍCH OPERACÍ |Fyzické |Řadiče |Ne |Označuje stav každý konektor SAS, který se používá pro připojení k SAS expander integrované úložiště. |
| SBB střední roviny propojení |VSTUPNĚ-VÝSTUPNÍCH OPERACÍ |Fyzické |Řadiče |Ne |Označuje stav střední roviny konektor, který se používá k připojení každého řadiče se střední rovinou. |
| Jádra procesoru |VSTUPNĚ-VÝSTUPNÍCH OPERACÍ |Fyzické |Řadiče |Ne |Označuje stav jader procesoru v rámci každého řadiče. |
| Skříň electronics napájení |VSTUPNĚ-VÝSTUPNÍCH OPERACÍ |Fyzické |Řadiče |Ne |Označuje stav napájení systému používá skříni. |
| Skříň electronics diagnostiky |VSTUPNĚ-VÝSTUPNÍCH OPERACÍ |Fyzické |Řadiče |Ne |Označuje stav subsystémů diagnostiky poskytované kontroleru. |
| Řadič pro správu základní desky (BMC) |VSTUPNĚ-VÝSTUPNÍCH OPERACÍ |Fyzické |Řadiče |Ne |Označuje stav řadič správy základní desky (BMC), což je procesor specializované služby, který monitoruje hardwarové zařízení prostřednictvím senzory a komunikuje s správce systému prostřednictvím nezávislé připojení. |
| Ethernet |VSTUPNĚ-VÝSTUPNÍCH OPERACÍ |Fyzické |Řadiče |Ne |Označuje stav jednotlivých síťových rozhraní, to znamená, správu a porty data k dispozici na kontroleru. |
| PAMĚTI NVRAM |VSTUPNĚ-VÝSTUPNÍCH OPERACÍ |Fyzické |Řadiče |Ne |Označuje stav paměti NVRAM, paměť s náhodným přístupem stálé zálohovaná baterie, která slouží k zachování aplikace důležitých informací v případě výpadku napájení. |

## <a name="component-list-for-ebod-enclosure-of-storsimple-device"></a>Seznam komponent pro EBOD skříň zařízení StorSimple
Následující tabulka popisuje fyzické a logické součásti, které jsou obsažené v skříni EBOD z vašeho místního zařízení StorSimple.

| Komponenta | Modul | Typ | Umístění | FRU? | Popis |
| --- | --- | --- | --- | --- | --- |
| Jednotka ve slotu [0-11] |Diskové jednotky |Fyzické |Shared |Ano |Pro každou HDD disků ve skříni EBOD se zobrazí jeden řádek. |
| Senzor teploty vedlejším |Skříň |Fyzické |Shared |Ne |Měří teploty v rámci skříň. |
| Senzor teploty střední roviny |Skříň |Fyzické |Shared |Ne |Měří teplota rovinu střední. |
| Zvukové poplašné |Skříň |Fyzické |Shared |Ne |Určuje, zda je funkční subsystém zvukové poplašné v rámci skříň. |
| Skříň |Skříň |Fyzické |Shared |Ano |Indikuje přítomnost skříň. |
| Nastavení skříň |Skříň |Fyzické |Shared |Ne |Odkazuje OPS nebo přední panel skříň. |
| Řádek napěťovými senzory |PCM |Fyzické |Shared |Ne |Řada řádku napěťovými senzory mít jejich stavu zobrazí, která určuje, zda je měřená napětí v toleranci. |
| Aktuální senzorů řádku |PCM |Fyzické |Shared |Ne |Řada senzorů aktuálního řádku mít jejich stavu zobrazí, která určuje, zda je aktuální měřená v rámci tolerance. |
| Senzory teploty v PCM |PCM |Fyzické |Shared |Ne |Řada senzory teploty, jako je například Inlet a senzory hotspotů mít jejich stavu zobrazí, která určuje, zda je teplota měřená v toleranci. |
| Zdroj napájení [0-1] |PCM |Fyzické |Shared |Ano |U každého zdroje napájení ve dvou PCMs umístěný zadní zařízení se zobrazí jeden řádek. |
| Chladicí [0-1] |PCM |Fyzické |Shared |Ano |Pro každou čtyři chladicí ventilátory, které se nacházejí ve dvou PCMs se zobrazí jeden řádek. |
| Místní úložiště [HDD] |Není k dispozici |Logické |Shared |Není k dispozici |Zobrazuje stav fondu úložiště logické, který je vytvořený z zařízení pevné disky. |
| Řadič [0-1] [stavu] |VSTUPNĚ-VÝSTUPNÍCH OPERACÍ |Fyzické |Řadiče |Ano |Zobrazí stav řadičů v modulu EBOD. |
| Senzory teploty v EBOD |VSTUPNĚ-VÝSTUPNÍCH OPERACÍ |Fyzické |Řadiče |Ne |Řada senzory teploty z každého řadiče mít jejich stavu zobrazí, který určuje, zda je teploty došlo v rámci tolerance. |
| SAS expander |VSTUPNĚ-VÝSTUPNÍCH OPERACÍ |Fyzické |Řadiče |Ne |Označuje stav expander SAS, který se používá k připojení k řadiči integrované úložiště. |
| Konektor SAS [0-2] |VSTUPNĚ-VÝSTUPNÍCH OPERACÍ |Fyzické |Řadiče |Ne |Označuje stav každý konektor SAS, který se používá pro připojení k SAS expander integrované úložiště. |
| SBB střední roviny propojení |VSTUPNĚ-VÝSTUPNÍCH OPERACÍ |Fyzické |Řadiče |Ne |Označuje stav střední roviny konektor, který se používá k připojení každého řadiče se střední rovinou. |
| Skříň electronics napájení |VSTUPNĚ-VÝSTUPNÍCH OPERACÍ |Fyzické |Řadiče |Ne |Označuje stav napájení systému používá skříni. |
| Skříň electronics diagnostiky |VSTUPNĚ-VÝSTUPNÍCH OPERACÍ |Fyzické |Řadiče |Ne |Označuje stav subsystémů diagnostiky poskytované kontroleru. |
| Připojení k řadiči zařízení |VSTUPNĚ-VÝSTUPNÍCH OPERACÍ |Fyzické |Řadiče |Ne |Označuje stav připojení mezi modul EBOD vstupně-výstupních operací a řadiče zařízení. |

## <a name="next-steps"></a>Další kroky
* Chcete-li používat službu StorSimple Manager ke správě zařízení, přejděte na [použít službu StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).
* Pokud potřebujete vyřešit komponenty zařízení, která je degradovaný nebo ve stavu, podívejte se na [StorSimple monitorování indikátory](storsimple-monitoring-indicators.md). 
* Nahrazení chybných hardwarová komponenta, najdete v části [StorSimple hardwarové součásti nahrazení](storsimple-hardware-component-replacement.md).
* Pokud se nadále potíže zařízení [kontaktovat Microsoft Support](storsimple-contact-microsoft-support.md).

