# <a name="regions-and-availability-for-virtual-machines-in-azure"></a>Oblasti a dostupnost pro virtuální počítače v Azure
Je důležité pochopit, jak a kde se virtuální počítače (VM) v Azure provozují, a také vědět, jaké máte možnosti pro zajištění maximálního výkonu, dostupnosti a redundance. Azure funguje v několika datových centrech po celém světě. Tato datová centra jsou seskupená do v geografických oblastí. To vám poskytuje flexibilitu při výběru místa pro sestavení vaší aplikace. Tento článek poskytuje přehled funkcí redundance a dostupnosti Azure.

## <a name="what-are-azure-regions"></a>Co jsou oblasti Azure?
Azure umožňuje vytvářet prostředky, jako jsou virtuální počítače, v definované geografických oblastech, jako je Západ USA, Severní Evropa nebo Jihovýchodní Asie. V současnosti po celém světě existuje 30 oblastí Azure. [Seznam oblastí a jejich umístění](https://azure.microsoft.com/regions/) si můžete prohlédnout. V každé oblasti existuje několik datových center, která zajišťují redundanci a dostupnost. Tento přístup vám dává volnost při sestavování aplikací. Můžete vytvořit virtuální počítače co nejblíž k uživatelům a současně zajistit dodržování předpisů a splnění všech právních a daňových požadavků.

## <a name="special-azure-regions"></a>Speciální oblasti Azure
Existují speciální oblasti Azure pro právní účely nebo dodržování předpisů, které můžete při vytváření aplikací využít. Mezi tyto speciální oblasti patří:

* **USA (Gov) – Iowa** a **USA (Gov) – Virginia**.
  * Fyzická a logická síťově izolovaná instance Azure pro partnery a úřady státní správy USA, která je obsluhovaná prověřenými občany USA. Zahrnuje další certifikace dodržování předpisů, jako je [FedRAMP](https://www.microsoft.com/en-us/TrustCenter/Compliance/FedRAMP) a [DISA](https://www.microsoft.com/en-us/TrustCenter/Compliance/DISA). Další informace o službě [Azure Government](https://azure.microsoft.com/features/gov/)
* **Střední Indie**, **Jižní Indie** a **Západní Indie**
  * Tyto oblasti jsou aktuálně dostupné zákazníkům s multilicencemi a partnerům, kteří mají místní registraci v Indii. V roce 2016 k nim uživatelé měli přístup, pokud si zakoupili přímá předplatná.
* **Severní Čína** a **Východní Čína**
  * Tyto oblasti jsou dostupné prostřednictvím jedinečného partnerství mezi společnostmi Microsoft a 21Vianet, kdy společnost Microsoft nespravuje tato datová centra přímo. Další informace najdete v tématu [Microsoft Azure v Číně](http://www.windowsazure.cn/).
* **Střední Německo** a **Severovýchodní Německo**
  * Tyto oblasti jsou v současnosti dostupné prostřednictvím modelu důvěryhodných uživatelů dat, kdy zákaznická data zůstanou v Německu pod kontrolou společnosti T-Systems, která je součástí skupiny Deutsche Telekom a bude fungovat jako důvěryhodný uživatel dat pro Německo.

## <a name="region-pairs"></a>Párování oblastí
Každá oblast Azure je spárovaná s jinou oblastí na stejném území (jako je USA, Evropa nebo Asie). Tento přístup umožňuje replikaci prostředků (jako je třeba úložiště virtuálních počítačů) napříč daným územím, která by měla snížit pravděpodobnost, že přírodní katastrofy, občanské nepokoje, výpadky napájení nebo fyzické výpadky sítě ovlivní obě oblasti současně. Mezi další výhody párování oblastí patří:

* V případě rozsáhlejšího výpadku Azure je z každého páru jedna oblast určená jako prioritní. To pomáhá zkrátit čas potřebný k obnovení aplikací. 
* Plánované aktualizace Azure se pro spárované oblasti nasazují po jedné. Tím se minimalizují prostoje a riziko výpadku aplikací.
* Pro daňové účely a účely vymáhání zákonů jsou data nadále uložená na stejném území jako spárovaná oblast (a výjimkou oblasti Brazílie – jih).

Mezi příklady párů oblasti patří:

| Primární | Sekundární |
|:--- |:--- |
| Západní USA |Východ USA |
| Severní Evropa |Západní Evropa |
| Jihovýchodní Asie |Východní Asie |

Úplný [seznam spárovaných oblastí najdete tady](../articles/best-practices-availability-paired-regions.md#what-are-paired-regions).

## <a name="feature-availability"></a>Dostupnost funkcí
Některé služby nebo funkce virtuálních počítačů jsou dostupné jenom v některých oblastech, třeba konkrétní velikosti virtuálních počítačů nebo typy úložišť. Existují také globální služby Azure, které nevyžadují výběr konkrétní oblasti, jako je [Azure Active Directory](../articles/active-directory/active-directory-whatis.md), [Traffic Manager](../articles/traffic-manager/traffic-manager-overview.md) nebo [Azure DNS](../articles/dns/dns-overview.md). Možnost zkontrolovat [dostupnost služeb Azure v jednotlivých oblastech](https://azure.microsoft.com/regions/#services) vám pomůže při navrhování prostředí pro vaše aplikace. 

## <a name="storage-availability"></a>Dostupnost úložiště
Znalost území a oblastí Azure je důležitá, pokud zvažujete dostupné možnosti replikace úložiště. Možnosti replikace závisejí na typu úložiště.

**Azure Managed Disks**
* Místně redundantní úložiště (LRS)
  * Replikuje data třikrát v rámci oblasti, ve které jste vytvořili účet úložiště.

**Disky založené na účtu úložiště**
* Místně redundantní úložiště (LRS)
  * Replikuje data třikrát v rámci oblasti, ve které jste vytvořili účet úložiště.
* Zónově redundantní úložiště (ZRS)
  * Replikuje data třikrát v rámci dvou nebo tří zařízení, a to v jedné oblasti nebo napříč dvěma oblastmi.
* Geograficky redundantní úložiště (GRS)
  * Replikuje data do sekundární oblasti, která je od primární oblasti vzdálená stovky kilometrů.
* Geograficky redundantní úložiště s přístupem pro čtení (RA-GRS)
  * Replikuje data do sekundární oblasti (stejně jako u GRS), ale k datům v sekundárním umístění poskytuje přístup jenom pro čtení.

V následující tabulce najdete stručný přehled rozdílů jednotlivých typů replikace:

| Strategie replikace | LRS | ZRS | GRS | RA-GRS |
|:--- |:--- |:--- |:--- |:--- |
| Data se replikují napříč různými zařízeními. |Ne |Ano |Ano |Ano |
| Data je možné načíst ze sekundárního i primárního umístění. |Ne |Ne |Ne |Ano |
| Počet kopií dat uchovávaných na samostatných uzlech |3 |3 |6 |6 |

Další informace o [možnostech replikace služby Azure Storage najdete tady](../articles/storage/storage-redundancy.md). Další informace o spravovaných discích najdete v tématu [Přehled služby Azure Managed Disks](../articles/storage/storage-managed-disks-overview.md).

### <a name="storage-costs"></a>Cena za uložení
Ceny se liší v závislosti na zvolené dostupnosti a typu úložiště.

**Azure Managed Disks**
* Služba Managed Disks úrovně Premium se zálohuje pomocí disků SSD (Solid State Drive) a Standard Managed Disks se zálohuje pomocí běžných rotujících disků. Služba Managed Disks úrovně Premium i Standard se účtuje na základě zřízené kapacity disku.

**Nespravované disky**
* Storage úrovně Premium se zálohuje pomocí disků SSD (Solid State Drive) a účtuje se na základě kapacity disku.
* Storage úrovně Standard se zálohuje pomocí běžných rotujících disků a účtuje se na základě využité kapacity a požadované dostupnosti úložiště.
  * Za přenos dat geografické replikace se u RA-GRS účtují navíc náklady na šířku pásma pro replikaci těchto dat do jiné oblasti Azure.

Informace o cenách pro různé typy úložišť a možnosti dostupnosti najdete v tématu [Ceny za Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

## <a name="azure-images"></a>Image dostupné v Azure
V Azure se virtuální počítače vytvářejí z imagí. Image obvykle pocházejí z [Azure Marketplace](https://azure.microsoft.com/marketplace/), kde partneři mohou poskytnout předem nakonfigurované image kompletního operačního systému nebo aplikace.

Když vytvoříte virtuální počítač pomocí z Azure Marketplace, vlastně pracujete se šablonami. Šablony Azure Resource Manageru jsou deklarativní soubory JSON (JavaScript Object Notation), které se dají využít k vytváření komplexních aplikačních prostředí, která zahrnují virtuální počítače, úložiště, virtuální sítě atd. O použití [šablon Azure Resource Manageru](../articles/azure-resource-manager/resource-group-overview.md), včetně postupu při [vytváření vlastních šablon](../articles/resource-group-authoring-templates.md), si můžete přečíst víc.

Můžete také vytvořit vlastní image a nahrát je pomocí [rozhraní příkazového řádku Azure](../articles/virtual-machines/linux/upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo [Azure PowerShellu](../articles/virtual-machines/windows/upload-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). To vám umožní rychle vytvořit vlastní virtuální počítače splňující konkrétní požadavky na sestavení.

## <a name="availability-sets"></a>Skupiny dostupnosti
Skupina dostupnosti je logické seskupení virtuálních počítačů, které umožňují Azure pochopit, jak je sestavená vaše aplikace, aby bylo možné zajistit redundanci a dostupnost. Pro aplikace vyžadující vysokou dostupnost a pro zajištění [Azure SLA s 99,95 dostupností](https://azure.microsoft.com/support/legal/sla/virtual-machines/) se doporučuje vytvořit ve skupině dostupnosti dva nebo víc virtuálních počítačů. Pokud [Azure Premium Storage](../articles/storage/storage-premium-storage.md) využívá jeden virtuální počítač, Azure SLA se vztahuje na neplánovanou údržbu. Skupina dostupnosti se skládá ze dvou dalších seskupení, které chrání před selháním hardwaru a umožňují bezpečnou instalaci aktualizací – doména selhání (FD) a aktualizační doména (UD).

![Koncepční nákres konfigurace aktualizačních domén a domén selhání](./media/virtual-machines-common-regions-and-availability/ud-fd-configuration.png)

O správě dostupnosti [virtuálních počítačů s Linuxem](../articles/virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo [Windows](../articles/virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) si můžete přečíst víc.

### <a name="fault-domains"></a>Domény selhání
Doména selhání je logická skupina hardwarových komponent, které sdílejí společný zdroj napájení a síťový přepínač (je obdobou stojanu v místním datovém centru). Když vytváříte virtuální počítače v rámci skupiny dostupnosti, platforma Azure je automaticky distribuuje do těchto domén selhání. Tento přístup omezuje dopady potenciálního selhání fyzického hardwaru, výpadků sítě nebo přerušení napájení.

#### <a name="managed-disk-fault-domains-and-availability-sets"></a>Skupiny dostupnosti a domény selhání pro Managed Disks
Virtuální počítače, které používají [Azure Managed Disks](../articles/storage/storage-faq-for-disks.md), odpovídají doménám selhání spravovaných disků (pokud se použije spravovaná skupina dostupnosti). Toto uspořádání zajišťuje, aby všechny spravované disky připojené k virtuálnímu počítači byly umístěné ve stejné doméně selhání spravovaných disků. Ve spravované skupině dostupnosti je možné vytvořit jenom virtuální počítače se spravovanými disky. Počet domén selhání spravovaných disků se liší podle oblasti – buď dvě, nebo tři domény selhání na oblast.

### <a name="update-domains"></a>Aktualizační domény
Aktualizační doména je logická skupina hardwarových komponent, u kterých je možné provést údržbu nebo restart současně. Když vytváříte virtuální počítače v rámci skupiny dostupnosti, platforma Azure je automaticky distribuuje do těchto aktualizačních domén. Tento přístup zajišťuje, že při pravidelné údržbě Azure zůstává vždycky nejméně jedna instance vaší aplikace spuštěná. Restartování aktualizačních domén během plánované údržby nemusí probíhat sekvenčně, ale vždycky se restartuje jenom jedna aktualizační doména.

## <a name="next-steps"></a>Další kroky
Teď můžete tyto funkce pro zajištění redundance a dostupnosti začít používat a vytvořit prostředí Azure. Informace o doporučených postupech najdete v tématu věnovaném [osvědčeným postupům pro zajištění dostupnosti v Azure](../articles/best-practices-availability-checklist.md).

