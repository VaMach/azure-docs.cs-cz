---
title: "Vytvářet a přizpůsobovat plán obnovení pro převzetí služeb při selhání a obnovení v Azure Site Recovery | Microsoft Docs"
description: "Naučte se vytvářet a přizpůsobovat plány obnovení v Azure Site Recovery. Tento článek popisuje, jak k převzetí služeb při selhání a obnovení virtuálních počítačů a fyzických serverů."
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
ms.openlocfilehash: 6ad82a8a2f8e16ab794ba90c109904bd45cb6b89
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="create-a-recovery-plan-by-using-site-recovery"></a>Vytvoření plánu obnovení pomocí Site Recovery

Tento článek popisuje, jak vytvářet a přizpůsobovat plán obnovení v [Azure Site Recovery](site-recovery-overview.md).

Vytvoření plánu obnovení provést následující akce:

* Definujte skupiny počítačů, které převzetí služeb při selhání společně a pak spusťte společně.
* Modelu závislosti mezi počítače seskupování je ve skupině pro plán obnovení. Převzetí služeb při selhání a zprovoznit konkrétní aplikaci, například obsahovat všechny virtuální počítače pro tuto aplikaci ve stejné skupině plánu obnovení.
* Spusťte převzetí služeb při selhání. Můžete spustit test plánované, nebo neplánované převzetí služeb při selhání pro plán obnovení.

## <a name="why-use-a-recovery-plan"></a>Proč používat plán obnovení

Plán obnovení můžete připravit pro proces systematické obnovení tak, že vytvoříte malých nezávislých jednotek, které můžete spravovat. Jednotky obvykle představují aplikace ve vašem prostředí. Plán obnovení můžete definovat pořadí, ve kterém se spustit virtuální počítače. Můžete taky plán obnovení pro automatizaci běžných úkolů během obnovení.

> [!TIP]
> Jeden způsob kontroly, jste připraveni na cloudu migraci nebo po havárii pro obnovení je zajistit, že všechny vaše aplikace je součástí plánu obnovení. Ujistěte se také, že každý plánu obnovení je testován pro obnovení do Azure. Tato připravenosti můžete s jistotou migrace nebo převzít dokončení datového centra Azure.
 
Plán obnovení má tři klíčové funkce:

* Model aplikace k zachycení závislosti.
* Automatizovat většinu úloh obnovení ke snížení RTO.
* Testovací převzetí služeb při selhání bude připravená pro havárie.

### <a name="model-an-application-to-capture-dependencies"></a>Model aplikace k zachycení závislosti

Plán obnovení je skupina virtuálních počítačů, které obvykle tvoří aplikace a které převzetí služeb při selhání společně. Použití plánu obnovení vytvoří, můžete vylepšit skupiny pro plán obnovení pro zachycení vlastnosti specifické pro aplikaci. 

V tomto článku používáme Typická třívrstvá aplikace, který může mít back end, middleware a front-end webové SQL. Můžete přizpůsobit plán obnovení k zajištění, že virtuální počítače spustit po převzetí služeb ve správném pořadí. Back-end SQL by se měl spustit první, by se měl spustit další middleware a má front-end webový by se měl spustit poslední. Toto pořadí zajistí, že aplikace funguje podle času poslední virtuální počítač spustí. 

Například když se spustí middleware, pokusí se připojit k vrstvě SQL. Plán obnovení zajistí, že vrstvě SQL je již spuštěna. S spuštění serveru front-end poslední také pomáhá zajistit, že koncoví uživatelé Nepřipojovat na adresu URL aplikace předtím, než jsou všechny součásti a spuštěna a aplikace je připravena přijímat požadavky. Pokud chcete vytvořit tyto závislosti, přizpůsobit plánu obnovení přidat skupiny a potom vyberte virtuální počítač. Chcete-li přesunout virtuální počítač mezi skupinami, změňte skupinu virtuálního počítače.

![Snímek obrazovky plán obnovení příklad ve službě Site Recovery](./media/site-recovery-create-recovery-plans/rp.png)

Po dokončení přizpůsobení můžete vizualizovat přesný postup obnovení. Tady je pořadí kroků, které jsou spouštěny během převzetí služeb při selhání plánu obnovení:

1. Chcete-li vypnout virtuální počítače na místní pokusy o krok vypnutí. (S výjimkou testovací převzetí služeb při selhání, během které primární lokality musí proběhnout.)
2. Pokus o ukončení aktivuje převzetí služeb při selhání všech virtuálních počítačů v plánu obnovení paralelně. Převzetí služeb při selhání kroku připraví disky virtuálního počítače pomocí replikovaná data.
3. Spuštění skupiny v pořadí podle jejich spuštění a spuštění virtuálních počítačů v každé skupině. Nejprve provede 1. skupina, pak provede 2. skupina a nakonec 3. skupina provádí. Pokud existuje více než jeden virtuální počítač v kterékoli skupině (například Vyrovnávání zatížení sítě front-end webové), všechny virtuální počítače se spustí paralelně.

> [!TIP]
> Sekvencování skupinami zajistí, že jsou dodržení závislosti mezi různými aplikačními vrstvami. Paralelismu, kde je to vhodné, pokud chcete použít, zlepšuje RTO obnovení aplikace.

   > [!NOTE]
   > Počítače, které jsou součástí jedné skupiny selhání paralelně. Počítače, které jsou součástí různých skupin převzetí služeb při selhání v pořadí podle skupiny. Na počítače 2. skupina spustit jejich převzetí služeb při selhání pouze po převzetí služeb při selhání a spustit všechny počítače 1. skupina.

### <a name="automate-most-recovery-tasks-to-reduce-rto"></a>Automatizovat většinu úloh obnovení ke snížení RTO

Obnovení velké aplikace může být složité úlohy. S mnoha Ruční postup mějte na paměti, časů chaos je obtížné a díky je proces náchylné k chybám. Navíc může být někoho, kdo nemá informace o rozbor všech aplikací, kteří ve skutečnosti se aktivuje převzetí služeb při selhání. 

Plán obnovení můžete použít k automatizaci požadované akce, které je nutné vzít v každém kroku. Pomocí sad Azure Automation runbook můžete nastavit požadované akce. Pomocí sady runbook je možné automatizovat běžné úkoly, obnovení, jako úlohy v následujících příkladech. Pro úlohy, které nelze je možné automatizovat můžete ručně prováděné akce vložit do plánu obnovení.

* **Úlohy na virtuálním počítači Azure post převzetí**: tyto úlohy jsou obvykle vyžaduje být schopni připojit k virtuálnímu počítači. Příklady:
    * Vytvoření veřejné IP adresy na post převzetí virtuálního počítače.
    * Přiřadíte skupinu zabezpečení sítě na síťový adaptér na virtuálním počítači při selhání.
    * Nástroj pro vyrovnávání zatížení přidáte do skupiny dostupnosti.
* **Úlohy uvnitř virtuálního počítače post převzetí**: tyto úlohy znovu nakonfigurovat aplikaci tak, aby nadále fungovat správně v novém prostředí. Příklady:
    * Upravte připojovací řetězec databáze ve virtuálním počítači.
    * Změna konfigurace webového serveru nebo pravidla.

> [!TIP]
> Dosáhnout převzetí služeb při selhání jedním kliknutím a optimalizovat RTO tak, že vytvoříte plán dokončení obnovení, který automatizuje úlohy po obnově pomocí runbooků Automation.

### <a name="test-failover-to-be-ready-for-a-disaster"></a>Testovací převzetí služeb při selhání bude připravená pro havárie

Plán obnovení můžete použít k aktivaci převzetí služeb při selhání nebo testovací převzetí služeb. Vždy dokončení převzetí služeb při selhání u aplikace před převzetím služeb. Nápověda testovací převzetí služeb při selhání, můžete zkontrolovat, zda aplikace se dodává v lokalitě pro obnovení. Pokud se neuskutečnily něco v nastavení aplikace, můžete snadno aktivovat čištění a potom znovu provést testovací převzetí služeb. Proveďte testovací převzetí služeb vícekrát, dokud jste si jisti, plynule obnoví aplikace.

![Snímek obrazovky příklad testovací plán obnovení ve službě Site Recovery](./media/site-recovery-create-recovery-plans/rptest.png)

> [!TIP]
> Protože každá aplikace je jedinečný, budete muset vytvořit plány obnovení, které jsou přizpůsobené pro každou aplikaci. 
>
> Také v dnešní době je dynamické a zaměřené na datovém centru, aplikace a jejich závislosti často mění. Abyste měli jistotu, že plán obnovení aktuální, testovací převzetí služeb při selhání pro vaše aplikace každé čtvrtletí.

## <a name="create-a-recovery-plan"></a>Vytvoření plánu obnovení

1. Vyberte **plány obnovení** > **vytvoření plánu obnovení**.
   Zadejte název plánu obnovení a zdroj a cíl. Zdrojové umístění musí mít virtuální počítače, které jsou povolené pro převzetí služeb při selhání a obnovení. Vyberte zdroj a cíl založená na virtuálních počítačích, které mají být součástí plánu obnovení. 

   |Scénář                   |Zdroj               |Cíl           |
   |---------------------------|---------------------|-----------------|
   |Azure do Azure             |Oblast Azure         |Oblast Azure     |
   |Z VMware do Azure            |Konfigurace serveru |Azure            |
   |Nástroj Virtual Machine Manager (VMM) do Azure               |Zobrazovaný název VMM    |Azure            |
   |Z lokality Hyper-V do Azure      |Název lokality technologie Hyper-V    |Azure            |
   |Fyzické počítače do Azure |Konfigurace serveru |Azure            |
   |Nástroj VMM do nástroje VMM                 |Popisný název VMM    |Zobrazovaný název VMM|

   > [!NOTE]
   > Plán obnovení mohou obsahovat virtuálních počítačů, které mají stejné zdrojové a cílové. Virtuální počítače VMware a System Center Virtual Machine Manager (VMM) nemůže být v rámci stejného plánu obnovení. Můžete však přidat virtuální počítače VMware a fyzické počítače do stejného plánu obnovení. Zdroj pro oba počítače v tomto případě je konfigurační server.

2. V části **vybrat virtuální počítače**, vyberte virtuální počítače (nebo replikační skupiny), který chcete přidat do výchozí skupiny (skupina 1) v plánu obnovení. Můžete zvolit pouze virtuální počítače, které byly chráněné ve zdroji (jako je vybraný v plánu obnovení) a které jsou chráněny na cíli (jako je vybraný v plánu obnovení).

## <a name="customize-and-extend-recovery-plans"></a>Přizpůsobit a rozšířit plánů obnovení

Přizpůsobit a rozšířit plány obnovení, přejděte do podokna prostředek plánu obnovení Site Recovery. Vyberte **přizpůsobit** kartě. Můžete přizpůsobit a rozšířit plány obnovení s použitím následujících možností:

- **Přidat nové skupiny**: můžete přidat až sedm skupiny plánu obnovení dodatečné do výchozí skupiny. Potom můžete přidat další počítače nebo skupiny replikace do těchto skupin plánu obnovení. Skupiny jsou číslované v pořadí, ve kterém je přidat. Virtuální počítač nebo skupinu replikace, můžete zahrnout pouze do jedné skupiny plánu obnovení.
- **Přidání ručně prováděné akce**: můžete přidat ručně prováděné akce, které spustit před nebo po skupiny pro plán obnovení. Při spuštění plánu obnovení, zastaví se v okamžiku, kdy jste vložili manuální akce. Dialogové okno zobrazí výzvu k určení, že je dokončená manuální akce.
- **Skript přidáte**: můžete přidat skripty, které spustit před nebo po skupiny pro plán obnovení. Při přidání skript přidá novou sadu akcí pro skupinu. Například se vytvoří sadu předběžné kroky 1. skupina s názvem *1. skupina: předběžné kroky*. Všechny předběžné kroky jsou uvedeny v této sadě. Skript v primární lokalitě můžete přidat pouze v případě, že máte server VMM nasazený. Další informace najdete v tématu [skript VMM přidat do plánu obnovení](site-recovery-how-to-add-vmmscript.md).
- **Přidat runbooky Azure**: plány obnovení můžete rozšířit pomocí runbooky služby Azure. Můžete například sady runbook k automatizaci úloh, nebo vytvořte krokování obnovení. Další informace najdete v tématu [přidat Azure Automation runbook do plánů obnovení](site-recovery-runbook-automation.md).


## <a name="add-a-script-runbook-or-manual-action-to-a-plan"></a>Přidejte skript, runbook nebo manuální akci do plánu

Po přidání virtuálních počítačů nebo skupiny replikace do výchozí skupiny plánu obnovení, můžete přidat skript nebo manuální akci do skupiny plánu obnovení.

1. Otevřete plánu obnovení.
2. V **krok** seznamu, vyberte položku. Pak vyberte **skriptu** nebo **ruční akce**.
3. Zadejte, zda chcete přidat skript nebo akce před nebo po vybranou položku. Pozice skript přesunout nahoru nebo dolů, vyberte **nahoru** nebo **přesunout dolů** tlačítka.
4. Pokud chcete přidat skript VMM, vyberte **převzetí služeb při selhání do skriptu VMM**. V **cestu ke skriptu**, zadejte relativní cestu ke sdílené složce. Například **\RPScripts\RPScript.PS1**.
5. Pokud přidáte runbook služby automatizace, zadejte účet Automation, ve kterém je sada runbook. Potom vyberte skript runbooku Azure.
6. Aby se zajistilo, že skript funguje podle očekávání, proveďte převzetí služeb při selhání plánu obnovení.

Skript nebo sadu runbook možnosti jsou k dispozici pouze v těchto scénářích a při převzetí služeb při selhání a navrácení služeb po obnovení. Ruční akce je k dispozici pro převzetí služeb při selhání a navrácení služeb po obnovení.


|Scénář               |Převzetí služeb při selhání |Navrácení služeb po obnovení |
|-----------------------|---------|---------|
|Azure do Azure         |Runbook |Runbook  |
|Z VMware do Azure        |Runbook |Není k dispozici       | 
|Z VMM do Azure           |Runbook |Skript   |
|Z lokality Hyper-V do Azure  |Runbook |Není k dispozici       |
|Nástroj VMM do nástroje VMM             |Skript   |Skript   |


## <a name="next-steps"></a>Další postup

* Další informace o [systémem převzetí služeb při selhání](site-recovery-failover.md).  
* Obnovení plánování v praxi, najdete v sekci toto video:
    
    > [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/One-click-failover-of-a-2-tier-WordPress-application-using-Azure-Site-Recovery/player]
