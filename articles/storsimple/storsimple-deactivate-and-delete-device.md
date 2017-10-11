---
title: "Deaktivace a odstranění zařízení StorSimple | Microsoft Docs"
description: "Popisuje postup odstranění zařízení StorSimple ze služby tak, že nejprve deaktivace služby a poté ji odstranit."
services: storsimple
documentationcenter: 
author: SharS
manager: timlt
editor: 
ms.assetid: 155cda38-c5ae-45dc-b7e8-6444494afc9e
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/17/2017
ms.author: anbacker
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c000a642aa088ac80cc7077453b87e9a47f96900
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deactivate-and-delete-a-storsimple-8000-series-device-via-storsimple-manager-service"></a>Deaktivace a odstranění zařízení řady StorSimple 8000 prostřednictvím služby StorSimple Manager
## <a name="overview"></a>Přehled
Můžete chtít provést zařízení StorSimple mimo provoz (např. Pokud jsou výměna nebo upgrade zařízení nebo pokud už používáte StorSimple). Pokud je to tento případ, musíte před odstraněním jej deaktivovat zařízení. Deaktivace přeruší připojení mezi zařízením a odpovídající služby StorSimple Manager. Tento kurz vysvětluje, jak odebrat zařízení StorSimple ze služby tak, že první deaktivace ji a poté ji odstranit. 

Po deaktivaci zařízení žádná data, která byla uložená místně na zařízení už nebude přístupný. Lze obnovit pouze data, které jsou přidružené k zařízení, která byla uložená v cloudu.  

> [!WARNING]
> Deaktivace je trvalé a nedá se vrátit zpátky. Deaktivované zařízení nemůže být zaregistrován u služby StorSimple Manager Pokud nejprve se resetují na výchozí tovární nastavení. 
> 
> Objekt factory resetovat proces odstraní všechna data, která byla uložená místně na vašem zařízení. Proto je důležité provést cloudový snímek všechna vaše data, před deaktivací zařízení. To vám umožní obnovit všechna data v pozdější fázi.
> 
> 

Tento kurz vysvětluje postup:

* Deaktivace zařízení a odstranění dat
* Deaktivace zařízení a ukládat data

Taky se dozvíte, jak deaktivaci a odstranění funguje na virtuální zařízení StorSimple.

> [!NOTE]
> Před deaktivací fyzický nebo virtuální zařízení StorSimple, zajistěte, aby pro zastavení nebo odstranit klienty a hostitele, které jsou závislé na tomto zařízení.
> 
> 

## <a name="deactivate-and-delete-data"></a>Deaktivace a odstranění dat
Pokud mají zájem o odstraňování zařízení zcela a nechcete ukládat data na zařízení, potom proveďte následující kroky.

#### <a name="to-deactivate-the-device-and-delete-the-data"></a>Chcete-li deaktivovat zařízení a odstranění dat
1. Před deaktivace zařízení, je nutné odstranit všechny svazku kontejnery (a svazcích) přidružené k zařízení. Kontejnery svazků můžete odstranit až poté, co jste odstranili přidružených záloh.
2. Deaktivace zařízení následujícím způsobem:
   
   1. Ve službě StorSimple Manager **zařízení** vyberte zařízení, které chcete deaktivovat a v dolní části stránky klikněte na tlačítko **deaktivovat**.
   2. Zobrazí potvrzovací zpráva. Klikněte na tlačítko **Ano** pokračujte. Proces deaktivovat spustí a trvat několik minut na dokončení.
3. Po deaktivaci můžete odstranit zařízení zcela. Odstranění zařízení odebere ze seznamu zařízení připojená ke službě. Služba poté již nebude možné spravovat zařízení. Pomocí následujících kroků můžete odstranit zařízení:
   
   1. Ve službě StorSimple Manager **zařízení** vyberte deaktivované zařízení, které chcete odstranit.
   2. V dolní části na stránce, klikněte na tlačítko **odstranit**.
   3. Zobrazí se výzva k potvrzení. Klikněte na tlačítko **Ano** pokračujte.
      
      To může trvat několik minut, než se zařízení odstranit.

## <a name="deactivate-and-retain-data"></a>Deaktivovat a zachovat data
Pokud jsou zájem o odstraňování zařízení, ale chcete zachovat data, proveďte následující kroky.

#### <a name="to-deactivate-a-device-and-retain-the-data"></a>Chcete-li deaktivovat zařízení a ukládat data
1. Deaktivujte zařízení. Všechny kontejnery svazků a snímky zařízení zůstanou.
   
   1. Ve službě StorSimple Manager **zařízení** vyberte zařízení, které chcete deaktivovat a v dolní části stránky klikněte na tlačítko **deaktivovat**.
   2. Zobrazí potvrzovací zpráva. Klikněte na tlačítko **Ano** pokračujte. Proces deaktivovat spustí a trvat několik minut na dokončení.
2. Nyní můžete převzít kontejnery svazků a přidružené snímky. Postupy, přejděte na [převzetí služeb při selhání a zotavení po havárii pro zařízení StorSimple](storsimple-device-failover-disaster-recovery.md).
3. Po deaktivaci a převzetí služeb při selhání můžete odstranit zařízení zcela. Odstranění zařízení odebere ze seznamu zařízení připojená ke službě. Služba poté již nebude možné spravovat zařízení. Proveďte následující kroky pro odstranění zařízení:
   
   1. Ve službě StorSimple Manager **zařízení** vyberte deaktivované zařízení, které chcete odstranit.
   2. V dolní části na stránce, klikněte na tlačítko **odstranit**.
   3. Zobrazí se výzva k potvrzení. Klikněte na tlačítko **Ano** pokračujte.
      
      To může trvat několik minut, než se zařízení odstranit.

## <a name="deactivate-and-delete-a-virtual-device"></a>Deaktivace a odstranění virtuálního zařízení
Pro virtuální zařízení StorSimple deaktivace zruší přidělení virtuálního počítače. Potom můžete odstranit virtuální počítač a prostředky vytvořené při jeho zřizování. Po deaktivaci virtuálního zařízení je nelze obnovit do předchozího stavu. 

Deaktivaci má za následek následující akce:

* Virtuální zařízení StorSimple je odebráno.
* OSDisk a datové disky, které jsou vytvořené pro virtuální zařízení StorSimple se odeberou.
* Hostované služby a virtuální sítě, které byly vytvořeny při zřizování zůstanou zachovány. Pokud nepoužíváte tyto entity, odstraňte je ručně.
* Cloudové snímky vytvořené virtuální zařízení StorSimple se zachovají.

## <a name="next-steps"></a>Další kroky
* Chcete-li obnovit deaktivované zařízení výchozí tovární nastavení, přejděte na [zařízení resetovat výchozí tovární nastavení](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).
* Pro technickou podporu [kontaktovat Microsoft Support](storsimple-contact-microsoft-support.md).
* Další informace o tom, jak používat službu StorSimple Manager, přejděte na [použít službu StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md). 

