---
title: "Řešení potíží s nasazené zařízení StorSimple | Microsoft Docs"
description: "Popisuje, jak diagnostikovat a opravit chyby, ke kterým došlo v zařízení StorSimple, který je aktuálně nasazená a funkční."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: ea5d89ae-e379-423f-b68b-53785941d9d0
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/16/2016
ms.author: v-sharos
ms.openlocfilehash: 8d1b4905d0a24c8df9eb2c986459286909fd20dc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-an-operational-storsimple-device"></a>Řešení potíží provozní zařízení StorSimple
## <a name="overview"></a>Přehled
Tento článek obsahuje pokyny k odstraňování problémů užitečná pro řešení problémů s konfigurací, můžete setkat po zařízení StorSimple je nasazen a provozní. Popisuje běžné problémy, možné příčiny a doporučené kroky, aby pomáhal vyřešit problémy, které se můžete setkat při spuštění Microsoft Azure StorSimple. Tyto informace platí pro místní fyzického zařízení StorSimple a virtuální zařízení StorSimple.

Na konci tohoto článku, které můžete najít seznam kódy chyb, které se můžou vyskytnout během operace Microsoft Azure StorSimple a také kroky můžete využít chyby vyřešit. 

## <a name="setup-wizard-process-for-operational-devices"></a>Průvodce instalací pro provozní zařízení
Pomocí Průvodce instalací ([Invoke-HcsSetupWizard][1]) ke kontrole konfigurace zařízení a v případě potřeby proveďte opravné akce.

Když spustíte Průvodce instalací na zařízení dříve nakonfigurované a funkční, průběh zpracování se liší. Můžete změnit pouze následující položky:

* IP adresa, maska podsítě a bránu
* Primární server DNS.
* Primární server NTP
* Konfigurace volitelné webového proxy serveru

Průvodce instalací neprovede operace týkající se shromažďování a zařízení registrace hesla.

## <a name="errors-that-occur-during-subsequent-runs-of-the-setup-wizard"></a>Chyb vzniklých při dalším spuštění Průvodce instalací
Následující tabulka popisuje chyby může dojít, když spustíte Průvodce instalací na provozní zařízení, možné příčiny chyby a doporučené akce, které je vyřešit. 

| Ne. | Chybová zpráva nebo podmínku | Možné příčiny | Doporučená akce |
|:--- |:--- |:--- |:--- |
| 1 |Chyba 350032: Toto zařízení již bylo deaktivováno. |Tato chyba se zobrazí, pokud spustíte Průvodce instalací na zařízení, které je deaktivována. |[Kontaktujte Microsoft Support](storsimple-contact-microsoft-support.md) pro další kroky. Deaktivované zařízení nelze vložit do služby. Předtím, než může být zařízení znovu aktivovat, může být vyžadováno obnovení továrního nastavení. |
| 2 |Invoke-HcsSetupWizard: ERROR_INVALID_FUNCTION (výjimky z HRESULT: 0x80070001) |Aktualizace serveru DNS se nezdařilo. Nastavení DNS jsou globální nastavení a jsou použity napříč všechny povolené síťové rozhraní. |Povolte rozhraní a znovu použít nastavení DNS. To mohou narušit sítě pro ostatní povoleno rozhraní, protože tato nastavení jsou globální. |
| 3 |Zařízení se zobrazí jako online na portálu služby StorSimple Manager, ale při pokusu o dokončení minimální instalace a uložte konfiguraci se operace nezdaří. |Během počáteční instalace nebyla nakonfigurována webový proxy server, i když došlo k skutečné proxy serveru na místě. |Použití [rutiny Test-HcsmConnection] [ 2] Vyhledejte chybu. [Kontaktujte Microsoft Support](storsimple-contact-microsoft-support.md) nejde k odstranění problému. |
| 4 |Invoke-HcsSetupWizard: Hodnota nespadá do očekávaný rozsah. |Maska podsítě nesprávný vytvoří této chybě. Možné příčiny jsou: <ul><li> Maska podsítě je chybí nebo je prázdný.</li><li>Formátu předpony protokolu Ipv6 je nesprávný.</li><li>Rozhraní je povolenou podporu cloudu, ale brána je chybí nebo není správný.</li></ul>Všimněte si, že rozhraní DATA 0 má automaticky povolenou podporu cloudu pokud nakonfigurovaný pomocí Průvodce instalací. |Pokud chcete zjistit problém, použijte podsíť 0.0.0.0 nebo 256.256.256.256 a podívejte se na výstup. Zadejte správné hodnoty pro masku podsítě, bránu a předponu Ipv6, podle potřeby. |

## <a name="error-codes"></a>Kódy chyb
Chyby jsou uvedeny v pořadí.

| Číslo chyby | Popis nebo text chyby | Akce doporučené uživatele |
|:--- |:--- |:--- |
| 10502 |Při přístupu k účtu úložiště došlo k chybě. |Počkejte několik minut a potom akci opakujte. Pokud potíže potrvají, požádejte prosím podporu Microsoftu pro další kroky. |
| 40017 |Operace zálohování se nezdařilo jako svazek, zadaný v zásady zálohování nebyla nalezena v zařízení. |Opakujte zálohování na operace, pokud potíže potrvají, obraťte se na Microsoft Support. pro další kroky. |
| 40018 |Operace zálohování se nezdařilo, jak na zařízení nebyl nalezen žádný z určených do zásad zálohování svazků. |Opakujte zálohování na operace, pokud potíže potrvají, obraťte se na Microsoft Support. pro další kroky. |
| 390061 |Systém je zaneprázdněn nebo není k dispozici. |Počkejte několik minut a potom akci opakujte. Pokud potíže potrvají, požádejte prosím podporu Microsoftu pro další kroky. |
| 390143 |Došlo k chybě s kódem chyby 390143. (Neznámá chyba.) |Pokud potíže potrvají, kontaktujte prosím Microsoft Support pro další kroky. |

## <a name="next-steps"></a>Další kroky
Pokud jste schopni vyřešit problém, [kontaktovat Microsoft Support](storsimple-contact-microsoft-support.md) o pomoc. 

[1]: https://technet.microsoft.com/en-us/%5Clibrary/Dn688135(v=WPS.630).aspx
[2]: https://technet.microsoft.com/en-us/%5Clibrary/Dn715782(v=WPS.630).aspx
