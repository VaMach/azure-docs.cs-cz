---
title: "Nainstalujte aktualizace 4 na zařízení StorSimple | Microsoft Docs"
description: "Popisuje postup instalace zařízení StorSimple 8000 řady aktualizace 4 na vašem zařízení řady StorSimple 8000."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/30/2017
ms.author: alkohli
ms.openlocfilehash: 7dc2f29db64218b9db17eaf555c43a7ad282ea07
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="install-update-4-on-your-storsimple-device"></a>Nainstalujte aktualizace 4 na zařízení StorSimple

## <a name="overview"></a>Přehled

Tento kurz vysvětluje, jak nainstalovat aktualizace 4 na zařízení StorSimple se starší verzí softwaru prostřednictvím portálu Azure classic a metodou opravu hotfix. Metoda opravy hotfix se používá při brána je nakonfigurovaná na síťovém rozhraní než DATA 0 zařízení StorSimple a pokoušíte se z verze 1 před aktualizací softwaru aktualizovat.

Aktualizace 4 zahrnuje software zařízení, Seznam USM firmware, aktualizacemi zabezpečení operačního systému LSI pro ovladače a firmware, Storport a Spaceport a hostitel jiné aktualizace operačního systému.  Software zařízení, firmwaru Seznam USM, Spaceport, Storport a dalších aktualizací operačního systému jsou omezovaly aktualizace. Prostřednictvím portálu Azure classic nebo prostřednictvím metody opravu hotfix lze použít omezovaly nebo pravidelné aktualizace. Aktualizace firmwaru disku rušivý aktualizace a lze použít pouze prostřednictvím oprav hotfix metodu, pomocí rozhraní Windows PowerShell zařízení. 

> [!IMPORTANT]
> * Sadu ruční a Automatická předběžné kontroly se provádějí před instalací, který měl zjistit stav zařízení z hlediska hardwaru stavu a připojení k síti. Tyto předběžné kontroly se provádí pouze v případě, že aktualizace použít z portálu Azure classic.
> * Doporučujeme nainstalovat software a další pravidelné aktualizace prostřednictvím portálu Azure classic. Má jenom přejděte na rozhraní prostředí Windows PowerShell na zařízení (instalovat aktualizace) Pokud selže kontrola před aktualizací brány na portálu. V závislosti na verzi, ze kterého je aktualizován, instalace aktualizací může trvat 4 hodiny (nebo vyšší) k instalaci. Aktualizace režimu údržby musí být nainstalován také prostřednictvím rozhraní Windows PowerShell zařízení. Jako rušivý aktualizace jsou aktualizace režimu údržby, tyto povede k výpadkům pro vaše zařízení.
> * Pokud běží volitelné Snapshot Manager zařízení StorSimple, zajistěte, aby upgradu vaší verzí Snapshot Manager na aktualizace 4 před aktualizací zařízení.


[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-4-via-the-azure-classic-portal"></a>Nainstalujte aktualizace 4 prostřednictvím portálu Azure classic
Proveďte následující kroky k aktualizaci zařízení [aktualizace 4](storsimple-update4-release-notes.md).

> [!NOTE]
> Chcete-li použít Update 2 nebo novější (včetně Update 2.1), Microsoft bude moci vyžádat jiné diagnostické informace ze zařízení. Výsledkem je když náš tým operations identifikuje zařízení, která došlo k potížím, jsme jsou lépe vybaveny shromažďovat informace ze zařízení a diagnostikovat problémy. Přijetím Update 2 nebo novější, můžete nám umožňují poskytovat tento proaktivní podporu. 

[!INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

Ověřte, zda je spuštěna vaše zařízení **StorSimple 8000 řady aktualizace 4 (6.3.9600.17820)**. **Datum poslední aktualizace** také by měl být upraven. 

* Nyní uvidíte, že jsou k dispozici aktualizace režim údržby (Tato zpráva může nadále zobrazovat až 24 hodin po instalaci aktualizace). Aktualizace režimu údržby jsou rušivý aktualizace, které vést k výpadkům zařízení a dají se použít jenom přes rozhraní Windows PowerShell vašeho zařízení.
 
* Stáhnout aktualizace režimu údržby pomocí kroků uvedených v [ke stažení opravy hotfix](#to-download-hotfixes) k hledání a stahování KB4011837, který nainstaluje aktualizace firmwaru disku (s jinými aktualizacemi musí už být nainstalovaný nyní). Postupujte podle kroků uvedených v [instalaci a ověření opravy hotfix režimu údržby](#to-install-and-verify-maintenance-mode-hotfixes) do režimu údržby instalace aktualizací. 

## <a name="install-update-4-as-a-hotfix"></a>Nainstalujte aktualizace 4 jako oprava hotfix
Doporučujeme nainstalovat aktualizace 4 je prostřednictvím portálu Azure classic.

Tento postup použijte, pokud selže kontrola brány při pokusu o instalaci aktualizací prostřednictvím portálu Azure classic. Kontrola selže, jak máte bránu přiřazené 0 síťové rozhraní bez dat a vaše zařízení používá verzi softwaru před Update 1.

Verze softwaru, které lze upgradovat pomocí metody opravy hotfix jsou:

* Aktualizovat 0.1, 0.2 a 0.3
* Aktualizací 1, 1.1 a 1.2
* Aktualizace 2, 2.1, 2.2
* Aktualizace 3, verze 3.1 


Metoda opravy hotfix zahrnuje následující tři kroky:

1. Stažení opravy hotfix z katalogu služby Microsoft Update.
2. Instalace a ověřte regulární režimu opravy hotfix.
3. Instalace a ověřte opravu hotfix režimu údržby.

#### <a name="download-updates-for-your-device"></a>Stažení aktualizací pro zařízení

Musíte stáhnout a nainstalovat následující opravy hotfix v předepsaných pořadí a navrhované složky:

| Pořadí | kB | Popis | Typ aktualizace | Čas instalace |Instalace ve složce|
| --- | --- | --- | --- | --- | --- |
| 1. |KB4011839 <br> (2 soubory) |Aktualizace softwaru zařízení <br> Aktualizace položek konfigurace nebo MDS agenta |Regulární <br></br>Bez přerušení |~ 25 minut |FirstOrderUpdate <br> _Instalace aktualizace softwaru zařízení před aktualizací agenta položek konfigurace nebo služby MDS_|
| 2A. |KB4011841 <br> KB4011842 |LSI ovladače a firmware aktualizace <br> Aktualizace firmwaru Seznam USM (verze 3.38) |Regulární <br></br>Bez přerušení |~ 3 hodiny <br> (včetně 2A. + 2B. + 2 C.)|SecondOrderUpdate|
| 2B. |KB3139398 KB3108381 <br> KB3205400 KB3142030 <br> KB3197873 KB3192392  <br> KB3153704 KB3174644 <br> KB3139914  |Balíček aktualizace zabezpečení operačního systému |Regulární <br></br>Bez přerušení |- |SecondOrderUpdate|
| 2C. |KB3210083 KB3103616 <br> KB3146621 KB3121261 <br> KB3123538 |Balíček aktualizace operačního systému |Regulární <br></br>Bez přerušení |- |SecondOrderUpdate|

Můžete také nainstalovat aktualizace firmwaru disku nad všechny aktualizace, které jsou uvedené v předchozí tabulky. Můžete ověřit, zda je nutné aktualizace firmwaru disku spuštěním `Get-HcsFirmwareVersion` rutiny. Pokud používáte tyto verze firmwaru: `XMGJ`, `XGEG`, `KZ50`, `F6C2`, `VR08`, `N002`, `0106`, pak nemusíte tyto aktualizace nainstalujete.

| Pořadí | kB | Popis | Typ aktualizace | Čas instalace | Instalace ve složce|
| --- | --- | --- | --- | --- | --- |
| 3. |KB4011837 |Firmware disku |Údržby <br></br>Rušivý |~ 30 minut | ThirdOrderUpdate |

<br></br>

> [!IMPORTANT]
> * Tento postup je nutné provést pouze jednou pro použití aktualizací 4. Portál Azure classic můžete použít následující aktualizace.
> * Pokud aktualizace Update 3 nebo 3.1, je čas celkový instalace blízké 4 hodiny.
> * Před použitím tohoto postupu k použití aktualizace, ujistěte se, že jak řadiče zařízení jsou online a jsou hardwarové součásti v pořádku.

Proveďte následující kroky ke stažení a instalaci opravy hotfix.

[!INCLUDE [storsimple-install-update4-hotfix](../../includes/storsimple-install-update4-hotfix.md)]

[!INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Další kroky
Další informace o [verze aktualizací 4](storsimple-update4-release-notes.md).

