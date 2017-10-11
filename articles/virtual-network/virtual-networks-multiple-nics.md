---
title: "Vytvoření virtuálního počítače (klasické) s více síťovými kartami pomocí prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak vytvořit a nakonfigurovat virtuální počítače s více síťovými kartami pomocí prostředí PowerShell."
services: virtual-network, virtual-machines
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
tags: azure-service-management
ms.assetid: a1a3952c-2dcc-4977-bd7a-52d623c1fb07
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.openlocfilehash: 68ccc1cac22e593b099729fe68c6bee63df44d9b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-vm-classic-with-multiple-nics"></a>Vytvoření virtuálního počítače (klasické) s více síťovými kartami
Můžete vytvořit virtuální počítače (VM) v Azure a připojit více síťových rozhraní (NIC) ke každému z virtuálních počítačů. Několik síťových adaptérů jsou nutné pro mnoho síťových virtuálních zařízení, například doručení aplikace a řešení optimalizace sítě WAN. Několik síťových adaptérů také poskytují izolaci provozu mezi síťové adaptéry.

![Síťový adaptér více virtuálních počítačů](./media/virtual-networks-multiple-nics/IC757773.png)

Obrázek zobrazuje virtuálního počítače se síťovými adaptéry, tři, že každý z nich připojený k jiné podsíti.

> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../resource-manager-deployment-model.md). Tento článek se věnuje použití klasického modelu nasazení. Společnost Microsoft doporučuje, aby většina nových nasazení používala Resource Manager.

* Internetový VIP (nasazení classic) je podporován pouze na síťový adaptér "default". Existuje pouze jeden virtuální IP adresy IP na výchozí síťový adaptér.
* V tomto okamžiku nejsou podporovány adresy Instance úroveň veřejné IP (LPIP) (nasazení classic) pro více virtuálních počítačů síťovou kartu.
* Pořadí síťových rozhraní ve virtuálním počítači je náhodné a může se také měnit následkem aktualizací infrastruktury Azure. Ale IP adresy a odpovídající ethernetová adresa MAC adres se zůstávají stejné. Předpokládejme například, **Eth1** 10.1.0.100 IP adresu a adresu MAC 00-0D-3A-B0-39-0D; po aktualizaci infrastruktury Azure a restartování, může změnit tak, aby **Eth2**, ale IP a MAC párování zůstane stejný. Pokud se restartování spouštěná zákazníka, zůstane stejný pořadí síťový adaptér.
* Adresa pro každý síťový adaptér na každý virtuální počítač se musí nacházet v podsíti, několik síťových adaptérů na jeden virtuální počítač můžete ke každé možné přiřadit adresy, které jsou ve stejné podsíti.
* Velikost virtuálního počítače určuje počet síťových ADAPTÉRŮ, který můžete vytvořit pro virtuální počítač. Odkaz [systému Windows Server](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) články k určení, kolik SÍŤOVÝCH každý velikost virtuálního počítače podporuje velikostí virtuálních počítačů. 

## <a name="network-security-groups-nsgs"></a>Skupiny zabezpečení sítě (Nsg)
V nasazení Resource Manager může být všechny síťové adaptéry na virtuálním počítači související s skupina zabezpečení sítě (NSG), včetně všech síťových adaptérů na virtuálním počítači, který má několik síťových adaptérů povoleno. Pokud síťový adaptér je přiřazena adresa v podsíti, kde je přidružen skupinu NSG podsítě, potom pravidla v NSG podsítě platí i pro tuto síťovou kartu. Kromě přidružení podsítí k skupin Nsg, lze přiřadit síťový adaptér s skupinu NSG.

Pokud podsíť je přidružen skupinu NSG a síťovou kartu v této podsíti jednotlivě souvisí s skupinu NSG, přidružená pravidla NSG se použijí v **toku pořadí** podle provoz předávány do nebo z síťový adaptér:

* **Příchozí provoz** jehož cílem je síťový adaptér v otázku nejprve prochází podsíť, která aktivuje pravidla NSG podsítě, před předáním do síťového adaptéru a potom aktivuje pravidla NSG na síťový adaptér.
* **Odchozí přenosy** jejichž zdrojem je na síťový adaptér toků první ze ze síťového adaptéru, spouštění pravidla NSG na síťový adaptér, před prošla podsíť a potom aktivuje pravidla NSG podsítě.

Další informace o [skupin zabezpečení sítě](virtual-networks-nsg.md) a jak se používají podle přidružení podsítě virtuálních počítačů a síťových karet...

## <a name="how-to-configure-a-multi-nic-vm-in-a-classic-deployment"></a>Jak nakonfigurovat více virtuálních počítačů síťovou kartu v nasazení classic
Následující pokyny vám pomůže vytvořit více virtuálních počítačů síťovou kartu obsahující 3 síťové adaptéry: výchozí síťový adaptér a další dva síťové adaptéry. Postup konfigurace vytvoří virtuální počítač, který bude nakonfigurovaný podle fragment souboru konfigurace služby níže:

    <VirtualNetworkSite name="MultiNIC-VNet" Location="North Europe">
    <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
    … Skip over the remainder section …
    </VirtualNetworkSite>


Před pokusem o spuštění příkazů prostředí PowerShell v příkladu potřebujete následující požadavky.

* Předplatné Azure.
* Nakonfigurované virtuální sítě. V tématu [Přehled virtuálních sítí](virtual-networks-overview.md) Další informace o virtuálních sítí.
* Nejnovější verzi prostředí Azure PowerShell stáhnout a nainstalovat. Viz téma [Instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

Vytvoření virtuálního počítače s více síťovými kartami, proveďte následující kroky zadáním každý příkaz v rámci jedné relace prostředí PowerShell:

1. Vyberte bitovou kopii virtuálního počítače z Galerie obrázků virtuálního počítače Azure. Všimněte si, že Image často mění a jsou dostupné podle oblasti. Image zadaná v následujícím příkladu nesmí změnit nebo může být ve vaší oblasti, takže je nutné zadat bitovou kopii, které potřebujete.

    ```powershell
    $image = Get-AzureVMImage `
    -ImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201410.01-en.us-127GB.vhd"
    ```

2. Vytvoření konfigurace virtuálního počítače.

    ```powershell
    $vm = New-AzureVMConfig -Name "MultiNicVM" -InstanceSize "ExtraLarge" `
    -Image $image.ImageName –AvailabilitySetName "MyAVSet"
    ```

3. Vytvoří výchozí přihlášení správce.

    ```powershell
    Add-AzureProvisioningConfig –VM $vm -Windows -AdminUserName "<YourAdminUID>" `
    -Password "<YourAdminPassword>"
    ```

4. Přidejte další síťové adaptéry ke konfiguraci virtuálních počítačů.

    ```powershell
    Add-AzureNetworkInterfaceConfig -Name "Ethernet1" `
    -SubnetName "Midtier" -StaticVNetIPAddress "10.1.1.111" -VM $vm
    Add-AzureNetworkInterfaceConfig -Name "Ethernet2" `
    -SubnetName "Backend" -StaticVNetIPAddress "10.1.2.222" -VM $vm
    ```

5. Zadejte podsíť a IP adresu pro síťový adaptér výchozí.

    ```powershell
    Set-AzureSubnet -SubnetNames "Frontend" -VM $vm
    Set-AzureStaticVNetIP -IPAddress "10.1.0.100" -VM $vm
    ```

6. Vytvoření virtuálního počítače ve virtuální síti.

    ```powershell
    New-AzureVM -ServiceName "MultiNIC-CS" –VNetName "MultiNIC-VNet" –VMs $vm
    ```

    > [!NOTE]
    > Virtuální síť, který zde určíte již musí existovat (jak je uvedeno v požadavky). Následující příklad určuje virtuální síť s názvem **MultiNIC-VNet**.
    >

## <a name="limitations"></a>Omezení
Při použití více síťových adaptérů platí následující omezení:

* Virtuální počítače s více síťovými kartami, musí být vytvořený v Azure virtuální sítě (virtuální sítě). Virtuální počítače non-VNet se nedá nakonfigurovat se několik síťových adaptérů.
* Všechny virtuální počítače v nastavení dostupnosti je potřeba použít několik síťových adaptérů nebo jeden síťový adaptér. Nemůže mít směs více virtuální síťový adaptér počítače a jeden síťový adaptér virtuálních počítačů v rámci skupiny dostupnosti. Stejná pravidla použít pro virtuální počítače v cloudové službě. Pro víc virtuálních počítačů síťovou kartu nemusí mít stejný počet síťových adaptérů, tak dlouho, dokud každá má alespoň dvě.
* Virtuální počítač s jednu síťovou kartu nelze konfigurovat pomocí více síťových adaptérů (a naopak) po jejím nasazení, bez odstranit a znovu ji vytvořit.

## <a name="secondary-nics-access-to-other-subnets"></a>Sekundární síťové adaptéry přístup do jiných podsítí
Ve výchozím nastavení se nenakonfigurují sekundární síťové adaptéry s výchozí bránou, kvůli které bude tok přenosů na sekundární síťové adaptéry omezenou být ve stejné podsíti. Pokud uživatele chcete povolit sekundární síťové adaptéry, aby komunikoval mimo vlastní podsíti, se bude nutné přidat položku do směrovací tabulky, pokud chcete konfigurovat bránu, jak je popsáno níže.

> [!NOTE]
> Virtuální počítače vytvořené před července 2015 může mít výchozí brána konfigurována pro všechny síťové adaptéry. Výchozí brána pro sekundární síťové adaptéry budou odebrány až tyto virtuální počítače se restartují. V operačních systémech, které používají model směrování slabé hostitele, jako je například Linux můžete přerušení připojení k Internetu, pokud příchozí a odchozí provoz používat různé síťové adaptéry.
> 

### <a name="configure-windows-vms"></a>Konfigurace virtuálních počítačů Windows
Předpokládejme, že máte virtuální počítač s Windows se dvěma síťovými adaptéry, následujícím způsobem:

* Primární síťovou kartu IP adresa: 192.168.1.4
* Sekundární síťový adaptér IP adresa: 192.168.2.5

Tabulka směrování IPv4 pro tento virtuální počítač bude vypadat takto:

    IPv4 Route Table
    ===========================================================================
    Active Routes:
    Network Destination        Netmask          Gateway       Interface  Metric
              0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
            127.0.0.0        255.0.0.0         On-link         127.0.0.1    306
            127.0.0.1  255.255.255.255         On-link         127.0.0.1    306
      127.255.255.255  255.255.255.255         On-link         127.0.0.1    306
        168.63.129.16  255.255.255.255      192.168.1.1      192.168.1.4      6
          192.168.1.0    255.255.255.0         On-link       192.168.1.4    261
          192.168.1.4  255.255.255.255         On-link       192.168.1.4    261
        192.168.1.255  255.255.255.255         On-link       192.168.1.4    261
          192.168.2.0    255.255.255.0         On-link       192.168.2.5    261
          192.168.2.5  255.255.255.255         On-link       192.168.2.5    261
        192.168.2.255  255.255.255.255         On-link       192.168.2.5    261
            224.0.0.0        240.0.0.0         On-link         127.0.0.1    306
            224.0.0.0        240.0.0.0         On-link       192.168.1.4    261
            224.0.0.0        240.0.0.0         On-link       192.168.2.5    261
      255.255.255.255  255.255.255.255         On-link         127.0.0.1    306
      255.255.255.255  255.255.255.255         On-link       192.168.1.4    261
      255.255.255.255  255.255.255.255         On-link       192.168.2.5    261
    ===========================================================================

Všimněte si, že výchozí trasa (0.0.0.0) je dostupná jenom na primární síťový adaptér. Nebudete mít přístup k prostředkům mimo podsíť sekundárního síťového adaptéru, jak vidíte níže:

    C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5

    Pinging 192.168.1.7 from 192.165.2.5 with 32 bytes of data:
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.

Přidat výchozí trasu na sekundární síťový adaptér, postupujte podle následujících kroků:

1. Z příkazového řádku spusťte následující příkaz k identifikaci číslo indexu pro sekundární síťový adaptér:
   
        C:\Users\Administrator>route print
        ===========================================================================
        Interface List
         29...00 15 17 d9 b1 6d ......Microsoft Virtual Machine Bus Network Adapter #16
         27...00 15 17 d9 b1 41 ......Microsoft Virtual Machine Bus Network Adapter #14
          1...........................Software Loopback Interface 1
         14...00 00 00 00 00 00 00 e0 Teredo Tunneling Pseudo-Interface
         20...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #2
        ===========================================================================
2. Všimněte si, druhý záznam v tabulce s indexem 27 (v tomto příkladu).
3. Z příkazového řádku, spusťte **přidat trasy** příkaz, jak je uvedeno níže. V tomto příkladu jsou určení 192.168.2.1 jako výchozí brána pro sekundární síťový adaptér:
   
        route ADD -p 0.0.0.0 MASK 0.0.0.0 192.168.2.1 METRIC 5000 IF 27
4. K testování připojení, přejděte zpět na příkazovém řádku a zkuste příkazem ping otestovat jiné podsíti z sekundárního síťového adaptéru jako uvedené int eh následujícím příkladu:
   
        C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5
   
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time=2ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
5. Můžete také zkontrolovat tabulky tras Zkontrolujte nově přidané postup, jak je uvedeno níže:
   
        C:\Users\Administrator>route print
   
        ...
   
        IPv4 Route Table
        ===========================================================================
        Active Routes:
        Network Destination        Netmask          Gateway       Interface  Metric
                  0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
                  0.0.0.0          0.0.0.0      192.168.2.1      192.168.2.5   5005
                127.0.0.0        255.0.0.0         On-link         127.0.0.1    306

### <a name="configure-linux-vms"></a>Konfigurovat virtuální počítače s Linuxem
Pro virtuální počítače s Linuxem protože používá výchozí chování slabé hostitele směrování, doporučujeme, aby sekundární síťové adaptéry jsou omezeny na přenosové toky pouze ve stejné podsíti. Pokud některé scénáře potřebují připojení mimo podsíť, ale měli uživatelé povolit na základě zásad směrování zajistit, že příchozí a odchozí provoz používá stejný síťový adaptér.

## <a name="next-steps"></a>Další kroky
* Nasazení [MultiNIC virtuálních počítačů v aplikaci na vrstvě 2 scénář v nasazení Resource Manager](virtual-network-deploy-multinic-arm-template.md).
* Nasazení [MultiNIC virtuálních počítačů v aplikaci na vrstvě 2 scénář v nasazení classic](virtual-network-deploy-multinic-classic-ps.md).

