---
title: "Přehled virtuálních počítačů s Linuxem v Azure | Dokumentace Microsoftu"
description: "Popis služeb Azure Compute, Storage a síťové služby Azure s použitím virtuálních počítačů s Linuxem."
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: rickstercdn
manager: timlt
editor: 
ms.assetid: 7965a80f-ea24-4cc2-bc43-60b574101902
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: overview
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/14/2016
ms.author: rclaus
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: cef9abddf980c695040e99995eb325eeb182fad4
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/17/2017
---
# <a name="azure-and-linux"></a>Azure a Linux
Microsoft Azure je rostoucí kolekce integrovaných veřejných cloudových služeb včetně analýz, virtuálních počítačů, databází, mobilních a síťových služeb, úložiště a webu &mdash; je tak ideální pro hostování vašich řešení.  Microsoft Azure poskytuje škálovatelnou výpočetní platformu, s kterou můžete platit jenom za to, co používáte a když to potřebujete – nemusíte tak investovat do hardwaru místně ve své firmě.  Platforma Azure dokáže flexibilně reagovat na vaše potřeby. Můžete tak libovolně škálovat kapacitu (vertikálně i horizontálně) pro svá řešení podle požadavků svých klientů.

Pokud už znáte různé funkce Amazon AWS, můžete prozkoumat [dokument věnovaný mapování definic](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/) Azure oproti AWS.

## <a name="regions"></a>Oblasti
Prostředky Microsoft Azure jsou distribuované mezi několik geografických oblastí po celém světě.  „Oblast“ představuje několik datacenter v jedné zeměpisné oblasti.  Po celém světě máme k dispozici 34 obecně dostupných oblastí a další 4 oblasti jsou oznámené. Vzhledem k tomu, že stále rozšiřujeme naše globální pokrytí, udržujeme aktualizovaný seznam stávajících a nově oznámených oblastí.

* [Oblasti Azure](https://azure.microsoft.com/regions/)

## <a name="availability"></a>Dostupnost
Oznámili jsme špičkovou smlouvu o úrovni služeb (SLA) na 99,9% dostupnost pro samostatné instance virtuálních počítačů za předpokladu, že virtuální počítač nasadíte se službou Storage úrovně Premium pro všechny disky.  Aby se na vaše nasazení vztahovala standardní záruka 99,95% dostupnosti virtuálního počítače podle smlouvy SLA, stále je nutné nasadit alespoň dva virtuální počítače, které vaši úlohu spouští v rámci skupiny dostupnosti. Tím se zajistí distribuce vašich virtuálních počítačů mezi více domén selhání v našich datacentrech a jejich nasazení na hostitele s různými časovými obdobími údržby. Úplná smlouva [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/) vysvětluje garantovanou dostupnost Azure jako celku.

## <a name="managed-disks"></a>Managed Disks

Služba Managed Disks se na pozadí stará o vytvoření a správu účtu služby Azure Storage za vás a zajišťuje, že si nemusíte dělat starosti s omezením škálovatelnosti účtu úložiště. Jednoduše zadáte velikost disku a úroveň výkonu (Standard nebo Premium) a Azure za vás disk vytvoří a postará se o jeho správu. S využitím úložiště si nemusíte dělat starosti ani v případě, že přidáváte disky nebo vertikálně navyšujete a snižujete kapacitu virtuálního počítače. Pokud vytváříte nové virtuální počítače, [pomocí Azure CLI 2.0](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo webu Azure Portal vytvořte virtuální počítače se spravovanými disky operačního systému a datovými disky. Pokud máte virtuální počítače s nespravovanými disky, můžete [virtuální počítače převést na využívání služby Managed Disks](convert-unmanaged-to-managed-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

V jednom účtu úložiště na oblast Azure můžete také spravovat vlastní image a vytvářet pomocí nich stovky virtuálních počítačů v rámci stejného předplatného. Další informace o službě Managed Disks najdete v tématu [Přehled služby Managed Disks](../windows/managed-disks-overview.md).

## <a name="azure-virtual-machines--instances"></a>Virtuální počítače Azure a instance
Microsoft Azure podporuje spouštění mnoha oblíbených distribucí Linuxu, které poskytuje a udržuje řada partnerů.  Na webu Azure Marketplace najdete distribuce, jako jsou Red Hat Enterprise, CentOS, SUSE Linux Enterprise, Debian, Ubuntu, CoreOS, RancherOS, FreeBSD a další. Aktivně spolupracujeme s různými komunitami okolo Linuxu, abychom ještě více rozšířili seznam [distribucí Linuxu schválených pro Azure](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Pokud vaše upřednostňovaná distribuce Linuxu aktuálně není v galerii, můžete pro virtuální počítač použít vlastní distribuci Linuxu tak, že [vytvoříte a nahrajete VHD s Linuxem v Azure](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Virtuální počítače Azure umožňují pružně nasadit řadu různých výpočetních řešení. Můžete nasadit prakticky jakoukoli úlohu v jakémkoli jazyce na téměř jakýkoli operační systém – Windows, Linux nebo vlastní systém vytvořený některým z našich partnerů, jejichž seznam se stále rozšiřuje. Stále jste nenašli, co hledáte?  Buďte bez obav – můžete používat také své vlastní image z místního prostředí.

## <a name="vm-sizes"></a>Velikosti virtuálních počítačů
Při nasazování virtuálního počítače v Azure si vyberete velikost virtuálního počítače z jedné z našich řad velikostí, která je vhodná pro vaši úlohu. Velikost také určuje výpočetní výkon, paměť a kapacitu úložiště virtuálního počítače. Účtuje se vám doba, po kterou je virtuální počítač spuštěný a spotřebovává přidělené prostředky. Úplný seznam [velikostí virtuálních počítačů](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Tady je několik základních pokynů pro výběr velikosti virtuálního počítače z jedné z našich řad (A, D, DS, G a GS).
* Virtuální počítače řady A-series jsou naše cenově přijatelné virtuální počítače na základní úrovni pro malé úlohy a scénáře vývoje/testování. Jsou široce dostupné ve všech oblastech a můžou se připojit ke všem standardním prostředkům dostupným pro virtuální počítače a používat je.
* Velikosti řady A-series (A8 až A11) představují speciální konfigurace s velkým výpočetním výkonem a jsou vhodné pro cloudové aplikace vysokovýkonného výpočetního prostředí.
* Virtuální počítače D-series slouží ke spouštění aplikací, které potřebují vyšší výpočetní výkon a krátkodobý vysoký diskový výkon. Virtuální počítače řady D-series mají rychlejší procesory, vyšší poměr paměti na virtuální procesor a jednotky SSD (solid-state-drive) pro dočasný disk.
* Řada Dv2-series je nejnovější verzí naší řady D-series, která zahrnuje výkonnější procesory. Procesor v Dv2-series je asi o 35 % rychlejší než procesor v D-series. Využívá nejnovější generaci procesorů Intel Xeon® E5-2673 v3 (Haskell) s frekvencí 2,4 GHz, kterou může technologie Intel Turbo Boost 2.0 zvýšit až na 3,2 GHz. Řada Dv2-series má stejnou konfiguraci paměti a disku jako řada D.
* Virtuální počítače G-series nabízejí nejvíce paměti a spouštějí se na hostitelích s procesory Intel Xeon E5 V3.

Poznámka: Virtuální počítače DS-series a GS-series mají přístup ke službě Storage úrovně Premium, což je naše vysoce výkonné úložiště využívající disky SSD s nízkou latencí pro úlohy náročné na vstupně-výstupní operace. V některých oblastech je dostupná služba Storage úrovně Premium. Podrobnosti najdete tady:

* [Storage úrovně Premium: Vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](../windows/premium-storage.md)

## <a name="automation"></a>Automation
K dosažení správné kultury DevOps musí být veškerá infrastruktura tvořená kódem.  Když je veškerá infrastruktura v kódu, je možné ji snadno vytvořit znovu (servery Phoenix).  Azure spolupracuje se všemi hlavními nástroji pro automatizaci, jako jsou Ansible, Chef, SaltStack a Puppet.  Azure obsahuje také vlastní nástroj pro automatizaci:

* [Šablony Azure](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Azure VMAccess](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Azure postupně zavádí podporu nástroje [cloud-init](http://cloud-init.io/) ve většině distribucí Linuxu, které ho podporují.  Aktuálně se s povoleným nástrojem cloud-init ve výchozím nastavení nasazují virtuální počítače s Ubuntu od firmy Canonical.  Red Hats RHEL, CentOS i Fedora podporují cloud-init, ale v imagích Azure, které udržuje RedHat, není nástroj cloud-init nainstalovaný.  Pokud chcete používat cloud-init v operačních systémech rodiny RedHat, musíte vytvořit vlastní image s nainstalovaným nástrojem cloud-init.

* [Použití cloud-init na virtuálních počítačích Azure s Linuxem](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="quotas"></a>Kvóty
Každé předplatné Azure má nastavené výchozí kvóty, které můžou ovlivnit nasazení velkého počtu virtuálních počítačů pro váš projekt. Aktuální limit jednoho předplatného je 20 virtuálních počítačů na oblast.  Limity kvót je možné rychle a snadno navýšit vyplněním lístku podpory s žádostí o navýšení limitu.  Další podrobnosti o limitech kvót:

* [Omezení služeb v předplatném Azure](../../azure-subscription-service-limits.md)

## <a name="partners"></a>Partneři
Microsoft úzce spolupracuje s partnery na zajištění aktualizace a optimalizace dostupných imagí pro modul runtime Azure.  Další informace o našich partnerech najdete níže na příslušných stránkách na marketplace.

* Linux v Azure – [Schválené distribuce](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* SUSE – [Azure Marketplace – SUSE Linux Enterprise Server](https://azuremarketplace.microsoft.com/en-us/marketplace/apps?search=%27SUSE%27)
* RedHat – [Azure Marketplace – RedHat Enterprise Linux 7.2](https://azure.microsoft.com/marketplace/partners/redhat/redhatenterpriselinux72/)
* Canonical – [Azure Marketplace – Ubuntu Server 16.04 LTS](https://azure.microsoft.com/marketplace/partners/canonical/ubuntuserver1604lts/)
* Debian –[Azure Marketplace – Debian 8 „Jessie“](https://azure.microsoft.com/marketplace/partners/credativ/debian8/)
* FreeBSD – [Azure Marketplace – FreeBSD 10.3](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/)
* CoreOS – [Azure Marketplace – CoreOS (stabilní verze)](https://azure.microsoft.com/marketplace/partners/coreos/coreosstable/)
* RancherOS – [Azure Marketplace – RancherOS](https://azure.microsoft.com/marketplace/partners/rancher/rancheros/)
* Bitnami – [Knihovna Bitnami pro Azure](https://azure.bitnami.com/)
* Mesosphere – [Azure Marketplace – Mesosphere DC/OS v Azure](https://azure.microsoft.com/marketplace/partners/mesosphere/dcosdcos/)
* Docker – [Azure Marketplace – Azure Container Service s Dockerem Swarm](https://azure.microsoft.com/marketplace/partners/microsoft/acsswarms/)
* Jenkins – [Azure Marketplace – Platforma CloudBees Jenkins](https://azure.microsoft.com/marketplace/partners/cloudbees/jenkins-platformjenkins-platform/)

## <a name="getting-started-with-linux-on-azure"></a>Začínáme s Linuxem v Azure
Abyste mohli začít používat Azure, potřebujete účet Azure, nainstalované Azure CLI a pár veřejného a privátního klíče SSH.

### <a name="sign-up-for-an-account"></a>Registrace účtu
Prvním krokem při používání cloudu Azure je registrace účtu Azure.  Začněte tím, že přejdete na stránku [Registrace účtu Azure](https://azure.microsoft.com/pricing/free-trial/).

### <a name="install-the-cli"></a>Instalace rozhraní příkazového řádku
S novým účtem Azure můžete rovnou začít pracovat pomocí webu Azure Portal, což je webový panel pro správu.  Pokud chcete spravovat cloud Azure přes příkazový řádek, nainstalujte `azure-cli`.  Nainstalujte na svou pracovní stanici se systémem Mac nebo Linux [Azure CLI 2.0](/cli/azure/install-azure-cli).

### <a name="create-an-ssh-key-pair"></a>Vytvoření páru klíčů SSH
Nyní máte účet Azure, webový portál Azure a Azure CLI.  Dalším krokem je vytvoření páru klíčů SSH, který slouží k připojení k Linuxu přes SSH bez použití hesla.  [Vytvořte v systému Linux nebo Mac klíče SSH](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) a umožněte přihlašování bez hesla a lepší zabezpečení.

### <a name="create-a-vm-using-the-cli"></a>Vytvoření virtuálního počítače pomocí rozhraní příkazového řádku
Vytvoření virtuálního počítače s Linuxem pomocí rozhraní příkazového řádku představuje rychlý způsob nasazení virtuálního počítače, aniž byste opustili terminál, ve kterém pracujete.  Vše, co můžete zadat na webovém portálu, je k dispozici i v příkazovém řádku prostřednictvím příznaku nebo přepínače.  

* [Vytvoření virtuálního počítače s Linuxem pomocí rozhraní příkazového řádku](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="create-a-vm-in-the-portal"></a>Vytvoření virtuálního počítače na portálu
Vytvoření virtuálního počítače na webovém portálu Azure představuje snadný způsob, jak se můžete proklikat různými možnostmi až k nasazení.  Místo používání příznaků nebo přepínačů v příkazovém řádku můžete zobrazit pěkné webové rozhraní s různými možnostmi a nastaveními.  Vše, co je k dispozici prostřednictvím rozhraní příkazového řádku, je dostupné také na portálu.

* [Vytvoření virtuálního počítače s Linuxem pomocí portálu](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="login-using-ssh-without-a-password"></a>Přihlášení bez hesla pomocí SSH
Virtuální počítač je teď spuštěný v Azure a vy jste připraveni se k němu přihlásit.  Používání hesel při přihlašování přes SSH je riskantní a časově náročné.  Nejbezpečnější a zároveň nejrychlejší způsob přihlášení představuje použití klíčů SSH.  Při vytváření virtuálního počítače s Linuxem prostřednictvím portálu nebo rozhraní příkazového řádku máte na výběr dvě metody ověřování.  Pokud pro SSH zvolíte heslo, Azure nakonfiguruje virtuální počítač tak, aby umožňoval přihlašování pomocí hesel.  Pokud se rozhodnete použít veřejný klíč SSH, Azure nakonfiguruje virtuální počítač tak, aby umožňoval přihlašování pouze pomocí klíčů SSH a zakázal přihlašování pomocí hesel. Pokud chcete svůj virtuální počítač s Linuxem zabezpečit tím, že povolíte přihlašování pouze pomocí klíče SSH, při vytváření virtuálního počítače na portálu nebo v rozhraní příkazového řádku použijte možnost veřejného klíče SSH.

## <a name="related-azure-components"></a>Související komponenty Azure
## <a name="storage"></a>Úložiště
* [Úvod do Microsoft Azure Storage](../../storage/common/storage-introduction.md)
* [Přidání disku do virtuálního počítače s Linuxem pomocí Azure CLI](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Postup připojení datového disku k virtuálnímu počítači s Linuxem na webu Azure Portal](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="networking"></a>Sítě
* [Přehled služby Virtual Network](../../virtual-network/virtual-networks-overview.md)
* [IP adresy v Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md)
* [Otevření portů k virtuálnímu počítači s Linuxem v Azure](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Vytvoření plně kvalifikovaného názvu domény (FQDN) na webu Azure Portal](portal-create-fqdn.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="containers"></a>Kontejnery
* [Virtuální počítače a kontejnery v Azure](containers.md)
* [Úvod do služby Azure Container Service](../../container-service/container-service-intro.md)
* [Nasazení clusteru Azure Container Service](../../container-service/dcos-swarm/container-service-deployment.md)

## <a name="next-steps"></a>Další kroky
Nyní máte přehled o Linuxu v Azure.  Můžete se pustit do dalšího kroku a vytvořit několik virtuálních počítačů.

* [Prozkoumejte náš rozrůstající se seznam ukázkových skriptů pro běžné úlohy prostřednictvím Azure CLI](cli-samples.md).
