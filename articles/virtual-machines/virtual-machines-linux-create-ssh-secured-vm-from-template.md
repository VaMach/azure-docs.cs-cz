<properties
    pageTitle="Vytvoření zabezpečeného virtuálního počítače s Linuxem pomocí šablony Azure | Microsoft Azure"
    description="Zabezpečený virtuální počítač s Linuxem můžete na Azure vytvořit pomocí šablony Azure Resource Manageru."
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
    ms.date="04/27/2016"
    ms.author="v-livech"/>

# Vytvoření zabezpečeného virtuálního počítače s Linuxem pomocí šablony Azure

K vytvoření virtuálního počítače s Linuxem ze šablony budete potřebovat [Azure CLI](../xplat-cli-install.md) v režimu Resource Manageru (`azure config mode arm`).

## Rychlý přehled příkazu

```bash
chrisl@fedora$ azure group create -n <exampleRGname> -l <exampleAzureRegion> [--template-uri <URL> | --template-file <path> | <template-file> -e <parameters.json file>]
```

## Podrobný postup

Šablony umožňují vytvořit virtuální počítače na platformě Azure s nastavením, které si pak při spouštění upravíte, například uživatelská jména a názvy hostitelů. V tomto článku se zaměříme na spuštění virtuálního počítače s Ubuntu pomocí šablony Azure, která vytvoří skupinu zabezpečení sítě (NSG) pouze s jedním otevřeným portem (22 pro SSH) a která pro přihlášení vyžaduje klíče SSH.

Šablony Azure Resource Manageru jsou soubory JSON, které je možné použít pro jednoduché jednorázové úlohy, jako je spuštění virtuálního počítače s Ubuntu (jako v tomto článku), nebo k vytvoření komplexní konfigurace Azure celých prostředí (například nasazení testovacího, vývojového nebo produkčního prostředí) ze sítě do nasazení operačního systému a aplikačního balíčku.

## Vytvoření virtuálního počítače s Linuxem

Následující příklad kódu ukazuje, jak voláním příkazu `azure group create` vytvořit skupinu prostředků a zároveň nasadit virtuální počítač s Linuxem se zabezpečením SSH pomocí [této šablony Azure Resource Manageru](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json). Je třeba myslet na to, že v příkladu musíte použít názvy, které jsou jedinečné pro vaše prostředí. V tomto příkladě se používá název skupiny prostředků `quicksecuretemplate`, název virtuálního počítače `securelinux` a název subdomény `quicksecurelinux`.

```bash
chrisl@fedora$ azure group create -n quicksecuretemplate -l eastus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
info:    Executing command group create
+ Getting resource group quicksecuretemplate
+ Creating resource group quicksecuretemplate
info:    Created resource group quicksecuretemplate
info:    Supply values for the following parameters
adminUserName: ops
sshKeyData: ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDDRZ/XB8p8uXMqgI8EoN3dWQw... user@contoso.com
dnsLabelPrefix: quicksecurelinux
vmName: securelinux
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/<guid>/resourceGroups/quicksecuretemplate
data:    Name:                quicksecuretemplate
data:    Location:            eastus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

Můžete vytvořit novou skupinu prostředků a nasadit virtuální počítač pomocí parametru `--template-uri`, případně můžete šablonu stáhnout nebo ji vytvořit místně a předat ji pomocí parametru `--template-file` a při tom cestu k souboru šablony uvést v argumentu. Rozhraní Azure CLI vás požádá o parametry vyžadované šablonou.

## Další kroky

Jakmile pomocí šablon vytvoříte virtuální počítače s Linuxem, bude potřeba se podívat, jaké aplikační architektury je možné se šablonami používat. V [galerii šablon](https://azure.microsoft.com/documentation/templates/) se podívejte, jaké aplikační architektury nasadit jako další.



<!--HONumber=Jun16_HO2-->


