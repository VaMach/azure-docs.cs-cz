---
title: "Poznámky k verzi 0,2 StorSimple 8000 Update | Microsoft Docs"
description: "Popisuje nové funkce a opravy, otevřete problémy a k dispozici řešení pro leden 2015 verze Microsoft Azure StorSimple (Update 0,2)."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: d9684ae3-b38f-4678-9d70-e5dbc6b03350
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/16/2016
ms.author: v-sharos
ms.openlocfilehash: 2fc50f7faddb4b61ea5e7d328469fc3cc0eb6f3e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="storsimple-8000-series-update-02-release-notes---january-2015"></a>Aktualizace řady StorSimple 8000 0,2 poznámky – leden 2015
## <a name="overview"></a>Přehled
Následující poznámky k verzi identifikovat kritická otevřené problémy pro Microsoft Azure StorSimple verze leden 2015. Také obsahují seznam StorSimple software a firmware aktualizace obsažené v této verzi. Toto je druhý verzi, po vydání řady StorSimple 8000 verze byla dostupná obecně červenec 2014.

Tato aktualizace se změní verze softwaru fyzického zařízení z října aktualizace. Nadále se verze 6.3.9600.17312. V této verzi došlo ke změně bitovou kopii používá bitovou kopii virtuálního zařízení. Proto všechny nové virtuální zařízení vytvořené po 20 1. 2015 se zobrazí jako 6.3.9600.17361 verze.  

Přečtěte si následující informace obsažené v poznámkách k verzi pro aktualizaci leden 2015.

> [!IMPORTANT]
> * Tato aktualizace není k dispozici prostřednictvím služby Windows Update a není možné nainstalovat jako jiné aktualizace. Vaše zařízení nebude přijímat tuto aktualizaci i v případě, že jste nainstalovali aktualizace pomocí portálu Azure classic. Tato aktualizace se uplatní jenom na virtuální zařízení vytvořili po 20 leden 2015. 
> * Leden verzi zařízení StorSimple neobsahuje žádné aktualizace fyzického zařízení StorSimple. Můžete také použít všechny dostupné aktualizace systému Windows k virtuálnímu zařízení, včetně poslední opravy zabezpečení, ale neuvidíte změny ve verzi fyzického zařízení StorSimple.
> 
> 

## <a name="whats-new-in-the-january-release"></a>Co je nového ve verzi leden
Tato aktualizace obsahuje opravy související s svazky na virtuálním zařízení přechod do stavu offline. (Viz [problémy opravit v této verzi](#issues-fixed-in-the-january-release).)  

Aktualizace neobsahuje nové funkce nebo funkce.  

## <a name="issues-fixed-in-the-january-release"></a>Chyby v lednu verze
Následující tabulka popisuje problém, který byl opraven v této aktualizaci.

| Ne. | Funkce | Problém | Platí pro fyzické zařízení | Platí pro virtuální zařízení |
| --- | --- | --- | --- | --- |
| 1 |Svazky přechod do stavu offline |Při zachování cloudu vysoké latenci pro několik minut, přejděte na hostitelích offline svazky virtuálního zařízení StorSimple. Tato oprava zvyšuje prahová hodnota latence cloudu, a současně minimalizujete její situacích, které by způsobily svazky, které chcete přejít do režimu offline v hostitelích. |Ne |Ano |

## <a name="known-issues-in-the-january-release"></a>Známé problémy v lednu verzi
Následující tabulka obsahuje souhrn známých problémů v této verzi.

| Ne. | Funkce | Problém | Komentáře nebo alternativní řešení | Platí pro fyzické zařízení | Platí pro virtuální zařízení |
| --- | --- | --- | --- | --- | --- |
| 1 |Obnovení továrního nastavení |V některých případech při provádění obnovení továrního nastavení zařízení StorSimple může být pomalé a zobrazit tato zpráva: **resetovat do výrobního probíhá (fáze 8).** To se stane, když stisknutím CTRL + C, když probíhá rutinu. |Není stiskněte kombinaci kláves CTRL + C po inicializaci obnovení továrního nastavení. Pokud jste již v tomto stavu, kontaktujte prosím Microsoft Support pro další kroky. |Ano |Ne |
| 2 |Disk kvora |Ve výjimečných případech Pokud jsou většinu disků ve skříni EBOD 8600 zařízení odpojeny, což vede k žádné disk kvora, pak fondu úložiště bude offline. Zůstane offline, i když jsou znovu připojeny disky. |Je potřeba restartovat zařízení. Pokud potíže potrvají, kontaktujte prosím Microsoft Support pro další kroky. |Ano |Ne |
| 3 |Selhání snímku cloudu |Ve výjimečných případech cloudový snímek může selhat s chybou **zálohování dosažen limit maximální**. K tomu dochází, je-li být delší než 255 online klony na stejném zařízení, ze stejné původního svazku, který byl odstraněn. | |Ano |Ano |
| 4 |Nesprávný řadiče ID |Když se provádí nahrazení řadiče, může řadič 0 zobrazí jako řadič 1. Při nahrazení řadiče při načtení bitovou kopii z uzlu sdílené ID řadič může zobrazují původně jako ID partnera řadiče.  Ve výjimečných případech může být toto chování vidět také po restartu systému. |Není třeba žádné akce uživatele. Tato situace bude automaticky vyřešen po dokončení nahrazení řadiče. |Ano |Ne |
| 5 |Monitorování grafy zařízení |Ve službě StorSimple Manager grafy monitorování zařízení nefungují, pokud základní nebo je v konfiguraci proxy serveru pro zařízení povolené ověřování protokolem NTLM. |Upravte konfiguraci webového proxy pro toto zařízení zaregistrovali služby StorSimple Manager, že ověřování je nastaven na hodnotu NONE. Chcete-li to provést, spusťte prostředí Windows PowerShell pro rutinu StorSimple Set-HcsWebProxy. |Ano |Ano |
| 6 |Účty úložiště |Pomocí služby úložiště odstranit účet úložiště se o nepodporovaný scénář. To povede k situaci, ve kterém nelze načíst data uživatele. | |Ano |Ano |
| 7 |Převzetí služeb při selhání zařízení |Více převzetí služeb při selhání kontejner svazků ze stejného zdrojového zařízení do jiné cílové zařízení není podporována. |Převzetí služeb při selhání z jednoho neaktivní zařízení do více zařízení bude kontejnery svazků na prvním převzal zařízení dojít ke ztrátě dat vlastnictví. Po takové převzetí služeb při selhání, budou tyto kontejnery svazků zobrazí nebo chovat jinak při zobrazení na portálu Azure classic. |Ano |Ne |
| 8 |Instalace |Během StorSimple adaptéru pro instalaci služby SharePoint je třeba zadat IP zařízení v pořadí pro instalaci úspěšně dokončit. | |Ano |Ne |
| 9 |Webový proxy server |Pokud vaše konfigurace webového proxy serveru má zadaný protokol HTTPS, bude mít vliv na komunikaci služby zařízení a zařízení přejde do režimu offline. Podpora balíčky se budou generovat také v procesu spotřebovávat značné množství prostředků vašeho zařízení. |Ujistěte se, že má adresu URL webového proxy serveru HTTP jako zadaný protokol. Zobrazit další informace o tom, jak [konfigurace webového proxy serveru pro vaše zařízení](storsimple-configure-web-proxy.md). |Ano |Ne |
| 10 |Webový proxy server |Pokud nakonfigurujete a povolíte webový proxy server na registrované zařízení, budete muset restartujte řadič active na vašem zařízení. | |Ano |Ne |
| 11 |Cloud vysokou latencí a vysokou vstupně-výstupní úlohy |Když v zařízení StorSimple dojde kombinaci cloudu velmi vysoké latenci (pořadí sekund) a vysoké vstupně-výstupní úlohy, svazky zařízení přejděte do sníženou stavu a vstupně-výstupních operací může selhat s chybou "zařízení není připraveno". |Je nutné ručně restartovat řadiče zařízení nebo provádět převzetí služeb při selhání zařízení obnovit z této situaci. |Ano |Ne |

## <a name="physical-device-updates-in-the-january-release"></a>Verze aktualizace fyzického zařízení v lednu
Tato aktualizace neobsahuje jiné změny zařízení StorSimple.

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-january-release"></a>Sériově připojené řadič SCSI (SAS) a aktualizace firmwaru v lednu verze
Tato verze neobsahuje žádné aktualizace ke kontroleru rozhraní serial-attached SCSI (SAS) nebo firmwaru. V říjnu 2014 verze byla aktualizace ovladačů. 

## <a name="virtual-device-updates-in-the-january-release"></a>Aktualizace virtuální zařízení v lednu verze
Tato verze obsahuje aktualizovanou bitovou kopii pro virtuální zařízení. Virtuální zařízení vytvořené po 20 leden 2015 se zobrazí jako 6.3.9600.17361 verze softwaru.

