<properties
    pageTitle="Různé způsoby vytvoření virtuálního počítače s Linuxem | Microsoft Azure"
    description="Uvádí různé způsoby, jak vytvořit virtuální počítač s Linuxem na platformě Azure, a odkazy na nástroje a kurzy pro jednotlivé metody."
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
    ms.date="07/06/2016"
    ms.author="iainfou"/>


# Různé způsoby, jak vytvořit virtuální počítač s Linuxem pomocí Resource Manageru

Azure umožňuje vytvářet virtuální počítače pomocí modelu nasazení s využitím Resource Manageru různými způsoby – podle potřeb uživatelů a pro konkrétní účely. Tento článek shrnuje tyto rozdíly a volby, které můžete zvolit pro vytvoření virtuálních počítačů s Linuxem.

## Azure CLI 

Rozhraní Azure CLI je k dispozici napříč platformami pomocí balíčku npm, balíčků distribuce nebo kontejneru Docker. Můžete si přečíst další informace o tom, [jak nainstalovat a nakonfigurovat Azure CLI](../xplat-cli-install.md). V následujících kurzech najdete příklady použití Azure CLI. V každém článku si přečtěte další podrobnosti o zobrazených příkazech rychlého startu CLI:

* [Vytvoření virtuálního počítače s Linuxem z Azure CLI pro vývoj a testování](virtual-machines-linux-quick-create-cli.md)

    ```bash
    azure vm quick-create -M ~/.ssh/azure_id_rsa.pub -Q CoreOS
    ```

* [Vytvoření zabezpečeného virtuálního počítače s Linuxem pomocí šablony Azure](virtual-machines-linux-create-ssh-secured-vm-from-template.md)

    ```bash
    azure group create --name TestRG --location WestUS 
        --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
    ```

* [Vytvoření virtuálního počítače s Linuxem od základů pomocí rozhraní příkazového řádku Azure CLI](virtual-machines-linux-create-cli-complete.md)

* [Přidání disku do virtuálního počítače s Linuxem](virtual-machines-linux-add-disk.md)

    ```bash
    azure vm disk attach-new --resource-group TestRG --vm-name TestVM <size-in-GB>
    ```

## portál Azure

Grafické uživatelské rozhraní [Portálu Azure](https://portal.azure.com) umožňuje snadno si vyzkoušet používání virtuálních počítačů, a to hlavně proto, že si nemusíte nic instalovat do svého počítače. Vytvoření virtuálního počítače pomocí Portálu Azure:

* [Vytvoření virtuálního počítače s Linuxem pomocí webu Azure Portal](virtual-machines-linux-quick-create-portal.md) 
* [Připojení disku pomocí webu Azure Portal](virtual-machines-linux-attach-disk-portal.md)

## Operační systém a volba image
Při vytváření virtuálního počítače zvolíte image podle operačního systému, který chcete používat. Azure a příslušní partneři nabízí celou řadu imagí, přičemž některé už obsahují předinstalované aplikace a nástroje. Můžete ale také nahrát některou ze svých vlastních imagí (viz dál).

### Image dostupné v Azure
Můžete použít příkazech CLI `azure vm image` a zobrazit, co je k dispozici prostřednictvím vydavatele, vydání distribuce a buildů.

Seznam dostupných vydavatelů:

```bash
azure vm image list-publishers --location WestUS
```

Seznam dostupných produktů (nabídek) pro daného vydavatele:

```bash
azure vm image list-offers --location WestUS --publisher Canonical
```

Seznam dostupných SKU (vydání distribuce) pro danou nabídku:

```bash
azure vm image list-skus --location WestUS --publisher Canonical --offer UbuntuServer
```

Seznam všech dostupných imagí pro dané vydání:

```bash
azure vm image list --location WestUS --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS
```

Další příklady vyhledávání a použití dostupných imagí najdete v tématu [Vyhledání a výběr imagí virtuálních počítačů Azure pomocí Azure CLI](virtual-machines-linux-cli-ps-findimage.md).

Příkazy `azure vm quick-create` a `azure vm create` mají také některé aliasy, které můžete použít pro rychlý přístup k častějším distribucím a jejich nejnovějším vydáním. Je to snazší než určování vydavatele, nabídky, SKU a verze při každém vytváření virtuálního počítače:

| Alias     | Vydavatel | Nabídka        | Skladová jednotka (SKU)         | Verze |
|:----------|:----------|:-------------|:------------|:--------|
| CentOS    | OpenLogic | Centos       | 7.2         | nejnovější  |
| CoreOS    | CoreOS    | CoreOS       | Stable      | nejnovější  |
| Debian    | credativ  | Debian       | 8           | nejnovější  |
| openSUSE  | SUSE      | openSUSE     | 13.2        | nejnovější  |
| RHEL      | Redhat    | RHEL         | 7.2         | nejnovější  |
| SLES      | SLES      | SLES         | 12-SP1      | nejnovější  |
| UbuntuLTS | Canonical | UbuntuServer | 14.04.4-LTS | nejnovější  |

### Použití vlastní image

Pokud budete potřebovat image se zvláštními úpravami, můžete *zachytit* existující virtuální počítač Azure nebo nahrát vlastní image uloženou na virtuálním pevném disku (VHD). Další informace o podporovaných distribucích a o tom, jak používat vlastní image, najdete v následujících článcích:

* [Distribuce schválené pro Azure](virtual-machines-linux-endorsed-distros.md)

* [Informace pro neschválené distribuce](virtual-machines-linux-create-upload-generic.md)

* [Jak zachytit virtuální počítač s Linuxem do šablony Resource Manageru](virtual-machines-linux-capture-image.md) Příkazy rychlého startu:

    ```bash
    azure vm deallocate --resource-group TestRG --vm-name TestVM
    azure vm generalize --resource-group TestRG --vm-name TestVM
    azure vm capture --resource-group TestRG --vm-name TestVM --vhd-name-prefix CapturedVM
    ```

## Další kroky

* Vyzkoušejte jeden z kurzů a vytvořte si virtuální počítač s Linuxem z [Portálu](virtual-machines-linux-quick-create-portal.md), přes [CLI](virtual-machines-linux-quick-create-cli.md) nebo pomocí [šablony](virtual-machines-linux-cli-deploy-templates.md) Azure Resource Manageru.

* Po vytvoření virtuálního počítače s Linuxem můžete snadno [přidat datový disk](virtual-machines-linux-add-disk.md).

* Rychlé kroky pro [resetování hesla nebo klíčů SSH a správu uživatelů](virtual-machines-linux-using-vmaccess-extension.md)



<!--HONumber=Sep16_HO3-->


