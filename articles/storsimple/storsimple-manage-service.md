---
title: "Nasazení služby StorSimple Manager | Microsoft Docs"
description: "Vysvětluje, jak vytvářet a odstraňovat na portálu Azure classic služby StorSimple Manager a popisuje, jak spravovat registrační klíč služby."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: bc1d5650-275c-42ed-bc77-cdb596f85943
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/14/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ba3637a3a8b15b45c16bf5a00c1f4225bcfc5af8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-the-storsimple-manager-service-in-the-azure-classic-portal"></a>Nasazení služby StorSimple Manager na portálu Azure classic

## <a name="overview"></a>Přehled
Služby StorSimple Manager běží v Microsoft Azure a připojí k více zařízení StorSimple. Po vytvoření služby, můžete ke správě zařízení z klasického portálu Microsoft Azure spuštěnému v prohlížeči. To vám umožňuje monitorovat všechna zařízení, které jsou připojené ke službě StorSimple Manager z jedné centrální umístění, a současně minimalizujete její související administrativní zátěže.

Cílová stránka StorSimple Manager uvádí všechny služby StorSimple Manager, které můžete použít ke správě zařízení StorSimple úložiště. Následující informace na stránce StorSimple Manager je k dispozici pro každou službu StorSimple Manager:

* **Název** – název, který byl přiřazen při vytváření služby StorSimple Manager. **Název služby nelze změnit po vytvoření služby. To platí také pro ostatní entity, jako jsou zařízení, svazky, kontejnery svazků a zásady zálohování, které nelze přejmenovat na portálu Azure classic.**
* **Stav** – stav služby, která může být **Active**, **vytváření**, nebo **Online**.
* **Umístění** – zeměpisné umístění, ve kterém se nasadí zařízení StorSimple.
* **Předplatné** – fakturace předplatného, které je přidružené k vaší službě.

Běžné úlohy, které lze provádět pomocí stránky StorSimple Manager jsou:

* Vytvoření služby
* Odstranění služby
* Získání registračního klíče služby
* Znovu vygenerovat registrační klíč služby

Tento kurz popisuje, jak každý z těchto úloh provést.

## <a name="create-a-service"></a>Vytvoření služby
Použití **rychle vytvořit** možnost pro vytvoření služby StorSimple Manager, pokud chcete nasadit zařízení StorSimple. Pokud chcete vytvořit službu, musíte mít:

* Předplatné s smlouvu Enterprise Agreement
* Aktivní účet úložiště Microsoft Azure
* Fakturační informace, které se používá pro správu přístupu

Můžete také vygenerovat výchozí účet úložiště při vytváření služby.

Jeden služby můžete spravovat více zařízení. Zařízení, ale nemůžou zahrnovat víc služeb. Velký podnik může mít víc instancí služby pro práci s různých předplatných, organizace nebo i umístění nasazení. Upozorňujeme, že potřebujete samostatné instance služby StorSimple Manager ke správě zařízení řady StorSimple 8000 a pole virtuální zařízení StorSimple.

> [!IMPORTANT] 
> Pokud máte před srpna 2016 nepoužívané služby vytvořili (žádná zařízení, které byly operace provést u tohoto prostředku), nelze spravovat prostřednictvím portálu Azure nebo portál Azure classic. Doporučujeme vám, že vytvoříte novou službu na portálu Azure.

Proveďte následující kroky k vytvoření služby.

[!INCLUDE [storsimple-create-new-service](../../includes/storsimple-create-new-service.md)]

## <a name="delete-a-service"></a>Odstranění služby
Před odstraněním služby, ujistěte se, že žádné připojená zařízení ji používají. Pokud služba se používá, deaktivujte připojená zařízení. Operaci deaktivovat severu připojení mezi zařízením a služby, ale zachovat data zařízení v cloudu.

> [!IMPORTANT] 
> Po odstranění služby nelze vrátit operaci zpět. Jakékoli zařízení, které se pomocí služby bude muset být tovární nastavení, než se dá použít s jinou službu. V tomto scénáři místní data na zařízení, jakož i konfigurace, budou ztracena.

Proveďte následující kroky pro odstranění služby.

### <a name="to-delete-a-service"></a>Chcete-li odstranit služby
1. Na **služby StorSimple Manager** vyberte službu, kterou chcete odstranit.
2. Klikněte na tlačítko **odstranit** v dolní části stránky.
3. Klikněte na tlačítko **Ano** v potvrzení oznámení. To může trvat několik minut, než službu, kterou chcete odstranit.

## <a name="get-the-service-registration-key"></a>Získání registračního klíče služby
Po úspěšném vytvoření služby musíte registrace zařízení StorSimple pomocí služby. K registraci svého prvního zařízení StorSimple, budete potřebovat registrační klíč služby. K registraci dalších zařízení s existující službu StorSimple, budete potřebovat registrační klíč a služby datového šifrovacího klíče (což je vygenerovaný na první zařízení během registrace). Další informace o šifrovacího klíče dat služby najdete v tématu [zabezpečení zařízení StorSimple](storsimple-security.md). Abyste mohli získat registrační klíč přístup k **registrační klíč** na **služby** stránky.

Proveďte následující kroky k získání registračního klíče služby.

[!INCLUDE [storsimple-get-service-registration-key](../../includes/storsimple-get-service-registration-key.md)]

Zachovat registrační klíč služby do bezpečného umístění. Budete potřebovat tento klíč, jakož i služby datový šifrovací klíč k registraci dalších zařízení s touto službou. Po získání registračního klíče služby, musíte nakonfigurovat zařízení pomocí Windows Powershellu pro StorSimple rozhraní.

Podrobnosti o tom, jak používat tento registrační klíč najdete v tématu [krok 3: Konfigurace a registrace zařízení pomocí Windows Powershellu pro StorSimple](storsimple-deployment-walkthrough.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

## <a name="regenerate-the-service-registration-key"></a>Znovu vygenerovat registrační klíč služby
Musíte se znova vygenerovat registrační klíč služby, pokud je nutné provést střídání klíče, nebo pokud došlo ke změně seznamu správců služeb. Pokud jste znovu vygenerovat klíč, nový klíč slouží pouze k registraci dalších zařízení. Zařízení, která již byla zaregistrována nejsou ovlivněny tímto procesem.

Proveďte následující kroky k opětovnému vytvoření registrační klíč služby.

### <a name="to-regenerate-the-service-registration-key"></a>Chcete-li znovu vygenerovat registrační klíč služby
1. Na **služby StorSimple Manager** klikněte na tlačítko **registrační klíč**.
2. V **registrační klíč služby** dialogové okno, klikněte na tlačítko **znovu vygenerovat**.
3. Zobrazí se potvrzovací zpráva. Klikněte na tlačítko **OK** pokračujte s opětovné vygenerování.
4. Zobrazí se nový registrační klíč služby.
5. Zkopírujte tento klíč a uložit ho pro registraci nové zařízení s touto službou.
6. Klikněte na ikonu zaškrtnutí ![Ikona zaškrtnutí](./media/storsimple-manage-service/HCS_CheckIcon.png) Zavřete toto dialogové okno.

## <a name="next-steps"></a>Další kroky
* Další informace o [proces nasazení zařízení StorSimple](storsimple-deployment-walkthrough-u2.md).
* Další informace o [Správa účtu úložiště StorSimple](storsimple-manage-storage-accounts.md).
* Další informace o tom, jak [použít službu StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).
