

## <a name="memory-preserving-updates"></a>Aktualizace zachovávající paměť
U určité třídy aktualizací v systému Microsoft Azure si zákazníci nevšimnou žádného dopadu na spuštěné virtuální počítače. Mnoho takových aktualizací se týká komponent nebo služeb, které je možné aktualizovat bez zásahů do běžící instance. Některé z těchto aktualizací jsou aktualizace infrastruktury platformy v hostitelském operačním systému, které je možné nainstalovat bez potřeby úplného restartování virtuálních počítačů.

Takové aktualizace jsou možné díky technologii umožňující místní migraci za provozu, která se označuje také jako aktualizace zachovávající paměť. Při aktualizace se virtuální počítač umístí do stavu „pozastaveno“ a paměť se zachová v paměti RAM, zatímco základní hostitelský operační systém obdrží potřebné aktualizace a opravy. Virtuální počítač se obnoví během 30 sekund od pozastavení. Po obnovení se hodiny virtuálního počítače automaticky synchronizují.

Ne všechny aktualizace je možné nasadit pomocí tohoto mechanismu, ale díky krátkému období pozastavení má tento způsob nasazení aktualizací výrazně nižší vliv na virtuální počítače.

Aktualizace více instancí (pro virtuální počítače ve skupině dostupnosti) se instalují vždy jenom v jedné aktualizační doméně.  

## <a name="virtual-machine-configurations"></a>Konfigurace virtuálních počítačů
Existují dva typy konfigurace virtuálních počítačů: s více instancemi a s jednou instancí. V konfiguraci s více instancemi jsou podobné virtuální počítače umístěné ve skupině dostupnosti.

Konfigurace s více instancemi poskytuje redundanci fyzických počítačů, napájení a sítě a je doporučenou konfigurací pro zajištění dostupnosti aplikace. Všechny virtuální počítače ve skupině dostupnosti by měly pro aplikaci sloužit ke stejnému účelu.

Další informace o konfiguraci virtuálních počítačů pro vysokou dostupnost najdete v tématu [Správa dostupnosti virtuálních počítačů s Windows](../articles/virtual-machines/virtual-machines-windows-manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) nebo [Správa dostupnosti virtuálních počítačů s Linuxem](../articles/virtual-machines/virtual-machines-linux-manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Naproti tomu konfigurace s jednou instancí se používá pro samostatné virtuální počítače, které nejsou umístěné v žádné skupině dostupnosti. Na takové virtuální počítače se nevztahuje smlouva SLA, která vyžaduje nasazení dvou nebo více virtuálních počítačů ve stejné skupině dostupnosti.

Další informace o smlouvách SLA najdete v částech Cloudové služby a Virtuální počítače na stránce [Smlouvy SLA](https://azure.microsoft.com/support/legal/sla/).

## <a name="multi-instance-configuration-updates"></a>Aktualizace konfigurací s více instancemi
Platforma Azure během plánované údržby nejprve aktualizuje sady virtuálních počítačů hostovaných v konfiguraci s více instancemi. Aktualizace způsobí restartování těchto virtuálních počítačů a přibližně 15minutový výpadek.

Aktualizace konfigurace s více instancemi předpokládá, že všechny virtuální počítače ve skupině dostupnosti mají podobnou funkci. V tomto nastavení se virtuální počítače aktualizují tak, aby se v průběhu celého procesu zachovala dostupnost.

Základní platforma Azure každému virtuálnímu počítači ve skupině dostupnosti přiřadí aktualizační doménu a doménu selhání. Každá aktualizační doména je skupina virtuálních počítačů, které se budou restartovat ve stejném časovém období. Každá doména selhání je skupina virtuálních počítačů, které sdílejí společný zdroj napájení a síťový přepínač.


Další informace o aktualizačních doménách a doménách selhání najdete v tématu [Konfigurace více virtuálních počítačů ve skupině dostupnosti pro zajištění redundance](../articles/virtual-machines/virtual-machines-windows-manage-availability.md#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy).

Za účelem zachování dostupnosti během aktualizace provádí Azure údržbu podle aktualizačních domén – aktualizuje vždy jenom jednu aktualizační doménu. Údržba v rámci aktualizační domény se skládá z vypnutí všech virtuálních počítačů v doméně, nainstalování aktualizace na hostitelských počítačích a následného restartování virtuálních počítačů. Jakmile je údržba v rámci domény dokončena, Azure tento proces zopakuje pro další aktualizační doménu a tak pokračuje s každou doménou, dokud nejsou aktualizované všechny.

Restartování aktualizačních domén během plánované údržby nemusí probíhat sekvenčně, ale vždy se restartuje jenom jedna aktualizační doména. Azure v současné době nabízí oznámení s týdenním předstihem o plánované údržbě virtuálních počítačů v konfiguraci s více instancemi.

Tady je příklad toho, co se může zobrazit v Prohlížeči událostí systému Windows po obnovení virtuálního počítače:

<!--Image reference-->
![][image2]


Pomocí webu Azure Portal, Azure PowerShellu nebo Azure CLI můžete nastavit, aby se v Prohlížeči hlásily virtuální počítače nakonfigurované v konfiguraci s více instancemi. Například na webu Azure Portal můžete do dialogového okna Procházet **virtuální počítače (Classic)** přidat položku _Skupina dostupnosti_. Virtuální počítače, u kterých je uvedena stejná skupina dostupnosti, jsou součástí konfigurace s více instancemi. V následujícím příkladu se konfigurace s více instancemi skládá z virtuálních počítačů SQLContoso01 a SQLContoso02.

<!--Image reference-->
  ![Zobrazení Virtuální počítače (Classic) z webu Azure Portal][image4]

## <a name="single-instance-configuration-updates"></a>Aktualizace konfigurací s jednou instancí
Po dokončení aktualizací konfigurací s více instancemi Azure provede aktualizace konfigurací s jednou instancí. Tyto aktualizace také způsobí restartování virtuálních počítačů, které nejsou spuštěné ve skupinách dostupnosti.

> [!NOTE]
> Pokud je ve skupině dostupnosti spuštěna pouze jedna instance virtuálního počítače, platforma Azure se bude chovat, jako kdyby šlo o aktualizaci v konfiguraci s více instancemi.
>

Údržba v rámci konfigurace s jednou instancí se skládá z vypnutí všech virtuálních počítačů spuštěných na hostitelském počítači, aktualizace hostitelského počítače a následného restartování virtuálních počítačů. Údržba vyžaduje výpadek přibližně 15 minut. Plánovaná událost údržby se spouští ve všech virtuálních počítačích v oblasti v jednom časovém období údržby.


V případě konfigurací s jednou instancí mají plánované události údržby dopad na dostupnost aplikací. Azure nabízí oznámení s týdenním předstihem o plánované údržbě virtuálních počítačů v konfiguracích s jednou instancí.

## <a name="email-notification"></a>E-mailové oznámení
Azure odesílá e-mailové upozornění o nadcházející plánované údržbě (týden předem) pouze pro konfigurace s jednou instancí a s více instancemi. Tento e-mail se odesílá na e-mailové účty správce a spolusprávce předplatného. Tady je příklad tohoto typu e-mailu:

<!--Image reference-->
![][image1]

## <a name="region-pairs"></a>Párování oblastí

Azure při provádění údržby aktualizuje pouze instance virtuálních počítačů v jedné oblasti z páru. Například při aktualizaci virtuálních počítačů v oblasti Střed USA – sever nebude Azure zároveň aktualizovat žádné virtuální počítače v oblasti Střed USA – jih. To se naplánuje na jindy – díky tomu je umožněno převzetí služeb při selhání nebo vyrovnávání zatížení mezi oblastmi. V ostatních oblastech, jako je Severní Evropa, však může údržba probíhat ve stejnou dobu jako v oblasti Východní USA.

Aktuální párování oblastí najdete v následující tabulce:

| Oblast 1 | Oblast 2 |
|:--- | ---:|
| Východ USA |Západní USA |
| Východ USA 2 |Střed USA |
| Střed USA – sever |Střed USA – jih |
| Západní střed USA |Západní USA 2 |
| Východní Kanada |Střední Kanada |
| Brazílie – jih |Střed USA – jih |
| USA (Gov) – Iowa |USA (Gov) – Virginia |
| US DoD – východ |US DoD – střed |
| Severní Evropa |Západní Evropa |
| Spojené království – západ |Spojené království – jih |
| Německo – střed |Německo – severovýchod |
| Jihovýchodní Asie |Východní Asie |
| Austrálie – jihovýchod |Austrálie – východ |
| Indie – střed |Indie – jih |
| Indie – západ |Indie – jih |
| Japonsko – východ |Japonsko – západ |
| Korea – střed |Korea – jih |
| Východní Čína |Severní Čína |


<!--Anchors-->
[image1]: ./media/virtual-machines-common-planned-maintenance/vmplanned1.png
[image2]: ./media/virtual-machines-common-planned-maintenance/EventViewerPostReboot.png
[image3]: ./media/virtual-machines-planned-maintenance/RegionPairs.PNG
[image4]: ./media/virtual-machines-common-planned-maintenance/availabilitysetexample.png


<!--Link references-->
[Virtual Machines Manage Availability]: ../articles/virtual-machines/virtual-machines-windows-hero-tutorial.md

[Understand planned versus unplanned maintenance]: ../articles/virtual-machines/virtual-machines-windows-manage-availability.md#Understand-planned-versus-unplanned-maintenance/
