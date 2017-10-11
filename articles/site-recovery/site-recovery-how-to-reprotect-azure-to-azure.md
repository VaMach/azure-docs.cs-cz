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
ms.openlocfilehash: 32f5d2d142940bc515849dcd0edb1bb1f152aa6d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="reprotect-from-failed-over-azure-region-back-to-primary-region"></a>Opětovné ochrany z převzal oblast Azure zpět na primární oblasti



>[!NOTE]
>
> Replikace obnovení lokality pro virtuální počítače Azure je aktuálně ve verzi preview.


## <a name="overview"></a>Přehled
Pokud jste [převzetí služeb při selhání](site-recovery-failover.md) virtuálních počítačů z jedné oblasti Azure do druhého, virtuální počítače jsou v nechráněném stavu. Pokud chcete, aby byly zpět na primární oblasti, je třeba nejprve chránit virtuální počítače a převzetí služeb při selhání znovu. Není žádný rozdíl mezi postupy můžete převzetí služeb při selhání v jednom směru nebo jiné. Podobně konečná povolení ochrany virtuálních počítačů, není žádný rozdíl mezi opětovné ochrany post převzetí služeb při selhání a navrácení služeb po obnovení post.
Vysvětlení, pracovní postupy opětovné ochrany a chcete předejít nejasnostem, použijeme jako oblast východní Asie a lokalitě pro obnovení počítačů primární lokality chráněných počítačů jako oblast jihovýchodní Asie. Během převzetí služeb při selhání převzetí služeb při selhání budete virtuální počítače do oblasti jihovýchodní Asie. Než budete navrácení služeb po obnovení budete muset znovu nastavte ochranu virtuálních počítačů z jihovýchodní Asie zpět do východní Asie. Tento článek popisuje kroky, o tom, jak znovu nastavte ochranu.

> [!WARNING]
> Pokud máte [dokončit migraci](site-recovery-migrate-to-azure.md#what-do-we-mean-by-migration), přesunout virtuální počítač do jiné skupiny prostředků nebo odstranit virtuální počítač Azure, nemůžete poté navrácení služeb po obnovení.

Po dokončení opětovné ochrany a jsou replikace chráněných virtuálních počítačů, můžete spustit převzetí služeb při selhání u virtuálních počítačů a vrátit je zpátky do oblast východní Asie.

POST dotazy nebo připomínky můžete na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="prerequisites"></a>Požadavky
1. Virtuální počítače by byly potvrzeny.
2. Je cílový webový server - v takovém případě oblast východní Asie Azure by měly být dostupné a byste měli mít přístup a vytvářet nové prostředky v této oblasti.

## <a name="steps-to-reprotect"></a>Kroky k nastavení opětné

Dále je uveden postup k nastavení opětné virtuálního počítače pomocí výchozí hodnoty.

1. V **trezoru** > **replikované položky**, klikněte pravým tlačítkem na virtuální počítač, který při selhání a pak vyberte **znovu nastavit ochranu**. Můžete také klikněte na počítač a vyberte **znovu nastavit ochranu** z příkazová tlačítka.

![Klikněte pravým tlačítkem k nastavení opětné](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotect.png)

2. V okně, Všimněte si, že směr ochrany, **jihovýchodní Asie k východní Asie**, je již vybrána.

![Znovu nastavte ochranu okno](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotectblade.png)

3. Zkontrolujte **prostředků skupiny, sítě, úložiště a dostupnost sady** informace a klikněte na tlačítko OK. Pokud jsou všechny prostředky označené (Nový), budou vytvořeny jako součást opětovné ochrany.

To bude aktivační události úlohy, znovu aktivujte ochranu úlohu, která se nejdřív počáteční hodnoty cílové lokalitě (v tomto případě SEA) s nejnovější data, a který dokončí, replikují se tak rozdílů před převzetím služeb je zpět do jihovýchodní Asie.

### <a name="reprotect-customization"></a>Znovu nastavte ochranu přizpůsobení
Pokud chcete zvolit účet úložiště extrakce nebo síti během opětovné ochrany, použijte možnost přizpůsobit zadané v okně opětovné ochrany.

![Přizpůsobení možnost](./media/site-recovery-how-to-reprotect-azure-to-azure/customize.png)

Následující vlastnosti he cílového virtuálního počítače můžete přizpůsobit během opětovné ochrany.

![Přizpůsobení okna](./media/site-recovery-how-to-reprotect-azure-to-azure/customizeblade.png)

|Vlastnost |Poznámky  |
|---------|---------|
|Cílová skupina prostředků     | Můžete změnit cílová skupina prostředků, ve kterém se vytvoří tý virtuálního počítače. V rámci opětovné ochrany budou odstraněny cílového virtuálního počítače, proto můžete novou skupinu prostředků, ve kterém můžete vytvořit virtuální počítač post převzetí služeb při selhání         |
|Cílová virtuální síť     | Sítě, nelze změnit během opětovné ochrany. Chcete-li změnit síť, znovu proveďte mapování sítě.         |
|Cílové úložiště     | Můžete změnit účet úložiště, do kterého bude virtuální počítač vytvořen post převzetí služeb při selhání.         |
|Úložiště mezipaměti     | Můžete zadat účet úložiště mezipaměti, který se použije během replikace. Pokud přejdete výchozí hodnoty, nový účet úložiště mezipaměti se mají vytvořit, pokud ještě neexistuje.         |
|Skupina dostupnosti     |Pokud virtuální počítač ve východní Asie je součástí skupiny dostupnosti, můžete vybrat sadu dostupnosti pro cílový virtuální počítač v jihovýchodní Asie. Výchozí nastavení vyhledá stávající sadu dostupnosti SEA a pokuste se ho používat. Během přizpůsobení můžete určit úplně novou sadu AV.         |


### <a name="what-happens-during-reprotect"></a>Co se stane při opětovné ochrany?

Po první povolit ochranu, podobně jako následující jsou rušivé vlivy, které vytvářeny Pokud použijete výchozí hodnoty.
1. V oblasti východní Asie získá vytvořit účet úložiště mezipaměti.
2. Pokud cílový účet úložiště (původní účet úložiště virtuálního počítače jihovýchodní Asie) neexistuje, vytvoří se nový. Název je na konci "Automatické obnovení systému" účet úložiště virtuálního počítače východní Asie.
3. Pokud sadu cíl AV neexistuje a výchozí hodnoty zjistit, který je potřeba vytvořit novou sadu AV, pak bude vytvořen jako součást úlohy opětovné ochrany. Pokud jste upravili opětovné ochrany, budou vybrané sady AV použít.
4.

Následuje seznam kroků, které dojít v případě, že spustíte úlohu opětovné ochrany. Toto je v případě, kdy cílový straně virtuální počítač existuje.

1. Požadované rušivé vlivy jsou vytvořené jako součást opětovné ochrany. Pokud již existují, pak jsou opakovaně využívány.
2. Cílový straně (jihovýchodní Asie) virtuální počítač nejdřív vypnutý, pokud je spuštěn.
3. Azure Site Recovery zkopírován disku cílového straně virtuálního počítače do kontejneru jako objekt blob počáteční hodnoty.
4. Cílový straně virtuální počítač se odstraní.
5. Objekt blob počáteční hodnoty je používán aktuální zdroj straně (východní Asie) virtuální počítač k replikaci. Tím se zajistí, že jsou replikovány pouze rozdíly.
6. Hlavní změny mezi zdrojového disku a objektů blob počáteční hodnoty jsou synchronizovány. To může trvat delší dobu.
7. Po dokončení úloh opětovné ochrany zahájí rozdílová replikace, který vytváří bod obnovení podle zásady.

> [!NOTE]
> Nelze chránit na úrovni plánu obnovení. Můžete pouze nastavte znovu v úrovni virtuálního počítače.

Po úspěšné opětovné ochrany, bude virtuální počítač zadejte chráněném stavu.

## <a name="next-steps"></a>Další kroky

Po zadání chráněném stavu virtuálního počítače můžete spustit převzetí služeb při selhání. Převzetí služeb při selhání vypnout virtuální počítač v oblasti východní Asie Azure a pak vytvoříte a jihovýchodní Asie oblast virtuální počítač spustit. Proto je malý výpadku pro aplikaci. Ano vyberte dobu, převzetí služeb při selhání, pokud vaše aplikace může tolerovat s prodlevou. Doporučujeme nejprve testovací převzetí služeb při selhání virtuálního počítače a ujistěte se, že ho na blížící se správně, před zahájením převzetí služeb při selhání.

-   [Postup zahájení testovací převzetí služeb při selhání virtuálního počítače](site-recovery-test-failover-to-azure.md)

-   [Postup zahájení převzetí služeb při selhání virtuálního počítače](site-recovery-failover.md)
