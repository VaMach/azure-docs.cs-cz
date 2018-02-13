---
title: "Vytváření a přizpůsobení plánů obnovení pro převzetí služeb při selhání a obnovení v Azure Site Recovery | Microsoft Docs"
description: "Popisuje, jak vytvořit a přizpůsobení plánů obnovení v Azure Site Recovery k převzetí služeb při selhání a obnovení virtuálních počítačů a fyzických serverů"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 72408c62-fcb6-4ee2-8ff5-cab1218773f2
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 01/26/2018
ms.author: raynew
ms.openlocfilehash: 9839a989246b28c1a194b8d1f0e99c1bd80ac2e5
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="create-recovery-plans"></a>Vytvořit plány obnovení


Tento článek obsahuje informace o vytváření a přizpůsobení plánů obnovení v [Azure Site Recovery](site-recovery-overview.md).

Jakékoli dotazy nebo připomínky můžete publikovat na konci tohoto článku nebo na [fóru služby Azure Site Recovery](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

 Vytvořte plány obnovení provést následující akce:

* Definujte skupiny počítačů, které převzetí služeb při selhání společně a pak spusťte společně.
* Modelu závislosti mezi počítači, když seskupíte společně do obnovení naplánujte skupiny. Například převzetí služeb při selhání a zprovoznit konkrétní aplikaci, seskupit všechny virtuální počítače pro tuto aplikaci do stejné skupiny plánu obnovení.
* Spusťte převzetí služeb při selhání. Můžete spustit test plánované, nebo neplánované převzetí služeb při selhání pro plán obnovení.

## <a name="why-use-recovery-plans"></a>Proč používat plány obnovení?

Plány obnovení můžete naplánovat proces systematické obnovení tak, že vytvoříte malých nezávislých jednotek, které můžete spravovat. Tyto jednotky se obvykle představují aplikace ve vašem prostředí. Plán obnovení nejen umožňuje definovat pořadí, ve kterém spustit virtuální počítače, ale taky umožňuje automatizaci běžných úkolů během obnovení.


**V podstatě jedním ze způsobů zkontrolujte, zda jsou připravené pro migrace na cloud nebo zotavení po havárii, je zajistit, aby každá žádost váš je součástí plánu obnovení a všechny plány obnovy je testován pro obnovení do služby Microsoft Azure. Pomocí této připravenosti, můžete s jistotou migrovat nebo převzetí služeb při selhání dokončení datového centra do služby Microsoft Azure.**
 
Tady jsou tři funkce hodnota klíče plánu obnovení:

### <a name="model-an-application-to-capture-dependencies"></a>Model aplikace k zachycení závislosti

Plán obnovení je skupina virtuálních počítačů obvykle zahrnující aplikace této převzetí služeb při selhání společně. Použití plánu obnovení vytvoří, můžete vylepšit této skupiny pro zachycení vlastnosti specifické pro aplikaci.
 
Dejte nám trvat příklad typické aplikace tři vrstvy s

* jeden back-end SQL
* jeden middlewaru
* jeden web front-endu

Plán obnovení můžete přizpůsobit tak, aby Ujistěte se, že virtuální počítače se objeví v post správném pořadí převzetí služeb při selhání. Back-end SQL musí pocházet si první, middleware by se spustit další a front-endu webové by se spustit poslední. Toto pořadí, které vám určité, že aplikace funguje podle času, který se zobrazí poslední virtuální počítač. Jakmile middleware, pokusí se připojit k vrstvě SQL a plán obnovení má zajistit, že vrstvě SQL je již spuštěna. Front-end servery objevuje poslední také zajistí, že koncovým uživatelům se nepřipojí k adresu URL aplikace omylem dokud všechny, které jsou komponenty běží, a aplikace je připravena přijímat požadavky. Pokud chcete vytvořit tyto závislosti, můžete přizpůsobit plánu obnovení přidat skupiny. Potom vyberte virtuální počítač a změňte jeho skupiny pro přesun mezi skupinami.

![Ukázkový plán obnovení](./media/site-recovery-create-recovery-plans/rp.png)

Po dokončení přizpůsobení můžete vizualizovat přesný postup obnovení. Tady je pořadí kroků provést během převzetí služeb při selhání plánu obnovení:

* Nejprve je vypnutí krok, který se pokouší vypnout virtuální počítače na místní (s výjimkou testovací převzetí služeb při selhání kde primární lokality musí používat i nadále)
* Potom aktivuje převzetí služeb při selhání všech virtuálních počítačů v plánu obnovení paralelně. Převzetí služeb při selhání kroku připraví disky virtuálních počítačů ze replikovaná data.
* Nakonec spuštění skupiny spustit v jejich pořadí spouštění virtuálních počítačů v každé skupině - 1. skupina nejprve pak 2. skupina a nakonec 3. skupina. Pokud máte více než jeden virtuální počítače v kterékoli skupině (například front-end webové vyrovnáváním zatížení) všechny z nich jsou si spuštěn paralelně.

**Sekvencování skupinami zajišťuje, že jsou dodržení závislosti mezi různými aplikačními vrstvami a paralelismus, kde je to vhodné zlepšuje RTO obnovení aplikace.**

   > [!NOTE]
   > Počítače, které jsou součástí jedné skupiny bude převzetí služeb při selhání paralelně. Počítače, které jsou součástí různé skupiny se převzetí služeb při selhání v Odry skupin. Až poté, co máte všechny počítače 1. skupina převzetí služeb při selhání a spuštěn, bude na počítače 2. skupina spustit jejich převzetí služeb při selhání.

### <a name="automate-most-recovery-tasks-to-reduce-rto"></a>Automatizovat většinu úloh obnovení ke snížení RTO

Obnovení velké aplikace může být složité úlohy. Je taky obtížné mějte na paměti, že kroky přesný přizpůsobení post převzetí služeb při selhání nebo migrace. V některých případech není můžete, ale někdo jiný, který nemá informace o rozbor všech aplikací, který potřebuje k aktivaci převzetí služeb při selhání. Zapamatování příliš mnoho ručních kroků časů chaos je obtížné a chyba náchylné k chybám. Plán obnovení poskytuje způsob, jak automatizovat požadované akce, které je třeba provést v každé fázi, pomocí Microsoft Azure Automation runbook. Pomocí sady runbook je možné automatizovat běžné úkoly, obnovení jako příklady vypsáni níže. Pro ty úlohy, které nelze automatizované plány obnovení také poskytují možnost vložit ruční akce.

* Post převzetí služeb při selhání úlohy na virtuální počítač Azure – to jsou požadované obvykle, aby mohl připojit k virtuálnímu počítači, například:
    * Vytvořit veřejnou IP adresu pro převzetí služeb při selhání virtuálního počítače post
    * Přiřadit skupinu NSG k neúspěšný přes síťový adaptér virtuálního počítače
    * Nástroj pro vyrovnávání zatížení přidat do skupiny dostupnosti
* Úlohy ve virtuálním počítači post převzetí služeb při selhání – tyto aplikace překonfigurovat tak, aby nadále fungovat správně v nové verzi prostředí, například:
    * Upravit připojovací řetězec databáze ve virtuálním počítači
    * Změňte pravidla konfigurace serveru webové

**S kompletní obnovení plán, který automatizuje úlohy obnovení post pomocí runbooků automation můžete dosáhnout převzetí služeb při selhání jedním kliknutím a optimalizovat RTO.**

### <a name="test-failover-to-be-ready-for-a-disaster"></a>Testovací převzetí služeb při selhání bude připravená pro havárie

Plán obnovení můžete využít ke spuštění převzetí služeb při selhání nebo testovací převzetí služeb. Vždy byste měli dokončit převzetí služeb při selhání u aplikace před převzetím služeb. Testovací převzetí služeb při selhání umožňuje zkontrolovat, zda bude aplikace spuštěna v lokalitě pro obnovení.  Pokud se neuskutečnily něco, můžete snadno aktivovat čištění a znovu provést testovací převzetí služeb. Proveďte testovací převzetí služeb více než jednou. dokud znáte s jistotou určit, který obnoví aplikace bez problémů.

![Testovací plán obnovení](./media/site-recovery-create-recovery-plans/rptest.png)

**Každá aplikace se liší a potřebujete vytvořit plány obnovení, které jsou přizpůsobené pro každou. Navíc tento dynamickém datacentru světě, aplikace a jejich závislosti zachovat mění. Testovací převzetí služeb při selhání aplikace jednou za tři měsíce, zkontrolujte, zda je aktuální plánu obnovení.**

## <a name="how-to-create-a-recovery-plan"></a>Postup vytvoření plánu obnovení

1. Klikněte na tlačítko **plány obnovení** > **vytvoření plánu obnovení**.
   Zadejte název plánu obnovení a zdroj a cíl. Zdrojové umístění musí mít virtuální počítače, které jsou povolené pro převzetí služeb při selhání a obnovení. Vyberte zdroj a cíl založená na virtuálních počítačích, které mají být součástí plánu obnovení. 

   |Scénář                   |Zdroj               |Cíl           |
   |---------------------------|---------------------|-----------------|
   |Azure do Azure             |Oblast Azure         |Oblast Azure     |
   |Z VMware do Azure            |Konfigurace serveru |Azure            |
   |Z VMM do Azure               |Popisný název VMM    |Azure            |
   |Technologie Hyper-v server do Azure      |Název lokality technologie Hyper-v    |Azure            |
   |Fyzické počítače do Azure |Konfigurace serveru |Azure            |
   |Nástroj VMM do nástroje VMM                 |Popisný název VMM    |Popisný název VMM|

   > [!NOTE]
   > Plán obnovení mohou obsahovat virtuálních počítačů, které mají stejné zdrojové a cílové. Virtuální počítače VMware a nástroj VMM nemůže být součástí stejného plánu obnovení. Virtuální počítače VMware a fyzické počítače, může ale být přidána do stejného plánu jako zdroj pro oba dva je konfigurační server.

2. V **vybrat virtuální počítače**, vyberte virtuální počítače (nebo replikační skupiny), který chcete přidat do výchozí skupiny (skupina 1) v plánu obnovení. Pro výběr bude možné jenom těch virtuálních počítačů, které byly chráněné na zdroji (jako je vybraný v plánu obnovení) a jsou chráněné k cíli (jako je vybraný v plánu obnovení).

## <a name="how-to-customize-and-extend-recovery-plans"></a>Tom, jak přizpůsobit a rozšířit plánů obnovení

Můžete přizpůsobit a rozšířit plány obnovení přejdete do okna prostředků plán obnovení Site Recovery a kliknutím na kartu přizpůsobit.

Můžete přizpůsobit a rozšířit plány obnovení:

- **Přidat nové skupiny**– přidat do skupiny výchozí skupiny plánu obnovení dodatečné (až 7) a poté přidejte další počítače nebo skupiny replikace do těchto skupin plánu obnovení. Skupiny jsou číslované v pořadí, ve kterém je přidat. Virtuální počítač nebo skupinu replikace můžete zahrnuty pouze do jedné obnovení plán skupiny.
- **Přidání ručně prováděné akce**– můžete přidat ručně prováděné akce, které spustit před nebo po skupiny pro plán obnovení. Při spuštění plánu obnovení, zastaví se v okamžiku, kdy jste vložili manuální akce. Dialogové okno zobrazí výzvu k určení, že je dokončená manuální akce.
- **Skript přidáte**– můžete přidat skripty, které spustit před nebo po skupiny pro plán obnovení. Při přidání skript přidá novou sadu akcí pro skupinu. Například se vytvoří sadu předběžné kroky 1. skupina s názvem: 1. skupina: předběžné kroky. Všechny předběžné kroky jsou uvedeny v této sadě. Pokud máte server VMM nasazený, můžete přidat pouze skript v primární lokalitě. [Další informace](site-recovery-how-to-add-vmmscript.md).
- **Přidat runbooky Azure**– můžete rozšířit plány obnovení s runbooky služby Azure. Například k automatizaci úloh, nebo vytvořte krokování obnovení. [Další informace](site-recovery-runbook-automation.md).


## <a name="how-to-add-a-script-runbook-or-manual-action-to-a-plan"></a>Postup přidání akce skriptu, sada runbook nebo ruční do plánu

Soubor skriptu nebo manuální akce můžete přidat do výchozí skupiny plánu obnovení po přidána virtuálních počítačů nebo skupin replikace a vytvořit plán.

1. Otevřete plánu obnovení.
2. Klikněte na položku v **krok** seznamu a pak klikněte na tlačítko **skriptu** nebo **ruční akce**.
3. Určete, zda chcete přidat skript nebo akce před nebo po vybranou položku. Pokud chcete pozici skript přesunout nahoru nebo dolů, použijte **nahoru** a **přesunout dolů** tlačítka.
4. Pokud chcete přidat skript VMM, vyberte **převzetí služeb při selhání do skriptu VMM**. V **cestu ke skriptu**, zadejte relativní cestu ke sdílené složce. V následujícím příkladu VMM, zadejte cestu: **\RPScripts\RPScript.PS1**.
5. Pokud přidáte Azure automation spustit adresáře, zadejte účet Azure Automation, ve kterém je sada runbook a vyberte odpovídající runbooku Azure skript.
6. Chcete-li se, že skript funguje podle očekávání, proveďte převzetí služeb při selhání plánu obnovení.

Při provádění převzetí služeb při selhání nebo navrácení služeb po obnovení, jsou k dispozici pouze v těchto scénářích možnosti skriptu nebo sadu runbook. Ruční akce je dostupná pro převzetí služeb při selhání a navrácení služeb po obnovení.


|Scénář               |Převzetí služeb při selhání |Navrácení služeb po obnovení |
|-----------------------|---------|---------|
|Azure do Azure         |Runbooky |Runbook  |
|Z VMware do Azure        |Runbooky |Není k dispozici       | 
|Z VMM do Azure           |Runbooky |Skript   |
|Web Hyper-v do Azure  |Runbooky |Není k dispozici       |
|Nástroj VMM do nástroje VMM             |Skript   |Skript   |


## <a name="next-steps"></a>Další postup

[Další informace](site-recovery-failover.md) o spuštění převzetí služeb při selhání.

Podívejte se na toto video, které najdete v části Obnovení plánování v akci.

> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/One-click-failover-of-a-2-tier-WordPress-application-using-Azure-Site-Recovery/player]
