## <a name="understand-planned-vs-unplanned-maintenance"></a>Vysvětlení rozdílů mezi plánovanou a neplánovanou údržbou
Existují dva typy událostí platformy Microsoft Azure, které můžou ovlivnit dostupnost vašich virtuálních počítačů: plánovaná údržba a neplánovaná údržba.

* **Plánované události údržby** jsou pravidelné aktualizace základní platformy Azure prováděné Microsoftem za účelem zlepšení celkové spolehlivosti, výkonu a zabezpečení infrastruktury platformy, na které běží vaše virtuální počítače. U většiny těchto aktualizací nemá jejich provedení žádný vliv na vaše virtuální počítače ani cloudové služby. Existují však případy, kdy tyto aktualizace k instalaci požadovaných aktualizací na infrastrukturu platformy vyžadují restartování virtuálních počítačů.
* **Neplánované události údržby** nastávají v případě, že dojde k nějakému selhání hardwaru nebo základní fyzické infrastruktury, na které stojí virtuální počítač. To může zahrnovat selhání místní sítě, selhání místního disku nebo další selhání na úrovni racku. Když se takové selhání detekuje, platforma Azure automaticky provede migraci vašeho virtuálního počítače z hostujícího fyzického počítače ve špatném stavu na fyzický počítač, který je v pořádku. Takové události se vyskytují jen vzácně, ale také můžou způsobit restartování vašeho virtuálního počítače.

Pokud chcete snížit dopad výpadků kvůli jedné nebo několika takovým událostem, doporučujeme pro vaše virtuální počítače následující osvědčené postupy z hlediska vysoké dostupnosti:

* [Konfigurace více virtuálních počítačů ve skupině dostupnosti pro zajištění redundance]
* [Konfigurace jednotlivých vrstev aplikace v samostatných skupinách dostupnosti]
* [Kombinace nástroje pro vyrovnávání zatížení se skupinami dostupnosti]
* [Použití více účtů úložiště pro každou skupinu dostupnosti]

## <a name="configure-multiple-virtual-machines-in-an-availability-set-for-redundancy"></a>Konfigurace více virtuálních počítačů ve skupině dostupnosti pro zajištění redundance
Pokud chcete zajistit redundanci pro vaši aplikaci, doporučujeme seskupit dva nebo více virtuálních počítačů do skupiny dostupnosti. Tato konfigurace zajišťuje, že během plánované nebo neplánované události údržby zůstane dostupný alespoň jeden virtuální počítač, který splňuje 99,95% Azure SLA. Další informace najdete v tématu [SLA pro virtuální počítače](https://azure.microsoft.com/support/legal/sla/virtual-machines/).

> [!IMPORTANT]
> Nenechávejte ve skupině dostupnosti samostatné instance virtuálních počítačů. Na virtuální počítače v takové konfiguraci se nevztahuje garance SLA a během plánovaných událostí údržby čelí výpadkům, kromě případu, kdy samostatný virtuální počítač využívá službu [Storage úrovně Premium](../articles/storage/storage-premium-storage.md). Na samostatné virtuální počítače, které využívají službu Storage úrovně Premium, se Azure SLA vztahuje.

Základní platforma Azure každému virtuálnímu počítači ve skupině dostupnosti přiřadí **aktualizační doménu** a **doménu selhání**. Dané skupině dostupnosti se ve výchozím nastavení přiřadí pět aktualizačních domén (u nasazení podle modelu Resource Manager je pak možné počet aktualizačních domén navýšit až na 20), které uživatel nemůže konfigurovat a které představují skupiny virtuálních počítačů a základního fyzického hardwaru, které lze restartovat současně. Pokud je v rámci jedné skupiny dostupnosti nakonfigurováno více než&5; virtuálních počítačů, šestý virtuální počítač se umístí do stejné aktualizační domény jako první virtuální počítač, sedmý se umístí do stejné aktualizační domény jako druhý atd. Restartování aktualizačních domén během plánované údržby nemusí probíhat sekvenčně, ale vždycky se restartuje jenom jedna aktualizační doména.

Domény selhání definují skupinu virtuálních počítačů, které sdílejí společný zdroj napájení a síťový přepínač. Ve výchozím nastavení jsou virtuální počítače konfigurované v rámci skupiny dostupnosti rozdělené až do tří domén selhání pro nasazení podle modelu Resource Manager (do dvou domén selhání pro model nasazení Classic). Přestože umístění virtuálních počítačů do skupiny dostupnosti neochrání vaši aplikaci před selháním operačního systému nebo selháním spojeným s konkrétní aplikací, omezí se tím dopad potenciálních selhání fyzického hardwaru, výpadků sítě nebo přerušení napájení.

<!--Image reference-->
   ![Koncepční nákres konfigurace aktualizačních domén a domén selhání](./media/virtual-machines-common-manage-availability/ud-fd-configuration.png)

### <a name="managed-disk-fault-domains-and-availability-sets"></a>Skupiny dostupnosti a domény selhání pro Managed Disks
Virtuální počítače, které používají [Azure Managed Disks](../articles/storage/storage-faq-for-disks.md), odpovídají doménám selhání spravovaných disků (pokud se použije spravovaná skupina dostupnosti). Toto uspořádání zajišťuje, aby všechny spravované disky připojené k virtuálnímu počítači byly umístěné ve stejné doméně selhání spravovaných disků. Ve spravované skupině dostupnosti je možné vytvořit jenom virtuální počítače se spravovanými disky. Počet domén selhání spravovaných disků se liší podle oblasti – buď dvě, nebo tři domény selhání na oblast.


## <a name="configure-each-application-tier-into-separate-availability-sets"></a>Konfigurace jednotlivých vrstev aplikace v samostatných skupinách dostupnosti
Pokud jsou všechny vaše virtuální počítače téměř identické a pro vaši aplikaci slouží stejnému účelu, doporučujeme nakonfigurovat skupinu dostupnosti pro každou vrstvu aplikace.  Pokud do stejné skupiny dostupnosti umístíte dvě různé vrstvy, bude možné restartovat všechny virtuální počítače na stejné úrovni aplikace. Nakonfigurováním alespoň dvou virtuálních počítačů ve skupině dostupnosti pro každou vrstvu můžete zajistit, že v každé skupině dostupnosti bude k dispozici alespoň jeden virtuální počítač.

Můžete například umístit všechny virtuální počítače ve front-endu vaší aplikace, na nichž služba běží IIS, Apache nebo Nginx, do jedné skupiny dostupnosti. Ujistěte se, že jsou ve stejné skupině dostupnosti umístěné pouze virtuální počítače ve front-endu. Podobně se ujistěte, že jsou ve vlastní skupině dostupnosti umístěné pouze virtuální počítače datové vrstvy, jako jsou například replikované virtuální počítače se systémem SQL Server nebo virtuální počítače se serverem MySQL.

<!--Image reference-->
   ![Úrovně aplikace](./media/virtual-machines-common-manage-availability/application-tiers.png)

## <a name="combine-a-load-balancer-with-availability-sets"></a>Kombinace nástroje pro vyrovnávání zatížení se skupinami dostupnosti
Kombinací služby [Azure Load Balancer](../articles/load-balancer/load-balancer-overview.md) se skupinou dostupnosti získáte největší odolnost aplikace. Azure Load Balancer rozděluje provoz mezi víc virtuálních počítačů. Azure Load Balancer je součástí virtuálních počítačů úrovně Standard. Ne všechny úrovně virtuálních počítačů zahrnují nástroj Azure Load Balancer. Další informace o vyrovnávání zatížení virtuálních počítačů najdete v článku [Vyrovnávání zatížení virtuálních počítačů](../articles/virtual-machines/virtual-machines-linux-load-balance.md).

Pokud nástroj pro vyrovnávání zatížení není nakonfigurovaný k vyrovnávání provozu mezi víc virtuálních počítačů, všechny plánované události údržby ovlivní jediný virtuální počítač obsluhující provoz a způsobí výpadek vrstvy aplikace. Umístěním více virtuálních počítačů na stejné vrstvě aplikace do stejné skupiny dostupnosti a pod jeden nástroj pro vyrovnávání zatížení umožníte nepřetržitou obsluhu provozu alespoň jednou instancí.

## <a name="use-multiple-storage-accounts-for-each-availability-set"></a>Použití více účtů úložiště pro každou skupinu dostupnosti
Pokud používáte službu Azure Managed Disks, následující pokyny můžete přeskočit. Služba Azure Managed Disks ze své podstaty poskytuje vysokou dostupnost a redundanci, protože disky jsou uložené v doménách selhání, které odpovídají skupinám dostupnosti vašich virtuálních počítačů. Další informace najdete v tématu [Přehled služby Azure Managed Disks](../articles/storage/storage-managed-disks-overview.md).

Pokud používáte nespravované disky, existují osvědčené postupy, kterými se můžete řídit s ohledem na to, jaké účty úložiště používají virtuální pevné disky v rámci virtuálního počítače. Každý virtuální pevný disk je objektem blob stránky v účtu služby Azure Storage. Pokud chcete zajistit vysokou dostupnost virtuálních počítačů v rámci skupiny dostupnosti, je důležité zajistit redundanci a izolaci napříč účty úložiště.

1. **Uchovávejte všechny disky (s operačním systémem i s daty) přidružené k virtuálnímu počítači ve stejném účtu úložiště.**
2. Při přidávání dalších virtuálních pevných disků k účtu úložiště byste **měli zvážit [omezení](../articles/storage/storage-scalability-targets.md) účtu úložiště**.
3. **Pro každý virtuální počítač ve skupině dostupnosti použijte samostatný účet úložiště.** Více virtuálních počítačů ve stejné skupině dostupnosti NESMÍ sdílet účty úložiště. Sdílení účtů úložiště virtuálními počítači v různých skupinách dostupnosti je přijatelné, pokud dodržíte předcházející osvědčené postupy.

<!-- Link references -->
[Konfigurace více virtuálních počítačů ve skupině dostupnosti pro zajištění redundance]: #configure-multiple-virtual-machines-in-an-availability-set-for-redundancy
[Konfigurace jednotlivých vrstev aplikace v samostatných skupinách dostupnosti]: #configure-each-application-tier-into-separate-availability-sets
[Kombinace nástroje pro vyrovnávání zatížení se skupinami dostupnosti]: #combine-a-load-balancer-with-availability-sets
[Avoid single instance virtual machines in availability sets]: #avoid-single-instance-virtual-machines-in-availability-sets
[Použití více účtů úložiště pro každou skupinu dostupnosti]: #use-multiple-storage-accounts-for-each-availability-set



<!--HONumber=Feb17_HO2-->


