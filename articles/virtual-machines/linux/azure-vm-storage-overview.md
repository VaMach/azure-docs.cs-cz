---
title: "Virtuální počítače s Linuxem v Azure a úložištěm Azure | Microsoft Docs"
description: "Popisuje Azure Standard a Premium Storage a spravované i nespravované disky se virtuální počítače s Linuxem."
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: vlivech
manager: timlt
editor: 
ms.assetid: d364c69e-0bd1-4f80-9838-bbc0a95af48c
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 2/7/2017
ms.author: rasquill
ms.openlocfilehash: 934580f6fcfdbff6e61626ed685459478559717d
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/02/2017
---
# <a name="azure-and-linux-vm-storage"></a>Úložiště Azure a virtuální počítač s Linuxem
Azure Storage je řešení cloudového úložiště pro moderní aplikace, které se opírají o odolnost, dostupnost a škálovatelnost, aby mohly vyhovět potřebám zákazníků.  Kromě toho, že vývojářům vytvářet aplikace ve velkém měřítku, aby podporovala nové scénáře, Azure Storage také nabízí základy úložiště pro virtuální počítače Azure.

## <a name="managed-disks"></a>Managed Disks

Virtuální počítače Azure jsou nyní k dispozici pomocí [Azure spravované disky](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), která umožňuje vytvářet virtuální počítače bez vytváření a správě žádné [účty Azure Storage](../../storage/common/storage-introduction.md) sami. Můžete určit, zda chcete Premium nebo standardního úložiště a jak velký by měla být na disku a disky virtuálních počítačů pro vás vytvoří Azure. Virtuální počítače s spravované disky mají mnoho důležitých funkcí, včetně:

- Podpora automatického škálovatelnost. Azure vytvoří disky a spravuje podkladové úložiště pro podporu až 10 000 disky na jedno předplatné.
- Vyšší spolehlivost skupiny dostupnosti. Azure zajišťuje, aby byly disky virtuálního počítače automaticky od sebe navzájem oddělené v rámci skupiny dostupnosti.
- Řízení přístupu vyšší. Spravované disky vystavit různých operací, které řídí [řízení řízení přístupu (RBAC)](../../active-directory/role-based-access-control-what-is.md).

Ceny pro spravované disky je jiný než pro který nespravované disků. Podrobnosti naleznete v tématu [ceny a fakturace pro spravované disky](../windows/managed-disks-overview.md#pricing-and-billing).

Můžete převést stávající virtuální počítače, které používají nespravované disky použít spravované disky s [převést virtuální počítač az](/cli/azure/vm#convert). Další informace najdete v tématu [jak převést virtuální počítač s Linuxem z nespravovaných disky na disky Azure spravované](convert-unmanaged-to-managed-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Nelze převést nespravované disku se spravovaným diskem Pokud nespravované disk v účtu úložiště, který je nebo kdykoli, už šifrovat pomocí [Azure úložiště služby šifrování (SSE)](../../storage/common/storage-service-encryption.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Následující kroky jsou upřesněny jak na nespravované disky, které jsou, nebo byla v účtu úložiště šifrované převést:

- Zkopírujte virtuální pevný disk (VHD) s [az úložiště objektů blob kopie počáteční](/cli/azure/storage/blob/copy#start) na účet úložiště, který nikdy bylo povoleno šifrování služby úložiště Azure.
- Vytvoření virtuálního počítače, které používá spravované disky a zadejte tento soubor virtuálního pevného disku při vytvoření s [vytvořit virtuální počítač az](/cli/azure/vm#create), nebo
- Připojte zkopírovaného virtuálního pevného disku s [připojit disk virtuálního počítače az](/cli/azure/vm/disk#attach) pro spuštění virtuálního počítače s spravované disky.


## <a name="azure-storage-standard-and-premium"></a>Úložiště Azure: Standard a Premium
Virtuální počítače Azure – jestli pomocí disků spravované nebo nespravované – může být založena na standardní úložiště disků nebo disků úložiště premium. Při použití portálu ke vyberte virtuální počítač musí přepnout rozevírací seznam na **Základy** obrazovku zobrazíte disky standard a premium. Při zapnutém stavu, vztahuje k SSD, pouze storage úrovně premium povoleno virtuální počítače se zobrazí, všechny založenou na SSD disky.  Při zapnutém stavu, vztahuje na pevný disk, povoleno úložiště standard virtuální počítače zajišťoval roztočený diskové jednotky se zobrazují, společně s premium úložiště virtuálních počítačů založenou na jednotku SSD.

Při vytváření virtuálního počítače z `azure-cli` můžete zvolit standard a premium při výběru velikost virtuálního počítače prostřednictvím `-z` nebo `--vm-size` příznak rozhraní příkazového řádku.

## <a name="creating-a-vm-with-a-managed-disk"></a>Vytvoření virtuálního počítače se spravovaným diskem

Následující příklad vyžaduje 2.0 rozhraní příkazového řádku Azure, která můžete [nainstalovat zde](/cli/azure/install-azure-cli).

Nejprve vytvořte skupinu prostředků ke správě prostředků s [vytvořit skupinu az](/cli/azure/group#create):

```azurecli
az group create --location westus --name myResourceGroup
```

Teď vytvořte virtuální počítač s [vytvořit virtuální počítač az](/cli/azure/vm#create). Zadejte jedinečný `--public-ip-address-dns-name` argument, jako `mypublicdns` pravděpodobně nebyla provedena.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --public-ip-address-dns-name mypublicdns
```

Předchozí příklad vytvoří virtuální počítač se spravovaným diskem v standardní účet úložiště. Pokud chcete používat účet úložiště Premium, přidejte `--storage-sku Premium_LRS` argument, jako v následujícím příkladu:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --public-ip-address-dns-name mypublicdns \
    --storage-sku Premium_LRS
```

## <a name="standard-storage"></a>Storage úrovně Standard
Azure Standard Storage je výchozí typ úložiště.  Ale stále mít původce je nákladově efektivní úložiště Standard storage.  

## <a name="premium-storage"></a>Storage úrovně Premium
Azure Premium Storage nabízí podporu vysoce výkonné, nízkou latencí disku pro virtuální počítače spuštěné I náročnými úlohy. Virtuální počítač (VM) disků, které používají úložiště Premium ukládat data do jednotek SSD (Solid-State Drive). Disky virtuálních počítačů vaší aplikace můžete migrovat na Azure Premium Storage využívají rychlosti a výkonu z těchto disků.

Funkce úložiště Premium:

* Disky úložiště Premium: Azure Premium Storage podporuje disky virtuálních počítačů, které lze připojit k řady DS, DSv2 nebo GS virtuálních počítačích Azure.
* Objekt Blob stránky Premium: Premium Storage podporuje objekty BLOB stránky Azure, které jsou používané pro udržení trvalé disky pro virtuální počítače Azure (VM).
* Místně redundantní úložiště Premium: Účet úložiště Premium pouze podporuje místně redundantní úložiště (LRS) jako možnost replikace a udržuje tři kopie dat v jedné oblasti.
* [Storage úrovně Premium](../windows/premium-storage.md)

## <a name="premium-storage-supported-vms"></a>Storage úrovně Premium podporované virtuální počítače
Premium Storage podporuje DS-series, DSv2-series, GS-series a Fs-series Azure virtuální počítače (VM). Můžete vytvořit disky úložiště Standard a Premium Storage úrovně Premium podporované virtuálních počítačů. Ale disky úložiště Premium nelze použít s řadou virtuálních počítačů, které nejsou kompatibilní úložiště Premium.

Toto jsou distribucí Linux jsme ověřit Storage úrovně Premium.

| Distribuce | Verze | Podporované jádra |
| --- | --- | --- |
| Ubuntu |12.04 |3.2.0-75.110+ |
| Ubuntu |14.04 |3.13.0-44.73+ |
| Debian |7.x, 8.x |3.16.7-ckt4-1+ |
| SLES |SLES 12 |3.12.36-38.1+ |
| SLES |SLES 11 SP4 |3.0.101-0.63.1+ |
| CoreOS |584.0.0+ |3.18.4+ |
| Centos |6.5, 6.6, 6.7, 7.0, 7.1 |3.10.0-229.1.2.el7+ |
| RHEL |6.8+, 7.2+ | |

## <a name="azure-file-storage"></a>Úložiště Azure File
Azure File storage nabízí sdílené složky v cloudu přes standardní protokol SMB. Soubory Azure můžete migrovat podnikové aplikace, které jsou závislé na souborové servery do Azure. Aplikace běžící v Azure můžete snadno připojit sdílené složky z virtuálních počítačů Azure s Linuxem. A s nejnovější verzí úložiště File, můžete také připojit sdílenou složku z lokální aplikace, který podporuje protokol SMB 3.0.  Protože sdílené složky jsou sdílené složky protokolu SMB, můžete k nim přistupovat přes standardní systém souborů rozhraní API.

Úložiště souborů je založen na stejnou technologii jako úložiště objektů Blob, Table a Queue, takže File storage nabízí dostupnosti, odolnost, škálovatelnosti a geografická redundance, která je integrována do úložiště Azure platformy. Podrobnosti o souboru cíle výkonnosti úložiště a omezení najdete v tématu Azure úložiště škálovatelnost a cíle výkonnosti.

* [Jak používat Azure File Storage s Linuxem](../../storage/files/storage-how-to-use-files-linux.md)

## <a name="hot-storage"></a>Horkého úložiště
Úroveň horkého úložiště Azure je optimalizovaná pro ukládání dat, která se využívají často.  Horkého úložiště je výchozí typ úložiště pro úložiště objektů blob.

## <a name="cool-storage"></a>Studeného úložiště
Úroveň studeného úložiště Azure je optimalizovaná pro ukládání dat, která se nevyužívají dlohotrvající. Příklad případů použití pro studenou zahrnují zálohy, mediální obsah, vědeckých dat, dodržování předpisů a archivní data. Obecně platí všechna data, která přistupuje málokdy je ideální kandidátem na studených úložiště.

|  | Úroveň horkého úložiště | Úroveň studeného úložiště |
|:--- |:---:|:---:|
| Dostupnost |99,9 % |99 % |
| Dostupnost (čtení RA-GRS) |99,99 % |99,9 % |
| Poplatky za používání |Vyšší poplatky za uložení |Nižší poplatky za uložení |
| Nižší přístup |Vyšší přístup | |
| a cena za transakci |a cena za transakci | |

## <a name="redundancy"></a>Redundance
Data ve vašem účtu úložiště Microsoft Azure se vždycky replikují, aby zajistila stálost a vysoká dostupnost, splňuje smlouvy SLA pro úložiště Azure i při krátkodobém výpadku hardwaru.

Při vytvoření účtu úložiště si musíte vybrat jednu z těchto možností replikace:

* Místně redundantní úložiště (LRS)
* Zónově redundantní úložiště (ZRS)
* Geograficky redundantní úložiště (GRS)
* Geograficky redundantní úložiště s přístupem pro čtení (RA-GRS)

### <a name="locally-redundant-storage"></a>Místně redundantní úložiště
Místně redundantní úložiště (LRS) replikuje data do oblasti, ve které jste vytvořili účet úložiště. Pokud chcete maximalizovat odolnost, každý požadavek směřovaný na data ve vašem účtu úložiště se replikují třikrát. Tyto tři repliky každý nacházet v samostatné selhání domén a domén upgradu.  Žádost o vrátí úspěšně pouze po jeho byla zapsána na všechny tři repliky.

### <a name="zone-redundant-storage"></a>Zónově redundantní úložiště
Zónově redundantní úložiště (ZRS) replikuje data mezi dva nebo tři zařízeními, v jedné oblasti nebo v rámci dvou oblastí, nabízí tak větší odolnost než LRS. Pokud má váš účet úložiště povolená ZRS, vaše data byla odolná i v případě selhání v některém zařízení.

### <a name="geo-redundant-storage"></a>Geograficky redundantní úložiště
Geograficky redundantní úložiště (GRS) replikuje data do sekundární oblasti, která je stovky miles od primární oblasti. Pokud má váš účet úložiště GRS povoleno, vaše data byla odolná i v případě výpadku dokončení místní nebo havárii, ve kterém není použitelná pro obnovení primární oblasti.

### <a name="read-access-geo-redundant-storage"></a>Geograficky redundantní úložiště s přístupem pro čtení
Geograficky redundantní úložiště s přístupem pro čtení (RA-GRS) maximalizuje dostupnost pro váš účet úložiště, tím, že poskytuje přístup jen pro čtení k datům v sekundárním umístění, kromě replikace rámci dvou oblastí poskytované GRS. V případě, že data stane nedostupným v primární oblasti, vaše aplikace může číst data z sekundární oblast.

Pro podrobné informace do úložiště Azure redundance najdete v tématu:

* [Účet replikace Azure Storage](../../storage/common/storage-redundancy.md)

## <a name="scalability"></a>Škálovatelnost
Azure Storage je nesmírně škálovatelná služba, která vám umožní ukládat a zpracovávat stovky terabajtů dat pro scénáře s velkým objemem dat, které jsou předpokladem pro vědeckou nebo finanční analýzu a aplikace pracující s médii. Nebo můžete ukládat malá množství dat, například pro firemní webové stránky. Ať už budete potřebovat cokoli, platit budete jen za uložená data. Azure Storage aktuálně poskytuje úložiště pro bilión jedinečných zákaznických objektů a v průměru zpracovává miliony požadavků za sekundu.

Pro účty úložiště standard storage: standardní účet úložiště má rychlost maximální celkový počet požadavků z 20 000 IOPS. Celkový počet IOPS na všech discích virtuálních počítačů v účtu úložiště úrovně Standard by neměl překročit toto omezení.

Pro účty úložiště premium: prémiový účet úložiště je míra maximální celková propustnost 50 GB/s. Celková propustnost všech disků virtuálních počítačů by neměla překročit toto omezení.

## <a name="availability"></a>Dostupnost
U zpracování žádosti o čtení dat z účtů geograficky redundantního úložiště s přístupem pro čtení (RA-GRS) garantujeme úspěšnost nejméně po 99,99 % času (99,9 % pro vrstvu přístupu Cool) za předpokladu, že neúspěšné pokusy o čtení dat z primární oblasti se zopakují v sekundární oblasti.

U zpracování žádostí o čtení dat z účtů místně redundantního úložiště (LRS), zónově redundantního úložiště (ZRS) a geograficky redundantního úložiště (GRS) garantujeme úspěšnost nejméně po 99,9 % času (99 % pro vrstvu přístupu Cool).

U zápisu do účtů místně redundantního úložiště (LRS), zónově redundantního úložiště (ZRS) a geograficky redundantního úložiště (GRS) a účtů geograficky redundantního úložiště s přístupem pro čtení (RA-GRS) garantujeme úspěšnost zpracování žádostí nejméně po 99,9 % času (99 % pro vrstvu přístupu Cool).

* [Azure SLA pro úložiště](https://azure.microsoft.com/support/legal/sla/storage/v1_1/)

## <a name="regions"></a>Oblasti
Azure je obecně dostupná v 30 oblastech po celém světě a má oznámeno plány pro 4 další oblasti. Geografické rozšíření je pro Azure prioritou, protože našim zákazníkům umožňuje dosahovat vyšších výkonů a naplňuje jejich požadavky a preference týkající se umístění dat.  Je azures nejnovější oblast spustíte v Německu.

Německo Microsoft Cloud poskytuje rozlišené možnost službám cloudu společnosti Microsoft již k dispozici v rámci Evropa, vytváření vyšší příležitosti pro inovace a hospodářského růstu pro vysoce regulovaná partnery a zákazníky v Německu, Evropské unie (EU) a Evropského sdružení volného obchodu (ESVO).

Zákaznická data v těchto nových datových centrech v Magdeburgu a Frankfurt, je spravovaný pod kontrolou zplnomocněnec dat, systémy T mezinárodní, nezávislé němčině společnosti a pobočky německých Telekom. Komerční cloudové služby společnosti Microsoft v těchto datových centrech dodržet němčině data zpracování předpisy a poskytnout další výběr jak a kde je zpracování dat zákazníků.

* [Mapa oblastí Azure](https://azure.microsoft.com/regions/)

## <a name="security"></a>Zabezpečení
Úložiště Azure poskytuje komplexní sadu funkcí zabezpečení, které společně umožňují vývojářům vytvářet aplikace zabezpečené. Účet úložiště, samotné lze zabezpečit pomocí řízení přístupu na základě Role a Azure Active Directory. Data lze zabezpečit během přenosu mezi aplikací a Azure pomocí šifrování na straně klienta, HTTPS nebo SMB 3.0. Data můžete nastavit k zašifrování automaticky při zápisu do služby Azure Storage pomocí šifrování služby úložiště (SSE). Disky operačního systému a dat používaných virtuálními počítači může být nastaven na šifrovat pomocí Azure Disk Encryption. Delegovaný přístup k datové objekty ve službě Azure Storage můžete udělit pomocí podpisy sdíleného přístupu.

### <a name="management-plane-security"></a>Zabezpečení roviny Management
Roviny správu se skládá z prostředky používat ke správě vašeho účtu úložiště. V této části se budeme mluvit o modelu nasazení Azure Resource Manager a jak používat řízení přístupu na základě Role (RBAC) pro řízení přístupu účty úložiště. Bude také faktorech mluvíme o správě klíče účtu úložiště a jak se je znovu vygenerovat.

### <a name="data-plane-security"></a>Zabezpečení dat roviny
V této části se podíváme povolení přístupu k objektům skutečná data ve vašem účtu úložiště, jako jsou objekty BLOB, soubory, fronty a tabulky, pomocí uložené zásady přístupu a podpisy sdíleného přístupu. Vám nabídneme SAS úrovně služby a SAS účtu úrovni. Dozvíte se jsme taky, jak omezit přístup ke konkrétní IP adresu (nebo rozsah IP adres), jak omezit protokol použitý k HTTPS a postup odvolat sdíleného přístupového podpisu bez čekání na vypršení platnosti.

## <a name="encryption-in-transit"></a>Šifrování během přenosu
Tato část popisuje, jak k zabezpečení dat při přenosu do nebo z Azure Storage. Budeme mluvit o doporučené použití protokolu HTTPS a šifrování používá protokol SMB 3.0 pro sdílené složky Azure File. Také jsme bude trvat podívejte se na straně klienta šifrování, které vám umožní šifrování dat před přenosu do úložiště v aplikaci klienta a k dešifrování dat po se přenáší z úložiště.

## <a name="encryption-at-rest"></a>Šifrování v klidovém stavu
Bude faktorech mluvíme o šifrování služby úložiště (SSE) a jak můžete ji povolit pro účet úložiště, což vede k objektů BLOB bloku, objekty BLOB stránky a doplňovací objekty BLOB se šifrují automaticky při zápisu do služby Azure Storage. Podíváme se také na tom, jak můžete použít Azure Disk Encryption a prozkoumat základní rozdíly a případů šifrování disku versus SSE versus šifrování na straně klienta. Podíváme se stručně na soulad se standardem FIPS pro USA Počítače, Government.

* [Průvodce zabezpečením služby Azure Storage](../../storage/common/storage-security-guide.md)

## <a name="temporary-disk"></a>Dočasné disku
Každý virtuální počítač obsahuje dočasné disk. Dočasné disku poskytuje krátkodobé úložiště pro aplikace a procesy a slouží k uložení pouze data, jako jsou soubory stránky nebo odkládacího souboru. Data na dočasné disku mohou být ztraceny při [údržby](manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) nebo když jste [znovu nasadit virtuální počítač](redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Během restartu standardní virtuální počítač by měl uchovat data na dočasné jednotce.

Na virtuální počítače s Linuxem, disk je obvykle **/dev/sdb** a je naformátován a připojit k **/mnt** Azure Linux Agent. Velikost dočasné disku se liší, na základě velikosti virtuálního počítače. Další informace najdete v tématu [velikosti virtuálních počítačů Linux](sizes.md).

Další informace o tom, jak Azure používá dočasným diskovým najdete v tématu [pochopení dočasné jednotce ve virtuálních počítačích Microsoft Azure](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

## <a name="cost-savings"></a>Úspora nákladů
* [Náklady na úložiště](https://azure.microsoft.com/pricing/details/storage/)
* [Kalkulačky úložiště náklady.](https://azure.microsoft.com/pricing/calculator/?service=storage)

## <a name="storage-limits"></a>Limity úložiště
* [Omezení služby úložiště](../../azure-subscription-service-limits.md#storage-limits)
