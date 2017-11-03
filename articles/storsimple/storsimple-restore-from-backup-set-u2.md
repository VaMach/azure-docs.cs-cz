---
title: "Obnovit svazek StorSimple ze zálohy | Microsoft Docs"
description: "Vysvětluje, jak používat stránky zálohování katalogu služby StorSimple Manager obnovit svazek StorSimple ze zálohovacího skladu."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 6f289c39-96c7-4d57-b68a-4bc2e99aef9d
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 03/22/2017
ms.author: alkohli
ms.openlocfilehash: 99b76e3bc2939c65654cbf606fda6f8a45e0c44b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="restore-a-storsimple-volume-from-a-backup-set-update-2"></a>Obnovit svazek StorSimple ze zálohovacího skladu (Update 2)
[!INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a>Přehled
**Zálohování katalogu** stránka zobrazuje všechny zálohovací sklady, které vytvářejí, když jsou provedeny ruční nebo automatické zálohy. Pomocí této stránky lze zobrazit seznam a spravovat zálohy, obnovení ze zálohovacího skladu nebo klonovat svazku.

 ![Zálohování stránky katalogu](./media/storsimple-restore-from-backup-set-u2/restore.png)

Tento kurz vysvětluje, jak používat **zálohování katalogu** stránku k obnovení vašeho zařízení ze zálohovacího skladu.

Můžete obnovit svazek z místní nebo cloudové zálohování. V obou případech operace obnovení připojí svazek online okamžitě, když data se stáhne na pozadí. 

## <a name="before-you-restore"></a>Před obnovením
Před spuštěním operace obnovení, byste měli vědět o těchto aspektů:

* **Do offline režimu svazku** – trvat svazek offline na hostiteli a zařízení, před spuštěním operace obnovení. Přestože operace obnovení automaticky připojí svazek online na zařízení, je nutné ručně přenést zařízení online na hostiteli. Převedete online svazek na hostiteli, pokud je svazek online na zařízení. (Není nutné čekat, až po dokončení operace obnovení.) Postupy, přejděte na [do offline režimu svazku](storsimple-manage-volumes-u2.md#take-a-volume-offline).
* **Typ svazku po obnovení** – odstraněné svazky jsou obnoveny podle typu ve snímku. Svazky, které byly místně vázaný se obnoví jako místně vázaných svazků a svazky, které byly zřízeny vrstvené se obnoví jako vrstvené svazky.
  
    Pro existující svazky přepíše aktuální typ použití svazku typ, který je uložený ve snímku. Například pokud obnovujete svazek ze snímku, která se provede, když byla vrstvené typ svazku a typ svazku je nyní místně vázaný (z důvodu operaci převodu), pak svazek je obnoven jako místně vázaný svazek. Podobně pokud existující místně vázaný svazek je rozšířena a následně obnovit ze starší snímek pořízený při menší svazek, svazek obnovené zachová aktuální rozbalenou velikost.
  
    Nelze převést na svazek z vrstvený svazek místně vázaný svazek nebo _naopak_ při obnovení svazku. Počkejte na dokončení operace obnovení, a pak můžete převést svazek k jinému typu. Informace o převodu svazku, přejděte na [změnit typ svazku](storsimple-manage-volumes-u2.md#change-the-volume-type). 
* **Velikost svazku se odrazí v obnovené svazku** – Toto je důležitý faktor, pokud obnovujete místně vázaný svazek, který byl odstraněn, (protože místně vázaných svazků jsou plně zřízený). Ujistěte se, že máte dostatek místa, před dalším pokusem o obnovení místně vázaný svazek, který byl dříve odstraněn. 
* **Nelze rozbalit svazku, zatímco je obnovena** – počkejte na dokončení operace obnovení před dalším pokusem o rozšířit svazek. Informace o rozšiřování svazek, přejděte na [upravit svazek](storsimple-manage-volumes-u2.md#modify-a-volume).
* **Můžete provést zálohu, když obnovujete svazek místní** – postupy naleznete na [použít službu StorSimple Manager ke správě zásady zálohování](storsimple-manage-backup-policies.md).
* **Můžete zrušit operaci obnovení** – Pokud zrušíte úlohy obnovení, pak svazek je vrácena zpět do stavu, který byl předtím, než jste spustili obnovení. Postupy, přejděte na [zrušení úlohy](storsimple-manage-jobs-u2.md#cancel-a-job).

## <a name="how-does-restore-work"></a>Jak obnovit práci
Pro zařízení se systémem Update 4 nebo novější se implementuje na základě heatmap obnovení. Jako hostitele požadavky na přístup k datům přístup zařízení, tyto požadavky jsou sledovány a k vytvoření heatmap. Rychlost vysokou požadavků výsledkem bloky dat s vyšší heat, zatímco překládá nižší rychlost požadavků na bloky dat s nižší heat. Je nutné získat přístup dat alespoň dvakrát na označit jako _aktivní_. Soubor, který se mění je také označena jako _aktivní_. Jakmile zahájíte obnovení, nastane proaktivní dosazováním dat podle heatmap. Verze starší než aktualizace 4, data byla stažena během obnovení založená na přístupu jen. 

Na základě Heatmap sledování je povoleno pouze pro vrstvené svazky a místně vázaný svazky nejsou podporované. Obnovení na základě Heatmap není podporováno také při klonování svazku na jiné zařízení. Pokud dojde obnovení na místě a místní snímek pro svazek, který se má obnovit v zařízení existuje, pak jsme není rehydrataci při spotřebě (protože data je již k dispozici místně). Ve výchozím nastavení Pokud obnovujete, rehydrataci úlohy se spouští které proaktivně rehydrataci při spotřebě dat podle heatmap. V aktualizaci 4 lze použít rutiny prostředí Windows PowerShell pro dotazování spuštěné úlohy rehydrataci, zrušení úlohy rehydrataci a získat stav úlohy rehydrataci.

* `Get-HcsRehydrationJob`– Tato rutina načte stav úlohy rehydrataci. Aktivuje úlohu jeden rehydrataci se pro jeden svazek.
* `Set-HcsRehydrationJob`– Tato rutina umožňuje pozastavit, zastavit, obnovte úlohu rehydrataci, když probíhá rehydrataci. 

Další informace o rutinách rehydrataci, přejděte na [odkazu na rutiny Windows Powershellu pro StorSimple](https://technet.microsoft.com/library/dn688168.aspx).

S automatickou rehdyration obvykle vyšší výkon přechodný pro čtení se očekává. Skutečné magniutde vylepšení závisí na různých faktorech, jako je například vzor přístupu, mísení dat a typu dat. Pokud chcete zrušit úlohu rehydrataci, můžete použít rutinu prostředí PowerShell. Pokud chcete trvale zakázat rehydrataci úloh pro všechny budoucí obnovení, obraťte se na Microsoft Support.

## <a name="how-to-use-the-backup-catalog"></a>Použití zálohování katalogu
**Zálohování katalogu** stránka obsahuje dotaz, který umožňuje zúžit zálohování nastavte výběr. Můžete filtrovat zálohovací sklady, které jsou načteny na základě následujících parametrů:

* **Zařízení** – zařízení, v němž byla vytvořena zálohovacího skladu.
* **Zásady zálohování** nebo **svazku** – zásady zálohování nebo svazku přidruženém k tohoto zálohovacího skladu.
* **Z** a **k** – rozsah data a času v okamžiku vytvoření zálohovacího skladu.

Filtrované zálohovací sklady jsou pak poskytovalo na základě následujících atributů:

* **Název** – název zásady zálohování nebo svazku přidruženém k zálohovacího skladu.
* **Velikost** – skutečná velikost zálohovacího skladu.
* **Vytvořit v** – datum a čas, kdy byly vytvořeny zálohy. 
* **Typ** – zálohovací sklady může být místní snímky nebo cloudových snímků. Místní snímek je zálohování všech dat svazku uložených místně na zařízení. Cloudový snímek odkazuje na zálohování svazku dat umístěných v cloudu. Místní snímky poskytují rychlejší přístup, že jsou pro záleží na odolnosti dat zvolena cloudových snímků.
* **Zahájit** – zálohy lze inicializovat automaticky podle plánu nebo ručně vy. (Můžete použít zásady zálohování naplánovat zálohování. Alternativně můžete použít **provést zálohování** možnost provést zálohu interaktivní.)

## <a name="how-to-restore-your-storsimple-volume-from-a-backup"></a>Jak obnovit svazek StorSimple ze zálohy
Můžete použít **zálohování katalogu** stránku obnovit svazek StorSimple ze konkrétní zálohy. Mějte na paměti, ale, že obnovení svazku vrátí svazku stavu, ve kterém byl v době vytvoření zálohy. Žádná data, která byla přidána po ztrátě operace zálohování.

> [!WARNING]
> Obnovení ze zálohy nahradí existující svazky ze zálohy. To může způsobit ztrátu všechna data, která byla zapsána po vytvoření zálohy.
> 
> 

### <a name="to-restore-your-volume"></a>K obnovení svazku
1. Na stránce služby StorSimple Manager, klepněte **katalog zálohování** kartě.
   
    ![Zálohování katalogu](./media/storsimple-restore-from-backup-set-u2/restore.png)
2. Vyberte zálohovací sklad následujícím způsobem:
   
   1. Vyberte příslušné zařízení.
   2. V rozevíracím seznamu vyberte svazek nebo zálohování zásady pro zálohu, kterou chcete vybrat.
   3. Zadejte časový rozsah.
   4. Klikněte na ikonu zaškrtnutí ![ikona zaškrtnutí](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png) k provedení tohoto dotazu.
      
      Zálohování přidružené k vybranému svazku nebo zásady zálohování by se měla objevit v seznamu sad záloh.
3. Rozbalte zálohovací sklad zobrazíte přidružené svazky. Tyto svazky musí být převedeno do režimu offline v hostiteli a zařízení, než bude možné obnovit. Přístup ke svazkům na **kontejnery svazků** stránky a pak postupujte podle kroků v [do offline režimu svazek](storsimple-manage-volumes-u2.md#take-a-volume-offline) je uvedení do režimu offline.
   
   > [!IMPORTANT]
   > Ujistěte se, že jste provedli svazky do offline režimu na hostiteli nejprve, před provedením svazky do režimu offline v zařízení. Pokud neprovedete offline svazky na hostiteli, může potenciálně vést k poškození dat.
   > 
   > 
4. Přejděte zpět **zálohování katalogu** a vyberte zálohovacího skladu.
5. Klikněte na tlačítko **obnovení** v dolní části stránky.
6. Zobrazí se výzva k potvrzení. Zkontrolovat informace o obnovení a pak zaškrtněte políčko potvrzení.
   
    ![Stránka potvrzení](./media/storsimple-restore-from-backup-set-u2/ConfirmRestore.png)
7. Klikněte na ikonu zaškrtnutí ![ikona zaškrtnutí](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png). Spustí úlohu obnovení. Můžete zobrazit úlohy přístup **úlohy** stránky. 
8. Po dokončení obnovení můžete ověřit, že obsah svazků jsou nahrazovány svazků ze zálohy.

![Dostupné video](./media/storsimple-restore-from-backup-set-u2/Video_icon.png)**Dostupné video**

Pokud chcete přehrát video, které ukazuje, jak můžete použít klonu a funkce v zařízení StorSimple obnovit odstraněné soubory obnovit, klikněte na tlačítko [zde](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

## <a name="if-the-restore-fails"></a>Pokud se nezdaří obnovení
Obdržíte výstrahu, pokud z nějakého důvodu selže operace obnovení. Pokud k tomu dojde, aktualizujte seznamu zálohování a ověřte, zda zálohování je stále platný. Pokud záloha není platná a zda obnovujete z cloudu, pak problémy s připojením k může být příčinou problému. 

Chcete-li dokončit operaci obnovení, provést offline svazek na hostiteli a opakujte operaci obnovení. Veškeré úpravy data na svazku, které byly provedeny během procesu obnovení budou ztraceny.

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak [svazky spravovat zařízení StorSimple](storsimple-manage-volumes-u2.md).
* Zjistěte, jak [použít službu StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).

