---
title: "Postup opětovné ochrany z převzetí služeb při selhání virtuálních počítačích Azure zpět na primární oblasti Azure | Microsoft Docs"
description: "Po převzetí služeb při selhání virtuálních počítačů z jedné oblasti Azure do jiného můžete použít Azure Site Recovery k ochraně počítačů v opačném směru. Zjistěte, jaké kroky postup opětovné ochrany před převzetí služeb při selhání znovu."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/11/2017
ms.author: ruturajd
ms.openlocfilehash: 0004323fee44c1433cdd3c39fc1fa7dad965dbf7
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/30/2017
---
# <a name="reprotect-azure-vms-back-to-the-primary-region"></a>Zpět na primární oblasti znovu nastavte ochranu virtuálních počítačů Azure



>[!NOTE]
>
> Replikace obnovení lokality pro virtuální počítače Azure (VM) je aktuálně ve verzi preview.


Pokud jste [převzetí služeb při selhání](../site-recovery-failover.md) selhání virtuální počítače jsou virtuální počítače z jedné oblasti Azure do jiné v nechráněném stavu. Pokud chcete, aby byly zpět na primární oblasti, budete muset nejdřív zahájení replikace virtuálních počítačů a pak znovu převzetí služeb při selhání. Není žádný rozdíl v převzetí služeb při selhání v jednom směru nebo jiné. Podobně po povolení replikace virtuálního počítače není žádný rozdíl mezi vytvoření post převzetí nebo post-navrácení služeb po obnovení.

Proces vytvoření vysvětlit, jako například, že předpokládáme, že primární lokalita chráněných virtuálních počítačů je oblast východní Asie, a lokalitě pro obnovení je jihovýchodní Asie. Během převzetí služeb při selhání můžete převzít služby virtuálních počítačů do oblasti jihovýchodní Asie. Předtím, než jste navrácení služeb po obnovení, budete muset replikovat virtuální počítače z jihovýchodní Asie, zpět do východní Asie. Tento článek popisuje kroky potřebné pro vytvoření.

> [!WARNING]
> Máte-li dokončit migraci a přesunout virtuální počítač do jiné skupiny prostředků (nebo odstranit virtuální počítač Azure) můžete nelze navrácení služeb po obnovení.

Po dokončení vytvoření a jsou replikaci virtuálních počítačů, můžete spustit převzetí služeb při selhání na virtuálních počítačích, aby byly zpět pro oblast východní Asie.

## <a name="prerequisites"></a>Požadavky
1. Virtuální počítač by měl byly potvrzeny.
2. Je cílový webový server (východní Asie) by měly být dostupné a nyní byste měli mít přístup a vytvářet nové prostředky v této oblasti.

## <a name="reprotect"></a>Znovu nastavte ochranu

Postupujte podle těchto kroků opět proveďte ochranu virtuálního počítače pomocí výchozího nastavení.

1. V **trezoru** > **replikované položky**, klikněte pravým tlačítkem na virtuální počítač, který převzetí služeb při selhání a pak vyberte **znovu nastavit ochranu**. Můžete také klikněte na počítač a vyberte **znovu nastavit ochranu** z příkazová tlačítka.

  ![Nové provedení ochrany](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotect.png)

2. Ověřte, že směr replikace **jihovýchodní Asie k východní Asie**, je vybrána.

  ![Znovu nastavte ochranu](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotectblade.png)

3. Zkontrolujte **skupinu prostředků, sítě, úložiště a dostupnost sady** informace a klikněte na tlačítko **OK**. Pokud jsou všechny prostředky označené (Nový), jsou vytvořeny při vytvoření.

Tím se spustí úlohu vytvoření doplňuje pro cílovou lokalitu s nejnovější data, a po jejich dokončení replikaci se rozdíly před předáte služby zpět na jihovýchodní Asie.

### <a name="reprotect-customization"></a>Znovu nastavte ochranu přizpůsobení
Pokud chcete zvolit účet úložiště extrakce nebo síti během opětovné ochrany, použijte možnost upravit na stránce došlo.

![Přizpůsobení možnost](./media/site-recovery-how-to-reprotect-azure-to-azure/customize.png)

Při vytvoření můžete upravit následující vlastnosti cílového virtuálního počítače.

![Přizpůsobení](./media/site-recovery-how-to-reprotect-azure-to-azure/customizeblade.png)

|Vlastnost |Poznámky  |
|---------|---------|
|Cílová skupina prostředků     | Můžete změnit cílová skupina prostředků, ve kterém se vytvoří tý virtuálního počítače. V rámci opětovné ochrany budou odstraněny cílového virtuálního počítače, proto můžete novou skupinu prostředků, ve kterém můžete vytvořit virtuální počítač post převzetí služeb při selhání         |
|Cílová virtuální síť     | Sítě se během vytvoření nedá změnit. Chcete-li změnit síť, znovu proveďte mapování sítě.         |
|Cílové úložiště     | Můžete změnit účet úložiště, do kterého bude virtuální počítač vytvořen post převzetí služeb při selhání.         |
|Úložiště mezipaměti     | Můžete zadat účet úložiště mezipaměti, který se použije během replikace. Pokud přejdete výchozí hodnoty, nový účet úložiště mezipaměti se mají vytvořit, pokud ještě neexistuje.         |
|Skupina dostupnosti     |Pokud virtuální počítač ve východní Asie je součástí skupiny dostupnosti, můžete vybrat sadu dostupnosti pro cílový virtuální počítač v jihovýchodní Asie. Výchozí nastavení vyhledá stávající sadu dostupnosti SEA a pokuste se ho používat. Během přizpůsobení můžete určit úplně novou sadu AV.         |


### <a name="what-happens-during-reprotect"></a>Co se stane při opětovné ochrany?

Po první povolit ochranu, podobně jako následující jsou artefakty vytvořené získat Pokud použijete výchozí hodnoty.
1. V oblasti východní Asie získá vytvořit účet úložiště mezipaměti.
2. Pokud cílový účet úložiště (původní účet úložiště virtuálního počítače jihovýchodní Asie) neexistuje, vytvoří se nový. Název je na konci "Automatické obnovení systému" účet úložiště virtuálního počítače východní Asie.
3. Pokud sadu cíl AV neexistuje a výchozí hodnoty zjistit, který je potřeba vytvořit novou sadu AV, pak bude vytvořen jako součást úlohy vytvoření. Pokud jste upravili došlo, pak zvolené sady AV použít.


Následuje seznam kroků, které dojít v případě, že spustíte úlohu vytvoření. Toto je v případě, kdy cílový straně virtuální počítač existuje.

1. Požadované artefakty jsou vytvořené jako součást opětovné ochrany. Pokud již existují, pak jsou opakovaně využívány.
2. Cílový straně (jihovýchodní Asie) virtuální počítač nejdřív vypnutý, pokud je spuštěn.
3. Azure Site Recovery zkopírován disku cílového straně virtuálního počítače do kontejneru jako objekt blob počáteční hodnoty.
4. Cílový straně virtuální počítač se odstraní.
5. Objekt blob počáteční hodnoty je používán aktuální zdroj straně (východní Asie) virtuální počítač k replikaci. Tím se zajistí, že jsou replikovány pouze rozdíly.
6. Hlavní změny mezi zdrojového disku a objektů blob počáteční hodnoty jsou synchronizovány. To může trvat delší dobu.
7. Po dokončení úlohy vytvoření zahájí rozdílová replikace, který vytváří bod obnovení podle zásady.

> [!NOTE]
> Nelze chránit na úrovni plánu obnovení. Můžete pouze nastavte znovu v úrovni virtuálního počítače.

Poté, co vytvoření úspěšné, bude virtuální počítač zadejte chráněném stavu.

## <a name="next-steps"></a>Další kroky

Po zadání chráněném stavu virtuálního počítače můžete spustit převzetí služeb při selhání. Převzetí služeb při selhání vypnout virtuální počítač v oblasti východní Asie Azure a pak vytvoříte a jihovýchodní Asie oblast virtuální počítač spustit. Proto je malý výpadku pro aplikaci. Ano vyberte dobu, převzetí služeb při selhání, pokud vaše aplikace může tolerovat s prodlevou. Doporučuje se, že nejprve spustit testovací převzetí služeb virtuálního počítače, abyste měli jistotu, ho na blížící se správně, před zahájením převzetí služeb.

-   [Postup zahájení testovací převzetí služeb při selhání virtuálního počítače](../site-recovery-test-failover-to-azure.md)

-   [Postup zahájení převzetí služeb při selhání virtuálního počítače](../site-recovery-failover.md)
