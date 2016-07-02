<properties
    pageTitle="Různé způsoby vytvoření virtuálního počítače s Linuxem | Microsoft Azure"
    description="Uvádí různé způsoby, jak vytvořit virtuální počítač s Linuxem na platformě Azure, a nabízí odkazy na další pokyny."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="04/12/2016"
    ms.author="iainfou"/>

# Různé způsoby, jak vytvořit virtuální počítač s Linuxem pomocí Resource Manageru

Azure umožňuje vytvářet virtuální počítače pomocí modelu nasazení s využitím Resource Manageru různými způsoby – podle potřeb uživatelů a pro konkrétní účely. Tento článek shrnuje tyto rozdíly a volby, které můžete zvolit pro vytvoření virtuálních počítačů s Linuxem.

## Volba nástroje

### Příkazové prostředí: Azure CLI 

Z CLI použijte rozhraní příkazového řádku Azure. Můžete si přečíst další informace o tom, [jak nainstalovat Azure CLI](../xplat-cli-install.md) přes npm, kontejner Docker nebo instalační skript. V následujících kurzech nejdete příklady použití Azure CLI:

* [Vytvoření virtuálního počítače s Linuxem z Azure CLI pro vývoj a testování](virtual-machines-linux-quick-create-cli.md) 

* [Vytvoření zabezpečeného virtuálního počítače s Linuxem pomocí šablony Azure](virtual-machines-linux-create-ssh-secured-vm-from-template.md)

* [Vytvoření virtuálního počítače s Linuxem od základů pomocí rozhraní příkazového řádku Azure CLI](virtual-machines-linux-create-cli-complete.md)

### Grafické uživatelské rozhraní: Portál Azure

Grafické uživatelské rozhraní [Portálu Azure](https://portal.azure.com) umožňuje snadno si vyzkoušet používání virtuálních počítačů, a to hlavně proto, že si nemusíte nic instalovat do svého počítače. Vytvoření virtuálního počítače pomocí Portálu Azure:

* [Vytvoření virtuálního počítače s Linuxem pomocí Portálu Azure](virtual-machines-linux-quick-create-portal.md) 

## Operační systém a volba image

U obou metod zvolíte image podle operačního systému, který chcete používat. Azure a příslušní partneři nabízí celou řadu imagí, přičemž některé už obsahují předinstalované aplikace a nástroje. Můžete ale také nahrát některou ze svých vlastních imagí.

### Image dostupné v Azure

Ve všech výše uvedených článcích můžete snadno vytvořit virtuální počítač pomocí stávající image Azure a přizpůsobit si ho pro práci v síti, vyrovnávání zatížení a z dalších hledisek. Na Portálu můžete v Azure Marketplace vybírat z bitových kopií dostupných pro Azure. Podobné seznamy můžete získat pomocí příkazového řádku. Například spuštěním příkazu `azure vm image list` v Azure CLI získáte seznam všech dostupných imagí podle umístění a vydavatele. Příklady vyhledávání a použití dostupných imagí najdete v tématu [Vyhledání a výběr imagí virtuálních počítačů Azure pomocí Azure CLI](virtual-machines-linux-cli-ps-findimage.md).

### Použití vlastní image

Pokud budete potřebovat image se zvláštními úpravami, můžete *zachytit* existující virtuální počítač Azure nebo nahrát vlastní image uloženou na virtuálním pevném disku (VHD). Další informace o podporovaných distribucích a o tom, jak používat vlastní image, najdete v následujících článcích:

* [Distribuce schválené pro Azure](virtual-machines-linux-endorsed-distros.md)

* [Informace pro neschválené distribuce](virtual-machines-linux-create-upload-generic.md)

* [Jak zachytit virtuální počítač s Linuxem do šablony Resource Manageru](virtual-machines-linux-capture-image.md) 

## Další kroky

* Vyzkoušejte jeden z kurzů a vytvořte si virtuální počítač s Linuxem z [Portálu](virtual-machines-linux-quick-create-portal.md), přes [CLI](virtual-machines-linux-quick-create-cli.md) nebo pomocí [šablony](virtual-machines-linux-cli-deploy-templates.md) Azure Resource Manageru.

* Po vytvoření virtuálního počítače s Linuxem můžete snadno [přidat datový disk](virtual-machines-linux-add-disk.md).

* Rychlé kroky pro [resetování hesla nebo klíčů SSH a správu uživatelů](virtual-machines-linux-using-vmaccess-extension.md)



<!--HONumber=Jun16_HO2-->


