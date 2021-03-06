---
title: "Informace o službě Azure Migrate | Dokumentace Microsoftu"
description: "Obsahuje přehled služby Azure Migrate."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: overview
ms.date: 01/08/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: a9e04c7fa2a32ab7be8844b962f4bccdf260af23
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="about-azure-migrate"></a>Informace o službě Azure Migrate

Služba Azure Migrate posuzuje místní úlohy pro migraci do Azure. Služba posuzuje vhodnost místních počítačů k migraci do Azure a velikost na základě výkonu a poskytuje odhad nákladů na provoz místních počítačů v Azure. Pokud zvažujete migrace metodou „lift and shift“ nebo se nacházíte v počátečních fázích posuzování migrace, tato služba je určená přímo pro vás. Po posouzení můžete počítače migrovat do Azure pomocí služeb, jako jsou [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) a [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview).

## <a name="why-use-azure-migrate"></a>Proč používat službu Azure Migrate?

Azure Migrate vám pomůže s následujícími úlohami:

- **Posouzení připravenosti pro Azure:** Posuďte, jestli jsou vaše místní počítače vhodné pro provoz v Azure. 
- **Získání doporučení týkajících se velikosti:** Získejte doporučení k velikosti pro virtuální počítače Azure na základě historie výkonu místních virtuálních počítačů. 
- **Odhadované měsíční náklady:** Získejte odhadované náklady na provoz místních počítačů v Azure.  
- **Migrace s větší jistotou:** Vizualizací závislostí místních počítačů můžete vytvářet skupiny počítačů, které budete posuzovat a migrovat společně. 

## <a name="current-limitations"></a>Aktuální omezení

- Aktuálně můžete posuzovat vhodnost k migraci na virtuální počítače Azure pouze u místních virtuálních počítačů VMware. Virtuální počítače VMware musí být spravované systémem vCenter Server (verze 5.5, 6.0 nebo 6.5).

> [!NOTE]
> Podpora Hyper-V se plánuje a bude brzy povolená. Prozatím doporučujeme k plánování migrace úloh Hyper-V použít [Plánovač nasazení služby Azure Site Recovery](http://aka.ms/asr-dp-hyperv-doc). 

- Je možné vyhledat až 1 000 virtuálních počítačů v rámci jednoho zjišťování a až 1 500 virtuálních počítačů v jednom projektu. Kromě toho můžete v rámci jednoho interního hodnocení vyhodnotit až 400 virtuálních počítačů. Pokud jich potřebuje vyhledat nebo posoudit víc, můžete zvýšit počet zjišťování nebo hodnocení. [Další informace](how-to-scale-assessment.md).
- Projekt Azure Migrate můžete vytvořit pouze v oblasti Západní USA – střed nebo USA – východ. To však nemá vliv na možnost plánování migrace pro jiné cílové umístění Azure. Umístění projektu migrace slouží pouze k uložení metadat zjištěných v místním prostředí.
- Azure Migrate podporuje pro posouzení migrace jenom spravované disky.

## <a name="what-do-i-need-to-pay-for"></a>Za co musím platit?

Další informace o cenách služby Azure Migrate najdete [zde](https://azure.microsoft.com/en-in/pricing/details/azure-migrate/).


## <a name="whats-in-an-assessment"></a>Co je součástí posouzení?

Posouzení pomáhá identifikovat vhodnost Azure pro místní virtuální počítače a získat doporučení pro správné velikosti a odhady nákladů pro provoz virtuálních počítačů v Azure. Posouzení můžete přizpůsobit svým požadavkům změnou vlastností posouzení. Níže jsou uvedené vlastnosti, na které se bere ohled při vytváření posouzení. 

**Vlastnost** | **Podrobnosti**
--- | ---
**Cílové umístění** | Umístění Azure, do kterého chcete migrovat. Cílové umístění je standardně nastavené na Západní USA 2. 
**Redundance úložiště** | Typ [redundance úložiště](https://docs.microsoft.com/azure/storage/common/storage-redundancy), který budou po migraci využívat virtuální počítače Azure. Výchozí je místně redundantní úložiště (LRS). Poznámka: Azure Migrate podporuje pouze posouzení založená na spravovaných discích a spravované disky podporují pouze LRS, proto má vlastnost v současné době pouze možnost LRS. 
**Kritérium určení velikosti** | Kritérium, podle kterého Azure Migrate určí správnou velikost virtuálních počítačů pro Azure. Velikost virtuálních počítačů pro Azure můžete určit na základě *historie výkonu* místních virtuálních počítačů nebo stejnou *jako u místních* virtuálních počítačů bez zohlednění historie výkonu. Výchozí hodnota je určení velikosti na základě výkonu.
**Cenové plány** | Pro účely výpočtů nákladů posouzení zohledňuje, jestli máte program Software Assurance a jestli máte nárok na [Zvýhodněné hybridní využití Azure](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Zohledňuje také [Nabídky Azure](https://azure.microsoft.com/support/legal/offer-details/), které máte případně zaregistrované, a umožňuje zadat jakékoli slevy (%) specifické pro předplatné, na které máte nárok nad rámec nabídky. 
**Cenová úroveň** | Můžete zadat [cenovou úroveň (Basic nebo Standard)](../virtual-machines/windows/sizes-general.md) cílových virtuálních počítačů Azure. Pokud například plánujete migrovat produkční prostředí, měli byste zvážit úroveň Standard, která poskytuje virtuální počítače s nízkou latencí, ale může být dražší. Na druhou stranu, pokud máte prostředí pro vývoj a testování, měli byste zvážit úroveň Basic s virtuálními počítači s vyšší latencí, která je levnější. Ve výchozím nastavení se použije úroveň [Standard](../virtual-machines/windows/sizes-general.md).
**Historie výkonu** | Tuto možnost můžete použít, pouze pokud je kritérium určení velikosti na základě výkonu. Azure Migrate ve výchozím nastavení vyhodnocuje výkon místních počítačů s využitím historie výkonu za poslední den a hodnoty 95. percentilu. Tyto hodnoty můžete upravit ve vlastnostech posouzení. 
**Faktor komfortu** | Azure Migrate při posuzování počítá s rezervou (faktor komfortu). Tato rezerva se použije nad rámec dat o využití počítače pro virtuální počítače (procesor, paměť, disk a síť). Důvodem použití faktoru komfortu jsou problémy, jako jsou sezónní využití, krátká historie výkonu a pravděpodobný růst budoucího využití.<br/><br/> Například z virtuálního počítače s 10 jádry a 20% využitím je normálně ve výsledku virtuální počítač se 2 jádry. S faktorem komfortu 2,0× je však výsledkem virtuální počítač se 4 jádry. Výchozí nastavení komfortu je 1,3×.


## <a name="how-does-azure-migrate-work"></a>Jak služba Azure Migrate funguje?

1.  Vytvoříte projekt Azure Migrate.
2.  Azure Migrate pomocí místního počítače označovaného jako zařízení kolektoru zjistí informace o místních počítačích. Toto zařízení vytvoříte tak, že stáhnete instalační soubor ve formátu .ova (Open Virtualization Appliance) a importujete ho jako virtuální počítač na místní vCenter Server.
3.  Připojíte se k virtuálnímu počítači pomocí připojení konzoly ve vCenter Serveru, během připojování zadáte nové heslo pro virtuální počítač a potom ve virtuálním počítači spustíte aplikaci kolektoru, aby se zahájilo zjišťování.
4.  Kolektor pomocí rutin VMware PowerCLI shromáždí metadata virtuálního počítače. Zjišťování probíhá bez agenta a na hostitele VMware ani virtuální počítače se nic neinstaluje. Shromážděná metadata obsahují informace o virtuálním počítači (jádra, paměť, disky, velikosti disků a síťové adaptéry). Shromáždí se také data o výkonu virtuálních počítačů, včetně využití procesoru a paměti, IOPS disku, propustnosti disku (Mb/s) a výstupu sítě (MB/s).
5.  Metadata se vloží do projektu Azure Migrate. Můžete je zobrazit na webu Azure Portal.
6.  Pro účely posouzení shromáždíte zjištěné virtuální počítače do skupin. Můžete například seskupit virtuální počítače, na kterých běží stejná aplikace. Pro přesnější seskupení můžete použít vizualizaci závislostí, zobrazit závislosti pro konkrétní počítač nebo pro všechny počítače ve skupině a skupinu upřesnit.
7.  Jakmile je skupina zformována, vytvoříte posouzení pro skupinu. 
8.  Po dokončení můžete posouzení zobrazit na portálu nebo si ho stáhnout ve formátu aplikace Excel.



  ![Architektura Plánovače Azure](./media/migration-planner-overview/overview-1.png)

## <a name="what-are-the-port-requirements"></a>Jaké jsou požadavky na porty?

Tabulka shrnuje porty potřebné ke komunikaci služby Azure Migrate.

|Komponenta          |Komunikace s     |Požadovaný port  |Důvod   |
|-------------------|------------------------|---------------|---------|
|Kolektor          |Služba Azure Migrate   |TCP 443        |Kolektor se ke službě připojuje přes port SSL 443.|
|Kolektor          |vCenter Server          |Výchozí 9443   | Ve výchozím nastavení se kolektor připojuje k systému vCenter Server na portu 9443. Pokud server naslouchá na jiném portu, tento port musí být nakonfigurovaný jako odchozí port na virtuálním počítači kolektoru. |
|Místní virtuální počítač     | Pracovní prostor Operations Management Suite (OMS)          |[TCP 443](../log-analytics/log-analytics-windows-agent.md) |Agent MMA používá port TCP 443 pro připojení k Log Analytics. Tento port potřebujete pouze v případě, že využíváte funkci vizualizace závislostí a instalujete agenta Microsoft Monitoring Agent (MMA). |


  
## <a name="what-happens-after-assessment"></a>Co se stane po posouzení?

Po posouzení místních počítačů pro migraci pomocí služby Azure Migrate můžete provést migraci pomocí několika nástrojů:

- **Azure Site Recovery:** Pomocí Azure Site Recovery můžete provést migraci do Azure následujícím způsobem:
  - Připravte prostředky Azure, včetně předplatného Azure, virtuální sítě Azure a účtu úložiště.
  - Připravte místní servery VMware na migraci. Ověříte požadavky na podporu VMware pro Site Recovery, připravíte servery VMware na zjišťování a připravíte se na instalaci služby mobility Site Recovery na virtuální počítače, které chcete migrovat. 
  - Nastavte migraci. Nastavíte trezor služby Recovery Services, nakonfigurujete nastavení migrace pro zdroj a cíl, nastavíte zásady replikace a povolíte replikaci. Můžete spustit postup zotavení po havárii a zkontrolovat, že migrace virtuálního počítače do Azure funguje správně.
  - Spusťte převzetí služeb při selhání a migrujte místní počítače do Azure. 
  - [Další informace](../site-recovery/tutorial-migrate-on-premises-to-azure.md) najdete v kurzu migrace pomocí Site Recovery.

- **Azure Database Migration:** Pokud na místních počítačích běží databáze, jako je SQL Server, MySQL nebo Oracle, pomocí služby Azure Database Migration Service je můžete migrovat do Azure. [Další informace](https://azure.microsoft.com/campaigns/database-migration/).



## <a name="next-steps"></a>Další kroky 
[Postupujte podle kurzu](tutorial-assessment-vmware.md) a vytvořte posouzení pro místní virtuální počítač VMware.