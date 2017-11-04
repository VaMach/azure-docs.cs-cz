---
title: "Spravovat formátujte svazky zařízení StorSimple (U2) | Microsoft Docs"
description: "Vysvětluje, jak přidat, upravit, sledovat a odstraňovat svazky zařízení StorSimple a jak je převést do režimu offline, v případě potřeby."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 57896932-0aa5-4805-970c-d13403ae7551
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/28/2016
ms.author: alkohli
ms.openlocfilehash: a61c57cd74a0df8363648dd8df40e433b0e6489d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-volumes-update-2"></a>Pomocí služby StorSimple Manager můžete spravovat svazky (Update 2)
[!INCLUDE [storsimple-version-selector-manage-volumes](../../includes/storsimple-version-selector-manage-volumes.md)]

## <a name="overview"></a>Přehled
Tento kurz vysvětluje, jak pomocí služby StorSimple Manager vytvořit a spravovat svazky na zařízení StorSimple a virtuální zařízení StorSimple Update 2 nainstalován.

Služba StorSimple Manager je rozšíření na portálu Azure classic, která umožňuje správu vašeho řešení StorSimple z jedné webové rozhraní. Kromě správy svazků, můžete vytvořit a spravovat služby StorSimple, zobrazit a spravovat zařízení, zobrazovat výstrahy a zobrazení a Správa zásady zálohování a zálohování katalog služby StorSimple Manager.

## <a name="volume-types"></a>Typy svazku
Svazky zařízení StorSimple může být:

* **Místně vázaný svazky**: Data v těchto svazcích zůstane na místním zařízení StorSimple za všech okolností.
* **Zřízeny vrstvené svazky**: Data v těchto svazků můžete distribuována do cloudu.

Archivace svazek je typ vrstvený svazek. Použít pro archivaci svazky větší velikost bloku odstranění duplicitních dat umožňuje zařízení k přenosu větší segmenty dat do cloudu. 

Pokud potřeby můžete změnit svazek typu z místní pro vrstvené nebo z vrstvené na místní. Další informace, přejděte na [změnit typ svazku](#change-the-volume-type).

### <a name="locally-pinned-volumes"></a>Místně vázaných svazků
Místně vázaných svazků jsou zcela zřizované svazky, které provádějí není dat vrstev do cloudu, a zajistí tak místní záruky pro primární data, nezávisle na cloudu připojení. Není data na místně vázaných svazků s odstraněním duplicitních dat a komprimované; Nicméně jsou odstranění duplicit snímky místně vázaných svazků. 

Místně vázaných svazků jsou plně zřízený; proto musí mít dostatek místa na vašem zařízení, při jejich vytváření. Můžete zřídit místně vázaných svazků až maximální velikosti 8 TB zařízení StorSimple 8100 a 20 TB na zařízení 8600. StorSimple si vyhrazuje zbývající volné místo na zařízení pro snímky, metadata a data zpracování. Můžete zvýšit velikost místně vázaný svazek na maximální místo k dispozici, ale nelze snížit velikost svazku po vytvoření.

Když vytvoříte místně vázaný svazek, dostupné místo pro vytvoření vrstvené svazky se snižuje. Platí to i naopak: Pokud máte vrstvené svazky, volné místo dostupné pro vytváření místně připojené svazky bude nižší než maximální limit uvedené výše. Další informace o místní svazky, najdete v části [nejčastější dotazy na místně vázaných svazků](storsimple-local-volume-faq.md).   

### <a name="tiered-volumes"></a>Vrstvené svazky
Vrstvené svazky jsou dynamicky zřizované svazky, ve kterých často používaná data zůstává místní na zařízení a automaticky vrstveny méně často používaná data do cloudu. Dynamické zajišťování je technologie virtualizace, ve kterém se zobrazuje dostupné úložiště delší než fyzické prostředky. StorSimple používá k přidělení právě dostatek místa, aby splňovala aktuální požadavky místo rezervování dostatečné úložiště předem, dynamické zajišťování. Elastické povaha cloudového úložiště usnadňuje tento přístup, protože StorSimple můžete zvýšit nebo snížit úložiště v cloudu splňovat požadavky na změny.

Pokud používáte vrstvený svazek pro archivní data, pak výběrem **použít tento svazek pro archivní data s méně často používaná** políčko změní velikost bloku odstranění duplicitních dat pro svazek na 512 KB. Pokud tuto možnost nevyberete, příslušný vrstvený svazek bude používat velikost bloku dat 64 KB. Větší velikost deduplikačního bloku dat umožňuje zařízení urychlit přenos velkých objemů archivních dat do cloudu.

> [!NOTE]
> Archivace svazků vytvořených před aktualizací 2 verzí StorSimple bude importovat, protože vrstvené s archivace toto políčko zaškrtnuté.
> 
> 

### <a name="provisioned-capacity"></a>Zřízená kapacita
Naleznete v následující tabulce maximální zřízená kapacita pro každý typ zařízení a svazku. (Všimněte si, že nejsou dostupné na virtuálním zařízení místně vázaných svazků.)

|  | Vrstvený svazek maximální velikost | Maximální místně připnutý velikost svazku |
| --- | --- | --- |
| **Fyzické zařízení** | | |
| 8100 |64 TB |8 TB |
| 8600 |64 TB |20 TB |
| **Virtuální zařízení** | | |
| 8010 |30 TB |Není k dispozici |
| 8020 |64 TB |Není k dispozici |

## <a name="the-volumes-page"></a>Stránce svazky
**Svazky** stránka umožňuje správu svazků úložiště, které jsou zřízené v Microsoft Azure StorSimple zařízení pro vaše iniciátorů (serverů). Zobrazuje seznam svazků zařízení StorSimple.

 ![Stránka svazky](./media/storsimple-manage-volumes-u2/VolumePage.png)

Svazek se skládá z řady atributy:

* **Název svazku** – popisný název, který musí být jedinečný a pomáhá identifikovat svazku. Tento název se také používá v monitorování sestavy při filtrování na konkrétním svazku.
* **Stav** – může být online nebo offline. Pokud je svazek offline, není viditelná pro iniciátorů (serverů), které jsou povoleny přístup k použití svazku.
* **Kapacita** – Určuje celkový objem dat, která mohou být uloženy iniciátor (server). Místně vázaný svazky jsou plně zřízený a jsou umístěny v zařízení StorSimple. Vrstvené svazky jsou tence zřízený a data se odstraňují. S dynamicky zřizované svazky nebude zařízení předem přidělit kapacity fyzického úložiště, interně, nebo v cloudu podle kapacita nakonfigurované svazku. Kapacita svazku je přidělené a využívat na vyžádání.
* **Typ** – Určuje, zda je svazek **nastavování** (výchozí) nebo **místně vázaný**.
* **Zálohování** – označuje, zda existuje výchozí zásady zálohování pro daný svazek.
* **Přístup k** – určuje iniciátorů (serverů), které mají přístup do tohoto svazku. Iniciátory, které nejsou členy záznam řízení přístupu (ACR), který je přidružený svazek neuvidí svazku.
* **Monitorování** – Určuje, jestli je monitorována svazku. Svazek bude mít povoleno ve výchozím nastavení při vytvoření monitorování. Monitorování bude, ale, bude zakázán pro klonování svazku. Chcete-li povolit monitorování pro svazek, postupujte podle pokynů v [monitorování svazku](#monitor-a-volume). 

Postupujte podle pokynů v tomto kurzu k provádění následujících úloh:

* Přidání svazku 
* Upravit svazku 
* Změnit typ svazku
* Odstranění svazku 
* Svazek převést do režimu offline 
* Monitorování svazku 

## <a name="add-a-volume"></a>Přidání svazku
Můžete [vytvořili svazek](storsimple-deployment-walkthrough-u2.md#step-6-create-a-volume) během nasazování vašeho řešení StorSimple. Přidání svazku je podobným způsobem.

#### <a name="to-add-a-volume"></a>Chcete-li přidat svazek
1. Na **zařízení** vyberte zařízení, na ni dvakrát kliknete a pak klikněte na tlačítko **kontejnery svazků** kartě.
2. Vyberte kontejner svazků ze seznamu a dvojím kliknutím přístup ke svazkům přidružené kontejneru.
3. Klikněte na tlačítko **přidat** v dolní části stránky. Přidat spustí Průvodce svazku.
   
     ![Průvodce přidáním svazku základní nastavení](./media/storsimple-manage-volumes-u2/TieredVolEx.png)
4. V průvodci přidáním svazku v části **Základní nastavení** proveďte tyto kroky:
   
   1. Zadejte **Název** svazku.
   2. Vyberte **typ použití** z rozevíracího seznamu. Pro úlohy, které vyžadují dat k dispozici místně na zařízení za všech okolností, vyberte **místně vázaný**. Pro všechny ostatní typy dat, vyberte **nastavování**. (**Nastavování** je výchozí.)
   3. Pokud jste vybrali **nastavování** v kroku 2, můžete vybrat **použít tento svazek pro archivní data s méně často používaná** políčko konfigurace archivace svazku.
   4. Zadejte **zřízená kapacita** svazku v GB nebo TB. V tématu [zřídit kapacitu](#provisioned-capacity) pro maximální velikost pro každý typ zařízení a svazku. Podívejte se na **dostupné kapacity** k určení, jak velké úložiště je ve skutečnosti k dispozici ve vašem zařízení.
5. Kliknutím na ikonu šipky![Ikona šipky](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png). Pokud konfigurujete místně vázaný svazek, zobrazí se následující zpráva.
   
    ![Změnit typ zprávu svazku](./media/storsimple-manage-volumes-u2/LocalVolEx.png)
6. Klikněte na ikonu šipky ![ikonu šipky](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png)znovu a přejděte na **další nastavení** stránky.
   
    ![Přidat další nastavení Průvodce svazku](./media/storsimple-manage-volumes-u2/AddVolume2.png)<br>
7. V části **další nastavení**, přidejte nový záznam řízení přístupu (ACR):
   
   1. V rozevíracím seznamu vyberte záznam řízení přístupu (ACR). Alternativně můžete přidat nové ACR. ACRs určit, které hostitele může přístup svých svazků pomocí odpovídajících IQN hostitele se, že uvedená v záznamu. Pokud nezadáte ACR, zobrazí se následující zpráva.
      
        ![Zadejte ACR](./media/storsimple-manage-volumes-u2/SpecifyACR.png)
   2. Doporučujeme vám, že jste vybrali **povolit výchozí zálohování tohoto svazku** zaškrtávací políčko.
   3. Klikněte na ikonu zaškrtnutí ![Ikona zaškrtnutí](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png) k vytvoření svazku se zadaným nastavením.

Nový svazek je nyní připraven k použití.

> [!NOTE]
> Pokud vytvoříte místně vázaný svazek a pak vytvořit jiné místně vázaný svazek okamžitě později, vytvoření svazku, postupně spuštění úlohy. První úloha vytvoření svazku se musí ukončit předtím, než můžete začít další úloha vytvoření svazku.
> 
> 

## <a name="modify-a-volume"></a>Upravit svazku
Upravte svazek, když je potřeba ho rozšířit nebo změnit hostitelů, které přístup ke svazku.

> [!IMPORTANT]
> * Pokud změníte velikost svazku v zařízení, je potřeba změnit na hostitele a velikost svazku. 
> * Hostitelské kroků popsaných v tomto poli se pro Windows Server 2012 (2012 R2). Postupy pro Linux nebo jiných hostitelských operačních systémech se liší. Vyhledejte vaše hostitele operačního systému při úpravě svazek na hostitele s jiným operačním systémem. 
> 
> 

#### <a name="to-modify-a-volume"></a>Chcete-li upravit svazku
1. Na **zařízení** vyberte zařízení, na ni dvakrát kliknete a pak klikněte na tlačítko **kontejnery svazků** kartě.
2. Vyberte kontejner svazků ze seznamu a dvojím kliknutím zobrazit svazky přidružené kontejneru.
3. Vyberte svazek a v dolní části stránky klikněte na tlačítko **upravit**. Spustí se Průvodce svazku upravit.
4. V Průvodci upravit svazku v části **základní nastavení**, můžete provést následující:
   
   * Upravit **název**.
   * Převést **typ použití** z místně vázaný k vrstvené nebo z vrstvené pro místně vázaný (najdete v části [změnit typ svazku](#change-the-volume-type) informace).
   * Zvýšit **zřídit kapacitu**. **Zřízená kapacita** může být pouze zvýšena. Po vytvoření, nelze zmenšit svazek.
5. V části **další nastavení**, můžete upravit ACR, za předpokladu, že je svazek offline. Pokud je svazek online, musíte do offline režimu se nejdřív. Najdete kroky v [do offline režimu svazek](#take-a-volume-offline) před změnou ACR.
   
   > [!NOTE]
   > Nelze změnit **povolit výchozí zálohování** možnost pro svazek.
   > 
   > 
6. Uložte změny kliknutím na ikonu zaškrtnutí ![ikona zaškrtnutí](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png). Portál Azure classic zobrazí zprávu aktualizací svazku. Zpráva o úspěšném provedení zobrazí při svazku byla úspěšně aktualizována.
7. Pokud se rozšířit svazek, proveďte následující kroky v hostitelském počítači Windows:
   
   1. Přejděte na **Správa počítače** ->**disku správu**.
   2. Klikněte pravým tlačítkem na **Správa disků** a vyberte **Prohledat disky**.
   3. V seznamu disků, vyberte svazek, který jste aktualizaci, klikněte pravým tlačítkem a pak vyberte **rozšířit svazek**. Spustí se Průvodce rozšířit svazek. Klikněte na **Další**.
   4. Dokončete průvodce, přijetím výchozích hodnot. Po dokončení průvodce by se zobrazit vyšší velikost svazku.
      
      > [!NOTE]
      > Pokud rozbalte místně vázaný svazek a potom rozbalte jiné místně připnuli svazku okamžitě později, úlohy rozšíření svazku spouští sekvenčně. První úloha rozšíření svazku se musí ukončit předtím, než můžete začít další úlohy rozšíření svazku.
      > 
      > 

![Dostupné video](./media/storsimple-manage-volumes-u2/Video_icon.png)**Dostupné video**

Pokud chcete přehrát video, které ukazuje, jak rozšířit svazek, klikněte na tlačítko [zde](https://azure.microsoft.com/documentation/videos/expand-a-storsimple-volume/).

## <a name="change-the-volume-type"></a>Změnit typ svazku
Můžete změnit typ svazku z vrstvené pro místně vázaný nebo z místně vázaný na vrstvené. Tento převod však by neměl být časté výskyt. Důvody pro převod svazku z vrstvené pro místně vázaný zahrnují:

* Místní záruky týkající se data dostupnosti a výkonu
* Odstranění cloudu latenci a problémy s připojením k cloudu.

Obvykle jsou malé existující svazky, které chcete pro přístup k často. Když je vytvořeno místně vázaný svazek plně zřízený. Při převodu vrstvený svazek k místně vázaný svazek, StorSimple ověřuje mít dostatek místa na zařízení, než začne převod. Pokud máte dostatek místa, dojde k chybě a operace bude zrušena. 

> [!NOTE]
> Před zahájením převodu z vrstvené pro místně vázaný, ujistěte se, vezměte v úvahu požadavky na místo vašich dalších zatížení. 
> 
> 

Můžete chtít změnit místně vázaný svazek na vrstvený svazek, pokud potřebujete další místa pro zřízení jiných svazků. Při převodu místně vázaný svazek k vrstvené dostupné kapacity na zařízení zvyšuje velikost vydaná kapacitu. Pokud problémy s připojením k zabránit převod svazku místního typu vrstvené typ, místní svazek bude mít květy vlastnosti vrstvený svazek, dokud se nedokončí převod. Je to proto, že některá data mohou přesahovat do cloudu. Tato data spilled bude zabírat volné místo na zařízení, které nelze uvolnit, dokud je restartovat a dokončit operaci.

> [!NOTE]
> Převod svazku může určitou dobu trvat a nelze zrušit převodu z po jeho spuštění. Zachování svazku online během převodu a můžete provést zálohování, ale nelze rozbalit nebo obnovit svazek během probíhající převod.  
> 
> 

Převod vrstvené na místně vázaný svazek může nepříznivě ovlivnit výkon zařízení. Kromě toho následující faktory může se prodloužit doba potřebná k dokončení převodu:

* Není k dispozici dostatečná šířka pásma.
* Není žádná aktuální záloha.

Chcete-li minimalizovat účinky, které může mít tyto faktory:

* Zkontrolujte vaše zásady omezení šířky pásma a ujistěte se, že vyhrazené šířky pásma 40 MB/s je k dispozici.
* Plánování převod hodiny mimo špičku.
* Pořízení snímku cloudu před zahájením převodu.

Pokud převádíte více svazků (Podpora různých zátěží), by měl tak, aby vyšší prioritu svazky se převedou nejprve upřednostňovat převodu svazku. Například je třeba převést svazky, které jsou hostiteli virtuálních počítačů (VM) nebo svazky s úlohami SQL před převodem svazky s úlohami sdílené složky souborů.

#### <a name="to-change-the-volume-type"></a>Chcete-li změnit typ svazku
1. Na **zařízení** vyberte zařízení, na ni dvakrát kliknete a pak klikněte na tlačítko **kontejnery svazků** kartě.
2. Vyberte kontejner svazků ze seznamu a dvojím kliknutím zobrazit svazky přidružené kontejneru.
3. Vyberte svazek a v dolní části stránky klikněte na tlačítko **upravit**. Spustí se Průvodce svazku upravit.
4. Na **základní nastavení** změňte typ použití výběrem nového typu z **typ použití** rozevíracího seznamu.
   
   * Pokud chcete změnit typ, který má **místně vázaný**, StorSimple zkontroluje, jestli je dostatečnou kapacitu.
   * Pokud chcete změnit typ, který má **nastavování** a použije tento svazek pro archivní data, vyberte **použít tento svazek pro archivní data s méně často používaná** zaškrtávací políčko.
     
       ![Zaškrtávací políčko archivu](./media/storsimple-manage-volumes-u2/ModifyTieredVolEx.png)
5. Klikněte na ikonu šipky ![ikonu šipky](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png) přejít na **další nastavení** stránky. Pokud konfigurujete místně vázaný svazek, zobrazí se následující zpráva.
   
    ![Změnit typ zprávu svazku](./media/storsimple-manage-volumes-u2/ModifyLocalVolEx.png)
6. Kliknutím na ikonu šipky ![ikona šipky](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png) znovu pro pokračování.
7. Klikněte na ikonu zaškrtnutí ![Ikona zaškrtnutí](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png) Chcete-li spustit proces převodu. Portál Azure se zobrazí zprávu aktualizací svazku. Zpráva o úspěšném provedení zobrazí při svazku byla úspěšně aktualizována.

## <a name="take-a-volume-offline"></a>Svazek převést do režimu offline
Musíte uvedení svazku do režimu offline, když máte v úmyslu upravit nebo odstranit. Pokud je svazek offline, není k dispozici pro přístup pro čtení a zápis. Musíte provést offline svazek na hostiteli, a také na zařízení. 

#### <a name="to-take-a-volume-offline"></a>Uvedení svazku do režimu offline
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

#### <a name="to-delete-a-volume"></a>Odstranění svazku
1. Na **zařízení** vyberte zařízení, na ni dvakrát kliknete a pak klikněte na tlačítko **kontejnery svazků** kartě.
2. Vyberte kontejner svazků, který má na svazek, který chcete odstranit. Klikněte na kontejneru svazků pro přístup **svazky** stránky.
3. Všechny svazky, které jsou přidružené k tento kontejner se zobrazí v tabulkovém formátu. Zkontrolujte stav svazku, který chcete odstranit. Pokud chcete odstranit svazek není v režimu offline, offline režimu se nejprve kroků v [do offline režimu svazku](#take-a-volume-offline).
4. Jakmile je svazek offline, klikněte na tlačítko **odstranit** v dolní části stránky.
5. Po zobrazení výzvy k potvrzení klikněte na **Ano**. Svazek bude odstraněno a **svazky** stránky se zobrazí aktualizovaný seznam svazky v kontejneru.
   
   > [!NOTE]
   > Pokud odstraníte místně vázaný svazek, místa na disku pro nové svazky nemusí být okamžitě aktualizován. Volné místo dostupné aktualizace služby StorSimple Manager pravidelně. Doporučujeme, že Počkejte několik minut, než se pokusíte vytvořit nový svazek.<br> Kromě toho pokud odstraníte místně vázaný svazek a potom odstraňte jiné místně připnuli svazku okamžitě později, postupně spuštění úlohy odstranění svazku. První úlohy odstranění svazku se musí ukončit předtím, než můžete začít další úlohy odstranění svazku.
   > 
   > 

## <a name="monitor-a-volume"></a>Monitorování svazku
Svazek monitorování umožňuje shromažďování statistik I/E-související pro svazek. Monitorování je povoleno ve výchozím nastavení nejprve 32 svazků, které vytvoříte. Ve výchozím nastavení vypnutá monitorování další svazky. Monitorování klonovaný svazků také ve výchozím nastavení vypnutá.

Proveďte následující kroky, které chcete povolit nebo zakázat monitorování pro svazek.

#### <a name="to-enable-or-disable-volume-monitoring"></a>Chcete povolit nebo zakázat monitorování svazku
1. Na **zařízení** vyberte zařízení, na ni dvakrát kliknete a pak klikněte na tlačítko **kontejnery svazků** kartě.
2. Vyberte kontejner svazků, ve kterém se nachází svazku a potom klikněte na kontejneru svazků pro přístup **svazky** stránky.
3. Všechny svazky, které jsou spojené s tímto kontejnerem jsou uvedeny v tabulkovém zobrazení. Klikněte a vyberte svazek nebo klonování svazku.
4. V dolní části stránky klikněte na tlačítko **upravit**.
5. V Průvodci upravit svazku v části **základní nastavení**, vyberte **povolit** nebo **zakázat** z **monitorování** rozevíracího seznamu.

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak [klonovat svazek StorSimple](storsimple-clone-volume.md).
* Zjistěte, jak [použít službu StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).

