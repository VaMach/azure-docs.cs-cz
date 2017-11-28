---
title: "Migrace počítačů po vyhodnocení s Azure migrovat | Microsoft Docs"
description: "Popisuje, jak získat doporučení k migraci počítačů po spuštění posouzení se službou Azure migrovat."
services: migrate
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 809c6e85-0928-45e2-a7c7-6824d860e134
ms.service: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/21/2017
ms.author: raynew
ms.openlocfilehash: d633f140635ba184642a2af999efcde845f3ec31
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/27/2017
---
# <a name="migrate-machines-after-assessment"></a>Migrujte počítače po vyhodnocení


[Azure migrací](migrate-overview.md) vyhodnocuje místní počítače, zkontrolujte, zda jsou vhodné pro migraci na Azure a poskytuje velikost a cenově odhady pro spuštění na počítač v Azure. V současné době Azure migrovat vyhodnocuje pouze počítače pro migraci. Migrace samotné právě probíhá, pomocí jiných služeb Azure.

Tento článek popisuje, jak získat návrhy pro nástroj pro migraci po spuštění migrace hodnocení.

## <a name="migration-methods"></a>Metody migrace

Po vyhodnocení pomocí Azure migrovat zde je, co doporučujeme:

1. Vytvoření projektu Azure migrovat, zjistit místní počítače a spustit migraci hodnocení. [Další informace](tutorial-assessment-vmware.md).
2. Stáhněte a nainstalujte agenty Azure migrovat na každém počítači v místě, pro který chcete zobrazit doporučené migrace metoda. [Pomocí následujícího postupu](how-to-create-group-machine-dependencies.md#prepare-machines-for-dependency-mapping) k instalaci agentů.
2. Identifikujte vaše místní počítače, které jsou vhodné pro migraci navýšení a shift. Toto jsou virtuální počítače, které nevyžadují žádné změny aplikace běžící na ně a můžete migrovat, protože je.
3. Pro migraci navýšení a shift doporučujeme pomocí Azure Site Recovery. [Další informace](../site-recovery/tutorial-migrate-on-premises-to-azure.md). Alternativně můžete použít 3. stran nástroje, které podporují migraci na Azure.
4. Pokud máte místní počítače, které nejsou vhodné pro migraci navýšení a shift, například pokud chcete migrovat konkrétní aplikaci, nikoli celý virtuální počítač, můžete použít jiné nástroje pro migraci. Například doporučujeme [migrace databáze Azure](https://azure.microsoft.com/campaigns/database-migration/) Pokud chcete migrovat místní databáze takové SQL Server, MySQL nebo Oracle do Azure.


## <a name="review-suggested-migration-methods"></a>Zkontrolujte navrhované migrace metody

1. Než získáte metoda navrhované migrace, budete muset vytvoření projektu Azure migrovat, zjistit místní počítače a spustit migraci hodnocení. [Další informace](tutorial-assessment-vmware.md).
2. Po vytvoření hodnocení ji zobrazit v projektu > **přehled** > **řídicí panel**. Klikněte na tlačítko **hodnocení připravenosti**

    ![Vyhodnocení připravenosti](./media/tutorial-assessment-vmware/assessment-report.png)  

3. V **nástroj navržený**, zkontrolujte návrhy pro nástroje pro migraci můžete použít.

    ![Navrhované nástroj](./media/tutorial-assessment-vmware/assessment-suitability.png) 




## <a name="next-steps"></a>Další kroky

[Další informace](concepts-assessment-calculation.md) o tom, jak jsou vypočítávány vyhodnocování.
