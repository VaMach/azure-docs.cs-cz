---
title: "Nainstalujte na zařízení StorSimple Update 3 | Microsoft Docs"
description: "Popisuje postup instalace zařízení StorSimple 8000 řady Update 3 na vašem zařízení řady StorSimple 8000."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: c6c4634d-4f3a-4bc4-b307-a22bf18664e1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2b99b9cd52dd28f7f62b5d8d5ffe32339a67f82a
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/07/2017
---
# <a name="install-update-3-on-your-storsimple-8000-series-device"></a>Nainstalujte na zařízení řady StorSimple 8000 Update 3

> [!NOTE]
> Portálu classic pro StorSimple je zastaralý. Váš správce zařízení StorSimple se automaticky přesune na nový portál Azure podle plánu vyřazení. Obdržíte e-mailu a portálu oznámení pro přesunutí. Tento dokument také vyřadí brzy k dispozici. Dotazy týkající se přechodu, najdete v části [– nejčastější dotazy: přesunout do portálu Azure](storsimple-8000-move-azure-portal-faq.md).


## <a name="overview"></a>Přehled

Tento kurz vysvětluje, jak nainstalovat aktualizace 3 na zařízení StorSimple se starší verzí softwaru prostřednictvím portálu Azure classic a metodou opravu hotfix. Metoda opravy hotfix se používá při brána je nakonfigurovaná na síťovém rozhraní než DATA 0 zařízení StorSimple a pokoušíte se z verze 1 před aktualizací softwaru aktualizovat.

Aktualizace 3 zahrnuje zařízení software, LSI ovladače a firmware, Storport a Spaceport aktualizace. Pokud aktualizace Update 2 nebo dřívější verzi, je také nutné pro použití protokolu iSCSI, rozhraní WMI a v některých případech je na disku při aktualizacích firmwaru. Zařízení software, rozhraní WMI, iSCSI, LSI ovladače, Spaceport a Storport opravy se omezovaly aktualizace. Tyto aktualizace můžete použít prostřednictvím portálu Azure classic. Aktualizace firmwaru disku rušivý aktualizace a lze použít pouze prostřednictvím rozhraní Windows PowerShell zařízení.

> [!IMPORTANT]
> * Sadu ruční a Automatická předběžné kontroly se provádějí před instalací, který měl zjistit stav zařízení z hlediska hardwaru stavu a připojení k síti. Tyto předběžné kontroly se provádí pouze v případě, že aktualizace použít z portálu Azure classic.
> * Doporučujeme instalovat aktualizace softwaru a ovladačů prostřednictvím portálu Azure classic. Pouze v případě, že selže kontrola před aktualizací brány na portálu, přejděte na rozhraní prostředí Windows PowerShell na zařízení (instalovat aktualizace). V závislosti na verzi, který chcete aktualizovat z se aktualizace může trvat hodiny 1.5 – 2.5 k instalaci. Aktualizace režimu údržby musí být nainstalován prostřednictvím rozhraní Windows PowerShell zařízení. Rušivý aktualizace jsou aktualizace režimu údržby dojde zařízení výpadku.
> * Pokud běží volitelné Snapshot Manager zařízení StorSimple, zajistěte, aby upgradu vaší verzí Snapshot Manager na Update 2 před aktualizací zařízení.

[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-3-via-the-azure-classic-portal"></a>Nainstalujte aktualizace 3 prostřednictvím portálu Azure classic
Proveďte následující kroky k aktualizaci zařízení [aktualizace 3](storsimple-update3-release-notes.md).

> [!NOTE]
> Chcete-li použít Update 2 nebo novější (včetně Update 2.1), Microsoft bude moci vyžádat jiné diagnostické informace ze zařízení. Tato data pomáhají identifikovat zařízení StorSimple, které mají s problémy a pomůcek diagnostikování problémů. Přijetím Update 2 nebo novější, můžete nám umožňují poskytovat tento proaktivní podporu.


[!INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

Ověřte, zda je spuštěna vaše zařízení **StorSimple 8000 řady Update 3 (6.3.9600.17759)**. **Datum poslední aktualizace** je upravit. 
   - Při aktualizaci z verze před Update 2, uvidíte, že jsou k dispozici aktualizace režimu údržby. Tato zpráva může nadále zobrazovat až 24 hodin po instalaci aktualizace.
     Aktualizace režimu údržby jsou rušivý aktualizace, které vést k výpadkům zařízení. Tyto aktualizace lze použít pouze prostřednictvím rozhraní Windows PowerShell vašeho zařízení. V některých případech při spuštění aktualizace 1.2, možná již je aktuální firmware disku a vy nemusíte instalovat všechny aktualizace režimu údržby.
   - Pokud jste aktualizaci z Update 2 nebo novější, zařízení by teď měly být aktuální. Můžete přeskočit na další krok.

Stáhnout aktualizace režimu údržby pomocí kroků uvedených v [ke stažení opravy hotfix](#to-download-hotfixes) k hledání a stahování KB3121899, který nainstaluje aktualizace firmwaru disku (s jinými aktualizacemi musí už být nainstalovaný nyní). Postupujte podle kroků uvedených v [instalaci a ověření opravy hotfix režimu údržby](#to-install-and-verify-maintenance-mode-hotfixes) do režimu údržby instalace aktualizací. 

## <a name="install-update-3-as-a-hotfix"></a>Instalaci aktualizace Update 3 jako oprava hotfix
Tento postup použijte, pokud selže kontrola brány při pokusu o instalaci aktualizací prostřednictvím portálu Azure classic. Kontrola selže, jak máte bránu přiřazené 0 síťové rozhraní bez dat a vaše zařízení používá verzi softwaru před Update 1.

Verze softwaru, které lze upgradovat pomocí metody opravy hotfix jsou:

* Aktualizovat 0.1, 0.2 a 0.3
* Aktualizací 1, 1.1 a 1.2
* Aktualizace 2, 2.1, 2.2 

> [!IMPORTANT]
> * Pokud zařízení používá verzi (GA) verze, kontaktujte prosím [Microsoft Support](storsimple-contact-microsoft-support.md) a pomůže vám při aktualizaci.
> 
> 

Metoda opravy hotfix zahrnuje následující tři kroky:

1. Stažení opravy hotfix z katalogu služby Microsoft Update.
2. Instalace a ověřte regulární režimu opravy hotfix.
3. Instalace a ověřte opravu hotfix režim údržby (jenom při aktualizaci z před aktualizací 2 softwaru).

#### <a name="download-updates-for-your-device"></a>Stažení aktualizací pro zařízení
**Pokud vaše zařízení se systémem Update 2.1 nebo 2.2**, musíte stáhnout a nainstalovat následující opravy hotfix v předepsaných pořadí:

| Pořadí | kB | Popis | Typ aktualizace | Čas instalace |
| --- | --- | --- | --- | --- |
| 1. |KB3186843 |Aktualizace softwaru &#42; |Regulární <br></br>Bez přerušení |~ 45 minut |
| 2. |KB3186859 |LSI ovladače a firmware |Regulární <br></br>Bez přerušení |~ 20 minut |
| 3. |KB3121261 |Oprava Storport a Spaceport </br> Windows Server 2012 R2 |Regulární <br></br>Bez přerušení |~ 20 minut |

&#42;  *Všimněte si, že aktualizace softwaru se skládá ze dvou binární soubory: aktualizace softwaru zařízení uvedena `all-hcsmdssoftwareupdate` a agent služby Mds a položek konfigurace, kterými `all-cismdsagentupdatebundle`. Aktualizace softwaru zařízení musí být nainstalován před Mds a položek konfigurace agenta. Také je nutné restartovat řadič prostřednictvím služby active `Restart-HcsController` aktualizovat rutiny po instalaci agenta položek konfigurace a služby Mds (a před použitím zbývající aktualizace).* 

**Pokud vaše zařízení používá Update 0.1, 0.2, 0.3, 1.0, 1.1, 1.2 nebo 2.0**, musíte stáhnout a nainstalovat následující opravy hotfix kromě softwaru, LSI ovladače a firmware aktualizace (zobrazené v předchozí tabulce), v předepsaných pořadí:

| Pořadí | kB | Popis | Typ aktualizace | Čas instalace |
| --- | --- | --- | --- | --- |
| 4. |KB3146621 |balíček iSCSI |Regulární <br></br>Bez přerušení |~ 20 minut |
| 5. |KB3103616 |Balíček služby WMI |Regulární <br></br>Bez přerušení |~ 12 minut |

<br></br>

**Pokud vaše zařízení používá verzi 0,2, 0,3, 1.0, 1.1 a 1.2**, může se také muset nainstalovat aktualizace firmwaru disku nad všechny aktualizace, které jsou uvedené v předchozí tabulky. Můžete ověřit, zda je nutné aktualizace firmwaru disku spuštěním `Get-HcsFirmwareVersion` rutiny. Pokud používáte tyto verze firmwaru: `XMGG`, `XGEG`, `KZ50`, `F6C2`, `VR08`, pak nemusíte tyto aktualizace nainstalujete.

| Pořadí | kB | Popis | Typ aktualizace | Čas instalace |
| --- | --- | --- | --- | --- |
| 6. |KB3121899 |Firmware disku |Údržby <br></br>Rušivý |~ 30 minut |

<br></br>

> [!IMPORTANT]
> * Tento postup je nutné provést pouze jednou použít Update 3. Portál Azure classic můžete použít následující aktualizace.
> * Pokud aktualizaci z 2.2 aktualizace, je čas celkový instalace blízké 1.1 hodin.
> * Před použitím tohoto postupu k použití aktualizace, ujistěte se, že jak řadiče zařízení jsou online a jsou hardwarové součásti v pořádku.
> 
> 

Proveďte následující kroky ke stažení a instalaci opravy hotfix.

[!INCLUDE [storsimple-install-update3-hotfix](../../includes/storsimple-install-update3-hotfix.md)]

[!INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Další kroky
Další informace o [verzi Update 3](storsimple-update3-release-notes.md).

