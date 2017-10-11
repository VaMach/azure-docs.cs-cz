---
title: "Replikace virtuálních počítačů VMware a fyzické servery do Azure (classic starší verze) | Microsoft Docs"
description: "Popisuje, jak replikovat místní virtuální počítače a fyzické servery Windows nebo Linuxem do Azure pomocí Azure Site Recovery ve starší verzi nasazení portálu classic."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: f980fb52-8ec2-4dd9-85e2-8e52e449f1ba
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: raynew
ms.openlocfilehash: 325be23cffc9c728a8af6f92a0f3dce6d31da4ae
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="replicate-vmware-virtual-machines-and-physical-servers-to-azure-with-azure-site-recovery-using-the-classic-portal-legacy"></a>Replikace virtuálních počítačů VMware a fyzické servery do Azure s Azure Site Recovery pomocí portálu classic (zastaralé)
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmware-to-azure.md)
> * [Portál Classic](site-recovery-vmware-to-azure-classic.md)
> * [Portál Classic (zastaralé)](site-recovery-vmware-to-azure-classic-legacy.md)
>
>

Vítejte v Azure Site Recovery! Tento článek popisuje starší verze nasazení pro replikaci na lokální virtuální počítače VMware nebo fyzických serverů Windows nebo Linuxem do Azure pomocí Azure Site Recovery na portálu classic.

## <a name="overview"></a>Přehled
Organizace potřebují strategii BCDR, která určuje, jak aplikace, úlohy a data zůstanou spuštěné a dostupné během plánovaných a neplánovaných výpadků a jak co nejdříve obnovit normální provozní podmínky. Strategie BCDR by měla zajistit bezpečnost a obnovitelnost firemních dat a zajistit, aby v případě, že dojde k havárii, byly zpracovávané úlohy stále k dispozici.

Site Recovery je služba Azure, která přispívá ke strategie BCDR orchestrací replikace místní fyzických serverů a virtuálních počítačů do cloudu (Azure) nebo do sekundárního datového centra. Pokud dojde k výpadkům ve vašem primárním umístění, předáte služby při selhání do sekundárního umístění, aby aplikace a úlohy zůstaly dostupné. Až se obnoví normální provozní podmínky, vrátíte služby po obnovení zpět do primárního umístění. Další informace najdete v článku [Co je Azure Site Recovery](site-recovery-overview.md).

> [!WARNING]
> Tento článek obsahuje **starší verze pokyny**. Nepoužívejte ho pro nová nasazení. Místo toho [postupujte podle těchto pokynů](site-recovery-vmware-to-azure.md) nasazení Site Recovery na portálu Azure nebo [použijte tyto pokyny](site-recovery-vmware-to-azure-classic.md) ke konfiguraci rozšířeného nasazení portálu classic. Pokud jste už nasazená pomocí metody popsané v tomto článku, doporučujeme migrovat do rozšířeného nasazení portálu classic.
>
>

## <a name="migrate-to-the-enhanced-deployment"></a>Migrace do rozšířeného nasazení
Tato část platí pouze pokud jste už nasazená Site Recovery pomocí pokynů v tomto článku.

Pokud chcete migrovat existující nasazení, které budete muset:

1. Nasaďte nové součásti Site Recovery ve vaší místní lokalitě.
2. Nakonfigurujte přihlašovací údaje pro automatické zjišťování virtuálních počítačů VMware na serveru nové konfigurace.
3. Vyhledat servery VMware s novým serverem konfigurace.
4. Vytvořte novou skupinu ochrany s novým serverem konfigurace.

Než začnete, potřebujete:

* Doporučujeme, abyste nastavili údržby pro migraci.
* **Migraci počítačů** možnost je dostupná pouze v případě, že máte existující skupiny ochrany, které byly vytvořeny při nasazení starší verze.
* Po dokončení kroků migrace může trvat 15 minut nebo déle aktualizujte přihlašovací údaje a zjistit a aktualizovat virtuální počítače, takže je můžete přidat do skupiny ochrany. Můžete je aktualizovat ručně místo čekání.

Migraci následujícím způsobem:

1. Přečtěte si informace o [rozšířené nasazení portálu classic](site-recovery-vmware-to-azure-classic.md). Zkontrolujte rozšířené [architektura](site-recovery-vmware-to-azure-classic.md), a [požadavky](site-recovery-vmware-to-azure-classic.md).
2. Odinstalujte službu Mobility z počítače, které aktuálně replikujete. Nová verze služby se nainstalují na počítačích, když přidáte do nové skupiny ochrany.
3. Stáhněte si [registrační klíč trezoru](site-recovery-vmware-to-azure-classic.md) a [spusťte Průvodce instalací jednotná](site-recovery-vmware-to-azure-classic.md) instalace konfigurační server, procesový server a hlavní cílový server součásti. Další informace o [plánování kapacity](site-recovery-vmware-to-azure-classic.md).
4. [Nastavit přihlašovací údaje](site-recovery-vmware-to-azure-classic.md) , Site Recovery můžete použít pro přístup k serveru VMware k automatickému zjišťování virtuálních počítačů VMware. Další informace o [požadovaná oprávnění](site-recovery-vmware-to-azure-classic.md).
5. Přidat [vCenter servery nebo hostitelů vSphere](site-recovery-vmware-to-azure-classic.md). Může trvat 15 minut, další informace pro servery se objeví na portálu Site Recovery.
6. Vytvoření [nové skupiny ochrany](site-recovery-vmware-to-azure-classic.md). To může trvat až 15 minut pro portál aktualizovat tak, aby virtuální počítače jsou zjišťovány a zobrazí. Pokud nechcete čekat můžete zvýraznit název serveru pro správu (nemáte klikněte na něj) > **aktualizovat**.
7. V části nové skupiny ochrany klikněte na **migraci počítačů**.

    ![Přidat účet](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration1.png)
8. V **vyberte počítače** vyberte skupinu ochrany, kterou chcete migrovat z a na počítače, které chcete migrovat.

    ![Přidat účet](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration2.png)
9. V **nakonfigurovat nastavení cílového** zadejte, zda chcete používat stejné nastavení pro všechny počítače a vyberte procesový server a účet úložiště Azure. Pokud nemáte samostatný procesový server bude adresu IP serveru konfigurační server.

    ![Přidat účet](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration3.png)

    > [AZURE.NOTE] [Migrace účtů úložiště](../resource-group-move-resources.md) napříč skupinami prostředků v rámci stejného předplatného nebo napříč předplatnými se pro účty úložiště použité k nasazení Site Recovery nepodporuje.

1. V **zadejte účty**, vyberte účet, který jste vytvořili u procesového serveru pro přístup k počítači tak, aby nabízel nová verze služby Mobility.

   ![Přidat účet](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration4.png)
2. Site Recovery bude migrovat replikovaných dat do účtu úložiště Azure, který jste zadali. Volitelně můžete znovu použít účet úložiště, které jste použili v starší verze nasazení.
3. Po dokončení úlohy virtuálních počítačů se automaticky synchronizují. Po dokončení synchronizace můžete odstranit virtuální počítače ze skupiny ochrany starší verze.
4. Po migraci všech počítačů můžete odstranit skupinu ochrany starší verze.
5. Nezapomeňte zadat vlastnosti převzetí služeb při selhání pro počítače a nastavení Azure sítě po dokončení synchronizace.
6. Pokud máte existující plány obnovení, můžete je migrovat do rozšířeného nasazení s **migrovat plán obnovení** možnost. Měli byste jenom to provést po migraci všechny chráněné počítače.

   ![Přidat účet](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration5.png)

> [!NOTE]
> Po dokončení migrace pokračovat [rozšířené článku](site-recovery-vmware-to-azure-classic.md). Zbývající část tohoto článku starší verze bude relevantní a vy nemusíte postupujte podle kroků popsaných v it ** žádné další.
>
>

## <a name="what-do-i-need"></a>Co musím udělat?
Tento diagram zobrazuje součásti nasazení.

![Nový trezor](./media/site-recovery-vmware-to-azure-classic-legacy/architecture.png)

Zde je seznam toho, co budete potřebovat:

| **Komponenta** | **Nasazení** | **Podrobnosti** |
| --- | --- | --- |
| **Konfigurační server** |Standardní A3 virtuální počítač Azure ve stejném předplatném jako Site Recovery. |Konfigurační server koordinuje komunikaci mezi chráněné počítače, procesový server a hlavních cílových serverů v Azure. Když dojde k převzetí služeb při selhání, nastaví se replikace a obnovení souřadnice v Azure. |
| **Hlavní cílový server** |Virtuální počítač Azure – serveru systému Windows na základě v galerii bitovou kopii systému Windows Server 2012 R2 (Chcete-li chránit počítače s Windows) nebo jako Linux server podle Galerie bitovou kopii OpenLogic CentOS 6.6 (k ochraně počítačů se systémem Linux).<br/><br/> Tři nastavení velikosti možností, které jsou k dispozici – standardní A4, standardní D14 a standardní DS4.<br/><br/> Server je připojený ke stejné síti Azure jako konfigurační server.<br/><br/> Nastavení na portálu Site Recovery |Přijetí a uchovává replikovaná data z vaší chráněných počítačů pomocí připojených virtuálních pevných disků na úložiště objektů blob v účtu úložiště Azure vytvořit.<br/><br/> Vyberte standardní DS4 speciálně pro konfiguraci ochrany pro úlohy vyžadující konzistentní vysoký výkon a nízkou latencí pomocí prémiový účet úložiště. |
| **Procesový server** |Místní virtuální nebo fyzická serveru se systémem Windows Server 2012 R2<br/><br/> Doporučujeme, abyste je umístěná ve stejné síti a segment sítě LAN jako na počítače, které chcete chránit, ale můžete spustit v jiné síti, dokud chráněné počítače mají viditelnost L3 sítě do ní.<br/><br/> Můžete ho nastavit a zaregistrujte ho na konfiguračním serveru na portálu Site Recovery. |Chráněné počítače odesílat data replikace na serveru místní proces. Obsahuje diskové mezipaměti pro mezipaměť replikace dat, kterou přijme. Provede několik akcí na tato data.<br/><br/> Optimalizuje data pomocí ukládání do mezipaměti, komprese a šifrování před odesláním na hlavním cílovém serveru.<br/><br/> Zpracovává nabízená instalace služby Mobility.<br/><br/> Provádí automatického zjišťování virtuálních počítačů VMware. |
| **Místní počítače** |Místní virtuální počítače VMware nebo fyzické servery se systémem Windows nebo Linux. |Nakonfigurujete nastavení replikace, které se vztahují na jeden nebo více počítačů. Přes jednotlivé počítače nebo běžně, může selhat více počítačů, které shromažďování do plánu obnovení. |
| **Služba mobility** |Nainstalovat na každý virtuální počítač nebo na fyzickém serveru, který chcete chránit<br/><br/> Můžete nainstalovat ručně nebo nabídnutých a automaticky nainstalují procesní server, když povolíte replikaci pro počítač. |Služba Mobility odesílá data na procesní server během počáteční replikace (synchronizaci). Po tento počítač je v chráněném stavu (po dokončení nové synchronizace) služba Mobility zaznamená zápisy na disk v paměti a odešle je na procesní server. Applicationconsistency pro systémy Windows Server je dosaženo pomocí služby VSS. |
| **Trezor služby Azure Site Recovery** |Vytvoření trezoru Site Recovery předplatné a zaregistrujte server v trezoru. |Trezor koordinuje a orchestruje replikaci, převzetí služeb při selhání a obnovení mezi místními servery a Azure. |
| **Mechanismus replikace** |**Přes Internet**– komunikuje a replikují data z chráněných místních serverů do Azure pomocí zabezpečené protokolem SSL/TLS kanálu přes internet. Toto je výchozí možnost.<br/><br/> **Sítě VPN nebo ExpressRoute**– komunikuje a replikují data mezi místními servery a Azure prostřednictvím připojení VPN. Budete muset nastavit síť site-to-site VPN nebo ExpressRoute připojení mezi místními servery a sítě Azure.<br/><br/> Vyberte způsob replikace během nasazování Site Recovery. Tento mechanismus nelze změnit po dokončení konfigurace bez dopadu na replikaci existující počítačů. |Žádná možnost vyžaduje, abyste povoleny porty příchozích síťových na chráněných počítačích. Veškerá komunikace sítě je zahájena z místního webu. |

## <a name="capacity-planning"></a>Plánování kapacity
Hlavní oblasti, které je potřeba vzít v úvahu:

* **Zdrojové prostředí**– infrastruktury VMware, nastavení zdrojového počítače a požadavky.
* **Serverech součástí**– procesový server, konfigurační server a hlavní cílový server

### <a name="considerations-for-the-source-environment"></a>Důležité informace týkající se zdrojové prostředí
* **Maximální velikost disku**– aktuální maximální velikost disku, který může být připojen k virtuálnímu počítači je 1 TB. Proto je maximální velikost zdrojového disku, který je možné replikovat také omezena na 1 TB.
* **Maximální velikost na zdroj**– maximální velikost jeden zdrojový počítač je 31 TB (s disky, 31) a s instancí D14 zřízené pro hlavní cílový server.
* **Počet zdrojů na hlavním cílovém serveru**– více zdrojového počítače se dají chránit pomocí jednoho hlavní cílový server. Ale jeden zdrojový počítač nelze chránit napříč více hlavních cílových serverů, protože jako disky replikovat, virtuálního pevného disku, které odpovídá velikost disku je vytvořen v úložišti objektů blob Azure a připojit k hlavnímu cílovému serveru jako datový disk.  
* **Maximální denní míry změn na zdroj**– existují tři faktory, které je potřeba zvážit při zvažování míru doporučené změn na zdroj. Důležité informace na základě cílové vyžadují se dvě IOPS na cílový disk jednotlivých operací ve zdroji. Je to proto, že se stane čtení stará data a zápis nových dat na cílový disk.
  * **Každý den změnit rychlost podporovanou procesový server**– zdrojový počítač nemůžou zahrnovat víc serverů procesu. Jeden proces serveru může podporovat až 1 TB denní míry změn. Proto je 1 TB maximální denních dat změnit rychlost pro zdrojový počítač nepodporuje.
  * **Maximální propustnost nepodporuje cílový disk**– maximální změn na zdrojový disk nemůže být více než 144 GB a den (s 8 kb velikost zápisu). Pro různé zápisu velikosti, najdete v tabulce v části hlavní cíl pro větší propustnost a IOPs cíle. Toto číslo musí být rozdělen ve dvou, protože každý zdroj IOP generuje 2 IOPS na cílový disk. Přečtěte si informace o [Azure škálovatelnosti a cílech výkonnosti](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks) při konfiguraci cíle pro účty úložiště premium.
  * **Maximální propustnost nepodporuje účet úložiště**– zdroj nemůžou zahrnovat víc účtů úložiště. Vzhledem k že trvá účtu úložiště nesmí být delší než 20 000 požadavků za sekundu a že každý zdroj IOP generuje 2 IOPS na hlavním cílovém serveru, doporučujeme, abyste že zachovat počet IOPS mezi zdroji s 10 000. Přečtěte si informace o [Azure škálovatelnosti a cílech výkonnosti](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks) při konfiguraci zdroje pro účty úložiště premium.

### <a name="considerations-for-component-servers"></a>Důležité informace týkající se serverech součástí
Tabulka 1 shrnuje velikostí virtuálních počítačů pro konfiguraci a hlavních cílových serverů.

| **Komponenta** | **Nasazené instancemi Azure** | **Počet jader** | **Paměť** | **Maximální počet disků** | **Velikost disku** |
| --- | --- | --- | --- | --- | --- |
| Konfigurace serveru |Standardní A3 |4 |7 GB |8 |1023 GB |
| Hlavní cílový server |Standardní A4 |8 |14 GB |16 |1023 GB |
| Standardní D14 |16 |112 GB |32 |1023 GB | |
| Standardní DS4 |8 |28 GB |16 |1023 GB | |

**Tabulka 1**

#### <a name="process-server-considerations"></a>Důležité informace o zpracování serveru
Obecně proces serveru velikosti závisí na denní míra změn mezi všechny chráněné úlohy.

* Budete potřebovat dostatek výpočetního k provádění úloh, jako je například vložené komprese a šifrování.
* Procesový server používá mezipaměti založené na disku. Zkontrolujte místo na doporučené mezipaměti a propustnost disku je k dispozici pro usnadnění změny dat, které jsou uložené v případě přetížení sítě nebo výpadek.
* Zajistěte dostatečnou šířku pásma, aby se procesový server můžete nahrát data do hlavní cílový server zajistit trvalou ochranu dat.

Tabulka 2 poskytuje souhrn podle pokynů proces serveru.

| **Míry změny dat** | **VYUŽITÍ PROCESORU** | **Paměť** | **Velikost disku mezipaměti** | **Propustnost disku mezipaměti** | **Vstupní/výstupní šířky pásma** |
| --- | --- | --- | --- | --- | --- |
| < 300 GB |4 Vcpu (2 sockets * @ 2.5 GHz 2 jádra) |4 GB |600 GB |7 až 10 MB za sekundu |30 MB/s nebo 21 MB/s |
| 300 až 600 GB |8 Vcpu (2 sockets * @ 2.5 GHz 4 jádra) |6 GB |600 GB |11 až 15 MB za sekundu |60 MB/s nebo 42 MB/s |
| 600 GB až 1 TB |12 Vcpu (2 sockets * @ 2.5 GHz 6 jader) |8 GB |600 GB |16 až 20 MB za sekundu |100 MB/s nebo 70 MB/s |
| > 1 TB |Nasazení jiný procesový server | | | | |

**Tabulka 2**

Kde:

* Příjem příchozích dat je stažení šířky pásma (intranetu mezi serverem pro zdroje a proces).
* Odchozí je nahrávání šířky pásma (internet mezi procesovým serverem a hlavní cílový server). Odchozí čísla předpokládá průměrná komprese 30 % procesu serveru.
* Pro mezipaměť samostatný disk operačního systému minimální 128 GB disk se doporučuje pro všechny servery procesu.
* Pro propustnost disku mezipaměti byl použit následující úložiště pro srovnávací testy: 8 jednotky SAS 10 ot. / s konfiguraci RAID 10.

#### <a name="configuration-server-considerations"></a>Požadavky na konfiguraci serveru
Každý konfigurační server může podporovat až 100 zdrojový počítač se svazky 3 – 4. Pokud vaše nasazení je větší, doporučujeme že nasadit další konfigurační server. Vlastnosti virtuálního počítače výchozí konfigurace serveru najdete v tabulce 1.

#### <a name="master-target-server-and-storage-account-considerations"></a>Hlavní cílový server a úložiště důležité informace o účtu
Úložiště pro každou hlavní cílový server obsahuje disk s operačním systémem, svazek pro uchovávání dat a datových disků. Jednotka pro uchování udržuje deník změny na disku po dobu trvání okna uchování definované v portálu Site Recovery.  Vlastnosti virtuálního počítače z hlavního cílového serveru naleznete v tabulce 1. Tabulka 3 ukazuje, jak se používají disky A4.

| **Instance** | **Disk s operačním systémem** | **Uchování** | **Datové disky** |
| --- | --- | --- | --- |
| **Uchování** |**Datové disky** | | |
| Standardní A4 |disk 1 (1 * 1023 GB) |disk 1 (1 * 1023 GB) |15 disky (15 * 1023 GB) |
| Standardní D14 |disk 1 (1 * 1023 GB) |disk 1 (1 * 1023 GB) |31 disky (15 * 1023 GB) |
| Standardní DS4 |disk 1 (1 * 1023 GB) |disk 1 (1 * 1023 GB) |15 disky (15 * 1023 GB) |

**Tabulka 3**

Plánování kapacity pro hlavní cílový server, závisí na:

* Výkon úložiště Azure a omezení
  * Maximální počet vysoce využívat disky pro standardní vrstvy virtuálního počítače, je o 40 (20 000/500 IOPS na disk) v jednom úložném účtu. Přečtěte si informace o [cíle škálovatelnosti pro účty úložiště standard storage](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks) a [prémiové účty úložiště](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks).
* Denní míry změn
* Uchování svazku úložiště.

Poznámky:

* Jeden zdroj nemůžou zahrnovat víc účtů úložiště. To platí pro datový disk, který přejít k účtům úložiště vybrali při konfiguraci ochrany. Operačního systému a uchování disky, které obvykle přejděte na účet úložiště automaticky nasazené.
* Svazek pro uchovávání dat úložiště vyžaduje, závisí na denní míra změn a počet dní uchovávání informací. Uchování úložiště požadované za hlavní cílový server = celkového objemu změn ze zdroje za den * počet dní uchovávání informací.
* Každý hlavní cílový server obsahuje pouze jeden svazek pro uchovávání. Svazek pro uchovávání dat byl sdílen napříč disky připojené k hlavnímu cílovému serveru. Například:
  * Pokud je zdrojový počítač s 5 disků a každý disk generuje 120 IOPS (8 kb velikost) ve zdroji, výsledkem 240 IOPS na disku (2 operací na cílový disk na zdroj vstupů/výstupů). 240 IOPS je v rámci Azure za maximální IOPS disku 500.
  * Na svazek pro uchovávání dat, to všechno bude 120 * 5 = 600 IOPS a to se může stát zúžené bottle. V tomto scénáři bude strategii je dobré přidejte další disky do je svazek pro uchovávání a span ji napříč, jako stripe konfigurace RAID. Tím se zvyšuje výkon, protože IOPS jsou rozmístěny v několika jednotkách. Počet jednotek, které mají být přidány do svazek pro uchovávání dat bude takto:
    * Celkový počet IOPS ze zdrojové prostředí / 500
    * Celkového objemu změn za den (nekomprimovaným) na zdrojové prostředí nebo 287 GB. 287 GB je maximální propustnost nepodporuje cílový disk za den. Tato metrika budou lišit v závislosti na velikosti zápisu s faktor 8 kB, protože v takovém případě je 8 kb thí předpokládá, že velikost zápisu. Například pokud je velikost zápisu 4 kB pak propustnost bude 287/2. A pokud je velikost zápisu 16 kB pak propustnost bude 287 * 2.
* Počet účtů úložiště vyžaduje = source celkový počet IOPs/10000.

## <a name="before-you-start"></a>Než začnete
| **Komponenta** | **Požadavky** | **Podrobnosti** |
| --- | --- | --- |
| **Účet Azure** |Budete potřebovat účet [Microsoft Azure](https://azure.microsoft.com/). Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/). | |
| **Úložiště Azure** |Budete potřebovat účet úložiště Azure k ukládání replikovaných dat<br/><br/> Musí být buď účet [standardní geograficky redundantní účet úložiště](../storage/common/storage-redundancy.md#geo-redundant-storage) nebo [prémiový účet úložiště](../storage/common/storage-premium-storage.md).<br/><br/> Musí být ve stejné oblasti jako služba Azure Site Recovery a musí být přidružený ke stejnému předplatnému. Nepodporujeme přesun účty úložiště vytvořené pomocí [nový portál Azure](../storage/common/storage-create-storage-account.md) mezi skupinami prostředků.<br/><br/> Další čtení [Úvod do Microsoft Azure Storage](../storage/common/storage-introduction.md) | |
| **Virtuální síť Azure** |Budete potřebovat virtuální síť Azure na kterém se bude nasazený konfigurační server a hlavní cílový server. To by měl být ve stejném předplatném, oblasti jako trezor Azure Site Recovery. Pokud chcete replikovat data přes ExpressRoute nebo VPN připojení musí být připojen virtuální síť Azure k místní síti prostřednictvím připojení ExpressRoute nebo VPN typu Site-to-Site. | |
| **Prostředky Azure** |Ujistěte se, že máte dostatek prostředků Azure k nasazení všech součástí. Další informace najdete v [limity předplatného Azure](../azure-subscription-service-limits.md). | |
| **Virtuální počítače Azure** |Virtuální počítače, které chcete chránit musí být v souladu s [požadavky Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).<br/><br/> **Disk počet**– na jednom chráněného serveru může podporovat maximálně 31 disků<br/><br/> **Velikost disku**– kapacita jednotlivých disků nesmí být větší než 1 023 GB<br/><br/> **Clustering**– servery clusteru nejsou podporovány.<br/><br/> **Spouštěcí**– Firmware rozhraní UEFI (Unified Extensible) / není podporované spouštění Extensible Firmware Interface<br/><br/> **Svazky**– Bitlocker šifrované svazky nejsou podporované.<br/><br/> **Názvy serverů**– názvy musí obsahovat 1 až 63 znaků (písmena, číslice a pomlčky). Název musí začínat písmenem nebo číslicí a končit písmenem nebo číslicí. Po počítač je chráněný můžete upravit název Azure. | |
| **Konfigurační server** |Standardní virtuální počítač A3 založena na imagi Galerie Azure Site Recovery Windows Server 2012 R2 se vytvoří ve vašem předplatném pro konfigurační server. Je vytvořen jako první instanci novou cloudovou službu. Pokud vyberete jako typ připojení pro konfigurační server veřejného Internetu, vytvoří se s vyhrazené veřejné IP adresy cloudové služby.<br/><br/> Cesta instalace musí být ve pouze anglické znaky. | |
| **Hlavní cílový server** |Virtuální počítač Azure, standardní A4, D14 nebo DS4.<br/><br/> Cesta instalace musí být ve pouze anglické znaky. Například by měla být cesta **/usr/local/ASR** u hlavního cílového serveru se systémem Linux. | |
| **Procesový server** |Můžete nasadit procesový server na fyzický nebo virtuální počítač se systémem Windows Server 2012 R2 s nejnovějšími aktualizacemi. Nainstalujte na jednotce C: /.<br/><br/> Doporučujeme že umístit server na stejnou síť a podsíť jako počítače, které chcete chránit.<br/><br/> Nainstalujte VMware rozhraní příkazového řádku vSphere 5.5.0 na procesovém serveru. Komponentu VMware vSphere rozhraní příkazového řádku je vyžadováno na procesovém serveru k zjišťování virtuálních počítačů spravovanou serverem vCenter server nebo virtuálních počítačů spuštěných na hostiteli ESXi.<br/><br/> Cesta instalace musí být ve pouze anglické znaky.<br/><br/> Systém souborů reFS nepodporuje. | |
| **VMware** |Správa vašeho hypervisory VMware vSphere server VMware vCenter. Měla by být spuštěna s vCenter verze 5.1 nebo 5.5 s nejnovějšími aktualizacemi.<br/><br/> Jeden nebo více hypervisory vSphere obsahující virtuální počítače VMware, které chcete chránit. Hypervisor by měl běžet ESX/ESXi verze 5.1 nebo 5.5 s nejnovějšími aktualizacemi.<br/><br/> Virtuální počítače VMware by měl mít nástroje VMware nainstalovaná a spuštěná. | |
| **Počítače s Windows** |Chráněné fyzických serverech nebo virtuálních počítačů VMware s Windows mají několik požadavků.<br/><br/> A podporovaný 64bitový operační systém: **Windows Server 2012 R2**, **systému Windows Server 2012**, nebo **Windows Server 2008 R2 s v minimálně SP1**.<br/><br/> Název hostitele, přípojné body, zařízení názvů, cestu v systému Windows (např: C:\Windows) by měly být pouze v angličtině.<br/><br/> Operační systém by měly být nainstalovány na jednotce C:\.<br/><br/> Jsou podporovány pouze základní disky. Dynamické disky nejsou podporovány.<br/><br/> Pravidla brány firewall na chráněné počítače by mohly dosáhne cílové servery konfigurace a hlavní v Azure.p ><p>Budete muset zadat účet správce (musí být místní správce na počítači systému Windows) na nabízenou instalaci služby Mobility na serverech s Windows. Pokud uvedený účet je jiný doménový účet musíte zakázat řízení vzdáleného přístupu uživatele v místním počítači. Stačí přidat položku registru LocalAccountTokenFilterPolicy DWORD s hodnotou 1 v části HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System. Chcete-li přidat položku registru z rozhraní příkazového řádku otevřené cmd nebo prostředí powershell a zadejte  **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`** . [Další informace](https://msdn.microsoft.com/library/aa826699.aspx) o řízení přístupu.<br/><br/> Po převzetí služeb při selhání Pokud chcete připojit k virtuální počítače s Windows v Azure pomocí vzdálené plochy Ujistěte se, zda je povoleno vzdálené plochy na místním počítači. Pokud se nepřipojujete prostřednictvím sítě VPN, pravidla brány firewall by měla umožnit připojení ke vzdálené ploše přes internet. | |
| **Počítače se systémem Linux** |Podporovaný 64bitový operační systém: **Centos 6.4, 6.5, 6.6**; **Oracle Enterprise Linux 6.4, 6.5 systémem Red Hat kompatibilní jádra nebo nedělitelné Enterprise jádra verze 3 (UEK3)**, **SUSE Linux Enterprise Server 11 SP3**.<br/><br/> Pravidla brány firewall na chráněné počítače by mohly dosáhnout konfigurace a hlavních cílových serverů v Azure.<br/><br/> soubory/etc/hosts na chráněných počítačích by měly obsahovat položky, které mapování názvu místního hostitele na IP adresy přidružené všechny síťové adaptéry <br/><br/> Pokud se chcete připojit k virtuální počítač Azure s Linuxem po převzetí služeb při selhání pomocí klienta Secure Shell (ssh), ujistěte se, že služba Secure Shell v chráněném počítači je nastavena na spustit automaticky při spuštění systému, a že povolit pravidla brány firewall ssh připojení k němu.<br/><br/> Název hostitele, přípojné body, názvy zařízení a Linux systémové cesty a názvy souborů (např/etc /; USR) by měla být v angličtině jenom.<br/><br/> Můžete povolit ochranu pro místní počítače s následující úložiště:-<br>Systém souborů: EXT3, ETX4, ReiserFS, XFS<br>Funkce Multipath softwaru zařízení Mapper (multipath)<br>Správce svazků: LVM2<br>Fyzické servery s HP CCISS řadič úložiště nejsou podporovány. | |
| **Třetí strany** |Některé součásti nasazení v tomto scénáři závisí na software jiných výrobců fungovat správně. Úplný seznam najdete v části [oznámení softwaru třetích stran a informace](#third-party) | |

### <a name="network-connectivity"></a>Připojení k síti
Máte dvě možnosti při konfiguraci připojení k síti mezi místními servery a virtuální síť Azure, na které jsou nasazeny součásti infrastruktury (konfigurační server, hlavních cílových serverů). Musíte se rozhodnout, která možnost připojení k síti předtím, než můžete nasadit konfigurační server. Budete potřebovat změnit tato nastavení v době nasazení. Nelze změnit později.

**Internet:** komunikace a replikaci dat mezi místními servery (procesový server, chráněných počítačů) a serverech součástí infrastrukturu Azure (konfigurační server, hlavní cílový server) se stane přes zabezpečené protokolem SSL/TLS připojení z místního na veřejné koncové body na cílové servery konfigurace a hlavní server. (Jedinou výjimkou je spojení mezi procesový server a hlavní cílový server na portu TCP 9080, což nešifrované. Jenom řízení informace související s protokolem replikace pro nastavení replikace se vyměňují na toto připojení.)

![Diagram nasazení Internetu](./media/site-recovery-vmware-to-azure-classic-legacy/internet-deployment.png)

**VPN**: komunikace a replikaci dat mezi místními servery (procesový server, chráněných počítačů) a serverech součástí infrastrukturu Azure (konfigurační server, hlavní cílový server) se stane prostřednictvím připojení VPN mezi v místní síti a virtuální síť Azure, na kterém je nasazená konfigurační server a hlavních cílových serverů. Zkontrolujte místní sítě je připojen k virtuální síti Azure připojení ExpressRoute nebo připojení site-to-site VPN.

![Diagram nasazení sítě VPN](./media/site-recovery-vmware-to-azure-classic-legacy/vpn-deployment.png)

## <a name="step-1-create-a-vault"></a>Krok 1: Vytvoření trezoru
1. Přihlaste se k [portálu pro správu](https://portal.azure.com).
2. Rozbalte položku **datové služby** > **služeb zotavení** a klikněte na tlačítko **trezor Site Recovery**.
3. Klikněte na **Vytvořit nový** > **Rychlé vytvoření**.
4. Jako **Název** zadejte popisný název pro identifikaci trezoru.
5. V rozevírací nabídce **Oblast** vyberte zeměpisnou oblast trezoru. Informace o tom, které oblasti jsou podporované, najdete v části s geografickou dostupností v tématu s [podrobnostmi o cenách Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Klikněte na **Vytvořit trezor**.

    ![Nový trezor](./media/site-recovery-vmware-to-azure-classic-legacy/quick-start-create-vault.png)

Podle informací na stavovém řádku si ověřte, že se trezor úspěšně vytvořil. Trezor bude uvedený jako **Active** v hlavním **služeb zotavení** stránky.

## <a name="step-2-deploy-a-configuration-server"></a>Krok 2: Nasazení konfigurace serveru
### <a name="configure-server-settings"></a>Nakonfigurujte nastavení serveru
1. Na stránce **Služby zotavení** kliknutím na trezor otevřete stránku Rychlý start. Stránku Rychlý Start je možné kdykoli otevřít pomocí ikony.

    ![Ikona Rychlý start](./media/site-recovery-vmware-to-azure-classic-legacy/quick-start-icon.png)
2. V rozevíracím seznamu vyberte **mezi místní lokalitě VMware nebo fyzické servery a Azure**.
3. V **Příprava prostředků Target(Azure)** klikněte na tlačítko **nasazení konfigurační Server**.

    ![Nasazení konfigurace serveru](./media/site-recovery-vmware-to-azure-classic-legacy/deploy-cs2.png)
4. V **nové podrobnosti o konfiguraci serveru** zadejte:

   * Název konfiguračního serveru a přihlašovací údaje pro připojení k němu.
   * V typu připojení k síti rozevírací nabídky vyberte **veřejného Internetu** nebo **VPN**. Všimněte si, že nelze změnit toto nastavení po jeho použití.
   * Vyberte síť Azure, na kterém server by měl být umístěn. Pokud používáte VPN zkontrolujte zda síť Azure je připojen k síti na pracovišti podle očekávání.
   * Zadejte interní IP adresu a podsíť, která bude přiřazena k serveru. Všimněte si, že první čtyři IP adresy v žádné podsíti jsou vyhrazené pro interní použití Azure. Použijte všechny další dostupnou IP adresu.

     ![Nasazení konfigurace serveru](./media/site-recovery-vmware-to-azure-classic-legacy/cs-details.png)
5. Když kliknete na tlačítko **OK** standardní virtuální počítač A3 založena na imagi Galerie Azure Site Recovery Windows Server 2012 R2 se vytvoří ve vašem předplatném pro konfigurační server. Je vytvořen jako první instanci novou cloudovou službu. Pokud jste vybrali pro připojení přes internet cloudové služby se vytvoří pomocí vyhrazené veřejné IP adresy. Můžete sledovat průběh **úlohy** kartě.

    ![Monitorování průběhu](./media/site-recovery-vmware-to-azure-classic-legacy/monitor-cs.png)
6. Pokud se připojujete přes internet, po konfigurační server je nasazený Poznámka veřejnou IP adresu k němu přiřazen na **virtuální počítače** na portálu Azure. Pak na **koncové body** poznamenejte si veřejné HTTPS portem namapován na privátní port 443. Tyto informace později budete potřebovat při registraci hlavního cíle a proces servery s konfiguračním serverem. Konfigurační server je nasazeno pomocí těchto koncových bodů:

   * HTTPS: Veřejný port slouží ke koordinaci komunikaci mezi servery součásti a Azure přes internet. Privátní port 443 se používá k řízení komunikace mezi servery součásti a Azure prostřednictvím sítě VPN.
   * Vlastní: Veřejný port se používá pro navrácení služeb po obnovení nástroj komunikaci přes internet. Privátní port 9443 se používá pro navrácení služeb po obnovení nástroj komunikaci prostřednictvím sítě VPN.
   * Prostředí PowerShell: Privátní port 5986
   * Vzdálená plocha: privátní port 3389

   ![Koncové body virtuálních počítačů](./media/site-recovery-vmware-to-azure-classic-legacy/vm-endpoints.png)

   > [!WARNING]
   > Nepřidávejte odstranit, nebo změňte číslo portu veřejných nebo privátních žádné koncové body vytvořené během nasazení konfiguračního serveru.
   >
   >

Konfigurační server je nasazena v automaticky vytvořené cloudové služby Azure s vyhrazenou IP adresu. Rezervovanou adresu je potřebný k tomu, že je IP adresa konfigurace serveru cloudové služby, zůstává stejná napříč restartování virtuálních počítačů (včetně konfigurační server) cloudové služby. Vyhrazené veřejné IP adresy bude muset být ručně nerezervované, pokud se konfigurační server je vyřazena z provozu nebo budete zůstanou vyhrazené. Výchozí limit 20 vyhrazené veřejné IP adresy každé předplatné není k dispozici. [Další informace](../virtual-network/virtual-networks-reserved-private-ip.md) o vyhrazené IP adresy.

### <a name="register-the-configuration-server-in-the-vault"></a>Zaregistrujte konfigurační server v trezoru
1. V **rychlý Start** klikněte na stránce **Příprava cílové prostředky** > **stáhněte si registrační klíč**. Soubor klíče je generován automaticky. Je platný po dobu 5 dní, po jeho vygenerování. Zkopírujte ho na konfiguračním serveru.
2. V **virtuální počítače** vyberte konfigurační server ze seznamu virtuálních počítačů. Otevřete **řídicí panel** a klikněte na **Connect**. **Otevřete** stažený soubor RDP přihlásit se na konfigurační server pomocí vzdálené plochy. Pokud používáte sítě VPN, použijte pro připojení ke vzdálené ploše z místní lokality interní IP adresu (adresa, kterou jste zadali při nasazení konfigurační server). Při prvním přihlášení se automaticky spustí Průvodce instalací na konfiguraci serveru Azure Site Recovery.

    ![Registrace](./media/site-recovery-vmware-to-azure-classic-legacy/splash.png)
3. V **instalace softwaru třetích stran** klikněte na tlačítko **souhlasím** ke stažení a instalaci MySQL.

    ![Instalace MySQL](./media/site-recovery-vmware-to-azure-classic-legacy/sql-eula.png)
4. V **podrobnosti o serveru databáze MySQL** vytvořit přihlašovací údaje pro přihlášení na instanci serveru MySQL.

    ![Přihlašovací údaje databáze MySQL](./media/site-recovery-vmware-to-azure-classic-legacy/sql-password.png)
5. V **nastavení Internetu** zadejte, jak se konfigurační server budou připojovat k Internetu. Poznámky:

   * Pokud chcete používat vlastní proxy server, měli byste ho nastavit před instalací zprostředkovatele.
   * Když kliknete na tlačítko **Další** testu se spustí zkontrolujte připojení k proxy serveru.
   * Pokud používáte vlastní proxy server nebo váš výchozí proxy server vyžaduje ověření, budete muset zadat podrobnosti o proxy serveru, včetně adresu, port a přihlašovací údaje.
   * Následující adresy URL musí být přístupné přes proxy server:
     * *.hypervrecoverymanager.windowsazure.com
     * *.accesscontrol.windows.net
     * *.backup.windowsazure.com
     * *.blob.core.windows.net
     * *.store.core.windows.net
   * Pokud máte IP adresy na základě pravidla brány firewall Ujistěte se, že pravidla umožňují komunikaci z konfiguračního serveru na IP adresy popsané v nastavení [rozsahy IP Datacentra Azure](https://msdn.microsoft.com/library/azure/dn175718.aspx) a protokol HTTPS (443). Museli byste se rozsahy IP seznamu povolených oblasti Azure, který chcete použít a západní USA.

     ![Registrace proxy serveru](./media/site-recovery-vmware-to-azure-classic-legacy/register-proxy.png)
6. V **nastavení zprostředkovatele chybových zpráv lokalizace** zadat jazyk, který chcete chybové zprávy, které se zobrazí.

    ![Chybová zpráva registrace](./media/site-recovery-vmware-to-azure-classic-legacy/register-locale.png)
7. V **Azure Site Recovery registrace** Procházet a vyberte soubor klíče zkopírovány na server.

    ![Soubor klíče registrace](./media/site-recovery-vmware-to-azure-classic-legacy/register-vault.png)
8. Na stránce dokončení průvodce vyberte tyto možnosti:

   * Vyberte **spustit dialogové okno správy účtu** k určení, že by měla otevřít dialogové okno Správa účtů, po dokončení průvodce.
   * Vyberte **vytvoření ikony na ploše pro Cspsconfigtool** přidat zástupce na ploše na konfiguračním serveru tak, aby můžete otevřít **Správa účtů** dialogové okno kdykoli bez nutnosti znovu spusťte průvodce.

     ![Dokončení registrace](./media/site-recovery-vmware-to-azure-classic-legacy/register-final.png)
9. Klikněte na tlačítko **Dokončit** dokončete průvodce. Generuje se přístupové heslo. Zkopírujte jej do zabezpečeného umístění. Budete potřebovat k ověřování a registrovat proces a hlavních cílových serverů s konfiguračním serverem. Slouží také k zajištění integrity kanál v komunikaci se serverem konfigurace. Můžete obnovit heslo, ale pak budete muset znovu zaregistrovat hlavní cíl a zpracovat servery pomocí nové přístupové heslo.

    ![Přístupové heslo](./media/site-recovery-vmware-to-azure-classic-legacy/passphrase.png)

Po registraci bude konfigurační server uvedený na **konfigurační servery** stránky v úložišti.

### <a name="set-up-and-manage-accounts"></a>Nastavit a spravovat účty
Během nasazování Site Recovery požadavků pověření pro následující akce:

* Účet VMware, takže thatSite obnovení můžete automaticky zjišťování virtuálních počítačů na server vCenter nebo hostitelů vSphere.
* Když přidáte počítačů pro ochranu, tak, aby Site Recovery můžete instalaci služby Mobility na ně.

Poté, co jste registrováni konfigurační server můžete otevřít **Správa účtů** dialogovém okně můžete přidávat a spravovat účty, které se mají použít pro tyto akce. Existuje několik způsobů, jak to udělat:

* Otevřete zkratku, kterou jste se rozhodli pro vytvoření pro dialogové okno na poslední stránce instalačním programu serveru nástroje configuration (cspsconfigtool).
* Otevřete dialog v dokončení instalačního programu Konfigurace serveru.

1. V **Správa účtů** klikněte na tlačítko **přidat účet**. Můžete také upravit a odstranit existující účty.

    ![Správa účtů](./media/site-recovery-vmware-to-azure-classic-legacy/manage-account.png)
2. V **Podrobnosti účtu** zadejte název účtu používat ve službě Azure a přihlašovací údaje (název domény a uživatelské).

    ![Správa účtů](./media/site-recovery-vmware-to-azure-classic-legacy/account-details.png)

### <a name="connect-to-the-configuration-server"></a>Připojení k serveru konfigurace
Existují dva způsoby, jak připojit na konfigurační server:

* Přes VPN typu site-to-site nebo připojením ExpressRoute
* Přes internet

Poznámky:

* Připojení k Internetu používá ve spojení s veřejná virtuální IP adresou serveru koncových bodů virtuálního počítače.
* Připojení VPN používá interní IP adresu serveru spolu s privátní porty koncových bodů.
* Je jednorázové rozhodnutí můžete rozhodnout, jestli pro připojení (řízení a replikace dat) z místní servery na různé servery součásti (konfigurační server, hlavní cílový server) běžící v Azure prostřednictvím připojení k síti VPN nebo internet. Nelze změnit, tato nastavení později. V takovém případě budete potřebovat k nasazení tohoto scénáře a znovu aktivujte ochranu vašeho počítače.  

## <a name="step-3-deploy-the-master-target-server"></a>Krok 3: Nasazení hlavní cílový server
1. Klikněte na tlačítko **Příprava prostředků Target(Azure)** > **nasadit hlavní cílový server**.
2. Zadejte podrobnosti o hlavního cílového serveru a přihlašovací údaje. Server nasadí ve stejné síti Azure jako konfigurační server. Když kliknete na tlačítko Dokončit virtuální počítač Azure bude vytvořen s Galerie bitovou kopii systému Windows nebo Linux.

    ![Nastavení cílového serveru](./media/site-recovery-vmware-to-azure-classic-legacy/target-details.png)

Všimněte si, že první čtyři IP adresy v žádné podsíti jsou vyhrazené pro interní použití Azure. Zadejte jakékoli další dostupnou IP adresu.

> [!NOTE]
> Při konfiguraci ochrany pro úlohy, které vyžadují konzistentní vysoké vstupně-výstupní výkon a nízkou latenci pro hostování zatížení s intenzivním vstupně-výstupních operací pomocí vyberte standardní DS4 [prémiový účet úložiště](../storage/common/storage-premium-storage.md).
>
>

1. Windows hlavní cílový server, který virtuální počítač je vytvořen s těmito koncovými body. Všimněte si, že veřejné koncové body jsou vytvořeny pouze v případě připojení prostřednictvím Internetu.

   * Vlastní: Veřejný port procesní server používá k odesílání dat replikace přes internet. Privátní port 9443 procesní server slouží k odesílání replikace dat do hlavní cílový server prostřednictvím sítě VPN.
   * Vlastní1: Veřejný port procesní server používá k odesílání metadata přes internet. Privátní port 9080 umožňuje procesní server poslat metadata do hlavní cílový server prostřednictvím sítě VPN.
   * Prostředí PowerShell: Privátní port 5986
   * Vzdálená plocha: privátní port 3389
2. Hlavní cílový server Linux virtuálního počítače je vytvořen s těmito koncovými body. Všimněte si, že veřejné koncové body jsou vytvořeny pouze v případě, že se připojujete přes internet.

   * Vlastní: Veřejný port proces server používá k odesílání replikace dat přes internet. Privátní port 9443 procesní server slouží k odesílání replikace dat do hlavní cílový server prostřednictvím sítě VPN.
   * Vlastní 1: Veřejný port je používán serverem proces odeslat metadata přes internet. Privátní port 9080 proces server používá k odeslání metadata pro hlavní cílový server prostřednictvím sítě VPN
   * SSH: Privátní port 22

     > [!WARNING]
     > Nepřidávejte odstranit, nebo změňte číslo portu veřejných nebo privátních každého koncové body vytvořené během nasazení v hlavní cílový server.
     >
     >
3. V **virtuální počítače** čekat na virtuální počítač spustil.

   * Pokud je Windows server Poznámka dolů vzdálené plochy podrobnosti.
   * Pokud je Linux server a se připojujete prostřednictvím sítě VPN Poznámka: na interní adresu IP virtuálního počítače. Pokud se připojujete přes internet Poznámka: veřejnou IP adresu.
4. Přihlášení k serveru pro dokončení instalace a zaregistrovat ji pomocí konfigurační server.
5. Pokud používáte systém Windows:

   1. Inicializujte připojení ke vzdálené ploše k virtuálnímu počítači. Při prvním přihlášení skript se spustí v okně prostředí PowerShell. Nemáte, zavřete ji. Po dokončení nástroj Konfigurace hostitele agenta se automaticky otevře k registraci serveru.
   2. V **konfigurace agenta hostitele** zadejte interní IP adresu konfiguračního serveru a port 443. I když nejsou připojení prostřednictvím sítě VPN, protože virtuální počítač je připojený ke stejné síti Azure jako konfigurační server, můžete použít interní adresa a privátní port 443. Nechte **použití HTTPS** povolena. Zadejte heslo pro konfigurační server, který jste si předtím poznamenali. Klikněte na tlačítko **OK** k registraci serveru. Možnosti NAT, můžete ignorovat. Nejsou používají.
   3. Pokud vaše odhadovaná uchování požadavek jednotky je více než 1 TB můžete nakonfigurovat svazek pro uchovávání dat (R:) pomocí virtuální disk a [prostory úložiště](http://blogs.technet.com/b/askpfeplat/archive/2013/10/21/storage-spaces-how-to-configure-storage-tiers-with-windows-server-2012-r2.aspx)

   ![Hlavní cílový server Windows](./media/site-recovery-vmware-to-azure-classic-legacy/target-register.png)
6. Pokud používáte systém Linux:

   1. Ujistěte se, že jste nainstalovali nejnovější Linux integrační služby (LIS) nainstalována před instalací hlavní cílový server. Můžete najít nejnovější verzi LIS společně s pokyny k instalaci [zde](https://www.microsoft.com/download/details.aspx?id=46842). Po instalaci LIS, restartujte počítač.
   2. V **prostředky Příprava cíle (Azure)** klikněte na tlačítko **stáhnout a nainstalovat další software (jenom pro Linux hlavní cílový Server)**. Zkopírujte soubor stažený vkládání k virtuálnímu počítači pomocí protokolu sftp klienta. Případně můžete přihlásit k nasazené linux hlavní cílový server a použít *wget http://go.microsoft.com/fwlink/?LinkID=529757&clcid=0x409* ke stažení na soubor.
   3. Přihlaste se k serveru pomocí klienta Secure Shell. Pokud jste připojení k síti Azure prostřednictvím sítě VPN použijte interní IP adresu. Jinak použijte externí IP adresu a veřejný koncový bod SSH.
   4. Extrahujte soubory z instalačního programu algoritmem gzip spuštěním: **funkce vkládání – xvzf Microsoft-ASR_UA_8.4.0.0_RHEL6-64***
      ![Linux hlavní cílový server](./media/site-recovery-vmware-to-azure-classic-legacy/linux-tar.png)
   5. Ujistěte se, že jste v adresáři, do které jste extrahovali obsah souboru vkládání.
   6. Kopírovat heslo konfiguračního serveru do místního souboru pomocí příkazu **echo  *`<passphrase>`*  > passphrase.txt**
   7. Spusťte příkaz "**sudo. / install -t obou - a -R hostitele /usr/local/ASR MasterTarget -d -i  *`<Configuration server internal IP address>`*  -p 443 -s y - c https -P passphrase.txt**".

      ![Registrovat cílový server](./media/site-recovery-vmware-to-azure-classic-legacy/linux-mt-install.png)
7. Počkejte několik minut (10 až 15) a na stránce zkontrolujte, jestli je hlavní cílový server uvedený registrovaný v **servery** > **konfigurační servery** **podrobnosti o serveru** kartě. Pokud používáte systém Linux a nezaregistroval hostitele konfigurační nástroj spusťte znovu z /usr/local/ASR/Vx/bin/hostconfigcli. Budete muset nastavit oprávnění k přístupu spuštěním chmod jako kořenového adresáře.

    ![Zkontrolujte cílový server](./media/site-recovery-vmware-to-azure-classic-legacy/target-server-list.png)

> [!NOTE]
> Může trvat až 15 minut, po dokončení pro hlavní cílový server uveden v portálu registrace. Chcete-li aktualizovat okamžitě, klikněte na tlačítko **aktualizovat** na **konfigurační servery** stránky.
>
>

## <a name="step-4-deploy-the-on-premises-process-server"></a>Krok 4: Nasazení na místním serveru procesu
Před zahájením doporučujeme nakonfigurovat statickou IP adresu na procesovém serveru tak, aby se zaručena bezpečnost pro trvalé po restartování počítače.

1. Klikněte na tlačítko rychlý Start > **místní instalaci Server proces** > **stáhnout a nainstalovat procesní server**.

    ![Instalace serveru procesu](./media/site-recovery-vmware-to-azure-classic-legacy/ps-deploy.png)
2. Zkopírujte soubor zip staženého na server, na který se chystáte instalovat procesový server. Tento soubor zip obsahuje dva soubory instalace:

   * Microsoft-ASR_CX_TP_8.4.0.0_Windows*
   * Microsoft-ASR_CX_8.4.0.0_Windows*
3. Rozbalte archivu a zkopírujte instalační soubory do umístění na serveru.
4. Spustit **Microsoft-ASR_CX_TP_8.4.0.0_Windows*** instalace souboru a postupujte podle pokynů. Tím se nainstaluje komponenty jiných výrobců, které jsou potřebné pro nasazení.
5. Spusťte **Microsoft-ASR_CX_8.4.0.0_Windows***.
6. Na **režim serveru** vyberte **procesový Server**.
7. Na **prostředí podrobnosti** proveďte následující:

    - Pokud chcete chránit, klikněte na virtuální počítače VMware **Ano**
    - Pokud chcete pouze chránit fyzických serverů a proto není nutné vCLI VMware na procesovém serveru nainstalované. Klikněte na tlačítko **ne** a pokračovat.

1. Při instalaci VMware vCLI, pamatujte na tyhle:

   * **Je podporován pouze VMware rozhraní příkazového řádku vSphere 5.5.0**. Procesový server nefunguje s jinými verzemi nebo aktualizace rozhraní příkazového řádku vSphere.
   * Stáhnout 5.5.0 rozhraní příkazového řádku vSphere z [sem.](https://my.vmware.com/web/vmware/details?downloadGroup=VCLI550&productId=352)
   * Pokud jste nainstalovali rozhraní příkazového řádku vSphere těsně před jste spustili instalaci procesový server a instalační program nezjišťuje, počkejte, než až pět minut, než znovu spusťte instalaci. Tím se zajistí, že všechny proměnné prostředí potřebné pro zjišťování rozhraní příkazového řádku vSphere mít správně inicializován.
2. V **výběr síťovou kartu pro procesový Server** vyberte síťový adaptér, který by měly používat procesový server.

   ![Vyberte adaptér](./media/site-recovery-vmware-to-azure-classic-legacy/ps-nic.png)
3. V **podrobnosti o konfiguraci serveru**:

   * Pro IP adresu a port Pokud se připojujete prostřednictvím sítě VPN zadejte interní IP adresu konfiguračního serveru a 443 port. V opačném případě zadejte veřejná virtuální IP adresy a namapované veřejný koncový bod HTTP.
   * Zadejte heslo konfiguračního serveru.
   * Zrušte **ověřte Mobility service softwaru podpis** Pokud chcete zakázat ověření při použití automatické nabízené instalace služby. Ověření podpisu vyžaduje připojení k Internetu z procesového serveru.
   * Klikněte na **Další**.

   ![Zaregistrujte konfigurační server](./media/site-recovery-vmware-to-azure-classic-legacy/ps-cs.png)
4. V **vyberte instalační jednotce** vyberte jednotky mezipaměti. Procesový server potřebuje mezipaměti jednotku s alespoň 600 GB volného místa. Pak klikněte na **Nainstalovat**.

   ![Zaregistrujte konfigurační server](./media/site-recovery-vmware-to-azure-classic-legacy/ps-cache.png)
5. Všimněte si, že pravděpodobně nutné restartovat server a dokončete instalaci. V **konfigurační Server** > **podrobnosti o serveru** zkontrolujte, zda se zobrazí procesový server a je úspěšně zaregistrován v trezoru.

> [!NOTE]
> Může trvat až 15 minut, po dokončení procesu serveru zobrazit, jak je uvedeno v části konfigurační server registrace. Chcete-li aktualizovat okamžitě, aktualizujte konfigurační server kliknutím na tlačítko Aktualizovat v dolní části stránky konfigurace serveru
>
>

![Ověření procesového serveru](./media/site-recovery-vmware-to-azure-classic-legacy/ps-register.png)

Pokud zakážete nebylo ověření podpisu u služby Mobility a při registraci procesový server, můžete provést později následujícím způsobem:

1. Přihlaste se procesový server jako správce a otevřete soubor C:\pushinstallsvc\pushinstaller.conf pro úpravy. V části **[PushInstaller.transport]** přidejte tento řádek: **SignatureVerificationChecks = "0"**. Uložte a zavřete soubor.
2. Restartujte službu InMage PushInstall.

## <a name="step-5-update-site-recovery-components"></a>Krok 5: Aktualizace Site Recovery součásti
Čas od času jsou aktualizované součásti Site Recovery. Když jsou k dispozici nové aktualizace byste měli instalovat v následujícím pořadí:

1. Konfigurace serveru
2. Procesový server
3. Hlavní cílový server
4. Nástroj pro navrácení služeb po obnovení (vContinuum)

### <a name="obtain-and-install-the-updates"></a>Získání a instalace aktualizací
1. Aktualizace pro konfiguraci, proces a hlavních cílových serverů můžete získat od obnovení lokality **řídicí panel**. Pro Linux instalace extrahujte soubory z algoritmem gzip instalačního programu a spusťte příkaz "sudo. / install" k instalaci aktualizace.
2. [Stáhněte si](http://go.microsoft.com/fwlink/?LinkID=533813) nejnovější aktualizace pro tool(vContinuum) navrácení služeb po obnovení.
3. Pokud používáte virtuální počítače nebo fyzické servery, které už máte nainstalovánu službu Mobility, můžete získat aktualizace pro službu následujícím způsobem:

   * **Možnost 1**: Stažení aktualizací:
     * [Windows Server (pouze 64bitová verze)](http://download.microsoft.com/download/8/4/8/8487F25A-E7D9-4810-99E4-6C18DF13A6D3/Microsoft-ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe)
     * [CentOS 6.4,6.5,6.6 (pouze 64bitová verze)](http://download.microsoft.com/download/7/E/D/7ED50614-1FE1-41F8-B4D2-25D73F623E9B/Microsoft-ASR_UA_8.4.0.0_RHEL6-64_GA_28Jul2015_release.tar.gz)
     * [Oracle 6.4,6.5 Enterprise Linux (pouze 64bitová verze)](http://download.microsoft.com/download/5/2/6/526AFE4B-7280-4DC6-B10B-BA3FD18B8091/Microsoft-ASR_UA_8.4.0.0_OL6-64_GA_28Jul2015_release.tar.gz)
     * [SUSE Linux Enterprise Server SP3 (pouze 64bitová verze)](http://download.microsoft.com/download/B/4/2/B4229162-C25C-4DB2-AD40-D0AE90F92305/Microsoft-ASR_UA_8.4.0.0_SLES11-SP3-64_GA_28Jul2015_release.tar.gz)
     * Po aktualizaci serveru proces bude aktualizovaná verze služby Mobility k dispozici ve složce C:\pushinstallsvc\repository na procesovém serveru.
   * **Možnost 2**: Pokud máte počítač s starší verze služby Mobility, který je nainstalován, dokáže automaticky upgradovat služba Mobility na počítači z portálu pro správu.

     1. Zajistěte, aby aktualizovala procesový server.
     2. Ujistěte se, že chráněný počítač splňuje [požadavky](#install-the-mobility-service-automatically) pro automaticky nabízení služby Mobility, tak, aby aktualizace funguje podle očekávání.
     3. Vyberte skupinu ochrany, zvýrazněte chráněný počítač a klikněte na **služba Mobility aktualizace**. Toto tlačítko je k dispozici, pokud existuje novější verze služby Mobility pouze.

         ![Vyberte vCenter server](./media/site-recovery-vmware-to-azure-classic-legacy/update-mobility.png)

Vyberte účty určete účet správce, který se má použít k aktualizaci služby mobility na chráněném serveru. Klikněte na tlačítko OK a počkejte na dokončení spouštěná úlohy.

## <a name="step-6-add-vcenter-servers-or-vsphere-hosts"></a>Krok 6: Přidejte servery vCenter nebo hostitelů vSphere
1. Klikněte na tlačítko **servery** > **konfigurační servery** > konfigurační server >**přidání systému vCenter Server** k přidání systému vCenter server nebo vSphere hostitele.

    ![Vyberte vCenter server](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter.png)
2. Zadejte podrobnosti o serveru nebo hostitele a vyberte procesový server, který se použije chcete zjistit.

   * Pokud vCenter server není spuštěna na výchozím portu 443 zadejte číslo portu, na kterém je spuštěný vCenter server.
   * Procesový server musí být ve stejné síti jako hostitel vSphere serveru vCenter a má mít VMware vSphere 5.5.0 nainstalované rozhraní příkazového řádku.

     ![nastavení serveru vCenter](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter4.png)
3. Po dokončení zjišťování serveru vCenter se objeví v části Podrobnosti o konfiguraci serveru.

    ![nastavení serveru vCenter](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter2.png)
4. Pokud používáte účet bez oprávnění správce k přidání serveru nebo hostitele, ujistěte se, že má účet oprávnění následující:

   * vCenter účty musí mít Datacenter, úložiště, složky, hostitele, sítě, prostředků, úložiště zobrazení, virtuální počítač a vSphere distribuované přepínač oprávnění povoleno.
   * účty hostitele vSphere by měl mít Datacenter, úložiště, složky, hostitele, sítě, prostředků, virtuální počítač a vSphere distribuované přepínač oprávnění povoleno

## <a name="step-7-create-a-protection-group"></a>Krok 7: Vytvoření skupiny ochrany
1. Otevřete **chráněné položky** > **skupiny ochrany** > **vytvořit skupinu ochrany**.

    ![Vytvořit skupinu ochrany](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg1.png)
2. Na **zadejte nastavení skupiny ochrany** stránky zadejte název pro skupinu a vyberte konfigurační server, na kterém chcete vytvořit skupinu.

    ![Nastavení skupiny ochrany](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg2.png)
3. Na **zadejte nastavení replikace** stránku konfigurace nastavení replikace, která se použije pro všechny počítače ve skupině.

    ![Skupiny ochrany replikace](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg3.png)
4. Nastavení:

   * **Konzistence pro víc Virtuálních**: Pokud tuto možnost zapnout se vytváří body obnovení sdílené konzistentní s aplikací mezi počítači ve skupině ochrany. Toto nastavení je nejdůležitější, když všechny počítače ve skupině ochrany běží stejné zatížení. Všechny počítače se obnoví do stejného datového bodu. K dispozici je pouze pro servery systému Windows.
   * **Prahová hodnota RPO**: výstrahy se vygeneruje, pokud replikace průběžné ochrany dat RPO překročí nakonfigurovanou prahovou hodnotu RPO.
   * **Uchování bodu obnovení**: Určuje okno uchování. Chráněné počítače můžete obnovit do libovolného bodu v rámci tohoto okna.
   * **Frekvence snímkování konzistentní aplikace vzhledem**: Určuje, jak často budou vytvořeny body obnovení obsahující snímky konzistentní s aplikacemi.

Skupiny ochrany můžete sledovat, jak jste vytvořili na **chráněné položky** stránky.

## <a name="step-8-set-up-machines-you-want-to-protect"></a>Krok 8: Nastavení počítače, které chcete chránit
Budete potřebovat pro instalaci služby Mobility na virtuální počítače a fyzické servery, které chcete chránit. Můžete provést dvěma způsoby:

* Automaticky push a nainstalujte službu na každém počítači z procesového serveru.
* Ručně nainstalujte službu.

### <a name="install-the-mobility-service-automatically"></a>Automaticky nainstalovat službu Mobility
Při přidání počítačů do skupiny ochrany služba Mobility je automaticky instaluje a nainstalován na každém počítači procesní server.

**Automaticky nabízené instalaci služby mobility na serverech Windows:**

1. Nainstalujte nejnovější aktualizace u procesového serveru, jak je popsáno v [krok 5: nainstalujte nejnovější aktualizace](#step-5-install-latest-updates)a ujistěte se, že procesový server je k dispozici.
2. Zkontrolujte, je k dispozici síťové připojení mezi zdrojovým počítačem a procesový server a že zdrojový počítač je dostupný z procesového serveru.  
3. Konfigurace brány Windows firewall povolit **sdílení souborů a tiskáren** a **Windows Management Instrumentation**. V části nastavení brány Windows Firewall vyberte možnost "Povolit aplikace nebo funkci průchod bránou Firewall" a vyberte aplikace, jak je znázorněno na obrázku níže. Pro počítače, které patří k doméně můžete nakonfigurovat zásady brány firewall pomocí objektu zásad skupiny.

    ![Nastavení brány firewall](./media/site-recovery-vmware-to-azure-classic-legacy/push-firewall.png)
4. Účet používaný k provedení instalace push musí být ve skupině Administrators na počítači, který chcete chránit. Tyto přihlašovací údaje se používají jenom pro nabízenou instalaci služby Mobility a poskytnete jim při přidat počítač do skupiny ochrany.
5. Pokud uvedený účet není účet domény budete muset zakázat řízení vzdáleného přístupu uživatele v místním počítači. Stačí přidat položku registru LocalAccountTokenFilterPolicy DWORD s hodnotou 1 v části HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System. Chcete-li přidat položku registru z rozhraní příkazového řádku otevřené cmd nebo prostředí powershell a zadejte  **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`** .

**Automaticky nabízenou instalaci služby mobility na servery se systémem Linux:**

1. Nainstalujte nejnovější aktualizace u procesového serveru, jak je popsáno v [krok 5: nainstalujte nejnovější aktualizace](#step-5-install-latest-updates)a ujistěte se, že procesový server je k dispozici.
2. Zkontrolujte, je k dispozici síťové připojení mezi zdrojovým počítačem a procesový server a že zdrojový počítač je dostupný z procesového serveru.  
3. Zajistěte, aby byl účet uživatele root na zdrojovém serveru Linux.
4. Ujistěte se, že soubor/etc/hosts na zdrojovém serveru Linux obsahuje položky, které mapování názvu místního hostitele na IP adresy přidružené všechny síťové adaptéry.
5. Nainstalujte nejnovější openssh, openssh-server, balíčky openssl na počítači, na kterém chcete chránit.
6. Ujistěte se, že je povolený protokol SSH, a že běží na portu 22.
7. V souboru sshd_config povolte následujícím způsobem podsystém SFTP a ověřování heslem:

   * (a) Přihlaste se jako kořenový adresář.
   * b) v souboru /etc/ssh/sshd_config souboru najít řádek, který začíná **PasswordAuthentication**.
   * c) zrušte komentář u řádku a změňte hodnotu z "Ne" a "Ano".

       ![Linux mobility](./media/site-recovery-vmware-to-azure-classic-legacy/linux-push.png)
   * d) najít řádek, který začíná subsystém a zrušte komentář u řádku.

       ![Linux nabízené mobility](./media/site-recovery-vmware-to-azure-classic-legacy/linux-push2.png)    
8. Zkontrolujte, zda že variant zdrojový počítač Linux je podporována.

### <a name="install-the-mobility-service-manually"></a>Nainstalujte službu Mobility ručně
Jsou balíčky softwaru použít k instalaci služby Mobility na procesovém serveru v C:\pushinstallsvc\repository. Přihlaste se procesový server a zkopírujte příslušné instalační balíček do zdrojového počítače podle následující tabulky:-

| Zdrojový operační systém | Balíček služby mobility na procesovém serveru |
| --- | --- |
| Windows Server (pouze 64bitová verze) |`C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe` |
| CentOS 6.4, 6.5, 6.6 (pouze 64bitové verze) |`C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_RHEL6-64_GA_28Jul2015_release.tar.gz` |
| SUSE Linux Enterprise Server 11 SP3 (pouze 64bitová verze) |`C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_SLES11-SP3-64_GA_28Jul2015_release.tar.gz` |
| Oracle Enterprise Linux 6.4, 6.5 (pouze 64bitová verze) |`C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_OL6-64_GA_28Jul2015_release.tar.gz` |

**Ruční instalace služby Mobility na Windows server**, postupujte takto:

1. Kopírování **Microsoft ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe** balíčku z cesty k adresáři serveru procesu uvedené v tabulce výše ke zdrojovému počítači.
2. Nainstalujte službu Mobility spuštěním spustitelného souboru na zdrojovém počítači.
3. Postupujte podle pokynů instalačního programu.
4. Vyberte **služba Mobility** role a klikněte na **Další**.

    ![Instalaci služby mobility](./media/site-recovery-vmware-to-azure-classic-legacy/ms-install.png)
5. Nechte instalační adresář jako výchozí cestu instalace a klikněte na **nainstalovat**.
6. V **konfigurace agenta hostitele** Určete IP adresu a port HTTPS konfigurace serveru.

   * Pokud se připojujete přes internet zadejte jako port veřejná virtuální IP adresy a veřejný koncový bod HTTPS.
   * Pokud se připojujete prostřednictvím sítě VPN zadejte interní IP adresu a 443 port. Nechte **použití HTTPS** zaškrtnutí.

     ![Instalaci služby mobility](./media/site-recovery-vmware-to-azure-classic-legacy/ms-install2.png)
7. Zadejte heslo konfiguračního serveru a klikněte na tlačítko **OK** při registraci služba Mobility s konfigurační server.

**Spuštění z příkazového řádku:**

1. Kopírování z CX přístupové heslo do souboru "C:\connection.passphrase" na serveru a spusťte tento příkaz. V našem příkladu CX i 104.40.75.37 a HTTPS port je 62519:

    `C:\Microsoft-ASR_UA_8.2.0.0_Windows_PREVIEW_20Mar2015_Release.exe" -ip 104.40.75.37 -port 62519 -mode UA /LOG="C:\stdout.txt" /DIR="C:\Program Files (x86)\Microsoft Azure Site Recovery" /VERYSILENT  /SUPPRESSMSGBOXES /norestart  -usesysvolumes  /CommunicationMode https /PassphrasePath "C:\connection.passphrase"`

**Nainstalujte službu Mobility ručně na Linux server**:

1. Zkopírujte příslušné vkládání archiv založený na tabulce výše, z procesového serveru ke zdrojovému počítači.
2. Otevřete program prostředí a extrahujte komprimované vkládání archivu do místní cesty tak, že provádění`tar -xvzf Microsoft-ASR_UA_8.2.0.0*`
3. Vytvořte soubor passphrase.txt v místním adresáři, do které jste extrahovali obsah archivu vkládání zadáním  *`echo <passphrase> >passphrase.txt`*  z prostředí.
4. Nainstalujte službu Mobility zadáním  *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`* .
5. Zadejte IP adresu a port:

   * Pokud se připojujete k serveru configuration přes internet zadejte konfigurace serveru virtuální veřejnou IP adresu a veřejný koncový bod HTTPS v `<IP address>` a `<port>`.
   * Pokud se připojujete prostřednictvím připojení VPN zadejte interní IP adresu a 443.

**Ke spuštění z příkazového řádku**:

1. Kopírování z CX přístupové heslo do souboru "passphrase.txt" na serveru a spusťte tento příkazy. V našem příkladu CX i 104.40.75.37 a HTTPS port je 62519:

Instalace na provozním serveru:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 62519 -s y -c https -P passphrase.txt

K instalaci na cílovém serveru:

    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 62519 -s y -c https -P passphrase.txt

> [!NOTE]
> Po přidání počítače do skupiny ochrany, které jsou již spuštěny příslušnou verzi pak nabízenou instalaci služby Mobility je přeskočeno.
>
>

## <a name="step-9-enable-protection"></a>Krok 9: Povolení ochrany
K povolení ochrany přidáte virtuálních počítačů a fyzických serverů do skupiny ochrany. Než začnete, Všimněte si, že:

* Virtuální počítače jsou zjišťovány každých 15 minut a může trvat až 15 minut se objevily v Azure Site Recovery po zjišťování.
* Změny prostředí na virtuálním počítači (jako je třeba instalace nástroje VMware) také může trvat až 15 minut k aktualizaci v Site Recovery.
* Čas poslední zjištěné můžete zkontrolovat ve **poslední obraťte se na** pole pro hostitele systému vCenter server/ESXi na **konfigurační servery** stránky.
* Pokud jste již vytvořili skupinu ochrany a přidání systému vCenter Server nebo hostiteli ESXi poté, bude trvat 15 minut pro portálu Azure Site Recovery k aktualizaci a virtuálních počítačů uvedené **přidat počítače do skupiny ochrany** dialogové okno.
* Pokud chcete okamžitě pokračujte přidáním počítače do skupiny ochrany, aniž by čekal na naplánovaného zjišťování, zvýrazněte konfigurační server (nemáte klikněte na něj) a klikněte na tlačítko **aktualizovat** tlačítko.
* Když přidáte virtuální počítače nebo fyzického počítače do skupiny ochrany, procesový server automaticky nabízených oznámení a nainstaluje služba Mobility na zdrojovém serveru, pokud ještě není nainstalovaný it.
* Pro automatické nabízené mechanismus pro práci Ujistěte se, že jste nastavili chráněné počítače podle popisu v předchozím kroku.

Přidejte počítače následujícím způsobem:

1. Klikněte na tlačítko **chráněné položky** > **skupiny ochrany** > **počítače** > **přidat počítače** . Jako osvědčený postup doporučujeme, aby skupin ochrany by měl zrcadlení úlohy tak, aby přidat počítače, které běží konkrétní aplikaci do stejné skupiny.
2. V **vyberte virtuální počítače** Pokud chráníte fyzické servery, v **přidání fyzických počítačů** průvodce zadejte popisný název a IP adresu. Potom vyberte operační systém řady.

    ![Přidejte server V Center](./media/site-recovery-vmware-to-azure-classic-legacy/physical-protect.png)
3. V **vyberte virtuální počítače** Pokud chráníte virtuální počítače VMware, vyberte server vCenter, který spravuje virtuální počítače (nebo EXSi hostitele, na kterém používáte systém) a potom vyberte počítače.

    ![Přidejte server V Center](./media/site-recovery-vmware-to-azure-classic-legacy/select-vms.png)    
4. V **zadat cílové prostředky** vyberte hlavních cílových serverů a úložiště a použít pro replikaci a vyberte, zda nastavení se použije pro všechny úlohy. Vyberte [prémiový účet úložiště](../storage/common/storage-premium-storage.md) při konfiguraci ochrany pro úlohy, které vyžadují konzistentní vysoké vstupně-výstupní výkon a nízká latence pro hostování zatížení s intenzivním vstupně-výstupní operace. Pokud chcete použít účet úložiště Premium pro disky zatížení, budete muset použít hlavní cíl DS-series. Disky úložiště Premium nelze použít s hlavního cíle DS řady.

   > [!NOTE]
   > Nepodporujeme přesun účty úložiště vytvořené pomocí [nový portál Azure](../storage/common/storage-create-storage-account.md) mezi skupinami prostředků.
   >
   >

    ![vCenter server](./media/site-recovery-vmware-to-azure-classic-legacy/machine-resources.png)
5. V **zadejte účty** vyberte účet, kterou chcete použít pro instalaci služby Mobility na chráněné počítače. Přihlašovací údaje účtu, je potřeba pro automatické instalaci služby Mobility. Pokud nemůžete vybrat účtu Ujistěte se, že nastavíte jeden jak je popsáno v kroku 2. Všimněte si, že tento účet není přístupný v Azure. Pro systém Windows server účet musí mít oprávnění správce na zdrojovém serveru. Pro Linux musí být účet root.

    ![Přihlašovací údaje systému Linux](./media/site-recovery-vmware-to-azure-classic-legacy/mobility-account.png)
6. Kliknutím na značku zaškrtnutí dokončete přidání počítače do skupiny ochrany a spustit počáteční replikaci pro každý počítač. Můžete sledovat stav na **úlohy** stránky.

    ![Přidejte server V Center](./media/site-recovery-vmware-to-azure-classic-legacy/pg-jobs2.png)
7. Kromě toho můžete sledovat stav ochrany kliknutím **chráněné položky** > název skupiny ochrany > **virtuální počítače** . Po dokončení počáteční replikace a na počítače jsou synchronizaci dat se zobrazí **chráněné** stavu.

    ![Úlohy virtuálního počítače](./media/site-recovery-vmware-to-azure-classic-legacy/pg-jobs.png)

### <a name="set-protected-machine-properties"></a>Vlastnosti sady chráněné počítače
1. Jakmile počítač má **chráněné** stav, můžete konfigurovat její vlastnosti převzetí služeb při selhání. V oblasti ochrany skupiny podrobnosti, vyberte počítač a otevřete **konfigurace** kartě.
2. Můžete upravit název, který bude mít k počítači v Azure po převzetí služeb při selhání a velikost virtuálního počítače Azure. Můžete také vybrat síť Azure, ke kterému se počítač připojí po převzetí služeb při selhání.

   > [!NOTE]
   > [Migrace sítí](../resource-group-move-resources.md) napříč skupinami prostředků v rámci stejného předplatného nebo napříč předplatnými se pro sítě použité k nasazení Site Recovery nepodporuje.
   >
   >

    ![Nastavit vlastnosti virtuálního počítače](./media/site-recovery-vmware-to-azure-classic-legacy/vm-props.png)

Poznámky:

* Název počítače Azure musí být v souladu s požadavky na Azure.
* Ve výchozím nastavení nejsou replikovaných virtuálních počítačů v Azure připojené sítě Azure. Pokud chcete replikované virtuální počítače komunikovat nezapomeňte nastavit stejnou síť Azure pro ně.
* Pokud změníte velikost svazku na virtuální počítač VMware nebo fyzický server přejde do kritického stavu. Pokud potřebujete změnit velikost, postupujte takto:

  * (a) velikost nastavení změňte.
  * b) v **virtuální počítače** , vyberte virtuální počítač a klikněte na **odebrat**.
  * c) v **odebrat virtuální počítač** vyberte možnost **zakažte ochranu (slouží pro a změnu velikosti svazku)**. Tato možnost zakáže ochranu, ale zachová body obnovení v Azure.

      ![Nastavit vlastnosti virtuálního počítače](./media/site-recovery-vmware-to-azure-classic-legacy/remove-vm.png)
  * d) znovu povolte ochranu pro virtuální počítač. Když znovu povolit ochranu dat změněnou velikostí svazku se nepřenesou do Azure.

## <a name="step-10-run-a-failover"></a>Krok 10: Spuštění převzetí služeb při selhání
Aktuálně lze spustit jen neplánované převzetí služeb při selhání pro chráněné virtuální počítače VMware a fyzické servery. Je třeba počítat s následujícím:

* Před spuštěním převzetí služeb při selhání, zajistěte, aby byly cílové servery konfigurace a hlavní server běží a je v pořádku. V opačném případě převzetí služeb při selhání se nezdaří.
* Zdrojového počítače nejsou vypnuté jako součást neplánované převzetí služeb při selhání. Provádění neplánované převzetí služeb při selhání se zastaví replikaci dat u chráněných serverů. Budete muset odstraňte počítače ze skupiny ochrany a znovu přidejte mohli začít znovu ochranu počítačů po dokončení neplánované převzetí služeb při selhání.
* Pokud chcete převzetí služeb při selhání bez ztráty dat, ujistěte se, že virtuálních počítačích primární lokality jsou vypnuty před spuštěním převzetí služeb při selhání.

1. Na **plány obnovení** stránky a přidejte plán obnovení. Zadejte podrobnosti pro plán a vyberte **Azure** jako cíl.

    ![Konfigurace plánu obnovení](./media/site-recovery-vmware-to-azure-classic-legacy/rplan1.png)
2. V **vybrat virtuální počítač** vyberte skupinu ochrany a pak vyberte počítače do skupiny přidat do plánu obnovení. [Další informace](site-recovery-create-recovery-plans.md) o plány obnovení.

    ![Přidat virtuální počítače](./media/site-recovery-vmware-to-azure-classic-legacy/rplan2.png)
3. V případě potřeby můžete upravit plán vytváření skupin a pořadí pořadí na počítačích, které v obnovení plánu jsou převzetí služeb při selhání. Můžete také přidat vyzve k zadání ručně prováděné akce a skripty. Skripty při obnovování do Azure můžete přidat pomocí [sad Azure Automation Runbook](site-recovery-runbook-automation.md).
4. V **plány obnovení** vyberte plán a klikněte na tlačítko **neplánované převzetí služeb při selhání**.
5. V **potvrzení převzetí služeb při selhání** ověřte směr převzetí služeb při selhání (do Azure) a vyberte bod obnovení pro převzetí služeb při selhání.
6. Počkejte na dokončení a potom ověřit, zda převzetí služeb při selhání funguje podle očekávání úlohy převzetí služeb při selhání a že replikované virtuální počítače se úspěšně spustit v Azure.

## <a name="step-11-fail-back-failed-over-machines-from-azure"></a>Krok 11: Selhání zpět převzetí služeb při selhání počítačů z Azure
[Další informace](site-recovery-failback-azure-to-vmware-classic-legacy.md) o tom, jak převést váš neúspěšné přes počítače, které běží v Azure zpět do místního prostředí.

## <a name="manage-your-process-servers"></a>Spravovat vaše servery procesu
Procesový server odešle data replikace hlavní cílový server v Azure a zjišťuje nové virtuální počítače VMware vCenter serveru přidat. V následujících případech můžete chtít změnit procesový server v nasazení:

* Pokud aktuální procesový server ocitne mimo provoz
* Pokud plánovaného bodu obnovení (RPO) se zvyšuje úroveň nepřijatelné pro vaši organizaci.

V případě potřeby můžete jiný procesový server přesunout replikaci některé nebo všechny virtuální počítače VMware na místě a fyzických serverů. Například:

* **Selhání**– Pokud procesový server selže nebo není k dispozici můžete přesunout replikaci chráněného počítače na jiný procesový server. Metadata zdrojového počítače a počítač repliky bude přesunut do nový procesový server a je-li synchronizovat data. Nový procesový server se automaticky připojí k serveru vCenter a provést automatické zjišťování. Můžete sledovat stav procesu serverů na řídicím panelu Site Recovery.
* **Chcete-li upravit RPO Vyrovnávání zatížení**– pro vylepšené můžete Vyrovnávání zatížení můžete vybrat jiný procesový server na portálu Site Recovery a přesuňte replikaci počítačů na jeden nebo více k němu pro vyrovnávání zatížení ruční. V takovém případě metadata z vybraných počítačů zdroje a repliky je přesunuta do nový procesový server. Původní procesového serveru zůstává připojený k serveru vCenter.

### <a name="monitor-the-process-server"></a>Monitorování procesového serveru
Pokud procesový server je v kritickém stavu stavu upozornění, zobrazí se na řídicím panelu Obnovení lokality. Kliknutím na stav, otevřete kartu události a poté přejít k podrobnostem a konkrétní úlohy na kartě úlohy.

### <a name="modify-the-process-server-used-for-replication"></a>Změnit procesový server používají pro replikaci
1. Otevřete **servery** > **konfigurační servery** > konfigurační server > **podrobnosti o serveru**.
2. Klikněte na tlačítko **proces servery** > **změnit procesový Server** vedle serveru, kterou chcete upravit.

    ![Změnit procesový Server 1](./media/site-recovery-vmware-to-azure-classic-legacy/change-ps1.png)
3. V **změnit procesový Server** > **cílový procesový Server** vyberte nový server, kterou chcete použít a potom vyberte virtuální počítače, které chcete replikovat na nový server. Klikněte na ikonu informace vedle názvu serveru podrobnosti volného místa a využité paměti. Průměrná prostor, který bude vyžadovat, aby replikovat každý vybraný virtuální počítač na nový server procesu se zobrazí vám pomůže provádět rozhodnutí týkající se načtení.

    ![Změnit procesový Server 2](./media/site-recovery-vmware-to-azure-classic-legacy/change-ps2.png)
4. Kliknutím na značku zaškrtnutí zahájíte replikace pro nový procesový server. Všimněte si, že když odeberete všechny virtuální počítače z procesového serveru, který byl kritické ho už zobrazí kritická upozornění na řídicím panelu.

## <a name="third-party-software-notices-and-information"></a>Oznámení software třetích stran a informace
Nechcete převede nebo lokalizaci

Software a firmware spuštěné v produktu společnosti Microsoft nebo služba je založena na nebo zahrnuje materiálu z projekty uvedené níže (dále souhrnně nazývané "kód třetí strany").  Společnost Microsoft se není původní autor kód třetích stran.  Původní o autorských právech a licenci, pod kterým společnost Microsoft takový kód třetích stran, jsou nastavené níže uvedenými.

Informace v části A je týkající se součásti kód třetích stran z projekty uvedené níže. Tyto licence a informace jsou uvedené pro pouze pro informační účely.  Tento kód třetích stran, je právě relicensed vám společnost Microsoft pod licenční podmínky softwaru společnosti Microsoft pro produkty společnosti Microsoft nebo služby.  

Informace v části B je týkající se součástí kód třetích stran, které jsou určeny jako dostupné pro vás společností Microsoft v rámci původní licenční podmínky.

Dokončení soubor najdete na [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=529428). Společnost Microsoft si vyhrazuje všechna práva, která nejsou výslovně udělena, zda implicitně, jako překážku uplatnění nároku či jiným způsobem.
