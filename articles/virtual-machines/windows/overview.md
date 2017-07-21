---
title: "Přehled virtuálních počítačů s Windows | Dokumentace Microsoftu"
description: "Zjistěte, jak vytvářet a spravovat virtuální počítače s Windows v Azure."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: fbae9c8e-2341-4ed0-bb20-fd4debb2f9ca
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: davidmu
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 5edc47e03ca9319ba2e3285600703d759963e1f3
ms.openlocfilehash: 7ddd9244558479f1fc77c0a9f3d02d0d3b95ca9f
ms.contentlocale: cs-cz
ms.lasthandoff: 05/31/2017

---
# Přehled virtuálních počítačů s Windows v Azure
<a id="overview-of-windows-virtual-machines-in-azure" class="xliff"></a>

Azure Virtual Machines (VM) je jedním z několika typů [škálovatelných výpočetních prostředků na vyžádání](../../app-service-web/choose-web-site-cloud-service-vm.md), které Azure nabízí. Obvykle zvolíte virtuální počítač, když potřebujete větší kontrolu nad výpočetním prostředí, než nabízí jiné možnosti. Tento článek obsahuje informace o tom, co byste měli zvážit před vytvořením virtuálního počítače, jak ho vytvořit a jak ho spravovat.

Virtuální počítač Azure vám nabídne flexibilitu virtualizace bez nutnosti zakoupení a údržby fyzického hardwaru, na kterém běží. Nevyhnete se však údržbě virtuálního počítače prováděním úloh, jako jsou konfigurace, aplikování oprav chyb a instalace softwaru, který na něm běží.

Virtuální počítače Azure lze použít různými způsoby. Tady je několik příkladů:

* **Vývoj a testování** – Virtuální počítače Azure nabízejí rychlý a snadný způsob, jak vytvořit počítač s konkrétní konfigurací pro kódování a testování aplikací.
* **Aplikace v cloudu** – Protože poptávka po aplikaci může kolísat, z ekonomického hlediska může mít smysl spouštět ji na virtuálním počítači v Azure. Za další virtuální počítače platíte, když je potřebujete, a když ne, tak je vypnete.
* **Rozšířené datové centrum** – Virtuální počítače ve virtuální síti Azure můžete snadno připojit k síti vaší organizace.

Počet virtuálních počítačů, které vaše aplikace používá, lze vertikálně nebo horizontálně navýšit pro splnění vašich požadavků.

## Co je třeba zvážit před vytvořením virtuálního počítače?
<a id="what-do-i-need-to-think-about-before-creating-a-vm" class="xliff"></a>
Při sestavování infrastruktury aplikace v Azure vždy existuje velké množství [aspektů návrhu](infrastructure-virtual-machine-guidelines.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Než začnete, je důležité zvážit následující aspekty virtuálního počítače:

* Názvy prostředků vaší aplikace
* Umístění, kam se ukládají prostředky
* Velikost virtuálního počítače
* Maximální počet virtuálních počítačů, které lze vytvořit
* Operační systém, který běží na virtuálním počítači
* Konfigurace virtuálního počítače po jeho spuštění
* Související prostředky, které virtuální počítač potřebuje

### Pojmenování
<a id="naming" class="xliff"></a>
Virtuální počítač má přiřazený [název](infrastructure-naming-guidelines.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a název počítače má nakonfigurovaný jako součást operačního systému. Název virtuálního může být až 15 znaků dlouhý.

Pokud k vytvoření disku operačního systému použijete Azure, název počítače a název virtuálního počítače budou stejné. Pokud [nahrajete a použijete vlastní image](upload-generalized-managed.md), která obsahuje dříve nakonfigurovaný operační systém, a použijete ji k vytvoření virtuálního počítače, mohou se tyto názvy lišit. Když nahráváte vlastní soubor s imagí, doporučujeme použít stejný název počítače v operačním systému jako název virtuálního počítače.

### Umístění
<a id="locations" class="xliff"></a>
Všechny prostředky vytvořené v Azure jsou distribuované mezi několik [geografických oblastí](https://azure.microsoft.com/regions/) po celém světě. Při vytváření virtuálního počítače se oblast obvykle nazývá **umístění**. U virtuálního počítače umístění určuje, kde jsou uloženy virtuální pevné disky.

Tato tabulka ukazuje několik způsobů, jak můžete získat seznam dostupných umístění.

| Metoda | Popis |
| --- | --- |
| portál Azure |Při vytváření virtuálního počítače vyberte umístění ze seznamu. |
| Azure PowerShell |Použijte příkaz [Get-AzureRmLocation](/powershell/module/azurerm.resources/get-azurermlocation). |
| REST API |Použijte operaci [Vypsat umístění](https://docs.microsoft.com/rest/api/resources/subscriptions#Subscriptions_ListLocations). |

### Velikost virtuálního počítače
<a id="vm-size" class="xliff"></a>
[Velikost](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) virtuálního počítače, který použijete, se určuje podle úlohy, kterou chcete spustit. Velikost, kterou vyberete, pak určuje další faktory, jako například výpočetní výkon, paměť a kapacitu úložiště. Azure nabízí širokou škálu velikostí, které podporují mnoho typů použití.

Azure účtuje [hodinovou cenu](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) na základě velikosti virtuálního počítače a na jeho operačním systému. V případě neúplných hodin Azure účtuje jenom využité minuty. Služba Storage je oceněna a účtována samostatně.

### Omezení virtuálního počítače
<a id="vm-limits" class="xliff"></a>
Vaše předplatné má nastavené výchozí [kvóty](../../azure-subscription-service-limits.md), které mohou ovlivnit nasazení velkého počtu virtuálních počítačů pro váš projekt. Aktuální limit jednoho předplatného je 20 virtuálních počítačů na oblast. Limity lze navýšit tak, že vyplníte lístek podpory s žádostí o navýšení.

### Disky a image operačních systémů
<a id="operating-system-disks-and-images" class="xliff"></a>
Virtuální počítače používají [virtuální pevné disky (VHD)](../../storage/storage-about-disks-and-vhds-windows.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), na které ukládají svůj operační systém (OS) a data. Virtuální pevné disky se používají i pro image, ze kterých si můžete nainstalovat operační systém. 

Azure poskytuje mnoho [imagí na webu Marketplace](https://azure.microsoft.com/marketplace/virtual-machines/) s různými verzemi a typy operačních systémů Windows Server, které můžete použít. Image pořízené na webu Marketplace jsou identifikované vydavatelem image, nabídkou, skladovou jednotkou (SKU) a verzí (verze je obvykle uvedena jako poslední). 

Tato tabulka ukazuje několik způsobů, jak můžete najít informace o imagi.

| Metoda | Popis |
| --- | --- |
| portál Azure |Hodnoty se pro vás zadají automaticky, když vyberete image, která se má použít. |
| Azure PowerShell |[Get-AzureRMVMImagePublisher](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.5.0/get-azurermvmimagepublisher) -Location "umístění"<BR>[Get-AzureRMVMImageOffer](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.5.0/get-azurermvmimageoffer) -Location "umístění" -Publisher "název_vydavatele"<BR>[Get-AzureRMVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) -Location "umístění" -Publisher "název_vydavatele" -Offer "název_nabídky" |
| Rozhraní REST API |[Vypsat vydavatele imagí](https://docs.microsoft.com/rest/api/compute/platformimages/platformimages-list-publishers)<BR>[Vypsat nabídky imagí](https://docs.microsoft.com/rest/api/compute/platformimages/platformimages-list-publisher-offers)<BR>[Vypsat skladové jednotky (SKU) imagí](https://docs.microsoft.com/rest/api/compute/platformimages/platformimages-list-publisher-offer-skus) |

Můžete se rozhodnout [nahrát a použít vlastní image](upload-generalized-managed.md#upload-the-vhd-to-your-storage-account). Pokud tak učiníte, název vydavatele, nabídka a skladová jednotka (SKU) se nepoužijí.

### Rozšíření
<a id="extensions" class="xliff"></a>
[Rozšíření](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) virtuálního počítače poskytují vašemu virtuálnímu počítači další schopnosti prostřednictvím konfigurace po nasazení a automatizovaných úloh.

Pomocí rozšíření můžete provádět tyto běžné úlohy:

* **Spuštění vlastních skriptů** – [Rozšíření vlastních skriptů](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) pomáhá konfigurovat úlohy na virtuálním počítači spuštěním vašeho skriptu při zřízení virtuálního počítače.
* **Nasazení a správa konfigurací** – [Rozšíření Konfigurace požadovaného stavu prostředí PowerShell](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) pomáhá nastavit konfiguraci požadovaného stavu na virtuálním počítači za účelem správy konfigurací a prostředí.
* **Shromažďování diagnostických dat** – [Rozšíření Azure Diagnostics](extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) pomáhá konfigurovat virtuální počítač pro shromažďování diagnostických dat, která lze použít k monitorování stavu aplikace.

### Související prostředky
<a id="related-resources" class="xliff"></a>
Prostředky v této tabulce používá virtuální počítač a je nutné, aby existovaly nebo byly vytvořeny při vytváření virtuálního počítače.

| Prostředek | Požaduje se | Popis |
| --- | --- | --- |
| [Skupina prostředků](../../azure-resource-manager/resource-group-overview.md) |Ano |Virtuální počítač musí být součástí skupiny prostředků. |
| [Účet úložiště](../../storage/storage-create-storage-account.md) |Ano |Virtuální počítač potřebuje účet úložiště k ukládání svých virtuálních pevných disků. |
| [Virtuální síť](../../virtual-network/virtual-networks-overview.md) |Ano |Virtuální počítač musí být členem virtuální sítě. |
| [Veřejná IP adresa](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) |Ne |Virtuální počítač může mít přiřazenou veřejnou IP adresu pro umožnění vzdáleného přístupu. |
| [Síťové rozhraní](../../virtual-network/virtual-network-network-interface.md) |Ano |Virtuální počítač potřebuje síťové rozhraní ke komunikaci v síti. |
| [Datové disky](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |Ne |Virtuální počítač může zahrnovat datové disky pro rozšíření možností úložiště. |

## Jak vytvořím svůj první virtuální počítač?
<a id="how-do-i-create-my-first-vm" class="xliff"></a>
Virtuální počítač můžete vytvořit několika způsoby. Způsob, který zvolíte, závisí na prostředí, ve kterém se nacházíte. 

Tato tabulka obsahuje informace, které vám pomůžou začít vytvářet virtuální počítač.

| Metoda | Článek |
| --- | --- |
| portál Azure |[Vytvoření virtuálního počítače s Windows pomocí portálu](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| Šablony |[Vytvoření virtuálního počítače s Windows pomocí šablony Resource Manageru](ps-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| Azure PowerShell |[Vytvoření virtuálního počítače s Windows pomocí prostředí PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| Klientské sady SDK |[Nasazení prostředků Azure pomocí jazyka C#](csharp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| Rozhraní REST API |[Vytvořit nebo aktualizovat virtuální počítač](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-create-or-update) |

Věříte, že se to nikdy nestane, ale občas dojde k nějaké chybě. Pokud taková situace nastane u vás, podívejte se na informace v tématu [Řešení potíží v nasazení Resource Manager s vytvářením virtuálního počítače s Windows v Azure](troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## Jak můžu spravovat vytvořený virtuální počítač?
<a id="how-do-i-manage-the-vm-that-i-created" class="xliff"></a>
Virtuální počítače můžete spravovat pomocí portálu (přes prohlížeč), nástrojů příkazového řádku s podporou pro skriptování, nebo přímo prostřednictvím rozhraní API. Mezi obvyklé úlohy správy, které můžete provádět, patří získávání informací o virtuálním počítači, připojování k virtuálnímu počítači, správa dostupnosti a provádění zálohování.

### Získání informací o virtuálním počítači
<a id="get-information-about-a-vm" class="xliff"></a>
Tato tabulka uvádí některé způsoby, jakými můžete získat informace o virtuálním počítači.

| Metoda | Popis |
| --- | --- |
| portál Azure |V nabídce centra klikněte na **Virtual Machines** a ze seznamu vyberte virtuální počítač. V okně příslušného virtuálního počítače máte přístup k souhrnným informacím, hodnotám nastavení a metrikám monitorování. |
| Azure PowerShell |Informace o použití PowerShellu ke správě virtuálních počítačů najdete v tématu popisujícím [vytvoření a správu virtuálních počítačů Windows pomocí modulu Azure PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). |
| REST API |Pro získání informací o virtuálním počítači použijte operaci [Získat informace o virtuálním počítači](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-get). |
| Klientské sady SDK |Informace o použití jazyka C# ke správě virtuálních počítačů najdete v tématu [Správa virtuálních počítačů Azure pomocí Resource Manageru a jazyka C#](csharp-manage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). |

### Přihlášení k virtuálnímu počítači
<a id="log-on-to-the-vm" class="xliff"></a>
Pomocí tlačítka [Připojit](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) na webu Azure Portal spouštíte relaci Vzdálené plochy (protokol RDP). Při pokusu o použití vzdáleného připojení se občas může něco pokazit. Pokud taková situace nastane u vás, podívejte se na informace nápovědy v tématu [Řešení potíží s připojením ke vzdálené ploše virtuálního počítače Azure s Windows](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### Správa dostupnosti
<a id="manage-availability" class="xliff"></a>
Je důležité, abyste porozuměli tomu, jak pro svoji aplikaci [zajistit vysokou dostupnost](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Tato konfigurace zahrnuje vytvoření více virtuálních počítačů pro zajištění, že je alespoň jeden spuštěný.

Aby se na vaše nasazení vztahovala záruka 99,95% dostupnosti virtuálního počítače podle smlouvy SLA, je nutné nasadit alespoň dva virtuální počítače, které vaši úlohu spouští v rámci [skupiny dostupnosti](infrastructure-availability-sets-guidelines.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Tato konfigurace zajišťuje distribuci vašich virtuálních počítačů mezi více domén selhání a jejich nasazení na hostitele s různými časovými obdobími údržby. Úplná smlouva [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/) vysvětluje garantovanou dostupnost Azure jako celku.

### Zálohování virtuálního počítače
<a id="back-up-the-vm" class="xliff"></a>
[Trezor služby Recovery Services](../../backup/backup-introduction-to-azure-backup.md) slouží k ochraně dat a assetů ve službě Backup a službách Azure Site Recovery. Pomocí trezoru služby Recovery Services můžete [nasadit a spravovat zálohy virtuálních počítačů nasazených Resource Managerem pomocí prostředí PowerShell](../../backup/backup-azure-vms-automation.md). 

## Další kroky
<a id="next-steps" class="xliff"></a>
* Pokud máte v úmyslu pracovat s virtuálními počítači s Linuxem, podívejte se na téma [Azure a Linux](../linux/overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Další informace o pokynech ohledně nastavení infrastruktury najdete v tématu [Průvodce ukázkovou infrastrukturou Azure](infrastructure-example.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Je třeba dodržovat [Osvědčené postupy pro spuštění virtuálního počítače s Windows v Azure](guidance-compute-single-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


