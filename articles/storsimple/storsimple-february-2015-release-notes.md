---
title: "Poznámky k verzi 0,3 StorSimple 8000 Update | Microsoft Docs"
description: "Popisuje nové funkce a opravy, otevřete problémy a k dispozici řešení pro únor 2015 verze Microsoft Azure StorSimple (Update 0.3)."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: b01bfd04-f9f8-45f4-ade8-95ac2b979e6a
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/18/2016
ms.author: v-sharos
ms.openlocfilehash: c059fd74854813615754e67547497b7ededbe4dd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="storsimple-8000-series-update-03-release-notes---february-2015"></a>Aktualizace řady StorSimple 8000 0,3 poznámky – únor 2015
## <a name="overview"></a>Přehled
Následující poznámky k verzi identifikovat kritická otevřené problémy pro aktualizaci řady StorSimple 8000 0,3 vydané v únor 2015. Také obsahují seznam StorSimple software a firmware aktualizace obsažené v této verzi. Toto je třetí verzi, po vydání řady StorSimple 8000 verze byla dostupná obecně červenec 2014.

Tato aktualizace z ledna aktualizace verze softwaru zařízení nezmění. Nadále se verze 6.3.9600.17312. Můžete ověřit, zda byla nainstalována aktualizace kontrolou **poslední aktualizovat** datum. Pokud je datum 2/10/2015 nebo novější, pak aktualizace byl úspěšně nainstalován.  

Doporučujeme, abyste vyhledat a použít všechny dostupné aktualizace ihned po instalaci zařízení StorSimple. Můžete také zapnout automatické aktualizace stáhnout a nainstalovat důležité aktualizace od společnosti Microsoft při jejich vydání. Další informace najdete v tématu [aktualizace zařízení StorSimple](storsimple-update-device.md).  

Přečtěte si informace obsažené v poznámkách k verzi před nasazením aktualizace v řešení StorSimple.  

> [!IMPORTANT]
> * K instalaci aktualizace února pomocí služby StorSimple Manager a není Windows Powershellu pro StorSimple.   
> * Jak dlouho trvá přibližně jednu hodinu nainstalovat tuto aktualizaci. Ale při instalaci kumulativní aktualizace, proces může trvat přibližně 3 hodiny dokončit.  
> * Února verzi zařízení StorSimple neobsahuje žádné aktualizace pro virtuální zařízení StorSimple. Můžete také použít všechny dostupné aktualizace systému Windows k virtuálnímu zařízení, včetně poslední opravy zabezpečení, ale neuvidíte změny ve verzi pro virtuální zařízení.  
> 
> 

Ujistěte se, že jsou splněny následující požadavky před aktualizací zařízení StorSimple.  

* Ujistěte se, že oba řadiče zařízení běží před aktualizací. Pokud není spuštěna buď řadiče, kontrola selže. Ověřte, zda jsou řadiče jsou v dobrém stavu, přejděte na **stavu hardwaru** pod **údržby** stránky. Pokud jsou komponenty, **vyžadují pozornost**, obraťte se na Microsoft Support soubory protokolů.
* Zajistěte, aby pevné IP adresy pro řadič 0 a řadič 1 jsou směrovatelné a může připojit k Internetu, jak se používají k doručování aktualizací do zařízení. Můžete použít [rutiny Test-Connection](https://technet.microsoft.com/library/hh849808.aspx) testování známé adresy mimo síť, jako jsou outlook.com, ověřte, že je řadič má připojení k vnější síti.
* Ujistěte se, že porty 80 a 443 jsou k dispozici v zařízení StorSimple pro odchozí komunikaci. Další informace najdete v tématu [požadavcích sítě pro zařízení StorSimple](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).
* Pokud je zařízení software verze starší než 6.3.9600.17312 (října 2014 aktualizace), zakažte Data 2 a Data 3 porty, pokud je povoleno, před spuštěním aktualizace. Ponechat Data 2 nebo Data 3 porty povolit, pokud byla aktualizace může dojít k řadiči zařízení uvést do režimu obnovení. Upozorňujeme, že při zakázání síťových rozhraní přidružené svazky budou do režimu offline a vstupně-výstupních operací se přeruší pro dobu aktualizace.  

## <a name="whats-new-in-the-february-release"></a>Co je nového ve verzi února
Tato aktualizace obsahuje opravy pro objekt pro vytváření resetování problém, který došlo k chybě v zařízení, kteří upgradovali z GA verze verzi z října 2014. Další informace najdete v tématu [problémy opravit v této verzi](#issues-fixed-in-the-february-release).   

Tato aktualizace neobsahuje nové funkce nebo funkce.  

## <a name="issues-fixed-in-the-february-release"></a>Chyby v únoru verze
Následující tabulka popisuje problém, který byl opraven v této aktualizaci.  

| Ne. | Funkce | Problém | Platí pro fyzické zařízení | Platí pro virtuální zařízení |
| --- | --- | --- | --- | --- |
| 1 |Obnovení továrního nastavení |Pokusíte obnovit tovární nastavení v zařízení, které původně obsahovala nainstalovaná verze GA (verze 6.3.9600.17215), ale byl aktualizovaný, aby říjnu verze (verze 6.3.9600.17312). Objekt factory resetovat selže a nestabilní zařízení. |Ano |Ne |

## <a name="known-issues-in-the-february-release"></a>Známé problémy v únoru verzi
Následující tabulka obsahuje souhrn známých problémů v této verzi.

| Ne. | Funkce | Problém | Komentáře nebo alternativní řešení | Platí pro fyzické zařízení | Platí pro virtuální zařízení |
| --- | --- | --- | --- | --- | --- |
| 1 |Obnovení továrního nastavení |V některých případech při provádění obnovení továrního nastavení zařízení StorSimple může být pomalé a zobrazit tato zpráva: **resetovat do výrobního probíhá (fáze 8)**. To se stane, když stisknutím CTRL + C, když probíhá rutinu. |Není stiskněte kombinaci kláves CTRL + C po inicializaci obnovení továrního nastavení. Pokud jste již v tomto stavu, kontaktujte prosím Microsoft Support pro další kroky. |Ano |Ne |
| 2 |Disk kvora |Ve výjimečných případech Pokud většinu disků ve skříni EBOD z 8600device odpojeny, což vede k žádné disk kvora, pak fondu úložiště bude v režimu offline. Zůstane offline, i když jsou znovu připojeny disky. |Je potřeba restartovat zařízení. Pokud potíže potrvají, kontaktujte prosím Microsoft Support pro další kroky. |Ano |Ne |
| 3 |Selhání snímku cloudu |Ve výjimečných případech cloudový snímek může selhat s chybou **zálohování dosažen limit maximální**. K tomu dochází, je-li být delší než 255 online klony na stejném zařízení, ze stejné původního svazku, který byl odstraněn. | |Ano |Ano |
| 4 |Nesprávný řadiče ID |Když se provádí nahrazení řadiče, může řadič 0 zobrazí jako řadič 1. Při nahrazení řadiče při načtení bitovou kopii z uzlu sdílené ID řadič může zobrazují původně jako ID partnera řadiče. Ve výjimečných případech může být toto chování vidět také po restartu systému. |Není třeba žádné akce uživatele. Tato situace bude automaticky vyřešen po dokončení nahrazení řadiče. |Ano |Ne |
| 5 |Monitorování grafy zařízení |Ve službě StorSimple Manager grafy monitorování zařízení nefungují, pokud základní nebo je v konfiguraci proxy serveru pro zařízení povolené ověřování protokolem NTLM. |Upravte konfiguraci webového proxy pro toto zařízení zaregistrovali služby StorSimple Manager, že ověřování je nastaven na hodnotu NONE. Chcete-li to provést, spusťte prostředí Windows PowerShell pro rutinu StorSimple Set-HcsWebProxy. |Ano |Ano |
| 6 |Účty úložiště |Pomocí služby úložiště odstranit účet úložiště se o nepodporovaný scénář. To povede k situaci, ve kterém nelze načíst data uživatele. | |Ano |Ano |
| 7 |Převzetí služeb při selhání zařízení |Více převzetí služeb při selhání kontejner svazků ze stejného zdrojového zařízení do jiné cílové zařízení není podporována.    Převzetí služeb při selhání z jednoho neaktivní zařízení do více zařízení bude kontejnery svazků na prvním převzal zařízení dojít ke ztrátě dat vlastnictví. Po takové převzetí služeb při selhání, budou tyto kontejnery svazků zobrazí nebo chovat jinak při zobrazení na portálu Azure classic. | |Ano |Ne |
| 8 |Instalace |Během StorSimple adaptéru pro instalaci služby SharePoint je třeba zadat IP zařízení v pořadí pro instalaci úspěšně dokončit. | |Ano |Ne |
| 9 |Webový proxy server |Pokud vaše konfigurace webového proxy serveru má zadaný protokol HTTPS, bude mít vliv na komunikaci služby zařízení a zařízení přejde do režimu offline. Podpora balíčky se budou generovat také v procesu spotřebovávat značné množství prostředků vašeho zařízení. |Ujistěte se, že má adresu URL webového proxy serveru HTTP jako zadaný protokol. Další informace o tom, jak [konfigurace webového proxy serveru pro vaše zařízení](storsimple-configure-web-proxy.md). |Ano |Ne |
| 10 |Webový proxy server |Pokud nakonfigurujete a povolíte webový proxy server na registrované zařízení, budete muset restartujte řadič active na vašem zařízení. | |Ano |Ne |
| 11 |Cloud vysokou latencí a vysokou vstupně-výstupní úlohy |Když v zařízení StorSimple dojde kombinaci cloudu velmi vysoké latenci (pořadí sekund) a vysoké vstupně-výstupní úlohy, svazky zařízení přejděte do sníženou stavu a vstupně-výstupních operací může selhat s chybou "zařízení není připraveno". |Je nutné ručně restartovat řadiče zařízení nebo provádět převzetí služeb při selhání zařízení obnovit z této situaci. |Ano |Ne |

## <a name="physical-device-updates-in-the-february-release"></a>Aktualizace fyzického zařízení v z února verze
Tato aktualizace řeší problém resetování objekt pro vytváření, ke kterým došlo na zařízení, kteří upgradovali z GA verze verzi z října 2014. Neobsahuje žádné další aktualizace zařízení StorSimple.  

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-february-release"></a>Sériově připojené řadič SCSI (SAS) a aktualizace firmwaru v z února verze
Tato verze neobsahuje žádné aktualizace ke kontroleru rozhraní serial-attached SCSI (SAS) nebo firmwaru. V říjnu 2014 verze byla aktualizace ovladačů.  

## <a name="virtual-device-updates-in-the-february-release"></a>Aktualizace virtuální zařízení v z února verze
Tato verze neobsahuje žádné aktualizace pro virtuální zařízení. Použití této aktualizace se nezmění softwaru verzi virtuálního zařízení.

