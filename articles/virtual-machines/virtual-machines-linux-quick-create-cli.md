---
title: Vytvoření virtuálního počítače s Linuxem v Azure pomocí rozhraní příkazového řádku | Microsoft Docs
description: Vytvoření virtuálního počítače s Linuxem v Azure pomocí rozhraní příkazového řádku
services: virtual-machines-linux
documentationcenter: ''
author: vlivech
manager: timlt
editor: ''

ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/08/2016
ms.author: v-livech

---
# Vytvoření virtuálního počítače s Linuxem v Azure pomocí rozhraní příkazového řádku
Tento článek ukazuje, jak rychle nasadit virtuální počítač s Linuxem na platformě Azure pomocí příkazu `azure vm quick-create` v rozhraní příkazového řádku (CLI) Azure. Příkaz `quick-create` nasadí virtuální počítač se základní zabezpečenou infrastrukturou, který můžete použít k rychlému vytvoření prototypu nebo otestování konceptu. Tento článek vyžaduje:

* účet Azure ([získejte bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/))
* [rozhraní příkazového řádku Azure](../xplat-cli-install.md) s přihlášením `azure login`
* Rozhraní příkazového řádku Azure *musí být v* režimu Azure Resource Manager. `azure config mode arm`

Virtuální počítač s Linuxem můžete rychle nasadit také pomocí webu [Azure Portal](virtual-machines-linux-quick-create-portal.md).

## Rychlé příkazy
Následující příklad ukazuje, jak nasadit virtuální počítač s CoreOS a připojit klíč SSH (Secure Shell). Vaše argumenty ale mohou být jiné:

```bash
azure vm quick-create -M ~/.ssh/azure_id_rsa.pub -Q CoreOS
```

Následující části popisují tento příkaz a jeho požadavky, pokud se k distribuci Linuxu použije Ubuntu Server 14.04 LTS.  

## Aliasy quick-create pro virtuální počítače
Rychlým způsobem, jak zvolit distribuci, je použít aliasy rozhraní příkazového řádku Azure namapované na nejběžnější distribuce operačních systémů. Tyto aliasy jsou uvedené v následující tabulce (pro rozhraní příkazového řádku Azure verze 0.10). Všechna nasazení, která využívají `quick-create`, standardně směřují na virtuální počítače zálohované úložištěm SSD (solid-state drive), které nabízí rychlejší zřizování a vysoce výkonný přístup na disk. (Tyto aliasy představují jenom nepatrnou část dostupných distribucí na platformě Azure. Další image můžete vyhledat v Azure Marketplace pomocí [hledání image](virtual-machines-linux-cli-ps-findimage.md), nebo můžete [nahrát vlastní image](virtual-machines-linux-create-upload-generic.md).)

| Alias | Vydavatel | Nabídka | Skladová jednotka (SKU) | Verze |
|:--- |:--- |:--- |:--- |:--- |
| CentOS |OpenLogic |CentOS |7.2 |nejnovější |
| CoreOS |CoreOS |CoreOS |Stable |nejnovější |
| Debian |credativ |Debian |8 |nejnovější |
| openSUSE |SUSE |openSUSE |13.2 |nejnovější |
| RHEL |Red Hat |RHEL |7.2 |nejnovější |
| UbuntuLTS |Canonical |Ubuntu Server |14.04.4-LTS |nejnovější |

V následujících částech se používá alias `UbuntuLTS` pro možnost **ImageURN** (`-Q`) k nasazení serveru Ubuntu 14.04.4 LTS.

## Podrobný postup
V předchozím příkladu `quick-create` se příznak `-M` využíval jenom k identifikaci veřejného klíče SSH pro odeslání a hesla SSH byla zakázaná, takže se zobrazí výzva k zadání následujících argumentů:

* název skupiny prostředků (pro první skupinu prostředků Azure to obvykle může být libovolný řetězec)
* název virtuálního počítače
* umístění (vhodné výchozí hodnoty jsou `westus` nebo `westeurope`)
* linux (aby se v Azure vědělo, který operační systém chcete)
* uživatelské jméno

V následujícím příkladu jsou všechny tyto hodnoty zadané, takže už není potřeba zobrazovat žádné další výzvy. Pokud jako soubor veřejného klíče ve formátu ssh-rsa používáte `~/.ssh/id_rsa.pub`, funguje tak, jak je:

```bash
azure vm quick-create \
-g exampleResourceGroup \
-n exampleVMName \
-l westus \
-y Linux \
-u exampleAdminUser \
-M ~/.ssh/id_rsa.pub \
-Q UbuntuLTS
```

Výstup by měl vypadat jako následující výstupní blok:

```bash
info:    Executing command vm quick-create
+ Listing virtual machine sizes available in the location "westus"
+ Looking up the VM "exampleVMName"
info:    Verifying the public key SSH file: /Users/ahmet/.ssh/id_rsa.pub
info:    Using the VM Size "Standard_DS1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Looking up the storage account cli16330708391032639673
+ Looking up the NIC "examp-westu-1633070839-nic"
info:    An nic with given name "examp-westu-1633070839-nic" not found, creating a new one
+ Looking up the virtual network "examp-westu-1633070839-vnet"
info:    Preparing to create new virtual network and subnet
/ Creating a new virtual network "examp-westu-1633070839-vnet" [address prefix: "10.0.0.0/16"] with subnet "examp-westu-1633070839-snet" [address prefix: "10.+.1.0/24"]
+ Looking up the virtual network "examp-westu-1633070839-vnet"
+ Looking up the subnet "examp-westu-1633070839-snet" under the virtual network "examp-westu-1633070839-vnet"
info:    Found public ip parameters, trying to setup PublicIP profile
+ Looking up the public ip "examp-westu-1633070839-pip"
info:    PublicIP with given name "examp-westu-1633070839-pip" not found, creating a new one
+ Creating public ip "examp-westu-1633070839-pip"
+ Looking up the public ip "examp-westu-1633070839-pip"
+ Creating NIC "examp-westu-1633070839-nic"
+ Looking up the NIC "examp-westu-1633070839-nic"
+ Looking up the storage account clisto1710997031examplev
+ Creating VM "exampleVMName"
+ Looking up the VM "exampleVMName"
+ Looking up the NIC "examp-westu-1633070839-nic"
+ Looking up the public ip "examp-westu-1633070839-pip"
data:    Id                              :/subscriptions/2<--snip-->d/resourceGroups/exampleResourceGroup/providers/Microsoft.Compute/virtualMachines/exampleVMName
data:    ProvisioningState               :Succeeded
data:    Name                            :exampleVMName
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :Canonical
data:        Offer                       :UbuntuServer
data:        Sku                         :14.04.4-LTS
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clic7fadb847357e9cf-os-1473374894359
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://cli16330708391032639673.blob.core.windows.net/vhds/clic7fadb847357e9cf-os-1473374894359.vhd
data:
data:    OS Profile:
data:      Computer Name                 :exampleVMName
data:      User Name                     :exampleAdminUser
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-33-42-FB
data:          Provisioning State        :Succeeded
data:          Name                      :examp-westu-1633070839-nic
data:          Location                  :westus
data:            Public IP address       :138.91.247.29
data:            FQDN                    :examp-westu-1633070839-pip.westus.cloudapp.azure.com
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://clisto1710997031examplev.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm quick-create command OK
```

Přihlaste se do vašeho virtuálního počítače pomocí veřejné IP adresy, která je uvedená ve výstupu. Můžete také využít uvedený plně kvalifikovaný název domény:

```bash
ssh -i ~/.ssh/id_rsa.pub exampleAdminUser@138.91.247.29
```

Proces přihlášení by měl vypadat podobně jako následující výstupní blok:

```bash
Warning: Permanently added '138.91.247.29' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 14.04.4 LTS (GNU/Linux 3.19.0-65-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

  System information as of Thu Sep  8 22:50:57 UTC 2016

  System load: 0.63              Memory usage: 2%   Processes:       81
  Usage of /:  39.6% of 1.94GB   Swap usage:   0%   Users logged in: 0

  Graph this data and manage this system at:
    https://landscape.canonical.com/

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.



The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

exampleAdminUser@exampleVMName:~$
```

## Další kroky
Příkaz `azure vm quick-create` představuje způsob, jak rychle nasadit virtuální počítač, abyste se mohli přihlásit k prostředí Bash a začít pracovat. Použití `vm quick-create` ale neposkytuje větší možnosti kontroly ani neumožňuje vytvářet složitější prostředí.  Pokud budete chtít nasadit virtuální počítač s Linuxem přizpůsobený vaší infrastruktuře, můžete postupovat podle některého z těchto článků:

* [Vytvoření konkrétního nasazení pomocí šablony Azure Resource Manageru](virtual-machines-linux-cli-deploy-templates.md)
* [Přímé vytvoření vlastního prostředí pro virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure](virtual-machines-linux-create-cli-complete.md)
* [Vytvoření virtuálního počítače s Linuxem se zabezpečením SSH na platformě Azure pomocí šablon](virtual-machines-linux-create-ssh-secured-vm-from-template.md)

K [rychlému vytvoření linuxového virtuálního počítače jako hostitele Docker můžete také využít ovladač Azure `docker-machine` s různými příkazy](virtual-machines-linux-docker-machine.md).

<!--HONumber=Sep16_HO5-->


