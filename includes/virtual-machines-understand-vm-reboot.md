Virtuální počítače Azure (VM) může někdy restartuje bez zjevné příčiny, aniž by váš s iniciované operace restartování. V tomto článku jsou uvedené akce a události, které může způsobit, že virtuální počítače restartovat a poskytuje přehled o tom, jak předešli problémům s neočekávané restartování nebo snížit dopad tyto problémy.

## <a name="configure-the-vms-for-high-availability"></a>Konfigurace virtuálních počítačů pro vysokou dostupnost
Nejlepší způsob, jak chránit aplikace, která běží v Azure pro virtuální počítač se restartuje a výpadky, je nakonfigurovat pro zajištění vysoké dostupnosti virtuálních počítačů.

Pokud chcete zadat tato úroveň redundance, aby vaše aplikace, doporučujeme seskupit dva nebo více virtuálních počítačů v nastavení dostupnosti. Tato konfigurace zajistí, že během buď plánované i neplánované údržby alespoň jeden virtuální počítač je k dispozici a splňuje 99,95 % [smlouva Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_5/).

Další informace o nastavení dostupnosti najdete v následujících článcích:

- [Správa dostupnosti virtuálních počítačů.](../articles/virtual-machines/windows/manage-availability.md)
- [Konfigurace dostupnosti virtuálních počítačů.](../articles/virtual-machines/windows/classic/configure-availability.md)

## <a name="resource-health-information"></a>Informace o stavu prostředků 
Azure Resource Health je služba, která zveřejňuje stav jednotlivých prostředků Azure a poskytuje řešitelné pokyny při řešení problémů. V cloudovém prostředí, kde není možné přímý přístup k serverům nebo prvky infrastruktury je zkrátit čas strávený řešení potíží s cílem stav prostředku. Cílem je konkrétně zkrátit čas strávený určení, zda je kořenový problému v aplikaci nebo v události uvnitř platformy Azure. Další informace najdete v tématu [Rady pro pochopení a používání prostředků stavu](../articles/resource-health/resource-health-overview.md).

## <a name="actions-and-events-that-can-cause-the-vm-to-reboot"></a>Akce a události, které můžou způsobit, že virtuální počítač na restartování

### <a name="planned-maintenance"></a>Plánovaná údržba
Microsoft Azure pravidelně provádí aktualizace po celém světě ke zlepšení spolehlivosti, výkonu a zabezpečení infrastruktury hostitele, který je základem virtuálních počítačů. Mnoho z těchto aktualizací, včetně zachování paměti aktualizací, se provádí bez dopady na virtuální počítače nebo cloudové služby.

Ale některé aktualizace vyžadují restartování. V takových případech jsou virtuální počítače vypnout při jsme oprava infrastruktury, a pak se restartuje virtuální počítače.

Zjistit, jaké Azure plánované údržby a jak může ovlivnit dostupnost virtuálních počítačů Linux, najdete v článcích tady. Články poskytují informace o Azure plánované proces údržby a naplánování plánované údržby, pokud chcete dál snížit dopad.

- [Plánovaná údržba virtuálních počítačů v Azure](../articles/virtual-machines/windows/planned-maintenance.md)
- [Naplánování plánované údržby na virtuálních počítačích Azure](../articles/virtual-machines/windows/classic/planned-maintenance-schedule.md)

### <a name="memory-preserving-updates"></a>Aktualizace zachovávající paměť   
Pro tuto třídu aktualizací ve službě Microsoft Azure uživatelé nezaznamenají žádný vliv na jejich spuštěné virtuální počítače. Mnoho takových aktualizací se týká komponent nebo služeb, které je možné aktualizovat bez zásahů do běžící instance. Některé jsou platformy aktualizace infrastruktury na hostitelský operační systém, který lze použít bez restartování virtuálních počítačů.

Tyto aktualizace se zachováním paměti jsou provést pomocí technologie, která umožňuje migraci za provozu na místě. Když se právě aktualizuje, virtuální počítač je umístěn v *pozastavena* stavu. Tento stav zachovává paměť v paměti RAM při její příslušný operační systém hostitele přijímat potřebné aktualizace a opravy. Virtuální počítač je obnoven běh v rámci 30 sekund byla pozastavena. Po obnovení virtuálního počítače je automaticky synchronizovat jeho hodiny.

Z důvodu období krátké pozastavení nasazení aktualizací díky tomuto mechanismu výrazně snižuje dopad na virtuálních počítačích. Tímto způsobem lze nasadit, ale ne všechny aktualizace. 

Aktualizace s více instancemi (pro virtuální počítače v nastavení dostupnosti) jsou použité jednu aktualizační doménu najednou.

> [!NOTE]
> Počítače s Linuxem, které mají staré verze jádra jsou ovlivněné jádra paniky během tato metoda aktualizace. K tomuto problému vyhnout, aktualizujte na verzi 3.10.0-327.10.1 jádra nebo novější. Další informace najdete v tématu [virtuálního počítače Azure Linux na jádro na základě 3.10 panics po upgradu uzlu hostitele](https://support.microsoft.com/help/3212236).     
    
### <a name="user-initiated-reboot-or-shutdown-actions"></a>Restartování nebo vypnutí akce zahájená uživatelem
 
Pokud provádíte restartování z portálu Azure, Azure PowerShell, rozhraní příkazového řádku nebo resetovat rozhraní API, můžete najít událost v [protokol činnosti Azure](../articles/monitoring-and-diagnostics/monitoring-overview-activity-logs.md).

Pokud provádíte akce z operačního systému Virtuálního počítače, můžete najít události v protokolu systému.

Další scénáře, které obvykle způsobí restartování virtuálního počítače zahrnují více akcí změně konfigurace. Normálně uvidíte, že zpráva s upozorněním, která spouštění konkrétní akce bude mít za následek restartování virtuálního počítače. Mezi příklady patří žádné virtuálního počítače změňte velikost operace, změna heslo účtu správce a nastavení statické IP adresy.

### <a name="azure-security-center-and-windows-update"></a>Azure Security Center a aktualizace systému Windows
Azure Security Center monitoruje denní Windows a virtuální počítače s Linuxem pro chybějící aktualizace operačního systému. Security Center načte seznam dostupných zabezpečení a důležité aktualizace ze služby Windows Update nebo Windows Server Update Services (WSUS), podle toho, která je nakonfigurovaná služba virtuální počítač s Windows. Security Center také zkontroluje nejnovější aktualizace pro systémy Linux. Pokud virtuální počítač je chybějící aktualizace systému, Security Center doporučuje použít aktualizace systému. Použití těchto aktualizací systému je řízen pomocí Security Center na portálu Azure. Po instalaci některé aktualizace, může být vyžadováno restartování virtuálního počítače. Další informace najdete v tématu [aktualizace systému v Azure Security Center](../articles/security-center/security-center-apply-system-updates.md).

Jako na místní servery Azure není nabízená aktualizace ze služby Windows Update virtuálních počítačích Windows Azure, protože tyto počítače jsou určené ke správě svých uživatelů. Jste, ale doporučujeme ponechat povolené automatické nastavení služby Windows Update. Automatická instalace aktualizací ze služby Windows Update může také způsobit restartování dochází po použití aktualizací. Další informace najdete v tématu [nejčastější dotazy týkající se Windows Update](https://support.microsoft.com/help/12373/windows-update-faq).

### <a name="other-situations-affecting-the-availability-of-your-vm"></a>Další situace, které mají vliv na dostupnost virtuálního počítače
Existují další případy, ve kterých Azure může aktivně, pozastavit virtuálního počítače. Obdržíte e-mailová oznámení před provedením této akce, takže budete mít možnost vyřešit základní problémy. Mezi příklady problémy, které mít vliv na dostupnost virtuálního počítače patří narušení zabezpečení a vypršení platnosti způsoby platby.

### <a name="host-server-faults"></a>Hostitel serveru chyb 
Virtuální počítač je hostován na fyzickém serveru, který běží v datovém centru Azure. Fyzický server běží agent volá Agent hostitele kromě několik dalších komponent Azure. Když se tyto Azure softwarové komponenty na fyzickém serveru přestat reagovat, systém monitorování aktivuje restartování hostitelského serveru k pokusu o obnovení. Virtuální počítač je obvykle dostupná znovu za pět minut a pokračuje v za provozu na stejném hostiteli jako dříve.

Server chyby jsou obvykle způsobeno selhání hardwaru, jako je selhání pevného disku nebo jednotku SSD. Azure nepřetržitě monitoruje tyto události, identifikuje základní chyby a vydává aktualizace po zmírnění byla implementována a otestovat.

Protože některé chyby serveru hostitele může být specifické pro tento server, opakované situaci restartování virtuálního počítače může vylepšené pomocí ručně opětovného nasazení virtuálního počítače na jiný server hostitele. Tato operace můžete spustit pomocí **znovu nasaďte** možnost na stránce podrobností virtuálního počítače nebo zastavením a restartováním virtuálního počítače na portálu Azure.

### <a name="auto-recovery"></a>Automatické obnovení
Pokud z nějakého důvodu nelze restartovat hostitelský server, zahájí platformy Azure akci automatické obnovení, vadný hostitelský server mimo otočení dále prozkoumat. 

Všechny virtuální počítače na tomto hostiteli jsou automaticky přemístění do jiné, pořádku hostitelský server. Tento proces je obvykle dokončení do 15 minut. Další informace o procesu automatického obnovení najdete v tématu [automatického obnovení virtuálních počítačů](https://azure.microsoft.com/blog/service-healing-auto-recovery-of-virtual-machines).

### <a name="unplanned-maintenance"></a>Neplánovaná Údržba
Ve výjimečných případech může Azure provozní tým potřeba provést údržby aktivity pro zaručení dobrého celkového stavu na platformě Azure. Toto chování může mít vliv na dostupnost virtuálního počítače a obvykle výsledkem je to stejné akce automatického obnovení jak bylo popsáno výše.  

Neplánované maintenances zahrnují následující:

- Defragmentace naléhavé uzlu
- Aktualizace přepínač naléhavé sítě

### <a name="vm-crashes"></a>Dojde k chybě virtuálního počítače
Virtuální počítače může restartovat kvůli problémům v rámci virtuální počítač. Úlohy nebo role, která běží ve virtuálním počítači může spustit kontrolu chyb v rámci hostovaného operačního systému. Pro pomoc při rozhodování, z důvodu při chybě můžete zobrazte systému a protokoly aplikací pro virtuální počítače Windows a protokol sériového portu pro virtuální počítače s Linuxem.

### <a name="storage-related-forced-shutdowns"></a>Související s úložištěm vynucené vypnutí
Virtuální počítače v Azure využívají virtuální disky pro operační systém a úložiště dat, který je hostován na infrastrukturu Azure Storage. Vždy, když pro více než 120 sekundách je vliv na dostupnost nebo připojení mezi virtuální počítač a přidruženými virtuálními disky, provede platformy Azure vynucené vypnutí virtuálních počítačů, aby se zabránilo poškození dat. Virtuální počítače jsou automaticky zapnutý zpět po obnovení připojení úložiště. 

Doba trvání ukončení může být co nejkratší pět minut, ale mohou být podstatně delší. Toto je jedna z konkrétní případů, které souvisí s související s úložištěm vynucené vypnutí systému: 

**Překročení vstupně-výstupní operace omezení**

Virtuální počítače může dočasně vypnout, při vstupně-výstupní požadavky jsou omezené konzistentně, protože objem vstupně-výstupních operací za sekundu (IOPS) přesahuje omezení vstupně-výstupní operace disku. (Úložiště – standardní disk je omezený na 500 IOPS.) Ke zmírnění tohoto problému, použijte prokládání disků nebo nakonfigurovat v prostoru úložiště uvnitř hostovaného virtuálního počítače, v závislosti na zatížení. Podrobnosti najdete v tématu [konfigurace virtuálních počítačů Azure pro optimální výkon úložiště](http://blogs.msdn.com/b/mast/archive/2014/10/14/configuring-azure-virtual-machines-for-optimal-storage-performance.aspx).

Vyšší limity IOPS jsou k dispozici prostřednictvím Azure Premium Storage s až 80 000 IOPS. Další informace najdete v tématu [vysoce výkonné úložiště Premium](../articles/virtual-machines/windows/premium-storage.md).

### <a name="other-incidents"></a>Jiné incidenty
Ve výjimečných případech může ovlivnit problém s rozšířeným více serverů v datovém centru Azure. Pokud se k tomuto problému dochází, tým Azure odešle e-mailová oznámení ovlivněných odběry. Můžete zkontrolovat [řídicí panel stavu služby Azure](https://azure.microsoft.com/status/) a portál Azure pro stav probíhající výpadky a za incidenty.
