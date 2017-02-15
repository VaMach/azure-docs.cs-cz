---
title: Co je Azure Backup? | Dokumentace Microsoftu
description: "Pomocí Azure Backup a Recovery Services můžete zálohovat a obnovovat data a aplikace z Windows Serverů, počítačů s Windows, serverů System Center DPM a virtuálních počítačů Azure."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "zálohování a obnovení; recovery services; řešení zálohování"
ms.assetid: 0d2a7f08-8ade-443a-93af-440cbf7c36c4
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 1/4/2017
ms.author: jimpark; trinadhk
translationtype: Human Translation
ms.sourcegitcommit: 0eb7b5c283c95503d076da486ba08df833f1acbd
ms.openlocfilehash: 5235a09822dc14040ca6d4353d00e938fefd0e43


---
# <a name="what-is-azure-backup"></a>Co je Azure Backup?
Azure Backup je služba Azure, kterou můžete využívat k zálohování (ochraně) a obnovování vašich dat v Microsoft Cloudu. Azure Backup nahrazuje současná řešení místního nebo odlehlého zálohování spolehlivým, bezpečným a cenově konkurenceschopným cloudovým řešením. Azure Backup nabízí několik komponent, které můžete stáhnout a nasadit na vhodném počítači, na serveru, nebo v cloudu. Nasazená komponenta nebo agent závisí na tom, co chcete chránit. Všechny komponenty Azure Backup (bez ohledu na to, jestli chráníte data v místním nebo cloudovém úložišti) je možné použít pro zálohování dat do trezoru služby Backup v Azure. Informace o tom, kterou komponentu použít pro ochranu konkrétních data, aplikací nebo úloh, najdete v [tabulce komponent Azure Backup](backup-introduction-to-azure-backup.md#which-azure-backup-components-should-i-use) (dále v tomto článku).

[Podívejte se na video s přehledem Azure Backup](https://azure.microsoft.com/documentation/videos/what-is-azure-backup/)

## <a name="why-use-azure-backup"></a>Proč používat Azure Backup?
Tradiční řešení zálohování se vyvinula tak, že cloud považují za koncový bod nebo cíl statického úložiště podobně jako disky nebo pásky. Tento přístup je sice jednoduchý, ale je omezený a neumožňuje úplné využívání základní cloudové platformy, takže se jedná o nákladné a neefektivní řešení. Jiná řešení jsou nákladná, protože nakonec platíte za nesprávný typ úložiště nebo za úložiště, které nepotřebujete. Jiná řešení jsou často neefektivní, protože vám nenabízejí typ nebo objem úložiště, který potřebujete, nebo úlohy správy vyžadují příliš mnoho času. Azure Backup naopak nabízí tyto klíčové výhody:

**Automatická správa úložiště** – Hybridní prostředí často vyžadují heterogenní úložiště – některá místní a některá cloudová. Azure Backup neznamená žádné náklady na používání místních zařízení úložiště. Azure Backup automaticky přiděluje a spravuje úložiště záloh a používá model založený na průběžných platbách. Průběžné platby znamenají, že platíte jen za úložný prostor, který spotřebujete. Další informace najdete v [článku o cenách Azure](https://azure.microsoft.com/pricing/details/backup).

**Neomezené škálování** – Azure Backup používá základní výkon a neomezený rozsah cloudu Azure k zajištění vysoké dostupnosti, a to bez jakékoli režie s údržbou nebo monitorováním. Můžete nastavit výstrahy pro poskytování informací o událostech, ale nemusíte se starat o vysokou dostupnost svých dat v cloudu.

**Více možností úložiště** – Jedním z aspektů vysoké dostupnosti je replikace úložiště. Azure Backup nabízí dva typy replikace: [místně redundantní úložiště](../storage/storage-redundancy.md#locally-redundant-storage) a [geograficky redundantní úložiště](../storage/storage-redundancy.md#geo-redundant-storage). Možnost úložiště pro zálohování zvolte podle potřeby:

* Místně redundantní úložiště (LRS) replikuje vaše data třikrát (vytváří tři kopie dat) ve spárovaném datovém centru ve stejné oblasti. Místně redundantní úložiště nabízí cenově úsporný způsob ochrany dat před selháním místního hardwaru.

* Geograficky redundantní úložiště (GRS) replikuje vaše data do sekundární oblasti (vzdálené stovky kilometrů od primárního umístění zdrojových dat). Geograficky redundantní úložiště je nákladnější než místně redundantní úložiště, ale nabízí vyšší úroveň odolnosti dat i v případě regionálního výpadku.

**Neomezené přenosy dat** – Azure Backup neomezuje množství příchozích ani odchozích dat, která přenesete. V rámci Azure Backup se přenesená data neúčtují. Pokud ale použijete službu importu/exportu v Azure k importu velkého množství dat, účtují se náklady související s příchozími daty. Další informace o těchto nákladech najdete v části [Pracovní postup zálohování offline v Azure Backup](backup-azure-backup-import-export.md). Odchozí data jsou data přenesená z trezoru služby Backup během operace obnovování.

**Šifrování dat** – Šifrování dat umožňuje bezpečný přenos a ukládání vašich dat ve veřejném cloudu. Šifrovací heslo máte uložené v místním úložišti a do Azure se nikdy nepřenáší ani se tam neukládá. Pokud je nutné obnovit některá data, máte šifrovací heslo (klíč) k dispozici jen vy.

**Zálohování konzistentní s aplikací** – Bez ohledu na to, jestli zálohujete souborový server, virtuální počítač nebo databázi SQL, si potřebujete být jistí, že bod obnovení obsahuje veškerá požadovaná data pro obnovení záložní kopie. Azure Backup poskytuje zálohy konzistentní s aplikací, a tím je zajištěno, že pro obnovení dat nejsou potřebné další opravy. Obnovování dat konzistentních s aplikací zkracuje čas obnovení, což vám umožní rychle se vrátit do funkčního stavu.

**Dlouhodobé uchovávání** – Do Azure můžete zálohovat data na 99 let. Namísto převádění záložní kopie z disku na pásku a následné přepravy pásky na místo mimo pracoviště za účelem dlouhodobého uložení můžete využívat krátkodobé i dlouhodobé uchovávání v Azure.

## <a name="which-azure-backup-components-should-i-use"></a>Které komponenty Azure Backup mám použít?
Pokud si nejste jisti, která komponenta Azure Backup splňuje vaše potřeby, najdete v následující tabulce informace o tom, co můžete chránit pomocí jednotlivých komponent. Na portálu Azure Portal je k dispozici průvodce, který je integrovaný v portálu a provede vás výběrem komponenty ke stažení a nasazení. Průvodce, který je součástí vytváření trezoru služby Recovery Services, vás provede kroky výběru cíle zálohování a výběru dat nebo aplikace, kterou chcete chránit.

| Komponenta | Výhody | Omezení | Co se chrání? | Kde jsou zálohy uložené? |
| --- | --- | --- | --- | --- |
| Agent Azure Backup (MARS) |<li>Zálohování souborů a složek ve fyzickém nebo virtuálním operačním systému Windows (virtuální počítač může být místní nebo v Azure)<li>Není vyžadován samostatný záložní server. |<li>Zálohování 3x denně <li>Nerozpoznávají se aplikace; obnovování pouze na úrovni souboru, složky nebo svazku. <li>  Bez podpory Linux |<li>Soubory <li>Složky |Trezor služby Azure Backup |
| System Center DPM |<li>Snímky schopné rozeznávat aplikace (VSS)<li>Úplná flexibilita času zálohování<li>Členitost obnovení (všechny)<li>Může používat trezor služby Azure Backup<li>Podpora Linuxu ve virtuálních počítačích Hyper-V a VMware <li>Zálohování a obnovení virtuálních počítačů VMware pomocí DPM 2012 R2 |Nejde zálohovat úlohu Oracle.|<li>Soubory <li>Složky<li> Svazky <li>Virtuální počítače<li> Aplikace<li> Úlohy |<li>Trezor služby Azure Backup,<li> Místně připojený disk,<li>  Páska (pouze místní) |
| Server Azure Backup |<li>Snímky schopné rozeznávat aplikace (VSS)<li>Úplná flexibilita času zálohování<li>Členitost obnovení (všechny)<li>Může používat trezor služby Azure Backup<li>Podpora Linuxu ve virtuálních počítačích Hyper-V a VMware<li>Zálohování a obnovení virtuálních počítačů VMware <li>Nevyžaduje licenci produktu System Center |<li>Nejde zálohovat úlohu Oracle.<li>Vždy vyžaduje živé předplatné Azure<li>Nepodporuje zálohování na pásku |<li>Soubory <li>Složky<li> Svazky <li>Virtuální počítače<li> Aplikace<li> Úlohy |<li>Trezor služby Azure Backup,<li> Místně připojený disk |
| Zálohování virtuálních počítačů Azure IaaS |<li>Nativní zálohy pro Windows a Linux<li>Bez nutnosti instalace konkrétního agenta<li>Zálohování na úrovni prostředků infrastruktury bez potřeby infrastruktury zálohování |<li>Zálohování virtuálních počítačů jednou denně <li>Obnovení virtuálních počítačů pouze na úrovni disku<li>Nemožnost místního zálohování |<li>Virtuální počítače <li>Všechny disky (pomocí PowerShellu) |<p>Trezor služby Azure Backup</p> |

## <a name="what-are-the-deployment-scenarios-for-each-component"></a>Jaké jsou scénáře nasazení pro jednotlivé komponenty?
| Komponenta | Lze nasadit v Azure? | Lze nasadit místně? | Podpora cílového úložiště |
| --- | --- | --- | --- |
| Agent Azure Backup (MARS) |<p>**Ano**</p> <p>Agenta Azure Backup lze nasadit na jakýkoli virtuální počítač s Windows Serverem, který běží v Azure.</p> |<p>**Ano**</p> <p>Agenta služby Backup lze nasadit na jakýkoli virtuální nebo fyzický počítač s Windows Serverem.</p> |<p>Trezor služby Azure Backup</p> |
| System Center DPM |<p>**Ano**</p><p>Další informace o tom, [jak chránit úlohy v Azure pomocí aplikace System Center DPM](backup-azure-dpm-introduction.md).</p> |<p>**Ano**</p> <p>Další informace o tom, [jak chránit úlohy a virtuální počítače ve vašem datovém centru](https://technet.microsoft.com/en-us/system-center-docs/dpm/data-protection-manager).</p> |<p>Místně připojený disk,</p> <p>Trezor služby Azure Backup,</p> <p>páska (pouze místní)</p> |
| Server Azure Backup |<p>**Ano**</p><p>Další informace o tom, [jak chránit úlohy v Azure pomocí Serveru Azure Backup](backup-azure-microsoft-azure-backup.md).</p> |<p>**Ano**</p> <p>Další informace o tom, [jak chránit úlohy v Azure pomocí Serveru Azure Backup](backup-azure-microsoft-azure-backup.md).</p> |<p>Místně připojený disk,</p> <p>Trezor služby Azure Backup</p> |
| Zálohování virtuálních počítačů Azure IaaS |<p>**Ano**</p><p>Součást prostředků infrastruktury Azure</p><p>Specializované pro [zálohování virtuálních počítačů Azure IaaS (infrastruktura jako služba)](backup-azure-vms-introduction.md).</p> |<p>**Ne**</p> <p>Pro zálohování virtuálních počítačů ve svém datovém centru použijte aplikaci System Center DPM.</p> |<p>Trezor služby Azure Backup</p> |

## <a name="which-applications-and-workloads-can-be-backed-up"></a>Které aplikace a úlohy lze zálohovat?
Následující tabulka obsahuje matici dat a úloh, které se dají chránit s použitím Azure Backup. Sloupec řešení Azure Backup obsahuje odkazy na dokumentaci k nasazení pro příslušné řešení. Každou komponentu Azure Backup je možné nasadit v prostředí Classic (nasazení v Service Manageru) nebo v prostředí modelu nasazení Resource Manageru.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

| Data nebo úloha | Zdrojové prostředí | Řešení Azure Backup |
| --- | --- | --- |
| Soubory a složky |Windows Server |<p>[Agent Azure Backup](backup-configure-vault.md),</p> <p>[System Center DPM](backup-azure-dpm-introduction.md) (+ agent Azure Backup),</p> <p>[Server Azure Backup](backup-azure-microsoft-azure-backup.md) (zahrnuje agenta Azure Backup)</p> |
| Soubory a složky |Počítač s Windows |<p>[Agent Azure Backup](backup-configure-vault.md),</p> <p>[System Center DPM](backup-azure-dpm-introduction.md) (+ agent Azure Backup),</p> <p>[Server Azure Backup](backup-azure-microsoft-azure-backup.md) (zahrnuje agenta Azure Backup)</p> |
| Virtuální počítač s technologií Hyper-V (Windows) |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ agent Azure Backup),</p> <p>[Server Azure Backup](backup-azure-microsoft-azure-backup.md) (zahrnuje agenta Azure Backup)</p> |
| Virtuální počítač s technologií Hyper-V (Linux) |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ agent Azure Backup),</p> <p>[Server Azure Backup](backup-azure-microsoft-azure-backup.md) (zahrnuje agenta Azure Backup)</p> |
| Microsoft SQL Server |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ agent Azure Backup),</p> <p>[Server Azure Backup](backup-azure-microsoft-azure-backup.md) (zahrnuje agenta Azure Backup)</p> |
| Microsoft SharePoint |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ agent Azure Backup),</p> <p>[Server Azure Backup](backup-azure-microsoft-azure-backup.md) (zahrnuje agenta Azure Backup)</p> |
| Microsoft Exchange |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ agent Azure Backup),</p> <p>[Server Azure Backup](backup-azure-microsoft-azure-backup.md) (zahrnuje agenta Azure Backup)</p> |
| Virtuální počítače Azure IaaS (Windows) |spuštěno v Azure |[Azure Backup (rozšíření virtuálního počítače)](backup-azure-vms-introduction.md) |
| Virtuální počítače Azure IaaS (Linux) |spuštěno v Azure |[Azure Backup (rozšíření virtuálního počítače)](backup-azure-vms-introduction.md) |

## <a name="linux-support"></a>Podpora Linuxu
Následující tabulka uvádí komponenty Azure Backup s podporou pro Linux.  

| Komponenta | Podpora Linuxu (schváleného Azure) |
| --- | --- |
| Agent Azure Backup (MARS) |Ne (pouze agent založený na Windows) |
| System Center DPM |Záloha s konzistentními soubory jen v Hyper-V<br/> (není k dispozici pro virtuální počítač Azure) |
| Server Azure Backup |Záloha s konzistentními soubory jen v Hyper-V<br/> (není k dispozici pro virtuální počítač Azure) |
| Zálohování virtuálních počítačů Azure IaaS |Ano |

## <a name="using-premium-storage-vms-with-azure-backup"></a>Použití virtuálních počítačů služby Storage úrovně Premium s Azure Backup
Azure Backup chrání virtuální počítače služby Storage úrovně Premium. Azure Premium Storage představuje úložiště využívající disky SSD (solid-state drive) určené pro podporu úloh náročných na množství vstupně-výstupních operací. Služba Storage úrovně Premium je zajímavá pro úlohy virtuálních počítačů. Další informace o službě Storage úrovně Premium najdete v tématu [Premium Storage: vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](../storage/storage-premium-storage.md).

### <a name="back-up-premium-storage-vms"></a>Zálohování virtuálních počítačů služby Storage úrovně Premium
Během zálohování virtuálních počítačů služby Storage úrovně Premium vytvoří služba Backup v účtu Storage úrovně Premium dočasné pracovní umístění. Pracovní umístění s názvem „AzureBackup-“ odpovídá celkové velikosti dat na prémiových discích připojených k virtuálnímu počítači. Zkontrolujte, jestli v účtu úložiště je dostatek volného místa pro dočasné pracovní umístění. Další informace najdete v článku věnovaném [omezení Storage úrovně Premium](../storage/storage-premium-storage.md#premium-storage-scalability-and-performance-targets).

> [!NOTE]
> Pracovní umístění neměňte, ani neupravujte.
>
>

Po dokončení úlohy zálohování je pracovní umístění odstraněno. Cena úložiště použitého pro pracovní umístění je konzistentní s [Cenami Storage úrovně Premium](../storage/storage-premium-storage.md#pricing-and-billing).

### <a name="restore-premium-storage-vms"></a>Obnovení virtuálních počítačů služby Storage úrovně Premium
Virtuální počítače služby Storage úrovně Premium je možné obnovit do úložiště Storage úrovně Premium nebo do normálního úložiště. Typickým procesem obnovení je obnovení bodu zotavení virtuálního počítače služby Storage úrovně Premium do služby Storage úrovně Premium. Nicméně obnovení bodu zotavení virtuálního počítače služby Storage úrovně Premium do standardního úložiště může být cenově výhodnější. Tento typ obnovení lze použít, pokud potřebujete podmnožinu souborů z virtuálního počítače.

## <a name="what-are-the-features-of-each-backup-component"></a>Jaké jsou funkce jednotlivých komponent služby Backup?
Následující části obsahují tabulky, které shrnují dostupnost nebo podporu různých funkcí v jednotlivých komponentách Azure Backup. Informace o další podpoře nebo podrobnosti najdete pod jednotlivými tabulkami.

### <a name="storage"></a>Úložiště
| Funkce | Agent Azure Backup | System Center DPM | Server Azure Backup | Zálohování virtuálních počítačů Azure IaaS |
| --- | --- | --- | --- | --- |
| Trezor služby Azure Backup |![Ano][green] |![Ano][green] |![Ano][green] |![Ano][green] |
| Diskové úložiště | |![Ano][green] |![Ano][green] | |
| Páskové úložiště | |![Ano][green] | | |
| Komprese <br/>(v trezoru služby Azure Backup) |![Ano][green] |![Ano][green] |![Ano][green] | |
| Přírůstkové zálohování |![Ano][green] |![Ano][green] |![Ano][green] |![Ano][green] |
| Odstranění duplicit disku | |![Částečně][yellow] |![Částečně][yellow] | |

![klíč tabulky](./media/backup-introduction-to-azure-backup/table-key.png)

Trezor záloh je upřednostňovaným cílem úložiště napříč všemi komponentami. Aplikace System Center DPM a Azure Backup Server také poskytují možnost kopie místního disku. Pouze System Center DPM však nabízí možnost zápisu dat na zařízení páskového úložiště.

#### <a name="compression"></a>Komprese
Díky komprimování záloh dochází ke zmenšení potřebného prostoru úložiště. Jedinou komponentou, která nepoužívá komprimaci, je rozšíření virtuálního počítače. Rozšíření virtuálního počítače kopíruje veškerá zálohovaná data z vašeho účtu úložiště do trezoru služby Backup ve stejné oblasti. Při přenosu dat se nepoužívá žádná komprese. Přenosem dat bez komprese se mírně zvýší využití úložiště. Ukládání dat bez komprese však umožňuje rychlejší obnovení v případě, že potřebujete tento bod obnovení.

#### <a name="incremental-backup"></a>Přírůstkové zálohování
Všechny komponenty podporují přírůstkové zálohování bez ohledu na typ cílového úložiště (disk, páska, trezor záloh). Přírůstkové zálohování díky přenášení pouze změn od poslední zálohy zajišťuje efektivitu zálohování z hlediska úložiště a času.

#### <a name="disk-deduplication"></a>Odstranění duplicit disku
Výhody odstranění duplicit můžete využívat při nasazení aplikace System Center DPM nebo Azure Backup Serveru [na virtuálním počítači Hyper-V](http://blogs.technet.com/b/dpm/archive/2015/01/06/deduplication-of-dpm-storage-reduce-dpm-storage-consumption.aspx). Windows Server odstraňuje duplicity disku (na úrovni hostitele) na virtuálních pevných discích (VHD), které jsou k virtuálnímu počítači připojené jako úložiště pro zálohu.

> [!NOTE]
> Odstranění duplicit není v Azure k dispozici pro žádné komponenty služby Backup. Jsou-li aplikace System Center DPM a Server Backup nasazené v Azure, nelze na discích úložiště, připojených k virtuálnímu počítači, provést odstranění duplicit.
>
>

### <a name="security"></a>Zabezpečení
| Funkce | Agent Azure Backup | System Center DPM | Server Azure Backup | Zálohování virtuálních počítačů Azure IaaS |
| --- | --- | --- | --- | --- |
| Zabezpečení sítě<br/> (do Azure) |![Ano][green] |![Ano][green] |![Ano][green] |![Částečně][yellow] |
| Zabezpečení dat<br/> (v Azure) |![Ano][green] |![Ano][green] |![Ano][green] |![Částečně][yellow] |

![klíč tabulky](./media/backup-introduction-to-azure-backup/table-key.png)

#### <a name="network-security"></a>Zabezpečení sítě
Veškerý provoz zálohování z vašeho serveru do trezoru služby Backup je šifrován pomocí standardu AES (Advanced Encryption Standard) 256. Data zálohy se odesílají prostřednictvím zabezpečeného spojení HTTPS. Zálohovaná data jsou i v trezoru služby Backup uložena v šifrovaném tvaru. Heslo k odemknutí těchto dat má k dispozici pouze příslušný zákazník Azure. Microsoft za žádných okolností nemůže zálohovaná data dešifrovat.

> [!WARNING]
> Po vytvoření trezoru služby Backup máte k šifrovacímu klíči přístup jen vy. Microsoft nikdy neuchovává kopii vašeho šifrovacího klíče a nemá ke klíči přístup. Pokud dojde ke ztrátě klíče, Microsoft nemůže zálohovaná data obnovit.
>
>

#### <a name="data-security"></a>Zabezpečení dat
Zálohování virtuálních počítačů Azure vyžaduje nastavení šifrování *v rámci* virtuálního počítače. Na virtuálních počítačích se systémem Windows použijte BitLocker a na virtuálních počítačích s Linuxem použijte **dm-crypt**. Azure Backup neprovádí automatické šifrování zálohovaných dat, která přicházejí touto cestou.

### <a name="network"></a>Síť
| Funkce | Agent Azure Backup | System Center DPM | Server Azure Backup | Zálohování virtuálních počítačů Azure IaaS |
| --- | --- | --- | --- | --- |
| Komprese sítě <br/>(na **záložní server**) | |![Ano][green] |![Ano][green] | |
| Komprese sítě <br/>(do **trezoru záloh**) |![Ano][green] |![Ano][green] |![Ano][green] | |
| Síťový protokol <br/>(na **záložní server**) | |TCP |TCP | |
| Síťový protokol <br/>(do **trezoru záloh**) |HTTPS |HTTPS |HTTPS |HTTPS |

![klíč tabulky](./media/backup-introduction-to-azure-backup/table-key-2.png)

Rozšíření virtuálního počítače (ve virtuálním počítači IaaS) čte data přímo z účtu úložiště Azure přes síť úložiště, takže není potřeba tento provoz komprimovat.

Pokud svá data zálohujete do aplikace System Center DPM nebo na Azure Backup Server, komprese dat, komprimujte data z primárního serveru na záložní server. Komprese dat před jejich zálohováním do DPM nebo na Azure Backup Server šetří šířku pásma.

#### <a name="network-throttling"></a>Omezování šířky pásma sítě
Agent Azure Backup nabízí možnost omezování šířky pásma sítě, která vám umožňuje kontrolu využití šířky pásma sítě během přenosu dat. Omezování může být užitečné, pokud potřebujete zálohovat data v pracovní době, ale nechcete, aby proces zálohování narušoval ostatní internetový provoz. Omezování pro přenos dat platí pro činnosti zálohování a obnovení.

### <a name="backup-and-retention"></a>Zálohování a uchovávání

Azure Backup má limit 9999 bodů obnovení (označovaných také jako záložní kopie nebo snímky) na jednu *chráněnou instanci*. Chráněná instance je počítač, server (fyzický nebo virtuální) nebo úloha nakonfigurovaná pro zálohování dat do Azure. Další informace najdete v části [Co je chráněná instance](backup-introduction-to-azure-backup.md#what-is-a-protected-instance). Instance je chráněná, jakmile se uloží záložní kopie dat. Ochranou je právě tato záložní kopie dat. Pokud dojde ke ztrátě nebo poškození zdrojových dat, je možné je obnovit ze záložní kopie. V následující tabulce jsou uvedené maximální frekvence zálohování pro jednotlivé komponenty. Konfigurace zásady zálohování určuje, jak rychle se spotřebovávají body obnovení. Pokud například vytváříte bod obnovení každý den, můžete zachovat body obnovení 27 let, teprve potom vám dojdou. Pokud vytváříte bod obnovení každý měsíc, můžete zachovat body obnovení 833 let, teprve potom vám dojdou. Služba Backup pro body obnovení nenastavuje omezení času vypršení platnosti.

|  | Agent Azure Backup | System Center DPM | Server Azure Backup | Zálohování virtuálních počítačů Azure IaaS |
| --- | --- | --- | --- | --- |
| Frekvence zálohování<br/> (do trezoru služby Backup) |Tři zálohy za den |Dvě zálohy za den |Dvě zálohy za den |Jedna záloha za den |
| Frekvence zálohování<br/> (na disk) |Neuvedeno |<li>Každých 15 minut pro SQL Server <li>Každou hodinu pro ostatní úlohy |<li>Každých 15 minut pro SQL Server <li>Každou hodinu pro ostatní úlohy</p> |Neuvedeno |
| Možnosti uchovávání |Denně, týdně, měsíčně, ročně |Denně, týdně, měsíčně, ročně |Denně, týdně, měsíčně, ročně |Denně, týdně, měsíčně, ročně |
| Maximální počet bodů obnovení na chráněnou instanci |9999|9999|9999|9999|
| Maximální doba uchovávání |Závisí na četnosti zálohování |Závisí na četnosti zálohování |Závisí na četnosti zálohování |Závisí na četnosti zálohování |
| Body obnovení na místním disku |Neuvedeno |<li>64 pro souborové servery,<li>448 pro aplikační servery |<li>64 pro souborové servery,<li>448 pro aplikační servery |Neuvedeno |
| Body obnovení na pásku |Neuvedeno |Unlimited |Neuvedeno |Neuvedeno |

## <a name="what-is-a-protected-instance"></a>Co je chráněná instance
Chráněná instance je obecný odkaz na počítač se systémem Windows, server (fyzický nebo virtuální) nebo databázi SQL, která byla nakonfigurovaná pro zálohování do Azure. Instance je chráněná v okamžiku, kdy pro příslušný počítač, server nebo databázi nakonfigurujete zásady zálohování a vytvoříte záložní kopii dat. Další kopie záložních dat pro tuto chráněnou instanci (používá se pro ně označení body obnovení) zvyšují objem využitého úložiště. Pro chráněnou instanci je možné vytvořit až 9999 bodů obnovení. Pokud bod obnovení odstraníte z úložiště, nebude se do celkového počtu 9999 bodů obnovení započítávat.
Mezi běžné příklady chráněných instancí patří virtuální počítače, aplikační servery, databáze a osobní počítače s operačním systémem Windows. Například:

* Virtuální počítač se spuštěnými prostředky infrastruktury hypervisoru Azure IaaS nebo Hyper-V. Hostovanými operačními systémy pro virtuální počítač může být Windows Server nebo Linux.
* Aplikační server: Aplikačním serverem může být fyzický nebo virtuální počítač se systémem Windows Server a úlohy s daty, která je potřeba zálohovat. Mezi běžné úlohy patří Microsoft SQL Server, server Microsoft Exchange, server Microsoft SharePoint, Microsoft Dynamics a role souborového serveru ve Windows Serveru. K zálohování těchto úloh potřebujete System Center Data Protection Manager (DPM) nebo Azure Backup Server.
* Osobní nebo přenosný počítač s operačním systémem Windows.


## <a name="what-is-the-vault-credential-file"></a>Co je soubor s přihlašovacími údaji trezoru?
Soubor s přihlašovacími údaji trezoru je certifikát vytvořený portálem pro každý trezor služby Backup. Portál poté odešle veřejný klíč do Access Control Service (ACS). Privátní klíč budete mít k dispozici v rámci stahování přihlašovacích údajů. Použijte ho k registraci počítačů, které chráníte. Privátní klíč vám umožňuje ověřovat servery nebo počítače pro odesílání zálohovaných dat do konkrétního trezoru služby Backup.

Přihlašovací údaje úložiště slouží jen k registraci serverů nebo počítačů. S přihlašovacími údaji trezoru je ale třeba nakládat opatrně. V případě jejich ztráty nebo jejich získání jinými uživateli mohou být použity k registraci jiných počítačů pro stejný trezor. Vzhledem k tomu, že jsou zálohovaná data šifrovaná pomocí hesla přístupného jen pro vás, nemůže dojít k ohrožení bezpečnosti stávajících zálohovaných dat. Platnost přihlašovacích údajů trezoru vyprší po 48 hodinách. I když přihlašovací údaje pro trezor služby Backup můžete stahovat, jak často chcete, k registraci je možné použít jen nejnovější přihlašovací údaje.

## <a name="how-does-azure-backup-differ-from-azure-site-recovery"></a>Čím se liší Azure Backup od Azure Site Recovery?
Služby Azure Backup a Azure Site Recovery spolu souvisí v tom smyslu, že obě služby zálohují data a můžou tato data obnovovat, liší se však v základní nabídce hodnoty.

Azure Backup chrání data v místním a cloudovém úložišti. Azure Site Recovery koordinuje replikaci, převzetí služeb při selhání a navrácení služeb po obnovení mezi virtuálním počítačem a fyzickým serverem. Obě služby jsou důležité, protože je třeba, aby řešení zotavení po havárii zachovalo vaše data zabezpečená a obnovitelná (služba Backup) *a* aby vaše úlohy byly dostupné (služba Site Recovery) i v případě výpadku.

Následující koncepty vám mohou pomoct při rozhodování ohledně zálohování a zotavení po havárii.

| Koncept | Podrobnosti | Zálohování | Zotavení po havárii (DR) |
| --- | --- | --- | --- |
| Cíl bodu obnovení (RPO) |Přijatelná míra ztráty dat v případě potřeby provedení obnovení. |Řešení pro zálohování jsou velmi variabilní ohledně přijatelného RPO. Zálohy virtuálních počítačů obvykle mají RPO jeden den, zatímco zálohy databází mají RPO nižší, až 15 minut. |Řešení zotavení po havárii mají nízké RPO. Kopie DR může být o několik sekund až minut pozadu. |
| Plánovaná doba obnovení (RTO) |Množství času potřebného k dokončení obnovení nebo zotavení. |Z důvodu vyššího RPO je množství dat, které musí řešení zálohování zpracovat, typicky mnohem vyšší, což vede k delším RTO. Například obnovení dat z pásků může trvat i dny v závislosti na tom, jak dlouho trvá přenos pásku z odlehlého umístění. |Řešení zotavení po havárii mají nižší RTO, protože k jejich synchronizaci se zdrojem dochází častěji. Je tedy třeba zpracovat méně změn. |
| Uchovávání |Jak dlouho mají být data uložená |Pro scénáře, které vyžadují provozní obnovení (poškození dat, neúmyslné odstranění souborů, selhání operačního systému), se zálohovaná data obvykle uchovávají po dobu 30 dnů nebo méně.<br>Z hlediska dodržování předpisů může být nutné uchování dat po dobu několika měsíců nebo i let. V takovém případě jsou pro archivaci ideální zálohovaná data. |Zotavení po havárii vyžaduje pouze provozní data obnovení, která obvykle zabírají několik hodin nebo až den. Z důvodu zaznamenávání jemně odstupňovaných dat v řešeních zotavení po havárii nedoporučujeme používání dat zotavení po havárii pro dlouhodobé uchovávání.  |

## <a name="next-steps"></a>Další kroky
Podrobné pokyny pro ochranu dat na Windows Serveru nebo pro ochranu virtuálního počítače v Azure najdete v jednom z následujících kurzů:

* [Zálohování souborů a složek](backup-try-azure-backup-in-10-mins.md)
* [Zálohování virtuálních počítačů Azure](backup-azure-vms-first-look-arm.md)

Podrobnosti o ochraně jiných úloh můžete zkusit najít v některém z těchto článků:

* [Zálohování Windows Serveru](backup-configure-vault.md)
* [Zálohování úloh aplikací](backup-azure-microsoft-azure-backup.md)
* [Zálohování virtuálních počítačů Azure IaaS](backup-azure-vms-prepare.md)

[green]: ./media/backup-introduction-to-azure-backup/green.png
[yellow]: ./media/backup-introduction-to-azure-backup/yellow.png
[red]: ./media/backup-introduction-to-azure-backup/red.png



<!--HONumber=Jan17_HO1-->


