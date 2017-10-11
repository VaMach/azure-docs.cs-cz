---
title: "Správa služby StorSimple Manager | Microsoft Docs"
description: "Informace o správě zařízení StorSimple pomocí služby StorSimple Manager na portálu Azure classic."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 2586582e-d85c-42e1-afb3-be734c1c0461
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/21/2016
ms.author: alkohli
ms.openlocfilehash: 22924da07434a06f4c822d97a2afd02ea982e0e0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-manager-service-to-administer-your-storsimple-device"></a>Použít službu StorSimple Manager ke správě zařízení StorSimple
## <a name="overview"></a>Přehled
Tento článek popisuje rozhraní služby StorSimple Manager, včetně toho, jak se připojit k jeho, různé možnosti, které jsou k dispozici a odkazy na konkrétní pracovní postupy, které je možné provádět prostřednictvím tohoto uživatelského rozhraní. Tyto pokyny platí pro obě; StorSimple fyzické a virtuální zařízení.

Po přečtení tohoto článku, se naučíte:

* Připojení ke službě StorSimple Manager
* Přejděte z uživatelského rozhraní Správce zařízení StorSimple
* Spravovat zařízení StorSimple pomocí služby StorSimple Manager

## <a name="connect-to-storsimple-manager-service"></a>Připojení ke službě StorSimple Manager
Služby StorSimple Manager běží v Microsoft Azure a připojí k více zařízení StorSimple. Centrální klasického portálu Microsoft Azure spuštěnému v prohlížeči používáte ke správě těchto zařízení. Pro připojení ke službě StorSimple Manager, postupujte takto.

#### <a name="to-connect-to-the-service"></a>Pro připojení ke službě
1. Přejděte na [https://manage.windowsazure.com/](https://manage.windowsazure.com/).
2. Pomocí svých přihlašovacích údajů účtu Microsoft, přihlaste se k portálu Microsoft Azure classic (nachází se v pravé horní části podokna).
3. Přejděte dolů v levém navigačním podokně přístup ke službě StorSimple Manager.

## <a name="navigate-storsimple-manager-service-ui"></a>Přejděte služby StorSimple Manager uživatelského rozhraní
Navigační hierarchie pro službu StorSimple Manager uživatelského rozhraní je uvedené v následující tabulce.

* **StorSimple Manager** cílová stránka přejdete na úrovni služby uživatelského rozhraní stránky platí pro všechna zařízení v rámci služby.
* **Zařízení** stránky přejdete do uživatelského rozhraní stránky úrovni – zařízení, které jsou pro konkrétní zařízení.
* **Kontejnery svazků** stránky přejdete na stránku svazek, který ukazuje všechny svazky, které jsou přidružené k zařízení.

#### <a name="storsimple-manager-service-navigational-hierarchy"></a>Navigační hierarchii služby StorSimple Manager
| Cílová stránka | Stránky na úrovni služby | Stránky na úrovni zařízení | Stránky na úrovni zařízení |
| --- | --- | --- | --- |
| Služba StorSimple Manager |Řídicí panel služby |Řídicí panel zařízení | |
| Zařízení → |Monitorování | | |
| Zálohování katalogu |Containers→ svazku |Svazky | |
| Konfigurace (služba) |Zásady zálohování | | |
| Úlohy |Konfigurace (zařízení) | | |
| Výstrahy |Údržby | | |

![Dostupné video](./media/storsimple-manager-service-administration/Video_icon.png) **Dostupné video**

Pokud chcete přehrát video, které vás provede uživatelské rozhraní služby StorSimple Manager, klikněte na tlačítko [zde](https://azure.microsoft.com/documentation/videos/storsimple-manager-service-overview/).

## <a name="administer-storsimple-device-using-storsimple-manager-service"></a>Spravovat pomocí služby StorSimple Manager zařízení StorSimple
Následující tabulka obsahuje souhrn všechny běžné úlohy správy a komplexní pracovní postupy, které lze provést v rámci služby StorSimple Manager uživatelského rozhraní. Tyto úlohy jsou uspořádány podle uživatelského rozhraní stránky, na kterých se spouští.

Další informace o každém pracovním postupu klikněte na tlačítko vhodný postup uvedený v tabulce.

#### <a name="storsimple-manager-workflows"></a>Pracovní postupy StorSimple Manager
| Pokud chcete to udělat... | Přejděte na tuto stránku uživatelského rozhraní... | Pomocí tohoto postupu. |
| --- | --- | --- |
| Vytvoření služby</br>Odstranění služby</br>Získat registrační klíč služby</br>Znovu vygenerovat registrační klíč služby |Služba StorSimple Manager |[Nasazení služby StorSimple Manager](storsimple-manage-service.md) |
| Změna šifrovacího klíče dat služby</br>Zobrazit protokoly operací |Řídicí panel StorSimple Manager service → |[Pomocí řídicího panelu služby StorSimple Manager](storsimple-service-dashboard.md) |
| Deaktivace zařízení</br>Odstranění zařízení |Zařízení StorSimple Manager služby → |[Deaktivovat nebo odstranit zařízení](storsimple-deactivate-and-delete-device.md) |
| Další informace o havárii obnovení a zařízení převzetí služeb při selhání</br>Převzetí služeb při selhání fyzického zařízení</br>Převzetí služeb při selhání virtuálního zařízení</br>Obchodní kontinuity zotavení po havárii (BCDR) |Zařízení StorSimple Manager služby → |[Převzetí služeb při selhání a zotavení po havárii pro zařízení StorSimple](storsimple-device-failover-disaster-recovery.md) |
| Zálohování seznamu pro svazek clusteru</br>Vyberte zálohovací sklad</br>Odstranit zálohovacího skladu |Katalog zálohování StorSimple Manager service → |[Spravovat zálohy](storsimple-manage-backup-catalog.md) |
| Klonování svazku |Katalog zálohování StorSimple Manager service → |[Klonování svazku](storsimple-clone-volume.md) |
| Obnovení zálohovacího skladu |Katalog zálohování StorSimple Manager service → |[Obnovení zálohovacího skladu](storsimple-restore-from-backup-set.md) |
| Informace o účtech úložiště</br>Přidání účtu úložiště</br>Upravit účet úložiště</br>Odstranění účtu úložiště</br>Střídání klíče účtů úložiště |Konfigurace → služby StorSimple Manager |[Správa účtů úložiště](storsimple-manage-storage-accounts.md) |
| O šablonách pásma</br>Přidání šablony šířky pásma</br>Upravit šablonu šířky pásma</br>Odstranit šablonu šířky pásma</br>Použít výchozí šablonu šířky pásma</br>Vytvořit šablonu celodenní šířky pásma, která se spouští v zadanou dobu |Konfigurace → služby StorSimple Manager |[Správa šablon šířky pásma](storsimple-manage-bandwidth-templates.md) |
| Informace o záznamech řízení přístupu</br>Vytvoření záznamů řízení přístupu</br>Upravit záznam řízení přístupu</br>Odstranit záznam řízení přístupu |Konfigurace → služby StorSimple Manager |[Spravovat přístup k záznamům v ovládací prvek](storsimple-manage-acrs.md) |
| Zobrazení podrobností o úloze</br>Zrušení úlohy |Úlohy služby → StorSimple Manager |[Správa úloh](storsimple-manage-jobs.md) |
| Zobrazování oznámení o výstrahách</br>Správa výstrah</br>Zkontrolujte výstrahy |Výstrahy služeb → StorSimple Manager |[Zobrazovat a spravovat výstrahy StorSimple](storsimple-manage-alerts.md) |
| Zobrazit připojené iniciátory</br>Najít sériové číslo zařízení</br>Nalezen element target IQN |Zařízení StorSimple Manager služby → → řídicí panel |[Pomocí řídicího panelu zařízení StorSimple](storsimple-device-dashboard.md) |
| Vytvářet monitorování grafy |Zařízení StorSimple Manager služby → monitorování → |[Monitorování zařízení StorSimple](storsimple-monitor-device.md) |
| Přidat kontejner svazků</br>Upravit kontejneru svazků</br>Odstranit kontejner svazků |Kontejnery svazků → zařízení StorSimple Manager service → |[Správa kontejnerů svazků](storsimple-manage-volume-containers.md) |
| Přidání svazku</br>Upravit svazku</br>Svazek převést do režimu offline</br>Odstranění svazku</br>Monitorování svazku |Svazky zařízení StorSimple Manager service → zařízení → kontejnery svazků → |[Správa svazků](storsimple-manage-volumes.md) |
| Upravit nastavení zařízení</br>Upravit nastavení času</br>Upravit nastavení DNS.md</br>Nakonfigurujte rozhraní sítě |Zařízení StorSimple Manager služby → nakonfigurovat → |[Upravte konfiguraci zařízení pro zařízení StorSimple](storsimple-modify-device-config.md) |
| Nastavení proxy serveru webové zobrazení |Zařízení StorSimple Manager služby → nakonfigurovat → |[Konfigurace webového proxy serveru pro zařízení](storsimple-configure-web-proxy.md) |
| Změnit heslo správce zařízení</br>Změnit heslo Snapshot Manager zařízení StorSimple |Zařízení StorSimple Manager služby → nakonfigurovat → |[Změna hesel zařízení StorSimple](storsimple-change-passwords.md) |
| Konfigurace vzdálené správy |Zařízení StorSimple Manager služby → nakonfigurovat → |[Vzdálené připojení k zařízení StorSimple](storsimple-remote-connect.md) |
| Konfigurace nastavení výstrah |Zařízení StorSimple Manager služby → nakonfigurovat → |[Zobrazovat a spravovat výstrahy StorSimple](storsimple-manage-alerts.md) |
| Konfigurace protokolů CHAP pro vaše zařízení StorSimple |Zařízení StorSimple Manager služby → nakonfigurovat → |[Konfigurace protokolů CHAP pro vaše zařízení StorSimple](storsimple-configure-chap.md) |
| Přidání zásady zálohování</br>Přidat nebo změnit plán</br>Odstranit zásady zálohování</br>Proveďte ruční zálohy</br>Vytvořit vlastní zásady zálohování s více svazků a plány |Zásady zálohování StorSimple Manager service → zařízení → |[Správa zásad zálohování](storsimple-manage-backup-policies.md) |
| Zastavit řadiče zařízení</br>Restartujte řadiče zařízení</br>Vypnutí řadiče zařízení</br>Obnovit výchozí tovární nastavení v zařízení</br>(Vyšší jsou pouze místní zařízení) |Zařízení StorSimple Manager služby → → údržby |[Správa řadiče zařízení StorSimple](storsimple-manage-device-controller.md) |
| Další informace o hardwarových součástí StorSimple</br>Monitorování stavu hardwaru</br>(Vyšší jsou pouze místní zařízení) |Zařízení StorSimple Manager služby → → údržby |[Monitorování hardwarových součástí](storsimple-monitor-hardware-status.md) |
| Vytvoření balíčku pro podporu |Zařízení StorSimple Manager služby → → údržby |[Vytvoření a Správa balíčku pro podporu](storsimple-create-manage-support-package.md) |
| Instalovat aktualizace softwaru |Zařízení StorSimple Manager služby → → údržby |[Aktualizace zařízení](storsimple-update-device.md) |

## <a name="next-steps"></a>Další kroky
Pokud máte problémy s každodenní operace zařízení StorSimple nebo s žádným z jeho hardwarové součásti, podívejte se na:

* [Řešení potíží s provozní zařízení](storsimple-troubleshoot-operational-device.md)
* [Použít StorSimple monitorování kláves](storsimple-monitoring-indicators.md)

Pokud nelze vyřešit problémy a je potřeba vytvořit žádost o službu, podívejte se na [obraťte se na podporu společnosti Microsoft](storsimple-contact-microsoft-support.md).

