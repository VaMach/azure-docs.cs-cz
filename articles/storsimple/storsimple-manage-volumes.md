---
title: "Spravovat formátujte svazky zařízení StorSimple | Microsoft Docs"
description: "Vysvětluje, jak přidat, upravit, sledovat a odstraňovat svazky zařízení StorSimple a jak je převést do režimu offline, v případě potřeby."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: ccabd859-590c-4568-a81d-6ff38f125be3
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/11/2016
ms.author: v-sharos
ms.openlocfilehash: 31ed9dad8ba56a3746873b7b35e678e97743fbfe
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-volumes"></a>Použít službu StorSimple Manager ke správě svazků
[!INCLUDE [storsimple-version-selector-manage-volumes](../../includes/storsimple-version-selector-manage-volumes.md)]

## <a name="overview"></a>Přehled
Tento kurz vysvětluje, jak používat službu StorSimple Manager k vytváření a správě svazků v zařízení StorSimple a virtuální zařízení StorSimple.

Služba StorSimple Manager je rozšíření na portálu Azure classic, která umožňuje správu vašeho řešení StorSimple z jedné webové rozhraní. Kromě správy svazků, můžete vytvořit a spravovat služby StorSimple, zobrazit a spravovat zařízení, zobrazovat výstrahy a zobrazení a Správa zásady zálohování a zálohování katalog služby StorSimple Manager.

> [!NOTE]
> Azure StorSimple lze vytvářet pouze dynamicky zřizované svazky. Nelze vytvářet zcela či částečně zřizované svazky v systému Azure StorSimple.
> 
> Dynamické zajišťování je technologie virtualizace, ve kterém se zobrazuje dostupné úložiště delší než fyzické prostředky. Azure StorSimple používá k přidělení právě dostatek místa, aby splňovala aktuální požadavky místo rezervování dostatečné úložiště předem, dynamické zajišťování. Elastické povaha cloudového úložiště usnadňuje tento přístup, protože Azure StorSimple můžete zvýšit nebo snížit úložiště v cloudu splňovat požadavky na změny.
> 
> 

## <a name="the-volumes-page"></a>Stránce svazky
**Svazky** stránka umožňuje správu svazků úložiště, které jsou zřízené v Microsoft Azure StorSimple zařízení pro vaše iniciátorů (serverů). Zobrazuje seznam svazků zařízení StorSimple.

 ![Stránka svazky](./media/storsimple-manage-volumes/HCS_VolumesPage.png)

Svazek se skládá z řady atributy:

* **Název** – popisný název, který musí být jedinečný a pomáhá identifikovat svazku. Tento název se také používá v monitorování sestavy při filtrování na konkrétním svazku.
* **Stav** – může být online nebo offline. Pokud svazek, pokud je offline, není viditelná pro iniciátorů (serverů), které jsou povoleny přístup k použití svazku.
* **Kapacita** – Určuje, jak velké je svazek, kterou posuzuje iniciátoru (server). Kapacita Určuje celkový objem dat, která mohou být uloženy iniciátor (server). Svazky jsou tence zřízený a data se odstraňují. To znamená, že zařízení není předem přidělit kapacita fyzické úložiště interně, nebo v cloudu podle kapacita nakonfigurované svazku. Kapacita svazku je přidělené a využívat na vyžádání.
* **Typ** – typ svazku může být vrstvené nebo archivace (dílčí typ víceúrovňová)
* **Přístup k** – určuje iniciátorů (serverů), které mají přístup do tohoto svazku. Iniciátory, které nejsou členy záznam řízení přístupu (ACR), který je přidružený svazek neuvidí svazku.
* **Monitorování** – Určuje, jestli je monitorována svazku. Svazek bude mít povoleno ve výchozím nastavení při vytvoření monitorování. Monitorování bude, ale, bude zakázán pro klonování svazku. Chcete-li povolit monitorování pro svazek, postupujte podle pokynů v monitorování svazku.

Nejčastější úkoly spojené se svazkem jsou:

* Přidání svazku
* Upravit svazku
* Odstranění svazku
* Svazek převést do režimu offline
* Monitorování svazku

## <a name="add-a-volume"></a>Přidání svazku
Můžete [vytvořili svazek](storsimple-deployment-walkthrough-u1.md#step-6-create-a-volume) během nasazování vašeho řešení StorSimple. Přidání svazku je podobným způsobem.

### <a name="to-add-a-volume"></a>Chcete-li přidat svazek
1. Na **zařízení** vyberte zařízení, na ni dvakrát kliknete a pak klikněte na tlačítko **kontejnery svazků** kartě.
2. Vyberte kontejner svazků a klikněte na šipku v příslušném řádku o přístup ke svazkům přidružené kontejneru.
3. Klikněte na tlačítko **přidat** v dolní části stránky. Přidat spustí Průvodce svazku.
   
     ![Průvodce přidáním svazku základní nastavení](./media/storsimple-manage-volumes/AddVolume1.png)
4. V průvodci přidáním svazku v části **Základní nastavení** proveďte tyto kroky:
   
   1. Zadejte **Název** svazku.
   2. Zadejte hodnotu **Provisioned capacity** (Zřízená kapacita) pro svazek (v GB nebo TB). Kapacita musí být mezi 1 GB a 64 TB pro fyzické zařízení. Maximální kapacita, který může být zřízen svazek na virtuální zařízení StorSimple je 30 TB.
   3. Vyberte **typ použití** svazku. Pokud používáte vrstvený svazek pro archivní data, pak výběrem **použít tento svazek pro archivní data s méně často používaná** políčko změní velikost bloku odstranění duplicitních dat pro svazek na 512 KB. Pokud tuto možnost nevyberete, příslušný vrstvený svazek bude používat velikost bloku dat 64 KB. Větší velikost bloku odstranění duplicitních dat umožňuje zařízení urychlit přenos velkých objemů archivních dat do cloudu. (Vrstvené svazky se dřív označovaných jako primární svazky.)
   4. Klikněte na ikonu šipky ![ikonu šipky](./media/storsimple-manage-volumes/HCS_ArrowIcon.png)přejít na **další nastavení** stránky.
      
        ![Přidat další nastavení Průvodce svazku](./media/storsimple-manage-volumes/AddVolume2.png)
5. V části **další nastavení**, přidejte nový záznam řízení přístupu (ACR):
   
   1. V rozevíracím seznamu vyberte záznam řízení přístupu (ACR). Alternativně můžete přidat nové ACR. ACRs určit, které hostitele může přístup svých svazků pomocí odpovídajících IQN hostitele se, že uvedená v záznamu.
   2. Doporučujeme zaškrtnutím políčka **Povolit pro tento svazek výchozí zálohování** povolit výchozí zálohování.
   3. Klikněte na ikonu zaškrtnutí ![Ikona zaškrtnutí](./media/storsimple-manage-volumes/HCS_CheckIcon.png) k vytvoření svazku se zadaným nastavením.

Nový svazek je nyní připraven k použití.

## <a name="modify-a-volume"></a>Upravit svazku
Upravte svazek, když je potřeba ho rozšířit nebo změnit hostitelů, které přístup ke svazku.

> [!IMPORTANT]
> * Pokud změníte velikost svazku v zařízení, je potřeba změnit na hostitele a velikost svazku.
> * Hostitelské kroků popsaných v tomto poli se pro Windows Server 2012 (2012 R2). Postupy pro Linux nebo jiných hostitelských operačních systémech se liší. Vyhledejte vaše hostitele operačního systému při úpravě svazek na hostitele s jiným operačním systémem.
> 
> 

### <a name="to-modify-a-volume"></a>Chcete-li upravit svazku
1. Na **zařízení** vyberte zařízení, na ni dvakrát kliknete a pak klikněte na tlačítko **kontejneru svazků** kartě. Tato stránka obsahuje seznam v tabulkovém formátu všechny kontejnery svazků, které jsou přidruženy k zařízení.
2. Vyberte kontejner svazků a klikněte na tlačítko Zobrazit seznam všech svazků v kontejneru.
3. Na **svazky** , vyberte svazek a klikněte na tlačítko **upravit**.
4. V Průvodci upravit svazku v části **základní nastavení**, můžete provést následující:
   
   * Upravit **název** a **typ** Pokud chcete upravit vrstvený svazek na svazek archivace výběrem **použít tento svazek pro archivní data s méně často používaná** zaškrtávací políčko, chcete-li změnit velikost bloku odstranění duplicitních dat pro svazek na 512 KB.
   * Zvýšit **zřídit kapacitu**. **Zřízená kapacita** může být pouze zvýšena. Po vytvoření, nelze zmenšit svazek.
     
     > [!NOTE]
     > Kontejner svazků nelze změnit po přiřazení ke svazku.
     > 
     > 
5. V části **další nastavení**, můžete provést následující:
   
   * Upravte ACRs, za předpokladu, že je svazek offline. Pokud je svazek online, musíte do offline režimu se nejdřív. Najdete kroky v [do offline režimu svazek](#take-a-volume-offline) před změnou ACR.
   * Upravte seznam ACRs, jakmile je svazek offline.
     
     > [!NOTE]
     > Nelze změnit **povolit výchozí zálohování tohoto svazku** možnost pro svazek.
     > 
     > 
6. Uložte změny kliknutím na ikonu zaškrtnutí ![ikona zaškrtnutí](./media/storsimple-manage-volumes/HCS_CheckIcon.png). Portál Azure classic zobrazí zprávu aktualizací svazku. Zpráva o úspěšném provedení zobrazí při svazku byla úspěšně aktualizována.
7. Pokud se rozšířit svazek, proveďte následující kroky v hostitelském počítači Windows:
   
   1. Přejděte na **Správa počítače** ->**disku správu**.
   2. Klikněte pravým tlačítkem na **Správa disků** a vyberte **Prohledat disky**.
   3. V seznamu disků, vyberte svazek, který jste aktualizaci, klikněte pravým tlačítkem a pak vyberte **rozšířit svazek**. Spustí se Průvodce rozšířit svazek. Klikněte na **Další**.
   4. Dokončete průvodce, přijetím výchozích hodnot. Po dokončení průvodce by se zobrazit vyšší velikost svazku.

![Dostupné video](./media/storsimple-manage-volumes/Video_icon.png)**Dostupné video**

Pokud chcete přehrát video, které ukazuje, jak rozšířit svazek, klikněte na tlačítko [zde](https://azure.microsoft.com/documentation/videos/expand-a-storsimple-volume/).

## <a name="take-a-volume-offline"></a>Svazek převést do režimu offline
Musíte uvedení svazku do režimu offline, když máte v úmyslu upravit nebo odstranit. Pokud je svazek offline, není k dispozici pro přístup pro čtení a zápis. Musíte provést offline svazek na hostiteli, a také na zařízení. Proveďte následující kroky pro svazek offline.

### <a name="to-take-a-volume-offline"></a>Uvedení svazku do režimu offline
1. Ujistěte se, že dotyčný svazek není používána před přepnutím do režimu offline.
2. Proveďte offline svazek na hostiteli první. Tím se eliminuje všechny potenciální riziko poškození dat na svazku. Konkrétní kroky přečtěte si pokyny pro operační systém hostitele.
3. Až bude hostitel v režimu offline, proveďte svazek v zařízení do offline režimu provedením následujících kroků:
   
   1. Na **zařízení** vyberte zařízení, na ni dvakrát kliknete a pak klikněte na tlačítko **kontejnery svazků** kartě. **Kontejnery svazků** kartě jsou uvedeny v tabulkovém formátu všechny kontejnery svazků, které jsou přidruženy k zařízení.
   2. Vyberte kontejner svazků a klikněte na tlačítko Zobrazit seznam všech svazků v kontejneru.
   3. Vyberte svazek a klikněte na tlačítko **převést do režimu offline**.
   4. Po zobrazení výzvy k potvrzení klikněte na **Ano**. Svazek by teď měly být offline.
      
      Jakmile je svazek offline, **přepnout do režimu Online** bude k dispozici.

> [!NOTE]
> **Přepnout do režimu Offline** příkaz odešle požadavek na zařízení do offline režimu svazku. Pokud svazek stále používají hostitelé, to vede k přerušení připojení, ale nebudou svazku přepnutím do režimu offline.
> 
> 

## <a name="delete-a-volume"></a>Odstranění svazku
> [!IMPORTANT]
> Svazek můžete odstranit pouze v případě, že je offline.
> 
> 

Proveďte následující kroky odstranění svazku.

### <a name="to-delete-a-volume"></a>Odstranění svazku
1. Na **zařízení** vyberte zařízení, na ni dvakrát kliknete a pak klikněte na tlačítko **kontejnery svazků** kartě.
2. Vyberte kontejner svazků, který má na svazek, který chcete odstranit. Klikněte na kontejneru svazků pro přístup **svazky** stránky.
3. Všechny svazky, které jsou přidružené k tento kontejner se zobrazí v tabulkovém formátu. Zkontrolujte stav svazku, který chcete odstranit. Pokud chcete odstranit svazek není v režimu offline, offline režimu se nejprve kroků v [do offline režimu svazku](#take-a-volume-offline).
4. Jakmile je svazek offline, klikněte na tlačítko **odstranit** v dolní části stránky.
5. Po zobrazení výzvy k potvrzení klikněte na **Ano**. Svazek bude odstraněno a **svazky** stránky se zobrazí aktualizovaný seznam svazky v kontejneru.

## <a name="monitor-a-volume"></a>Monitorování svazku
Svazek monitorování umožňuje shromažďování statistik I/E-související pro svazek. Monitorování je povoleno ve výchozím nastavení nejprve 32 svazků, které vytvoříte. Ve výchozím nastavení vypnutá monitorování další svazky. Monitorování klonovaný svazků také ve výchozím nastavení vypnutá.

Proveďte následující kroky, které chcete povolit nebo zakázat monitorování pro svazek.

### <a name="to-enable-or-disable-volume-monitoring"></a>Chcete povolit nebo zakázat monitorování svazku
1. Na **zařízení** vyberte zařízení, na ni dvakrát kliknete a pak klikněte na tlačítko **kontejnery svazků** kartě.
2. Vyberte kontejner svazků, ve kterém se nachází svazku a potom klikněte na kontejneru svazků pro přístup **svazky** stránky.
3. Všechny svazky, které jsou spojené s tímto kontejnerem jsou uvedeny v tabulkovém zobrazení. Klikněte a vyberte svazek nebo klonování svazku.
4. V dolní části stránky klikněte na tlačítko **upravit**.
5. V Průvodci upravit svazku v části **základní nastavení**, vyberte **povolit** nebo **zakázat** z **monitorování** rozevíracího seznamu.
   
    ![Upravit nastavení základního svazku](./media/storsimple-manage-volumes/HCS_MonitorVolumeM.png)

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak [klonovat svazek StorSimple](storsimple-clone-volume.md).
* Zjistěte, jak [použít službu StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).

