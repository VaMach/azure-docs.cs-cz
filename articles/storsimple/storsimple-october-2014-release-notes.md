---
title: "Poznámky k verzi 0,1 StorSimple 8000 Update | Microsoft Docs"
description: "Popisuje nové funkce a opravy, otevřete problémy a k dispozici řešení pro října 2014 verze Microsoft Azure StorSimple (Update 0.1)."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: fd35e3c3-4770-460c-999d-f72ab7053a20
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/21/2016
ms.author: alkohli
ms.openlocfilehash: 4dfd3973593a94adfc15a6e15d69c697e13998af
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="storsimple-8000-series-update-01-release-notes--october-2014"></a>Aktualizace řady StorSimple 8000 0,1 poznámky – říjen 2014
## <a name="overview"></a>Přehled
Následující poznámky k verzi identifikovat kritická otevřené problémy pro aktualizaci řady StorSimple 8000 0,1 vydané v října 2014. Také obsahují seznam StorSimple software a firmware aktualizace obsažené v této verzi. Toto je první verzi, po vydání řady StorSimple 8000 verze byla dostupná obecně červenec 2014 a odpovídá verzi softwaru 6.3.9600.17312.  

Doporučujeme, abyste vyhledat a použít všechny dostupné aktualizace ihned po instalaci zařízení. Můžete také zapnout automatické aktualizace stáhnout a nainstalovat důležité aktualizace od společnosti Microsoft při jejich vydání. Další informace najdete v tématu Jak [aktualizace zařízení StorSimple](storsimple-update-device.md).  

Přečtěte si informace obsažené v poznámkách k verzi před nasazením aktualizace v řešení StorSimple.  

> [!IMPORTANT]
> * Instalace aktualizací říjen pomocí služby StorSimple Manager a není Windows Powershellu pro StorSimple.  
> * Aktualizace obvykle trvat přibližně 3 hodiny.  
> * Říjen verzi zařízení StorSimple neobsahuje žádné aktualizace pro virtuální zařízení StorSimple. Můžete také použít všechny dostupné aktualizace systému Windows, včetně poslední opravy zabezpečení, ale neuvidíte změny ve verzi pro virtuální zařízení.  
> 
> 

Ujistěte se, že jsou splněné následující předpoklady před aktualizací zařízení StorSimple.  

* Ujistěte se, že oba řadiče zařízení běží před aktualizací. Pokud není spuštěna buď řadiče, kontrola selže. Ověřte, zda jsou řadiče jsou v dobrém stavu, přejděte na **stavu hardwaru** pod **údržby** stránky. Pokud jsou komponenty, **vyžadují pozornost**, obraťte se na Microsoft Support soubory protokolů.  
* Zajistěte, aby pevné IP adresy pro obě řadič 0 a řadič 1 jsou směrovatelné a může připojit k Internetu, jak se používají k doručování aktualizací do zařízení. Můžete použít [rutiny Test-Connection](https://technet.microsoft.com/library/hh849808.aspx) testování známé adresy mimo síť, jako jsou outlook.com, ověřte, že je řadič má připojení k vnější síti.  
* Ujistěte se, že požadované Odchozí porty jsou k dispozici v zařízení StorSimple pro odchozí komunikaci. Další informace najdete v tématu [požadavcích sítě pro zařízení StorSimple](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).  
* Pokud je zařízení software verze starší než 6.3.9600.17312 (října 2014 aktualizace), zakažte Data 2 a Data 3 porty, pokud je povoleno, před spuštěním aktualizace. Pokud necháte Data 2 nebo Data 3 porty povolené při instalaci aktualizace, může to způsobit řadiči zařízení uvést do režimu obnovení. Upozorňujeme, že při zakázání síťových rozhraní přidružené svazky budou do režimu offline a vstupně-výstupní operace se přeruší pro dobu aktualizace.  

## <a name="whats-new-in-the-october-release"></a>Co je nového ve verzi z října
Tato aktualizace obsahuje následující vylepšení:

* Nyní můžete službu StorSimple Manager uživatelského rozhraní pro správu řadičů zařízení. Správy, které zahrnují akce restartování, vypnutí, nebo zapnout řadiči. Další informace, přejděte na [řadiče zařízení StorSimple spravovat](storsimple-manage-device-controller.md).  
* Můžete naplánovat přidělení šířky pásma sítě WAN podle kombinace den v týdnu a čas během dne. To umožňuje lepší využití šířky pásma sítě WAN mimo špičku. Různé šířky pásma šablony jsou povoleny pro kontejnery jiný svazek. Další informace, přejděte na [správu vašich šablon šířky pásma StorSimple](storsimple-manage-bandwidth-templates.md).  
* Můžete nakonfigurovat e-mailová oznámení proaktivně upozornit správci a jiné existující nebo které by mohly mít nadcházející problémy. Další informace, přejděte na [konfigurace nastavení výstrah](storsimple-manage-alerts.md#configure-alert-settings).  

## <a name="issues-fixed-in-the-october-release"></a>Chyby v říjnu verze
Následující tabulka obsahuje souhrn problémy, které jsme vyřešili v této aktualizaci.  

| Ne. | Funkce | Problém | Platí pro fyzické zařízení | Platí pro virtuální zařízení |
| --- | --- | --- | --- | --- |
| 1 |Síťová rozhraní |V předchozí verzi byly v softwaru prohodily síťová rozhraní DATA 2 a DATA 3. Tato chyba byla opravena v této aktualizaci. Vymažte nastavení a před instalací aktualizace zakažte tato síťová rozhraní. Po instalaci aktualizace, budete muset znovu nakonfigurujte tato rozhraní. |Ano |Ne |
| 2 |Balíček pro podporu |V předchozí verzi, pokud jste spustili prostředí Windows PowerShell **Export HcsSupportPackage** protokoluje rutiny načíst řadič správy základní desky (BMC), operace se nezdařila s následujícím upozorněním: "operaci úspěšně na tomto řadiči, ale na řadičem sdílené z důvodu následující chyby se nezdařila. Zkontrolujte zda partnerem je v pořádku a zda aktuálního uzlu se může připojit k partnerem." Tento problém je teď vyřešený. |Ano |Ne |
| 3 |Převzetí služeb při selhání zařízení |V předchozí verzi došlo riziko nekonzistenci dat. Pokud **zjistit zálohování** úloha se nezdařila při selhání zařízení. Tento problém je teď vyřešený. |Ano |Ne |
| 4 |Převzetí služeb při selhání zařízení |V předchozí verzi, po selhání zařízení zálohy byla viditelná, ale nebyla k dispozici na cílovém zařízení kontejneru přidružených svazků. Tento problém je teď vyřešený. |Ano |Ne |
| 5 |Převzetí služeb při selhání zařízení |V předchozí verzi se chyby ve výčtu zálohování do cloudu během operace obnovení registru, který může potenciálně vést k nekonzistenci dat. Pokud byly nějaké problémy s připojením k cloudu. |Ano |Ne |
| 6 |Aktualizace firmwaru |V předchozí verzi úloha aktualizace firmwaru zařízení se nezdařila a zobrazí chybu, která uvádí, že nebyly rozpoznatelném rutiny, a v důsledku selhání aktualizace. Kdy řadič pak přešel do režimu obnovení. Tento problém je teď vyřešený. |Ano |Ne |
| 7 |Instalace |Nyní bylo opraveno chyby způsobené zařízení není vytvoření bitové kopie správně během instalace. |Ano |Ne |
| 8 |Obnovení továrního nastavení |Nyní můžete volitelně Přeskočit kontrolu firmware obnovení továrního nastavení. Jedná se o změnu z předchozí verze. |Ano |Ne |
| 9 |Obnovení továrního nastavení |Při spuštění rutiny resetování objekt pro vytváření, v předchozí verzi provedeny kontroly verze firmwaru jenom pro některé součásti hardwaru. Další firmware kontroly byly provedeny po prvním restartování v procesu, což by mohlo způsobit, že aby se resetování nezdaří. Tato oprava zajistí, že všechny kontroly firmware jsou vytvářeny, pokud objekt factory resetovat rutiny spuštění a před první systém restartovat. |Ano |Ne |
| 10 |Střídání klíče účtu úložiště |**Invoke-HcsmServiceDataEncryptionKeyChange** rutina, kterou používá otočení klíče účtu úložiště teď vyzve uživatele k zadání šifrovacího klíče dat služby. Jedná se o změnu z předchozí verze, ve kterém šifrovacího klíče dat služby byl předán jako parametr vložené. |Ano |Ne |
| 11 |Navrácení služeb po obnovení do 24 hodin |Během zotavení po havárii čištění na zdrojového zařízení nebyly provedeny této aplikace, což způsobilo navrácení služeb po obnovení nezdaří. Tato chyba byla opravena v této verzi. |Ano |Ne |

## <a name="known-issues-in-the-october-release"></a>Známé problémy ve verzi z října
Následující tabulka obsahuje souhrn známých problémů v této verzi.

| Ne. | Funkce | Problém | Komentáře nebo alternativní řešení | Platí pro fyzické zařízení | Platí pro virtuální zařízení |
| --- | --- | --- | --- | --- | --- |
| 1 |Obnovení továrního nastavení |V některých případech při provádění obnovení továrního nastavení zařízení StorSimple může být pomalé a zobrazit tato zpráva: **resetovat do výrobního probíhá (fáze 8)**. To se stane, když stisknutím CTRL + C, když probíhá rutinu. |Není stiskněte kombinaci kláves CTRL + C po inicializaci obnovení továrního nastavení. Pokud jste již v tomto stavu, kontaktujte prosím Microsoft Support pro další kroky. |Ano |Ne |
| 2 |Obnovení továrního nastavení |Proveďte není obnovení továrního nastavení zařízení StorSimple, která je aktualizována z GA na října 2014 verzi. |Tato operace bude fungovat pouze pokud je nainstalovaná opravy. Kontaktujte Microsoft Support získat tato požadovaná oprava. |Ano |Ne |
| 3 |Disk kvora |Ve výjimečných případech Pokud jsou většinu disků ve skříni EBOD 8600 zařízení odpojeny, což vede k žádné disk kvora, pak fondu úložiště bude offline. Zůstane offline, i když jsou znovu připojeny disky. |Je potřeba restartovat zařízení. Pokud potíže potrvají, kontaktujte prosím Microsoft Support pro další kroky. |Ano |Ne |
| 4 |Selhání snímku cloudu |Ve výjimečných případech cloudový snímek může selhat s chybou **zálohování dosažen limit maximální**. K tomu dochází, je-li být delší než 255 online klony na stejném zařízení, ze stejné původního svazku, který byl odstraněn. | |Ano |Ano |
| 5 |Nesprávný řadiče ID |Když se provádí nahrazení řadiče, může řadič 0 zobrazí jako řadič 1. Při nahrazení řadiče při načtení bitovou kopii z uzlu sdílené ID řadič může zobrazují původně jako ID partnera řadiče. Ve výjimečných případech může být toto chování vidět také po restartu systému. |Není třeba žádné akce uživatele. Tato situace bude automaticky vyřešen po dokončení nahrazení řadiče. |Ano |Ne |
| 6 |Monitorování grafy zařízení |Ve službě StorSimple Manager grafy monitorování zařízení nefungují, pokud základní nebo je v konfiguraci proxy serveru pro zařízení povolené ověřování protokolem NTLM. |Upravte konfiguraci webového proxy pro toto zařízení zaregistrovali služby StorSimple Manager, že ověřování je nastaven na hodnotu NONE. Chcete-li to provést, spusťte prostředí Windows PowerShell pro rutinu StorSimple Set-HcsWebProxy. |Ano |Ano |
| 7 |Účty úložiště |Pomocí služby úložiště odstranit účet úložiště se o nepodporovaný scénář. To povede k situaci, ve kterém nelze načíst data uživatele. | |Ano |Ano |
| 8 |Převzetí služeb při selhání zařízení |Více převzetí služeb při selhání kontejner svazků ze stejného zdrojového zařízení do jiné cílové zařízení není podporována. |Převzetí služeb při selhání z jednoho neaktivní zařízení do více zařízení bude kontejnery svazků na prvním převzal zařízení dojít ke ztrátě dat vlastnictví. Po takové převzetí služeb při selhání, budou tyto kontejnery svazků zobrazí nebo chovat jinak při zobrazení na portálu Azure classic. |Ano |Ne |
| 9 |Instalace |Během StorSimple adaptéru pro instalaci služby SharePoint je třeba zadat IP zařízení v pořadí pro instalaci úspěšně dokončit. | |Ano |Ne |
| 10 |Webový proxy server |Pokud vaše konfigurace webového proxy serveru má zadaný protokol HTTPS, bude mít vliv na komunikaci služby zařízení a zařízení přejde do režimu offline. Podpora balíčky se budou generovat také v procesu spotřebovávat značné množství prostředků vašeho zařízení. |Ujistěte se, že má adresu URL webového proxy serveru HTTP jako zadaný protokol. Další informace o tom, jak [konfigurace webového proxy serveru pro vaše zařízení](storsimple-configure-web-proxy.md). |Ano |Ne |
| 11 |Webový proxy server |Pokud nakonfigurujete a povolíte webový proxy server na registrované zařízení, budete muset restartujte řadič active na vašem zařízení. | |Ano |Ne |
| 12 |Cloud vysokou latencí a vysokou vstupně-výstupní úlohy |Když v zařízení StorSimple dojde kombinaci cloudu velmi vysoké latenci (pořadí sekund) a vysoké vstupně-výstupní úlohy, svazky zařízení přejděte do sníženou stavu a vstupně-výstupních operací může selhat s chybou "zařízení není připraveno". |Je nutné ručně restartovat řadiče zařízení nebo provádět převzetí služeb při selhání zařízení obnovit z této situaci. |Ano |Ne |

## <a name="physical-device-updates-in-the-october-release"></a>Verze aktualizace fyzického zařízení v říjnu
Pokud tyto aktualizace jsou použity k fyzické zařízení, verze softwaru se změní na 6.3.9600.17312. Pokud není uvedeno jinak, tyto poznámky k verzi platí pro všechny modely zařízení StorSimple. Další informace o těchto aktualizací najdete v tématu [aktualizace softwaru fyzického zařízení října 2014 pro Microsoft Azure StorSimple zařízení](http://support.microsoft.com/kb/2986997).  

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-october-release"></a>Sériově připojené řadič SCSI (SAS) a aktualizace firmwaru v říjnu verze
Tato verze aktualizuje ovladače a firmware na kontroler SAS fyzického zařízení. Další informace o aktualizaci řadiče SAS najdete v tématu [aktualizace – říjen 2014 pro řadičů LSI SAS v Microsoft Azure StorSimple zařízení](http://support.microsoft.com/kb/2987020).   

Tato verze se týká také firmware kumulativní aktualizace, která řeší problémy se spolehlivosti s hardwarové součásti zařízení. Další informace o aktualizaci firmwaru najdete v tématu [aktualizaci firmwaru října 2014 pro Microsoft Azure StorSimple zařízení](http://support.microsoft.com/kb/2987015).  

## <a name="virtual-device-updates-in-the-october-release"></a>Aktualizace virtuální zařízení v říjnu verze
Tato verze neobsahuje žádné aktualizace pro virtuální zařízení. Použití této aktualizace se nezmění softwaru verzi virtuálního zařízení.

