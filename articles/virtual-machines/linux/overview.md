---
title: "Přehled virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Popisuje Azure Compute, úložiště a síťové služby s virtuální počítače s Linuxem."
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
ms.openlocfilehash: ae4b8f423489bf417f1086368db9b1043cd7f396
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/02/2017
---
# <a name="azure-and-linux"></a>Azure a Linux
Microsoft Azure je rostoucí kolekce integrovaných veřejné cloudové služby, včetně analýzy, virtuálních počítačů, databází, mobilní a sítě, úložiště a webové&mdash;ideální pro hostování vašeho řešení.  Microsoft Azure poskytuje škálovatelnou výpočetní platformu, s kterou můžete platit jenom za to, co používáte a když to potřebujete – nemusíte tak investovat do hardwaru místně ve své firmě.  Platforma Azure dokáže flexibilně reagovat na vaše potřeby. Můžete tak libovolně škálovat kapacitu (vertikálně i horizontálně) pro svá řešení podle požadavků svých klientů.

Pokud jste se seznámili s k různým funkcím služby Amazon na AWS, můžete zkontrolovat Azure vs AWS [definice mapování dokumentu](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/).

## <a name="regions"></a>Oblasti
Prostředky Microsoft Azure jsou rozmístěny v několika zeměpisných oblastech po celém světě.  "region" představuje více datových centrech v jedné zeměpisné oblasti.  Máme 34 oblastech je všeobecně dostupná po celém světě s další 4 oblastí, oznámí. Protože jsme se stále rozšiřuje naše globální pokrytí - uchováváme, že aktualizovaný seznam stávajících a nově oznámeno oblasti.

* [Oblasti Azure](https://azure.microsoft.com/regions/)

## <a name="availability"></a>Dostupnost
Jsme oznámila odvětví úvodní jedna instance virtuálního počítače smlouvy o úrovni služeb 99,9 %, pokud že nasadíte virtuální počítač s storage úrovně premium pro všechny disky.  Aby nasazení tak, aby se dosáhlo nároku na 99,95 % naše standardní smlouvu o úrovni služeb virtuálních počítačů budete muset dál nasazovat minimálně dva virtuální počítače spuštěné úlohy v rámci skupiny dostupnosti. Tím bude zajištěno, virtuální počítače jsou rozmístěny v několika domén selhání v našich datacentrech a také nasadit na hostitele se systémem windows odlišné údržby. Úplná smlouva [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/) vysvětluje garantovanou dostupnost Azure jako celku.

## <a name="managed-disks"></a>Managed Disks

Spravované disky obslužných rutin Azure Storage vytváření účtů a správu na pozadí pro vás a zajistí, že nemusíte si dělat starosti o limitech škálovatelnosti účtu úložiště. Jednoduše zadejte velikost disku a úroveň výkonu (Standard nebo Premium) a Azure vytváří a spravuje disku za vás. S využitím úložiště si nemusíte dělat starosti ani v případě, že přidáváte disky nebo vertikálně navyšujete a snižujete kapacitu virtuálního počítače. Pokud vytváříte nové virtuální počítače, [pomocí Azure CLI 2.0](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo portálu Azure k vytvoření virtuálních počítačů s spravované operačního systému a datové disky. Pokud máte virtuální počítače s nespravované disky, můžete [převést virtuální počítače na s spravované disky](convert-unmanaged-to-managed-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

V jednom účtu úložiště na oblast Azure můžete také spravovat vlastní image a vytvářet pomocí nich stovky virtuálních počítačů v rámci stejného předplatného. Další informace o službě Managed Disks najdete v tématu [Přehled služby Managed Disks](../windows/managed-disks-overview.md).

## <a name="azure-virtual-machines--instances"></a>Virtuální počítače Azure & instance
Microsoft Azure podporuje spuštění počet oblíbených Linuxových distribucích spravovaný společností číslo partnerů.  Zjistíte distribuce například Red Hat Enterprise, CentOS, SUSE Linux Enterprise, Debian, Ubuntu, CoreOS, RancherOS, FreeBSD a další obsah v Azure Marketplace. Jsme aktivně pracovat různé komunit Linux přidat i více typů k [schválené pro Azure distribucích systému Linux](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) seznamu.

Pokud vaše upřednostňované distro Linux výběru není momentálně nachází v galerii, vám může "přineste vlastní Linux" virtuální počítač podle [vytvoření a nahrání virtuálního pevného disku Linux v Azure](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Virtuální počítače Azure umožňuje nasazovat širokou škálu řešení výpočetní agilní způsobem. Můžete nasadit prakticky jakoukoli úlohu a jakýkoli jazyk na téměř všechny operační systém – Windows, Linux, nebo vytvořili vlastní z jednoho z našich rostoucí seznamu partnerů. Není stále zobrazen, co hledáte?  Nemusíte si dělat starosti – může také přinést si vlastní Image z místní.

## <a name="vm-sizes"></a>Velikosti virtuálních počítačů
Při nasazení virtuálního počítače v Azure, kterou budete vybrat velikost virtuálního počítače do jedné z našich řady velikostí, která je vhodné pro vaši úlohu. Velikost ovlivní také kapacity zpracování napájení, paměť a úložiště virtuálního počítače. Se účtují podle množství času, je virtuální počítač spuštěn a využívání jeho přidělené prostředky. Úplný seznam [velikosti virtuálních počítačů](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Tady jsou některé základní pokyny pro výběr velikost virtuálního počítače z jednoho z našich řady (A, D, DS, G a GS).
* Virtuální počítače, A-series jsou naše hodnotu za cenu vstupní úrovně virtuálních počítačů pro lehké úlohy a scénáře vývoje/testování. Jsou k dispozici ve všech oblastech a můžou se připojit a používat všechny standardní prostředky, které jsou k dispozici pro virtuální počítače.
* A-series velikosti (A8 - A11) jsou speciální výpočetní náročné konfigurace vhodná pro vysoce výkonné aplikace, výpočetní cluster.
* Virtuální počítače D-series slouží ke spouštění aplikací, které potřebují vyšší výpočetní výkon a krátkodobý vysoký diskový výkon. Virtuální počítače D-series mají rychlejší procesory, vyšší poměr paměti na jádro a jednotky SSD pro dočasný disk.
* Dv2-series, je nejnovější verzi naše D-series, funkce výkonnější procesor. Procesor v Dv2-series je asi o 35 % rychlejší než procesor v D-series. Je založena na nejnovější generace 2.4 v3® GHz Intel Xeon E5-2673 procesoru (Haskell) a s 2.0 technologie Intel Turbo nárůst, můžete přejít až 3,2 GHz. Řada Dv2-series má stejnou konfiguraci paměti a disku jako řada D.
* Virtuální počítače G-series nabízejí nejvíce paměti a spouštějí se na hostitelích s procesory Intel Xeon E5 V3.

Poznámka: DS-series a GS-series virtuální počítače mají přístup do úložiště úrovně Premium – naše SSD zálohovaný vysoce výkonné, nízkou latencí úložiště pro zatížení s intenzivním vstupně-výstupní operace. V některých oblastech je dostupná služba Storage úrovně Premium. Podrobnosti najdete tady:

* [Úložiště Premium: Vysoce výkonné úložiště pro úlohy virtuálního počítače Azure](../windows/premium-storage.md)

## <a name="automation"></a>Automation
K dosažení správné jazykovou verzi DevOps, musí být všechny infrastruktury kódu.  Pokud všechny infrastruktury žije v kód, který lze snadno znovu (Phoenix servery).  Azure funguje u všech hlavních automatizace tooling jako Ansible, Chef, Puppet a SaltStack.  Azure má také vlastní nástrojů pro automatizaci:

* [Šablony Azure](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Azure VMAccess](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Azure zavádí podporu pro [cloudu init](http://cloud-init.io/) napříč většina distribucích systému Linux, které ji podporují.  Ubuntu Canonical na virtuální počítače jsou aktuálně nasazená s inicializací cloudu ve výchozím nastavení povolené.  Red klobouky RHEL, CentOS a Fedora podporují init cloudu, ale Azure Image udržované RedHat nemají cloudu inicializací nainstalována.  Pokud chcete použít cloudu init na řadu RedHat operačního systému, musíte vytvořit vlastní image s inicializací cloudu nainstalována.

* [Pomocí init cloudu na virtuálních počítačích Azure Linux](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="quotas"></a>Kvóty
Každé předplatné Azure má výchozí kvótami v místě, ke kterému by mohlo mít vliv nasazení velkého počtu virtuálních počítačů pro váš projekt. Aktuální limit jednoho předplatného je 20 virtuálních počítačů na oblast.  Maximální kvóty mohou být vyvolány rychle a snadno o vyplnění lístek podpory požaduje limit zvýší.  Další informace o omezení kvót:

* [Omezení služby předplatného Azure](../../azure-subscription-service-limits.md)

## <a name="partners"></a>Partneři
Microsoft úzce spolupracuje s našimi partnery zajistit bitové kopie, které jsou aktualizovány a optimalizované pro Azure modulu runtime.  Další informace o našich partnerů zkontrolujte stránky své marketplace.

* Linux ve službě Azure - [distribuce schválené](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* SUSE - [Azure Marketplace - SUSE Linux Enterprise Server](https://azuremarketplace.microsoft.com/en-us/marketplace/apps?search=%27SUSE%27)
* Red Hat - [Azure Marketplace - Red Hat Enterprise Linux 7.2](https://azure.microsoft.com/marketplace/partners/redhat/redhatenterpriselinux72/)
* Kanonický - [Azure Marketplace - LTS Ubuntu Server 16.04](https://azure.microsoft.com/marketplace/partners/canonical/ubuntuserver1604lts/)
* Debian - [Azure Marketplace - 8 Debian "Klára"](https://azure.microsoft.com/marketplace/partners/credativ/debian8/)
* FreeBSD - [Azure Marketplace - FreeBSD 10.3](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/)
* CoreOS - [Azure Marketplace - CoreOS (stabilní)](https://azure.microsoft.com/marketplace/partners/coreos/coreosstable/)
* RancherOS - [Azure Marketplace - RancherOS](https://azure.microsoft.com/marketplace/partners/rancher/rancheros/)
* Bitnami - [Bitnami knihovny pro Azure.](https://azure.bitnami.com/)
* Mesosphere - [Azure Marketplace - Mesosphere DC/OS v Azure](https://azure.microsoft.com/marketplace/partners/mesosphere/dcosdcos/)
* Docker - [Azure Marketplace - Azure Container Service pomocí Docker Swarm](https://azure.microsoft.com/marketplace/partners/microsoft/acsswarms/)
* Volaných - [Azure Marketplace - CloudBees volaných platformy](https://azure.microsoft.com/marketplace/partners/cloudbees/jenkins-platformjenkins-platform/)

## <a name="getting-started-with-linux-on-azure"></a>Začínáme s Linuxem v Azure
Pokud chcete začít používat Azure budete potřebovat účet Azure, rozhraní příkazového řádku Azure, který je nainstalován a pár veřejného a privátního klíče SSH.

### <a name="sign-up-for-an-account"></a>Registrace účtu
Prvním krokem při použití Azure Cloud je zaregistrujte si účet Azure.  Přejděte na [registraci účtu Azure](https://azure.microsoft.com/pricing/free-trial/) stránky začít pracovat.

### <a name="install-the-cli"></a>Instalace rozhraní příkazového řádku
S váš nový účet Azure můžete začít používat okamžitě pomocí portálu Azure, který je panelu webové správy.  Ke správě cloudu Azure pomocí příkazového řádku, můžete nainstalovat `azure-cli`.  Nainstalujte [Azure CLI 2.0](/cli/azure/install-azure-cli) na pracovní stanici Mac nebo Linux.

### <a name="create-an-ssh-key-pair"></a>Vytvoření páru klíčů SSH
Nyní máte účet Azure, Azure webový portál a rozhraní příkazového řádku Azure.  Dalším krokem je vytvoření páru klíčů SSH, která se používá k SSH na Linux bez použití hesla.  [Vytvoření klíčů SSH na Linuxu a Macu](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) heslo bez přihlášení a lepší zabezpečení.

### <a name="create-a-vm-using-the-cli"></a>Vytvoření virtuálního počítače pomocí rozhraní příkazového řádku
Vytvoření virtuálního počítače s Linuxem pomocí rozhraní příkazového řádku je rychlý způsob, jak nasadit virtuální počítač, aniž byste museli opustit terminálu pracujete v.  Všechny objekty, které můžete zadat na webový portál je k dispozici prostřednictvím příkazového řádku příznak nebo přepínač.  

* [Vytvoření virtuálního počítače s Linuxem pomocí rozhraní příkazového řádku](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="create-a-vm-in-the-portal"></a>Vytvoření virtuálního počítače na portálu
Vytvoření virtuálního počítače s Linuxem v Azure webový portál je způsob, jak snadno bodu a klikněte na tlačítko prostřednictvím různých možností získat do nasazení.  Místo použití příznaky příkazového řádku nebo přepínače, budete moci zobrazit rozložení dobrý webové různé možnosti a nastavení.  Všechno, co k dispozici prostřednictvím rozhraní příkazového řádku je k dispozici na portálu.

* [Vytvoření virtuálního počítače s Linuxem pomocí portálu](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="login-using-ssh-without-a-password"></a>Přihlášení pomocí protokolu SSH bez zadání hesla
Virtuální počítač je nyní spuštěna v Azure a budete chtít přihlásit.  Použití hesel k přihlášení pomocí protokolu SSH je nezabezpečené a časově náročné.  Použití klíče SSH je nejbezpečnější způsob a také nejrychlejší způsob, jak přihlášení.  Můžete vytvořit virtuální počítač s Linuxem pomocí portálu nebo rozhraní příkazového řádku, máte dvě možnosti ověřování.  Pokud si zvolíte heslo SSH, Azure nakonfiguruje virtuální počítač povolit přihlášení pomocí hesla.  Pokud jste se rozhodli použít veřejný klíč SSH, Azure nakonfiguruje virtuálního počítače a Povolit jenom přihlášení pomocí klíče SSH a zakáže přihlášení heslo. Pro zabezpečení vašeho virtuálního počítače s Linuxem povolením jenom přihlášení klíče SSH, pomocí veřejného klíče možnost SSH při vytváření virtuálních počítačů v portálu nebo rozhraní příkazového řádku.

## <a name="related-azure-components"></a>Související Azure součásti
## <a name="storage"></a>Úložiště
* [Úvod do Microsoft Azure Storage](../../storage/common/storage-introduction.md)
* [Přidejte disk do virtuálního počítače s Linuxem pomocí příkazového řádku azure](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Tom, jak připojit datový disk do virtuálního počítače s Linuxem na portálu Azure](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="networking"></a>Sítě
* [Přehled virtuálních sítí](../../virtual-network/virtual-networks-overview.md)
* [IP adresy v Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md)
* [Otevření portů pro virtuální počítač s Linuxem v Azure](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Vytvořit plně kvalifikovaný název domény na portálu Azure](portal-create-fqdn.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="containers"></a>Kontejnery
* [Virtuální počítače a kontejnerů v Azure](containers.md)
* [Azure Container Service Úvod](../../container-service/container-service-intro.md)
* [Nasazení clusteru Azure Container Service](../../container-service/dcos-swarm/container-service-deployment.md)

## <a name="next-steps"></a>Další kroky
Teď máte přehled systému Linux v Azure.  Dalším krokem je podrobné informace a vytvořit několik virtuálních počítačů!

* [Prozkoumejte rostoucí seznam ukázkové skripty pro běžné úkoly prostřednictvím AzureCLI](cli-samples.md)
