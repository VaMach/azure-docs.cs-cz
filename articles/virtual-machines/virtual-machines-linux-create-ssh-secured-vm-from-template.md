<properties
    pageTitle="Vytvoření virtuálního počítače s Linuxem pomocí šablony Azure | Microsoft Azure"
    description="Virtuální počítač s Linuxem můžete na Azure vytvořit pomocí šablony Azure Resource Manageru."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/17/2016"
    ms.author="v-livech"/>


# Vytvoření virtuálního počítače s Linuxem pomocí šablony Azure

Tento článek ukazuje, jak rychle nasadit virtuální počítač s Linuxem na platformě Azure pomocí šablony Azure.  Článek vyžaduje účet Azure ([získat bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/)) a rozhraní příkazového řádku [Azure CLI](../xplat-cli-install.md) (`azure login`), které je přihlášené a v režimu Resource Manageru (`azure config mode arm`).  Virtuální počítač s Linuxem můžete také rychle nasadit pomocí webu [Azure Portal](virtual-machines-linux-quick-create-portal.md) nebo rozhraní příkazového řádku [Azure CLI](virtual-machines-linux-quick-create-cli.md).

## Rychlý přehled příkazu

```bash
azure group create \
-n quicksecuretemplate \
-l eastus \
--template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

## Podrobný postup

Šablony umožňují vytvořit virtuální počítače na platformě Azure s nastavením, které si pak při spouštění upravíte, například uživatelská jména a názvy hostitelů. Pro tento článek spouštíme šablonu Azure využívající virtuální počítač Ubuntu společně se skupinu zabezpečení sítě (NSG) s portem 22 otevřeným pro SSH.

Šablony Azure Resource Manageru jsou soubory JSON, které jde použít k jednoduchým jednorázovým úlohám, jako je spuštění virtuálního počítače Ubuntu VM pro účely tohoto článku.  Šablony Azure jde také použít k vytvoření složitých konfigurací Azure pro celé prostředí, jako je sada pro nasazení testovacího, vývojového nebo produkčního prostředí.

## Vytvoření virtuálního počítače s Linuxem

Následující příklad kódu ukazuje, jak voláním příkazu `azure group create` vytvořit skupinu prostředků a zároveň nasadit virtuální počítač s Linuxem se zabezpečením SSH pomocí [této šablony Azure Resource Manageru](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json). Je třeba myslet na to, že v příkladu musíte použít názvy, které jsou jedinečné pro vaše prostředí. V tomto příkladě se používá název skupiny prostředků `quicksecuretemplate`, název virtuálního počítače `securelinux` a název subdomény `quicksecurelinux`.

```bash
azure group create \
-n quicksecuretemplate \
-l eastus \
--template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

Výstup

```bash
info:    Executing command group create
+ Getting resource group quicksecuretemplate
+ Creating resource group quicksecuretemplate
info:    Created resource group quicksecuretemplate
info:    Supply values for the following parameters
sshKeyData: ssh-rsa AAAAB3Nza<..ssh public key text..>VQgwjNjQ== vlivech@azure
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/<..subid text..>/resourceGroups/quicksecuretemplate
data:    Name:                quicksecuretemplate
data:    Location:            eastus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

Tento příklad nasazuje virtuální počítač pomocí parametru `--template-uri`.  Můžete taky šablonu stáhnout nebo ji vytvořit místně a předat ji pomocí parametru `--template-file`cesta k souboru šablony. Rozhraní Azure CLI vás požádá o parametry vyžadované šablonou.

## Další kroky

V [galerii šablon](https://azure.microsoft.com/documentation/templates/) se podívejte, jaké aplikační architektury nasadit jako další.



<!--HONumber=Sep16_HO3-->


