---
title: "StorSimple převzetí služeb při selhání a zotavení po havárii | Microsoft Docs"
description: "Naučte se převzít zařízení StorSimple sám sebe, jiné fyzické zařízení nebo virtuálního zařízení."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 5751598e-49c8-42b3-8121-fea5857a7d83
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/16/2016
ms.author: alkohli
ms.openlocfilehash: bf92ffdb16b86c4033cc96ae2abb060d90f9505e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="failover-and-disaster-recovery-for-your-storsimple-device"></a>Převzetí služeb při selhání a zotavení po havárii pro zařízení StorSimple
## <a name="overview"></a>Přehled
Tento kurz popisuje kroky potřebné k převzetí služeb při selhání zařízení StorSimple v případě havárie. Převzetí služeb při selhání vám umožní migraci dat ze zdrojového zařízení v datovém centru do jiné fyzické nebo i virtuálního zařízení umístěné ve stejné nebo jiné zeměpisné umístění. 

Zotavení po havárii (DR) je pomocí funkce převzetí služeb při selhání zařízení orchestrovat a je zahájeno z **zařízení** stránky. Tato stránka podporován všechna zařízení StorSimple připojené k vaší službě StorSimple Manager. Pro každé zařízení se zobrazí popisný název, stav, zřízený a maximální kapacitu, typ a modelu.

![Stránka zařízení](./media/storsimple-device-failover-disaster-recovery/IC740972.png)

Pokyny v tomto kurzu platí pro fyzické a virtuální zařízení StorSimple mezi všemi verzemi softwaru.

## <a name="disaster-recovery-dr-and-device-failover"></a>Zotavení po havárii (DR) a převzetí služeb při selhání zařízení
Ve scénáři zotavení po havárii primární zařízení přestane fungovat. V takovém případě můžete přesunout data cloudu přidružené k selhání zařízení k jinému zařízení pomocí jako primární zařízení *zdroj* a se zadáním jiné zařízení jako *cíl*. Můžete vybrat jeden nebo více kontejnery svazků migrovat do cílové zařízení. Tento proces se označuje jako *převzetí služeb při selhání*. 

Kontejnery svazků ze zdrojového zařízení během převzetí služeb při selhání, změnit vlastnictví a jsou přeneseny na cílovém zařízení. Po kontejnery svazků změnit vlastnictví, tyto odstraněny ze zdrojového zařízení. Po dokončení odstranění cílové zařízení můžete pak se při selhání zpátky.

Následující zotavení po Havárii, obvykle nejnovější zálohy slouží k obnovení dat do cílové zařízení. Ale pokud existují více zásady zálohování pro stejný svazek, pak získá zachyceny zásady zálohování s největší počet svazků a nejnovější zálohy z této zásady se používá k obnovení dat na cílovém zařízení.

Jako příklad, pokud existují dvě zásady zálohování (jedno a jeden vlastní) *defaultPol*, *customPol* s následujícími podrobnostmi:

* *defaultPol* : jeden svazek, *vol1*, spustí denní od 22:30:00.
* *customPol* : čtyři svazky, *vol1*, *vol2*, *vol3*, *vol4*, spustí denní od 22:00:00.

V takovém případě *customPol* se použije jako má více svazků a jsme určit jejich prioritu pro konzistenci s havárií. Nejnovější zálohy z této zásady se používá k obnovení dat.

## <a name="considerations-for-device-failover"></a>Důležité informace týkající se zařízení převzetí služeb při selhání
V případě havárie můžete převzít zařízení StorSimple:

* Fyzické zařízení 
* Na sebe sama
* Na virtuální zařízení

Žádné zařízení převzetí služeb při selhání mějte na paměti následující:

* Požadavky pro zotavení po Havárii je, že jsou všechny svazky v rámci kontejnery svazků offline a kontejnery svazků mají přiřazený cloudový snímek. 
* Dostupných cílových zařízení pro zotavení po Havárii jsou zařízení, která mají dostatek místa pro uložení kontejnery vybraný svazek. 
* Zařízení, která jsou připojena k službě ale nesplňují kritéria dostatek místa, nebudete mít k dispozici jako cílová zařízení.
* Následující zotavení po Havárii po omezenou dobu, data přístup může mít vliv na výkon výrazně, zařízení bude potřeba pro přístup k datům z cloudu a uložte ho místně.

#### <a name="device-failover-across-software-versions"></a>Zařízení převzetí služeb při selhání mezi verzemi softwaru
Služba StorSimple Manager v nasazení může mít více fyzických i virtuálních zařízení, všechny verze spuštění různých softwaru. V závislosti na verzi softwaru, typy svazek v zařízení může také lišit. Například zařízení spuštěný Update 2 nebo vyšší místně připnutý a vrstvené svazky (s archivace probíhá podmnožinu víceúrovňová). Na druhé straně může mít zařízení před aktualizací 2 vrstvené a archivaci svazky. 

Následující tabulku použijte k určení, pokud jste můžete převzít jiná zařízení se systémem verze jiný software a chování typů svazku během zotavení po Havárii.

| Převzetí služeb při selhání z | Povolené pro fyzické zařízení | Povolené pro virtuální zařízení |
| --- | --- | --- |
| Aktualizace 2 pro před aktualizací 1 (verze 0.1, 0.2 a 0.3) |Ne |Ne |
| Aktualizace 2 k aktualizaci 1 (1, 1.1, 1.2) |Ano <br></br>Pokud pomocí místně připnuté nebo vrstvené svazky nebo kombinaci dva, svazky jsou vždy převzetí služeb při selhání jako vrstvené. |Ano<br></br>Pokud pomocí místně připnutý svazky, tyto došlo k selhání přes vrstvené. |
| Aktualizace 2 na Update 2 (novější verze) |Ano<br></br>Pokud používáte místně vázaný nebo vrstvené svazky nebo kombinaci dva, svazky jsou vždy převzetí služeb při selhání jako výchozí typ svazku; víceúrovňová jako vrstvené a místně vázaný jako místně vázaný. |Ano<br></br>Pokud pomocí místně připnutý svazky, tyto došlo k selhání přes vrstvené. |

#### <a name="partial-failover-across-software-versions"></a>Částečné převzetí služeb při selhání mezi verzemi softwaru
Tyto pokyny použijte, pokud máte v úmyslu provést částečné převzetí služeb při selhání pomocí zařízení StorSimple zdroj aplikací před aktualizací 1 na cíl verzi Update 1 nebo novější. 

| Částečné převzetí služeb při selhání | Povolené pro fyzické zařízení | Povolené pro virtuální zařízení |
| --- | --- | --- |
| Před aktualizací 1 (verze 0.1, 0.2 a 0.3) Update 1 nebo novější |Ano, najdete níže pro nejlepší postup tip. |Ano, najdete níže pro nejlepší postup tip. |

> [!TIP]
> Došlo ke cloudu metadata a data formátu změně v Update 1 a novějších verzích. Nedoporučujeme proto částečné převzetí z před aktualizací 1 Update 1 nebo novější verze. Pokud potřebujete provést částečné převzetí služeb při selhání, doporučujeme nejprve nainstalovat Update 1 nebo novější na obou zařízení (zdrojové a cílové) a poté pokračovat převzetí služeb při selhání. 
> 
> 

## <a name="fail-over-to-another-physical-device"></a>Převzetí služeb při selhání do jiného fyzického zařízení
Proveďte následující kroky k obnovení vašeho zařízení na cílové fyzické zařízení.

1. Ověřte, že kontejner svazků, které chcete převzetí služeb při selhání je spojen cloudových snímků.
2. Na **zařízení** klikněte na tlačítko **kontejnery svazků** kartě.
3. Vyberte kontejner svazků, které byste chtěli převzetí služeb při selhání na jiné zařízení. Klikněte na tlačítko kontejneru svazků, které chcete zobrazit seznam svazků v tomto kontejneru. Vyberte svazek a klikněte na tlačítko **přepnout do režimu Offline** uvedení svazku do režimu offline. Tento postup opakujte pro všechny svazky v kontejneru svazků.
4. Předchozí krok opakujte pro všechny kontejnery svazků, které byste chtěli převzetí služeb při selhání na jiné zařízení.
5. Na **zařízení** klikněte na tlačítko **převzetí služeb při selhání**.
6. V průvodci, které se otevře, v části **kontejneru svazků vyberte k převzetí služeb při selhání**:
   
   1. V seznamu kontejnery svazků vyberte kontejnery svazků, které byste chtěli převzetí služeb při selhání.
      **Zobrazí se pouze kontejnery svazků přidruženého cloudu snímky a offline svazky.**
   2. V části **vyberte cílové zařízení** pro svazky ve vybrané kontejnery, vyberte cílové zařízení z rozevíracího seznamu k dispozici pro zařízení. V rozevíracím seznamu se zobrazují jenom zařízení, které mají dostupné kapacity.
   3. Nakonec zkontrolujte všechna nastavení převzetí služeb při selhání v **potvrzení převzetí služeb při selhání**. Klikněte na ikonu zaškrtnutí ![ikona zaškrtnutí](./media/storsimple-device-failover-disaster-recovery/IC740895.png).
7. Se vytvoří úloha převzetí služeb při selhání, který je možné monitorovat pomocí **úlohy** stránky. Pokud kontejneru svazků, které při selhání má místní svazky, uvidíte jednotlivých úloh obnovení pro každý místní svazek (ne pro vrstvené svazky) v kontejneru. Tyto obnovení úloh může trvat dlouho. Je pravděpodobné, zda může dříve dokončí úlohu převzetí služeb při selhání. Všimněte si, že tyto svazky budou mít místní záruky pouze po dokončení úloh obnovení. Po dokončení převzetí služeb při selhání, přejděte na **zařízení** stránky.                                            
   
   1. Vyberte zařízení, která byla použita jako cílové zařízení pro proces převzetí služeb při selhání.
   2. Přejděte na **kontejnery svazků** stránky. Všechny kontejnery svazků, společně s svazky z původního zařízení by měl být uvedený.

## <a name="failover-using-a-single-device"></a>Převzetí služeb při selhání pomocí jednoho zařízení
Pokud jste pouze jedno zařízení a potřeba provést převzetí služeb při selhání, proveďte následující kroky.

1. Udělat snímky cloudu všechny svazky v zařízení.
2. Obnovte v zařízení výchozí tovární nastavení. Podrobné pokyny v [jak resetovat výchozí tovární nastavení zařízení StorSimple](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).
3. Konfigurace zařízení a zaregistrujte ho znovu pomocí služby StorSimple Manager.
4. Na **zařízení** stránky, jako by měl zobrazit staré zařízení **Offline**. Nově registrovaná zařízení by měl zobrazit jako **Online**.
5. Pro nové zařízení proveďte nejdřív minimální konfigurace zařízení. 
   
   > [!IMPORTANT]
   > **Minimální požadavky na konfiguraci neproběhne nejdřív, váš zotavení po Havárii se nezdaří v důsledku chyb v aktuální implementace. Toto chování bude opraven v novější verzi.**
   > 
   > 
6. Vyberte zařízení, staré (offline stavu) a klikněte na tlačítko **převzetí služeb při selhání**. V průvodci, který se zobrazí zadejte cílové zařízení jako nově registrovaná zařízení a selhání tohoto zařízení. Podrobné pokyny najdete v části [převzetí služeb při selhání do jiného fyzického zařízení](#fail-over-to-another-physical-device).
7. Úlohu obnovení zařízení se vytvoří, můžete monitorovat z **úlohy** stránky.
8. Po úspěšném dokončení úlohy, přístup k nové zařízení a přejděte do **kontejnery svazků** stránky. Všechny kontejnery svazků ze staré zařízení mají být migrovány nyní nové zařízení.

## <a name="fail-over-to-a-storsimple-virtual-device"></a>Převzetí služeb při selhání virtuálního zařízení StorSimple
Musíte mít StorSimple virtuální zařízení vytvořený a nakonfigurovaný před spuštěním tohoto postupu. Pokud se softwarem Update 2, zvažte použití virtuálního zařízení 8020 pro DR, který má 64 TB a používá úložiště Premium. 

Proveďte následující kroky a v zařízení obnovit do cílového virtuálního zařízení StorSimple.

1. Ověřte, že kontejner svazků, které chcete převzetí služeb při selhání je spojen cloudových snímků.
2. Na **zařízení** klikněte na tlačítko **kontejnery svazků** kartě.
3. Vyberte kontejner svazků, které byste chtěli převzetí služeb při selhání na jiné zařízení. Klikněte na tlačítko kontejneru svazků, které chcete zobrazit seznam svazků v tomto kontejneru. Vyberte svazek a klikněte na tlačítko **přepnout do režimu Offline** uvedení svazku do režimu offline. Tento postup opakujte pro všechny svazky v kontejneru svazků.
4. Předchozí krok opakujte pro všechny kontejnery svazků, které byste chtěli převzetí služeb při selhání na jiné zařízení.
5. Na **zařízení** klikněte na tlačítko **převzetí služeb při selhání**.
6. V průvodci, které se otevře, v části **kontejneru svazků vyberte převzetí služeb při selhání**, proveďte následující kroky:
   
    a. V seznamu kontejnery svazků vyberte kontejnery svazků, které byste chtěli převzetí služeb při selhání.
   
    **Zobrazí se pouze kontejnery svazků přidruženého cloudu snímky a offline svazky.**
   
    b. V části **vyberte cílové zařízení pro svazky v kontejnerech vybrané**, vyberte z rozevíracího seznamu k dispozici pro zařízení virtuálního zařízení StorSimple. **V rozevíracím seznamu se zobrazují jenom zařízení, které mají dostatečnou kapacitu.**  
7. Nakonec zkontrolujte všechna nastavení převzetí služeb při selhání v **potvrzení převzetí služeb při selhání**. Klikněte na ikonu zaškrtnutí ![ikona zaškrtnutí](./media/storsimple-device-failover-disaster-recovery/IC740895.png).
8. Po dokončení převzetí služeb při selhání, přejděte na **zařízení** stránky.
   
    a. Vyberte virtuální zařízení StorSimple, která byla použita jako cílové zařízení pro proces převzetí služeb při selhání.
   
    b. Přejděte na **kontejnery svazků** stránky. Všechny kontejnery svazků, společně s svazků ze staré zařízení by měl být nyní uvedený.

![Dostupné video](./media/storsimple-device-failover-disaster-recovery/Video_icon.png) **Dostupné video**

Pokud chcete přehrát video, které ukazuje, jak můžete obnovit nezdařené přes fyzického zařízení k virtuálního zařízení v cloudu, klikněte na tlačítko [zde](https://azure.microsoft.com/documentation/videos/storsimple-and-disaster-recovery/).

## <a name="failback"></a>Navrácení služeb po obnovení
Pro Update 3 a novějších verzích podporuje StorSimple také navrácení služeb po obnovení. Po dokončení převzetí provedou následující akce:

* Kontejnery svazků, které jsou převzetí služeb při selhání vyčištěním ze zdrojového zařízení.
* Úloha na pozadí na kontejneru svazků (převzetí služeb při selhání) se zahájí na zdrojového zařízení. Pokud se pokusíte o navrácení služeb po obnovení, když probíhá úloha, zobrazí se oznámení k tomuto účelu. Musíte počkat až do dokončení úlohy spusťte navrácení služeb po obnovení. 
  
    Čas dokončení odstranění kontejnery svazků, je závislá na různých faktorech, jako je množství dat, stáří dat, počet záloh a šířku pásma sítě dostupnou pro operaci. Pokud plánujete testovací převzetí služeb při selhání: navrácení: služeb nebo po: obnovení, doporučujeme, abyste otestovali kontejnery svazků s méně dat (GB). Ve většině případů můžete spustit navrácení služeb po obnovení 24 hodin po dokončení převzetí služeb při selhání. 

## <a name="frequently-asked-questions"></a>Nejčastější dotazy
OTÁZKY. **Co se stane, když zotavení po Havárii se nezdaří, nebo má částečný úspěch?**

A. Pokud zotavení po Havárii se nezdaří, doporučujeme vám, opakujte akci. Při druhém a zotavení po Havárii ví, co všechny provádělo a při procesu zastaveno poprvé. Z tohoto bodu dále spuštění procesu zotavení po Havárii. 

OTÁZKY. **Můžete odstranit zařízení, když probíhá převzetí služeb při selhání zařízení?**

A. Zařízení nelze odstranit, když probíhá zotavení po Havárii. Zařízení může odstranit pouze po dokončení zotavení po Havárii.

OTÁZKY.    **Pokud uvolnění paměti spustit na zdrojového zařízení, tak, aby místní data na zdrojového zařízení se odstraní?**

A. Uvolňování paměti povolí na zdrojového zařízení až po zařízení je zcela vyčištěna. Vyčištění zahrnuje čištění objektů, které mají převzetí služeb při selhání ze zdrojového zařízení, jako jsou svazky, zálohování objekty (ne data), kontejnery svazků a zásady.

OTÁZKY. **Co se stane, pokud se nezdaří úlohy odstranění spojené s kontejnery svazků v zdrojového zařízení?**

A.  Pokud se úloha Odstranit nezdaří, pak budete muset spustit ručně odstranění kontejnery svazků. V **zařízení** vyberte vaše zdrojové zařízení a klikněte na tlačítko **kontejnery svazků**. Vyberte kontejnery svazků, které při selhání přes a v dolní části stránky, klikněte na **odstranit**. Po odstranění všech neúspěšný přes kontejnery svazků na zdrojového zařízení, můžete spustit navrácení služeb po obnovení.

## <a name="business-continuity-disaster-recovery-bcdr"></a>Obchodní kontinuity zotavení po havárii (BCDR)
Obchodní kontinuity po havárii (BCDR) obnovení scénář nastane, když celého datového centra Azure přestane fungovat. To může mít vliv služby StorSimple Manager a jeho přiřazená zařízení StorSimple.

Pokud je zařízení StorSimple, které byly zaregistrovány těsně před došlo k havárii, tato zařízení StorSimple možná muset podstoupit obnovení továrního nastavení. Po havárii zařízení StorSimple se zobrazí v režimu offline. Zařízení StorSimple je nutné odstranit z portálu a obnovení továrního nastavení by mělo být provedeno, za nímž následuje novou registraci.

## <a name="next-steps"></a>Další kroky
* Po provedení převzetí služeb při selhání, budete muset [deaktivujte nebo odstraňte zařízení StorSimple](storsimple-deactivate-and-delete-device.md).
* Informace o tom, jak používat službu StorSimple Manager, přejděte na [použít službu StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).

