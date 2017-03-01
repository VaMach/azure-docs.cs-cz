---
title: "Vyloučení disku z ochrany pomocí Azure Site Recovery | Dokumentace Microsoftu"
description: "Tento článek popisuje, proč a jak vyloučit disky virtuálních počítačů z replikace pro scénáře z VMware do Azure a Hyper-V do Azure."
services: site-recovery
documentationcenter: 
author: nsoneji
manager: garavd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 1/24/2017
ms.author: nisoneji
translationtype: Human Translation
ms.sourcegitcommit: af0d66d92ca542f415de779fb638db166ba5f26a
ms.openlocfilehash: 5e0527fb0a41d8892c9e22d6d6d2f252972e69d0


---
#<a name="exclude-disk-from-replication"></a>Vyloučení disků z replikace
Tento článek popisuje, jak vyloučit disky z replikace za účelem optimalizace jak šířky pásma potřebné pro replikaci, tak i prostředků využívaných na cílové straně těmito disky. Tato funkce je podporovaná ve scénářích VMware do Azure a Hyper-V do Azure.

##<a name="prerequisites"></a>Požadavky

Ve výchozím nastavení se replikují všechny disky virtuálního počítače. Chcete-li některý disk z replikace vyloučit, je třeba na virtuální počítač ručně nainstalovat službu Mobility ještě předtím, než povolíte replikaci **z VMware do Azure**


## <a name="why-exclude-disks-from-replication"></a>Proč vylučovat disky z replikace?
Vyloučení disků z replikace je často nutné z těchto důvodů:

1. Často měněná data uložená na vyloučených discích nejsou důležitá nebo nemusí být replikována.

2. Odstraněním potřeby replikace těchto častých změn dat se šetří úložné a síťové prostředky.

##<a name="what-are-the-typical-scenarios"></a>Jaké jsou typické scénáře?
Existují některé konkrétní příklady často měněných dat, které je snadné identifikovat a které jsou skvělými kandidáty pro vyloučení – například všechny změny stránkovacích souborů, databází tempdb systému Microsoft SQL Server atd. V závislosti na zatížení a subsystému úložiště může stránkovací soubor zaznamenat značné množství změn dat. Replikace těchto dat z primární lokality do Azure by však byla náročná na prostředky. Replikaci virtuálního počítače s jedním virtuálním diskem obsahujícím operační systém i stránkovací soubor proto můžete optimalizovat takto:

1. Rozdělíte jeden virtuální disk na dva – jeden s operačním systémem a druhý se stránkovacím souborem.
2. Disk se stránkovacím souborem vyloučíte z replikace.

Podobně pro Microsoft SQL Server, který má databázi tempdb i systémovou databázi na jednom disku můžete replikaci optimalizovat takto:

1. Systémovou databázi a databázi tempdb budete uchovávat na dvou různých discích.
2. Disk s databází tempdb vyloučíte z replikace.

##<a name="how-to-exclude-disk-from-replication"></a>Jak vyloučit disk z replikace

###<a name="vmware-to-azure"></a>Z VMware do Azure
Postupujte podle návodu pro [povolení replikace](site-recovery-vmware-to-azure.md#enable-replication) za účelem ochrany virtuálního počítače z portálu Azure Site Recovery. Ve 4. kroku daného postupu uvidíte sloupec **DISK PRO REPLIKACI** který můžete použít pro vyloučení disku z replikace. Ve výchozím nastavení jsou vybrány všechny disky. Zrušte výběr disku, který chcete vyloučit z replikace, a dokončete postup povolení replikace. 

![Povolení replikace](./media/site-recovery-exclude-disk/v2a-enable-replication-exclude-disk1.png)
    
    
>[!NOTE]
> 
> * Můžete vyloučit jenom disky, na kterých už je nainstalovaná služba Mobility. Službu Mobility musíte nainstalovat ručně, protože se jinak instaluje pomocí mechanismu nabízených instalací až po povolení replikace.
> * Z replikace můžete vyloučit pouze běžné disky. Nemůžete vyloučit disk operačního systému ani dynamické disky.
> * Po povolení replikace už není možné přidávat nebo odebírat disky pro replikaci. Pokud chcete přidat nebo vyloučit disk, budete muset zakázat ochranu virtuálního počítače a potom ji znovu povolit.
> * Pokud vyloučíte disk, který je nezbytný pro provoz nějaké aplikace, po převzetí služeb při selhání do Azure ho budete muset v Azure znovu ručně vytvořit, aby se replikovaná aplikace mohla spustit. Alternativně můžete do plánu obnovení integrovat službu Azure Automation, která disk vytvoří během převzetí služeb při selhání počítače.
> * Virtuální počítače s Windows: Disky, které ručně vytvoříte v Azure, nebude možné po navrácení služeb obnovit. Například když při selhání převezmete tři disky a pak přímo ve virtuálním počítači Azure vytvoříte další dva, při navrácení služeb po obnovení se přenesou jen tři disky replikované při selhání. Ručně vytvořené disky není možné zahrnout do navrácení služeb po obnovení ani do zpětné replikace z místního počítače do Azure.
> * Virtuální počítače s Linuxem: Disky, které ručně vytvoříte v Azure, se při navrácení služeb obnoví. Například když při selhání převezmete tři disky a pak přímo v Azure vytvoříte další dva, při navrácení služeb se obnoví všech pět. Ručně vytvořené disky nemůžete vyloučit z obnovení při navrácení služeb.
> 

###<a name="hyper-v-to-azure"></a>Z Hyper-V do Azure
Postupujte podle návodu pro [povolení replikace](site-recovery-hyper-v-site-to-azure.md#step-6-enable-replication) za účelem ochrany virtuálního počítače z portálu Azure Site Recovery. Ve 4. kroku daného postupu uvidíte sloupec **DISK PRO REPLIKACI** který můžete použít pro vyloučení disku z replikace. Ve výchozím nastavení jsou pro replikaci vybrány všechny disky. Zrušte výběr disku, který chcete vyloučit z replikace, a dokončete postup povolení replikace. 

![Povolení replikace](./media/site-recovery-vmm-to-azure/enable-replication6-with-exclude-disk.png)
    
>[!NOTE]
> 
> * Z replikace můžete vyloučit pouze běžné disky. Nelze vyloučit disk operačního systému a nedoporučuje se ani vyloučení dynamických disků. Azure Site Recovery nemůže určit, které disky VHD ve virtuálním počítači hosta jsou běžné a které dynamické.  Pokud nejsou vyloučeny všechny disky se závislými dynamickými svazky, z chráněného dynamického disku se na virtuálním počítači po převzetím služeb při selhání stane chybný disk a data na takovém disku budou nepřístupná.    
> * Po povolení replikace už není možné přidávat nebo odebírat disky pro replikaci. Pokud chcete přidat nebo vyloučit disk, budete muset zakázat ochranu virtuálního počítače a potom ji znovu povolit.
> * Pokud vyloučíte disk, který je nezbytný pro provoz aplikace, po převzetí služeb při selhání do Azure jej budete muset v Azure znovu ručně vytvořit, aby se replikovaná aplikace mohla spustit. Alternativně můžete do plánu obnovení integrovat službu Azure Automation, která disk vytvoří během převzetí služeb při selhání počítače.
> * Disky, které ručně vytvoříte v Azure, nebude možné po obnovení navrátit. Například pokud provedete převzetí služeb při selhání u tří disků a dva disky vytvoříte přímo ve virtuálním počítači Azure, po obnovení z Azure do Hyper-V se navrátí pouze tři disky, u nichž se provedlo převzetí služeb při selhání. Ručně vytvořené disky není možné zahrnout do navrácení služeb po obnovení ani do zpětné replikace z Hyper-V do Azure.
 


##<a name="end-to-end-scenarios-of-exclude-disks"></a>Úplné scénáře vyloučení disků
Pojďme se podívat na dva scénáře pro lepší porozumění funkci vyloučení disku.

1. Disk s databází tempdb systému SQL Server
2. Disk se stránkovacím souborem

###<a name="excluding-the-sql-server-tempdb-disk"></a>Vyloučení disku s databází tempdb systému SQL Server
Uvažujme virtuální počítač se systémem SQL Server, který používá databázi tempdb, kterou chcete vyloučit z replikace.

Název virtuálního počítače: SalesDB Disks na zdrojovém virtuálním počítači:


**Název disku** | **Č. disku hostovaného OS** | **Písmeno jednotky** | **Typ dat na disku**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 | C:\ | Disk OS
DB-Disk1| Disk1 | D:\ | Databáze systému SQL a uživatelská databáze&1;
DB-Disk2 (disk vyloučený z ochrany) | Disk2 | E:\ | Dočasné soubory
DB-Disk3 (disk vyloučený z ochrany) | Disk3 | F:\ | SQL databáze tempdb (cesta ke složce (F:\MSSQL\Data\) --> poznamenejte si cestu ke složce před převzetím služeb při selhání
DB Disk4 | Disk4 |G:\ |Uživatelská databáze&2;

Protože změny dat na dvou discích virtuálního počítače jsou ve své podstatě dočasné, při ochraně virtuálního počítače SalesDB můžete vyloučit z replikace disky Disk2 a Disk3. Azure Site Recovery nebude tyto disky replikovat a po převzetí služeb při selhání tyto disky nebudou na cílovém virtuálním počítači v Azure připojené.

Disky ve virtuálním počítači Azure po převzetí služeb při selhání:

**Č. disku hostovaného OS** | **Písmeno jednotky** | **Typ dat na disku**
--- | --- | ---
DISK0 |    C:\ | Disk OS
Disk1 |    E:\ | Dočasné úložiště [Azure tento disk přidá a přiřadí mu první dostupné písmeno jednotky]
Disk2 | D:\ | Databáze systému SQL a uživatelská databáze&1;
Disk3 | G:\ | Uživatelská databáze&2;

Vzhledem k tomu, že Disk2 a Disk3 byly vyloučeny z virtuálního počítače SalesDB, je první volné písmeno jednotky E:. Azure přiřadí E: svazku dočasného úložiště. Pro všechny replikované disky zůstala písmena jednotek stejná.

Disk3, na kterém je SQL databáze tempdb (cesta ke složce databáze tempdb je F:\MSSQL\Data\) a který je z replikace vyloučený, není na virtuálním počítači po převzetí služeb při selhání dostupný. V důsledku toho je služba SQL zastavená a je nutné zajistit cestu F:\MSSQL\Data.

Tuto cestu můžete vytvořit dvěma způsoby.

1. Přidejte nový disk a přiřaďte mu cestu ke složce databáze tempdb, nebo
2. použijte stávající pro složku s databází tempdb disk dočasného úložiště.

####<a name="add-a-new-disk"></a>Přidání nového disku:

1. Poznamenejte si cestu k souborům SQL tempdb.mdf a tempdb.ldf před převzetím služeb při selhání.
2. Na webu Azure Portal přidejte do virtuálního počítače přebírajícího služby nový disk se stejnou nebo větší velikostí, jakou měl disk SQL databáze tempdb (Disk3).
3. Přihlaste se k virtuálnímu počítači Azure. Z konzoly pro správu disků (diskmgmt.msc) inicializujte a naformátujte nově přidaný disk.
4. Přiřadíte stejné písmeno jednotky, kterou používal disk SQL databáze tempdb (F:).
5. Vytvořte složku databáze tempdb na svazku F: (F:\MSSQL\Data).
6. Spusťte službu SQL z konzoly služby.

####<a name="use-existing-temporary-storage-disk-for-sql-tempdb-folder-path"></a>Použití stávajícího dočasného disku pro složku s databází tempdb:

1. Otevřete konzolu příkazového řádku.
2. Spusťte z příkazového řádku konzoly SQL Server v režimu obnovení.

        Net start MSSQLSERVER /f / T3608

3. Spuštěním následujícího příkazu sqlcmd změňte cestu k databázi tempdb na novou cestu.

        sqlcmd -A -S SalesDB        **Use your SQL DBname**
        USE master;        
        GO        
        ALTER DATABASE tempdb        
        MODIFY FILE (NAME = tempdev, FILENAME = 'E:\MSSQL\tempdata\tempdb.mdf');
        GO        
        ALTER DATABASE tempdb        
        MODIFY FILE (NAME = templog, FILENAME = 'E:\MSSQL\tempdata\templog.ldf');        
        GO


4. Zastavte službu Microsoft SQL Server.

        Net stop MSSQLSERVER
5. Spusťte službu Microsoft SQL Server.

        Net start MSSQLSERVER

Seznamte se s následujícími pravidly Azure pro dočasné disky:

* Použití disků SSD disky ve virtuálních počítačích Azure k uložení databáze TempDB systému SQL Server a rozšíření fondu vyrovnávací paměti
* Osvědčené postupy z hlediska výkonu pro SQL Server na Azure Virtual Machines

###<a name="failback-from-azure-to-on-premises"></a>Navrácení služeb po obnovení (z Azure na místní počítač)
Nyní se pojďme podívat na to, jak se budou všechny disky replikovat při navrácení služeb z Azure na místního hostitele VMware nebo Hyper-V. Disky, které vytvoříte v Azure ručně, se replikovat nebudou. Například když při selhání převezmete tři disky a pak přímo ve virtuálním počítači Azure vytvoříte další dva, při navrácení služeb po obnovení se přenesou jen tři disky replikované při selhání. Ručně vytvořené disky není možné zahrnout do navrácení služeb po obnovení ani do zpětné replikace z místního počítače do Azure. Také se nereplikují disky s dočasným úložištěm na místní počítač.

####<a name="failback-to-original-location-recovery-olr"></a>Navrácení služeb po obnovení do původního umístění (OLR)

V předchozím příkladu je konfigurace disků virtuálního počítače Azure následující:

**Č. disku hostovaného OS** | **Písmeno jednotky** | **Typ dat na disku** 
--- | --- | --- 
DISK0 | C:\ | Disk OS
Disk1 |    E:\ | Dočasné úložiště [Azure tento disk přidá a přiřadí mu první dostupné písmeno jednotky]
Disk2 |    D:\ | Databáze systému SQL a uživatelská databáze&1;
Disk3 |    G:\ | Uživatelská databáze&2;


####<a name="vmware-to-azure"></a>Z VMware do Azure
Po dokončení navrácení služeb po obnovení do původního umístění neobsahuje konfigurace disků obnoveného virtuálního počítače vyloučené disky. To znamená, že disky vyloučené z replikace z VMware do Azure nebudou po navrácení služeb po obnovení virtuálního počítače dostupné. 

Po naplánovaném převzetí služeb při selhání z Azure do místního počítače VMware budou disky na virtuálním počítači VMWare (v původním umístění) následující:

**Č. disku hostovaného OS** | **Písmeno jednotky** | **Typ dat na disku** 
--- | --- | --- 
DISK0 | C:\ | Disk OS
Disk1 |    D:\ | Databáze systému SQL a uživatelská databáze&1;
Disk2 |    G:\ | Uživatelská databáze&2;

####<a name="hyper-v-to-azure"></a>Z Hyper-V do Azure
Po dokončení navrácení služeb po obnovení do původního umístění zůstává konfigurace disků stejná jako konfigurace disků původního virtuálního počítače Hyper-V. To znamená, že disky vyloučené z replikace z Hyper-V do Azure budou po navrácení služeb po obnovení dostupné.

Po naplánovaném navrácení služeb z Azure do místního počítače Hyper-V budou disky na virtuálním počítači Hyper-V (v původním umístění) následující:

**Název disku** | **Č. disku hostovaného OS** | **Písmeno jednotky** | **Typ dat na disku**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 |    C:\ | Disk OS
DB-Disk1 | Disk1 | D:\ | Databáze systému SQL a uživatelská databáze&1;
DB-Disk2 (vyloučený disk) | Disk2 | E:\ | Dočasné soubory
DB-Disk3 (vyloučený disk) | Disk3 | F:\ | SQL databáze tempdb (cesta ke složce (F:\MSSQL\Data\)
DB Disk4 | Disk4 | G:\ | Uživatelská databáze&2;


####<a name="exclude-paging-file-disk"></a>Vyloučení disku se stránkovacím souborem

Uvažujme virtuální počítač s diskem pro stránkovací soubor, který je možné vyloučit z replikace.
Existují dva možné případy:

####<a name="case-1-pagefile-is-configured-on-the-d-drive"></a>Případ 1: Stránkovací soubor je nakonfigurován na jednotce D:
Konfigurace disků:


**Název disku** | **Č. disku hostovaného OS** | **Písmeno jednotky** | **Typ dat na disku**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 | C:\ | Disk OS
DB-Disk1 (disk vyloučený z ochrany) | Disk1 | D:\ | pagefile.sys
DB-Disk2 | Disk2 | E:\ | Uživatelská data 1
DB-Disk3 | Disk3 | F:\ | Uživatelská data 2

Nastavení stránkovacího souboru na zdrojovém virtuálním počítači:

![Povolení replikace](./media/site-recovery-exclude-disk/pagefile-on-d-drive-sourceVM.png)
    

Po převzetí služeb při selhání virtuálního počítače z VMware do Azure nebo z Hyper-V do Azure budou disky na virtuálním počítači v Azure následující:
**Název disku** | **Č. disku hostovaného OS** | **Písmeno jednotky** | **Typ dat na disku**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 | C:\ | Disk OS
DB-Disk1 | Disk1 | D:\ | Dočasné úložiště – > pagefile.sys
DB-Disk2 | Disk2 | E:\ | Uživatelská data 1
DB-Disk3 | Disk3 | F:\ | Uživatelská data 2

Vzhledem k tomu, že Disk&1; (D:) byl vyloučený, je D: první volné písmeno jednotky a Azure ho přiřadí svazku dočasného úložiště.  Protože je jednotka D: na virtuálním počítači Azure dostupná, nastavení stránkovacího souboru virtuálního počítače se nezmění.

Nastavení stránkovacího souboru na virtuálním počítači v Azure:

![Povolení replikace](./media/site-recovery-exclude-disk/pagefile-on-Azure-vm-after-failover.png)

####<a name="case-2-pagefile-file-is-configured-on-any-other-driveother-than-d-drive"></a>Případ 2: Soubor stránkovacího souboru je nakonfigurovaný na jakékoli jiné jednotce (než je D:)

Konfigurace disků zdrojového virtuálního počítače:

**Název disku** | **Č. disku hostovaného OS** | **Písmeno jednotky** | **Typ dat na disku**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 | C:\ | Disk OS
DB-Disk1 (disk vyloučený z ochrany) | Disk1 | G:\ | pagefile.sys
DB-Disk2 | Disk2 | E:\ | Uživatelská data 1
DB-Disk3 | Disk3 | F:\ | Uživatelská data 2

Nastavení stránkovacího souboru na místním virtuálním počítači:

![Povolení replikace](./media/site-recovery-exclude-disk/pagefile-on-g-drive-sourceVM.png)

Po převzetí služeb při selhání virtuálního počítače z VMware/Hyper-V do Azure budou disky na virtuálním počítači v Azure následující:

**Název disku**| **Č. disku hostovaného OS**| **Písmeno jednotky** | **Typ dat na disku**
--- | --- | --- | ---
DB-Disk0-OS | DISK0  |C:\ |Disk OS
DB-Disk1 | Disk1 | D:\ | Dočasné úložiště – > pagefile.sys
DB-Disk2 | Disk2 | E:\ | Uživatelská data 1
DB-Disk3 | Disk3 | F:\ | Uživatelská data 2

Vzhledem k tomu, že D: je první volné písmeno jednotky, Azure ho přiřadí svazku dočasného úložiště. Pro všechny replikované disky zůstala písmena jednotek stejná. Protože disk G: není k dispozici, systém použije pro stránkovací soubor jednotku C:.

Nastavení stránkovacího souboru na virtuálním počítači v Azure:

![Povolení replikace](./media/site-recovery-exclude-disk/pagefile-on-Azure-vm-after-failover-2.png)

## <a name="next-steps"></a>Další kroky
Po nasazení a zprovoznění nasazení si můžete přečíst [další informace](site-recovery-failover.md) o různých typech převzetí služeb při selhání.



<!--HONumber=Feb17_HO3-->


