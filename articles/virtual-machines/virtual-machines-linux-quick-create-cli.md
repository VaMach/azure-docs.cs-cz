<properties
   pageTitle="Vytvoření virtuálního počítače s Linuxem v Azure pomocí rozhraní příkazového řádku (CLI) | Microsoft Azure"
   description="Virtuální počítače s Linuxem si můžete vytvořit také pomocí rozhraní příkazového řádku (CLI)."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="vlivech"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="08/18/2016"
   ms.author="v-livech"/>


# Vytvoření virtuálního počítače s Linuxem v Azure pomocí rozhraní příkazového řádku (CLI)

Tento článek ukazuje, jak rychle nasadit virtuální počítač s Linuxem na platformě Azure pomocí příkazu `azure vm quick-create` rozhraní příkazového řádku Azure CLI. Příkaz `quick-create` nasadí virtuální počítač se základní okolní infrastrukturou, který můžete použít k rychlému vytvoření prototypu nebo otestování konceptu.  Článek vyžaduje účet Azure ([získat bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/)) a rozhraní příkazového řádku [Azure CLI](../xplat-cli-install.md) (`azure login`), které je přihlášené a v režimu Resource Manageru (`azure config mode arm`).  Virtuální počítač s Linuxem můžete také rychle nasadit pomocí webu [Azure Portal](virtual-machines-linux-quick-create-portal.md).

## Rychlý přehled příkazu

Jeden příkaz pro nasazení virtuálního počítače s CoreOS a připojení klíče SSH:

```bash
azure vm quick-create -M ~/.ssh/azure_id_rsa.pub -Q CoreOS
```

## Nasazení virtuálního počítače s Linuxem

Nyní si příkaz projdeme a vysvětlíme každý krok použití Red Hat Enterprise Linuxu 7.2.  

## Použití aliasu ImageURN

Příkaz `quick-create` rozhraní příkazového řádku Azure CLI má aliasy namapované na nejběžnější distribuce operačního systému. Následující tabulka uvádí aliasy distribucí (pro Azure CLI verze 0.10).  Pro všechna nasazení vytvořená pomocí příkazu `quick-create` se pro virtuální počítače ve výchozím nastavení používá úložiště SSD, které nabízí vysoký výkon.

| Alias     | Vydavatel | Nabídka        | Skladová jednotka (SKU)         | Verze |
|:----------|:----------|:-------------|:------------|:--------|
| CentOS    | OpenLogic | Centos       | 7.2         | nejnovější  |
| CoreOS    | CoreOS    | CoreOS       | Stable      | nejnovější  |
| Debian    | credativ  | Debian       | 8           | nejnovější  |
| openSUSE  | SUSE      | openSUSE     | 13.2        | nejnovější  |
| RHEL      | Redhat    | RHEL         | 7.2         | nejnovější  |
| UbuntuLTS | Canonical | UbuntuServer | 14.04.4-LTS | nejnovější  |



Pro možnost **ImageURN** (`-Q`) použijeme hodnotu `RHEL`, aby byl nasazen virtuální počítač s Linuxem ve verzi RedHat Enterprise Linux 7.2. Tyto aliasy `quick-create` představují nepatrnou část dostupného operačního systému na platformě Azure.  Vyhledejte další image na webu Marketplace pomocí [hledání image](virtual-machines-linux-cli-ps-findimage.md), nebo můžete [nahrát vlastní image](virtual-machines-linux-create-upload-generic.md).

V následující ukázce použití příkazu nahraďte výzvy hodnotami ze svého vlastního prostředí.

Postupujte podle jednotlivých výzev a zadejte vlastní názvy.

```bash
azure vm quick-create -M ~/.ssh/id_rsa.pub -Q RHEL
```

Výstup by měl vypadat jako následující výstupní blok.

```bash
info:    Executing command vm quick-create
Resource group name: rhel-quick
Virtual machine name: rhel
Location name: westus
Operating system Type [Windows, Linux]: linux
User name: ops
+ Listing virtual machine sizes available in the location "westus"
+ Looking up the VM "rhel"
info:    Verifying the public key SSH file: /Users/ops/.ssh/id_rsa.pub
info:    Using the VM Size "Standard_DS1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Looking up the storage account cli1630678171193501687
info:    Could not find the storage account "cli1630678171193501687", trying to create new one
+ Creating storage account "cli1630678171193501687" in "westus"
+ Looking up the storage account cli1630678171193501687
+ Looking up the NIC "rhel-westu-1630678171-nic"
info:    An nic with given name "rhel-westu-1630678171-nic" not found, creating a new one
+ Looking up the virtual network "rhel-westu-1630678171-vnet"
info:    Preparing to create new virtual network and subnet
+ Creating a new virtual network "rhel-westu-1630678171-vnet" [address prefix: "10.0.0.0/16"] with subnet "rhel-westu-1630678171-snet" [address prefix: "10.0.1.0/24"]
+ Looking up the virtual network "rhel-westu-1630678171-vnet"
+ Looking up the subnet "rhel-westu-1630678171-snet" under the virtual network "rhel-westu-1630678171-vnet"
info:    Found public ip parameters, trying to setup PublicIP profile
+ Looking up the public ip "rhel-westu-1630678171-pip"
info:    PublicIP with given name "rhel-westu-1630678171-pip" not found, creating a new one
+ Creating public ip "rhel-westu-1630678171-pip"
+ Looking up the public ip "rhel-westu-1630678171-pip"
+ Creating NIC "rhel-westu-1630678171-nic"
+ Looking up the NIC "rhel-westu-1630678171-nic"
+ Looking up the storage account clisto909893658rhel
+ Creating VM "rhel"
+ Looking up the VM "rhel"
+ Looking up the NIC "rhel-westu-1630678171-nic"
+ Looking up the public ip "rhel-westu-1630678171-pip"
data:    Id                              :/subscriptions/<guid>/resourceGroups/rhel-quick/providers/Microsoft.Compute/virtualMachines/rhel
data:    ProvisioningState               :Succeeded
data:    Name                            :rhel
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :RedHat
data:        Offer                       :RHEL
data:        Sku                         :7.2
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clic5abbc145c0242c1-os-1462425492101
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://cli1630678171193501687.blob.core.windows.net/vhds/clic5abbc145c0242c1-os-1462425492101.vhd
data:
data:    OS Profile:
data:      Computer Name                 :rhel
data:      User Name                     :ops
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-32-0F-DD
data:          Provisioning State        :Succeeded
data:          Name                      :rhel-westu-1630678171-nic
data:          Location                  :westus
data:            Public IP address       :104.42.236.196
data:            FQDN                    :rhel-westu-1630678171-pip.westus.cloudapp.azure.com
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://clisto909893658rhel.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm quick-create command OK
```

SSH do vašeho virtuálního počítače na portu 22 a veřejná adresa IP, která je uvedená ve výstupu. (Můžete také použít uvedený plně kvalifikovaný název domény.)

```bash
ssh ops@rhel-westu-1630678171-pip.westus.cloudapp.azure.com
```
Proces přihlášení by měl vypadat přibližně takto:

```bash
The authenticity of host 'rhel-westu-1630678171-pip.westus.cloudapp.azure.com (104.42.236.196)' can't be established.
RSA key fingerprint is 0e:81:c4:36:2d:eb:3c:5a:dc:7e:65:8a:3f:3e:b0:cb.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'rhel-westu-1630678171-pip.westus.cloudapp.azure.com,104.42.236.196' (RSA) to the list of known hosts.
[ops@rhel ~]$ ls -a
.  ..  .bash_logout  .bash_profile  .bashrc  .cache  .config  .ssh
```

## Další kroky

Příkaz `azure vm quick-create` představuje způsob, jak rychle nasadit virtuální počítač, abyste se mohli přihlásit k prostředí Bash a začít pracovat. Použitím příkazu `vm quick-create` nezískáte další výhody komplexního prostředí.  Pokud budete chtít nasadit virtuální počítač s Linuxem přizpůsobený vaší infrastruktuře, můžete postupovat podle některého z těchto článků.

- [Vytvoření konkrétního nasazení pomocí šablony Azure Resource Manageru](virtual-machines-linux-cli-deploy-templates.md)
- [Přímé vytvoření vlastního prostředí pro virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure CLI](virtual-machines-linux-create-cli-complete.md).
- [Vytvoření virtuálního počítače s Linuxem se zabezpečením SSH na platformě Azure pomocí šablon](virtual-machines-linux-create-ssh-secured-vm-from-template.md)



<!--HONumber=sep16_HO1-->


