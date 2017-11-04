---
title: "Síťová rozhraní pro přidání nebo odebrání z virtuálních počítačů Azure | Microsoft Docs"
description: "Naučte se síťová rozhraní pro přidání nebo odebrání síťová rozhraní z virtuálních počítačů."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: jdial
ms.openlocfilehash: 7df1dfbea8c985907d5330819dc1e7bf1578aafa
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2017
---
# <a name="add-network-interfaces-to-or-remove-from-virtual-machines"></a>Síťová rozhraní pro přidání nebo odebrání z virtuálních počítačů

Informace o postupu přidání existujícího síťového rozhraní, při vytváření virtuálního počítače nebo přidat nebo odebrat síťová rozhraní ze stávajícího virtuálního počítače v zastaveném stavu (deallocated). Síťové rozhraní umožňuje virtuálního počítače (virtuální počítač Azure) ke komunikaci s Internetem, Azure a místních prostředků. Virtuální počítač může mít jeden nebo víc síťových rozhraní. 

Pokud potřebujete přidat, změnit nebo odebrat IP adresy pro síťové rozhraní, přečtěte si [Správa IP adres rozhraní sítě](virtual-network-network-interface-addresses.md) článku. Pokud třeba chcete vytvořit, změnit nebo odstranit síťové rozhraní, přečtěte si [Spravovat síťová rozhraní](virtual-network-network-interface.md) článku.

## <a name="before"></a>Než začnete

Před dokončením všech kroků v žádné části tohoto článku dokončete následující úlohy:

- Další informace o tom, kolik síťových rozhraní velikost pro všechny systémy Linux a virtuální počítač s Windows podporuje kontrolou [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) nebo [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) články velikostí virtuálních počítačů.
- Přihlaste se k Azure [portál](https://portal.azure.com), rozhraní příkazového řádku Azure (CLI) nebo Azure PowerShell s účet Azure. Pokud nemáte účet Azure, si zaregistrovat [Bezplatný zkušební účet](https://azure.microsoft.com/free).
- Pokud pomocí příkazů prostředí PowerShell k dokončení úloh v tomto článku [instalace a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Ujistěte se, že máte nejnovější verzi rutin prostředí Azure PowerShell nainstalovaný. Chcete-li získat nápovědu pro příkazy prostředí PowerShell s příklady, zadejte `get-help <command> -full`.
- Pokud používáte rozhraní příkazového řádku Azure (CLI) příkazy k dokončení úloh v tomto článku [instalace a konfigurace rozhraní příkazového řádku Azure](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Zajistěte, že abyste měli nejnovější verzi rozhraní příkazového řádku Azure, který je nainstalovaný. Chcete-li získat nápovědu pro příkazy rozhraní příkazového řádku, zadejte `az <command> --help`. Místo instalace rozhraní příkazového řádku a jeho požadavky, můžete použít prostředí cloudové služby Azure. Služba Azure Cloud Shell je volně dostupné prostředí Bash, které můžete spustit přímo z portálu Azure Portal. Má předinstalované rozhraní Azure CLI, které je nakonfigurované pro použití s vaším účtem. Chcete-li použít cloudové prostředí, klikněte na tlačítko prostředí cloudu **> _** tlačítka v horní části [portál](https://portal.azure.com).

## <a name="about"></a>O rozhraní sítě a virtuální počítače

Můžete přidat (připojení) existující rozhraní sítě k virtuálnímu počítači při vytváření virtuálních počítačů, které poskytuje síťového rozhraní není aktuálně připojena k jiným virtuálním Počítačem. Můžete přidat síťové rozhraní k nebo odebrat (odpojit) síťové rozhraní ze stávajícího virtuálního počítače, zadaný virtuální počítač je v zastaveném stavu (deallocated). Pokud vytvoříte virtuální počítač pomocí portálu Azure, portál vytvoří síťové rozhraní s výchozím nastavením. Na portálu neumožňuje vám:

- Zadejte existující rozhraní sítě přidat při vytváření virtuálního počítače
- Vytvoření virtuálního počítače s několika síťovými rozhraními
- Zadejte název pro rozhraní sítě (portálu vytvoří síťové rozhraní s výchozím názvem)

Azure PowerShell nebo rozhraní příkazového řádku můžete použít k vytvoření virtuálního počítače nebo síťové rozhraní s předchozí atributy, které nelze použít na portálu. Před dokončením úkoly v následující části, vezměte v úvahu následující omezení a chování:

- Všechny velikosti virtuálních počítačů podpory aspoň dvě rozhraní sítě, avšak některé velikosti virtuálních počítačů podporovat více než dvě síťová rozhraní. V minulosti některé velikosti virtuálních počítačů podporována pouze jedno síťové rozhraní. Chcete-li zjistit, kolik síťových rozhraní každý podporuje velikost virtuálního počítače, přečtěte si [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) nebo [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) články velikostí virtuálních počítačů. 
- V minulosti nebylo možné síťových rozhraní přidat pouze do virtuálních počítačů, které jsou podporovány více síťovými rozhraními a byly vytvořeny s aspoň dvě rozhraní sítě. Síťové rozhraní nelze přidat k virtuálnímu počítači, který byl vytvořen s jedno síťové rozhraní, i v případě, že velikost virtuálního počítače podporované více síťových rozhraní. Naopak je může odebrat pouze síťová rozhraní z virtuálního počítače s aspoň tři síťová rozhraní, protože virtuální počítače vytvořené pomocí aspoň dva síťové rozhraní se vždy museli mít aspoň dvě rozhraní sítě. Ani jeden z těchto omezení platí už. Můžete nyní vytvořte virtuální počítač s libovolný počet síťových rozhraní (až číslo nepodporuje velikost virtuálního počítače) a přidat nebo odebrat libovolný počet síťových rozhraní (z virtuálních počítačů v zastaveném stavu (deallocated)), tak dlouho, dokud virtuální počítač má vždy alespoň jedno síťové rozhraní.
- Ve výchozím nastavení, je definován první síťové rozhraní ve virtuálním počítači, jako *primární* síťové rozhraní. Všechny ostatní síťová rozhraní virtuálních počítačů jsou *sekundární* síťových rozhraní.
- Primární síťová rozhraní přiřazené výchozí bránu servery Azure DHCP, ale nejsou sekundární síťová rozhraní. Vzhledem k tomu, že sekundární síťová rozhraní nejsou přiřazené výchozí bránu, nemohou komunikovat s prostředky mimo jejich podsíť, ve výchozím nastavení. Pokud chcete povolit sekundární síťová rozhraní pro komunikaci s prostředky mimo jejich podsíť, najdete v části [směrování v rámci operačního systému virtuálního počítače s více síťovými rozhraními](#routing-within-a-virtual-machine-operating-system-with-multiple-network-interfaces).
- Ve výchozím nastavení všechny odchozí přenosy z virtuálního počítače odeslání přiřazené primární konfiguraci IP primární síťové rozhraní IP adresy. Můžete řídit, která IP adresa se používá pro odchozí provoz v rámci operačního systému Virtuálního počítače, ale ve výchozím nastavení, je prostřednictvím primární síťové rozhraní.
- V minulosti všechny virtuální počítače ve stejné skupině dostupnosti musely mít jeden nebo víc síťových rozhraní. Virtuální počítače s libovolný počet síťových rozhraní může nyní existovat ve stejné sadě dostupnosti, až na číslo nepodporuje velikost virtuálního počítače. Virtuální počítač můžete přidat pouze pro sadu, když je vytvořeno, když dostupnosti. Další informace o nastavení dostupnosti, najdete [Správa dostupnosti virtuálních počítačů v Azure](../virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) článku.
- Při síťová rozhraní stejného virtuálního počítače může být připojen k různým podsítím v rámci virtuální sítě, rozhraní sítě musí všechny připojeny ke stejné síti VNet.
- Přidáním jakékoli IP adresu pro všechny konfigurace IP jakékoli primární nebo sekundární síťové rozhraní na fond back-end pro vyrovnávání zatížení Azure. V minulosti pouze primární IP adresa pro primární síťové rozhraní nebylo možné přidat do fondu back-end. Další informace o IP adresy a konfigurace, najdete [přidat, změnit nebo odebrat IP adres](virtual-network-network-interface-addresses.md) článku.
- Odstranění virtuálního počítače neodstraní rozhraní sítě, které jsou připojené k němu. Při odstranění virtuálního počítače jsou z virtuálního počítače odpojit rozhraní sítě. Můžete přidat rozhraní sítě do jiné virtuální počítače, nebo je odstranit.
- Pokud síťové rozhraní s privátní adresou IPv6 přiřazen, můžete připojit k virtuálnímu počítači při vytváření virtuálního počítače. Síťové rozhraní s přiřazenou adresu IPv6 nelze připojit k virtuálnímu počítači po vytvoření virtuálního počítače. Pokud je při vytváření virtuálního počítače připojit síťové rozhraní s přiřazenou privátní adresu IPv6, je možné ještě připojit rozhraní sítě k virtuálnímu počítači, bez ohledu na to, kolik síťových rozhraní, které podporuje velikost virtuálního počítače. V tématu [sítě IP adresy rozhraní](virtual-network-network-interface-addresses.md) Další informace o přiřazování IP adresy pro síťová rozhraní.

## <a name="vm-create"></a>Přidat existující rozhraní sítě na nový virtuální počítač

Při vytváření virtuálního počítače prostřednictvím portálu portál vytvoří rozhraní sítě s výchozím nastavením a připojí jej k virtuálnímu počítači. Nelze přidat existující rozhraní sítě na nový virtuální počítač nebo vytvoření virtuálního počítače s více síťovými rozhraními, pomocí portálu Azure. Můžete provést i pomocí rozhraní CLI nebo Powershellu. Můžete přidat libovolný počet síťových rozhraní pro virtuální počítač podporuje velikost virtuálního počítače, kterou vytváříte. Další informace o tom, kolik síťových rozhraní podporuje každý velikost virtuálního počítače, přečtěte si [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) nebo [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) články velikostí virtuálních počítačů. Síťových rozhraní, které přidáte do virtuálního počítače nemůže aktuálně připojen k jiným virtuálním Počítačem. Další informace o vytváření síťových rozhraní, najdete [Spravovat síťová rozhraní](virtual-network-network-interface.md#create-a-network-interface) článku.

### <a name="routing-within-a-virtual-machine-operating-system-with-multiple-network-interfaces"></a>Směrování v rámci operačního systému virtuálního počítače s více síťovými rozhraními

Azure přiřadí výchozí bránu na první (primární) síťové rozhraní připojené k virtuálnímu počítači. Azure nepřiřazuje výchozí bránu pro další (sekundární) síťových rozhraní připojených k virtuálnímu počítači. Jste tedy nemůže komunikovat s prostředky mimo podsítě, která je sekundární síťové rozhraní, ve výchozím nastavení. Sekundární síťová rozhraní může, ale komunikovat s prostředky mimo jejich podsíť, když postup povolení komunikace se liší pro různé operační systémy.

### <a name="windows"></a>Windows

Proveďte následující kroky z příkazového řádku systému Windows:

1. Spustit `route print` příkaz, který vrátí výstup podobný následujícímu výstupu pro virtuální počítač s dvě rozhraní připojené síti:

    ```
    ===========================================================================
    Interface List
    3...00 0d 3a 10 92 ce ......Microsoft Hyper-V Network Adapter #3
    7...00 0d 3a 10 9b 2a ......Microsoft Hyper-V Network Adapter #4
    ===========================================================================
    ```
 
    V tomto příkladu **Microsoft Hyper-V síťový adaptér č. 4** (rozhraní 7) je sekundární síťové rozhraní, který nemá přiřazen výchozí bránu.

2. Z příkazového řádku, spusťte `ipconfig` příkazu zobrazte IP adresu, která je přiřazena k sekundární síťové rozhraní. V tomto příkladu je 192.168.2.4 přiřazená rozhraní 7. Pro sekundární síťové rozhraní je vrácena žádná adresu výchozí brány.

3. Směrovat všechny přenosy určené pro adresy mimo podsíť sekundární síťové rozhraní k bráně pro podsíť, spusťte následující příkaz:

    ```
    route add -p 0.0.0.0 MASK 0.0.0.0 192.168.2.1 METRIC 5015 IF 7
    ```

    Adresa brány podsítě je první IP adresa (končící na.1) do rozsahu adres, které jsou definované pro podsíť. Pokud nechcete, aby směrovat všechny přenosy mimo podsítě, můžete přidat jednotlivých tras místo toho k určitým příjemcům. Například, pokud chcete směrovat přenosy z sekundární síťové rozhraní 192.168.3.0 síť, zadejte příkaz:

      ```
      route add -p 192.168.3.0 MASK 255.255.255.0 192.168.2.1 METRIC 5015 IF 7
      ```
  
4. Pro potvrzení úspěšné komunikace s k prostředku 192.168.3.0 sítě, například zadejte následující příkaz k otestování příkazem ping 192.168.3.4 pomocí rozhraní 7 (192.168.2.4):

    ```
    ping 192.168.3.4 -S 192.168.2.4
    ```

    Budete muset otevřít ICMP přes bránu Windows firewall, zařízení, které jste otestováním pomocí následujícího příkazu:
  
      ```
      netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
      ```
  
5. Pokud chcete potvrdit, cesta přidala je v tabulce směrování, zadejte `route print` příkaz, který vrátí výstup podobný následujícímu:

    ```
    ===========================================================================
    Active Routes:
    Network Destination        Netmask          Gateway       Interface  Metric
              0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4     15
              0.0.0.0          0.0.0.0      192.168.2.1      192.168.2.4   5015
    ```

    Trasy označené *192.168.1.1* pod **brány**, je trasy, která je k dispozici ve výchozím nastavení pro primární síťové rozhraní. Trasa *192.168.2.1* pod **brány**, je trasy, které jste přidali.

### <a name="linux"></a>Linux

Vzhledem k tomu, že výchozí chování používá slabé hostitele směrování, doporučujeme omezení sekundární síťový provoz mezi prostředky ve stejné podsíti. Pokud budete potřebovat komunikace mimo podsíť pro sekundární síťová rozhraní, musíte vytvořit pravidla směrování, které umožňují odesílat a přijímat přenosy přes konkrétní síťové rozhraní virtuálnímu počítači. Jinak provoz, který patří do eth1, například nelze zpracovat správně podle definovaného výchozí trasu. Zjistěte, jak nakonfigurovat pravidla směrování, najdete v tématu [Linux nakonfigurovat pro několik síťových adaptérů](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics).

> [!WARNING]
> Pokud síťové rozhraní s privátní adresou IPv6 přiřazen, pouze přidáním síťové rozhraní virtuálnímu počítači při vytváření virtuálního počítače. Více než jedno síťové rozhraní nelze připojit k virtuálnímu počítači při vytváření virtuálního počítače nebo po virtuální počítač vytvořený, stejně dlouho jako IPv6 adresa je přiřazena k síťovému rozhraní připojené k virtuálnímu počítači. V tématu [sítě IP adresy rozhraní](virtual-network-network-interface-addresses.md) Další informace o přiřazování IP adresy pro síťová rozhraní.

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Rozhraní příkazového řádku|[Vytvoření virtuálního počítače az](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Nový AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vm-add-nic"></a>Přidat existující rozhraní sítě do stávajícího virtuálního počítače

Můžete přidat libovolný počet síťových rozhraní pro virtuální počítač velikost virtuálního počítače, kterou chcete přidat rozhraní sítě, které se podporuje. Chcete-li zjistit, kolik síťových rozhraní každý podporuje velikost virtuálního počítače, přečtěte si [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) nebo [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) články velikostí virtuálních počítačů. Virtuální počítač, který chcete přidat rozhraní sítě, aby musí podporovat počet síťových rozhraní, které chcete přidat a musí být v zastaveném stavu (deallocated). Síťových rozhraní, které chcete přidat nemůže aktuálně připojen k jiným virtuálním Počítačem. Síťová rozhraní nelze přidat do stávajícího virtuálního počítače pomocí portálu Azure. Pokud chcete přidat do existující virtuální počítač síťových rozhraní, musíte použít rozhraní CLI nebo Powershellu. 

Azure přiřadí výchozí bránu na první (primární) síťové rozhraní připojené k virtuálnímu počítači. Azure nepřiřazuje výchozí bránu pro další (sekundární) síťových rozhraní připojených k virtuálnímu počítači. Jste tedy nemůže komunikovat s prostředky mimo podsítě, která je sekundární síťové rozhraní, ve výchozím nastavení. Sekundární síťová rozhraní může, ale komunikovat s prostředky mimo jejich podsítě. Pokud požadujete, aby sekundární síťová rozhraní komunikace s prostředky mimo jejich podsíť, najdete v části [směrování v rámci operačního systému virtuálního počítače s více síťovými rozhraními](#routing-within-a virtual-machine-operating-system-with-multiple-network-interfaces).

> [!WARNING]
> Pokud síťové rozhraní s privátní adresou IPv6 přiřazen, nelze přidat do existujícího virtuálního počítače. Při vytváření virtuálního počítače, můžete přidat pouze síťové rozhraní s přiřazená privátní adresa IPv6 k virtuálnímu počítači. V tématu [sítě IP adresy rozhraní](virtual-network-network-interface-addresses.md) Další informace o přiřazování IP adresy pro síťová rozhraní.

|Nástroj|Příkaz|
|---|---|
|Rozhraní příkazového řádku|[Přidejte síťový adaptér virtuálního počítače az](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#add) (referenční dokumentace) nebo [podrobné kroky](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-a-vm)|
|PowerShell|[Přidat AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (referenční dokumentace) nebo [podrobné kroky](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-an-existing-vm)|

## <a name="vm-view-nic"></a>Zobrazení síťových rozhraní pro virtuální počítač

Můžete zobrazit rozhraní sítě, který je aktuálně připojen k virtuálnímu počítači další informace o konfiguraci každé síťové rozhraní a IP adresy přiřazené k každé síťové rozhraní. 

1. Přihlaste se k [portál Azure](https://portal.azure.com) pomocí účtu, který má přiřazenou roli vlastník, Přispěvatel nebo Přispěvatel sítě pro vaše předplatné. Další informace o přiřazení rolí na účty, najdete v části [předdefinované role pro řízení přístupu Azure na základě rolí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Do pole, která obsahuje text *vyhledávání prostředků* v horní části portálu Azure, zadejte *virtuální počítače*. Když **virtuální počítače** se zobrazí ve výsledcích hledání klikněte na něj.
3. V **virtuální počítače** okno, které se zobrazí, klikněte na název virtuálního počítače, které chcete zobrazit síťová rozhraní pro.
4. V **nastavení** části okně virtuálního počítače, který se zobrazí pro virtuální počítač jste vybrali, klikněte na tlačítko **sítě**. Další informace o nastavení síťového rozhraní a jak ji změnit, přečtěte si [Spravovat síťová rozhraní](virtual-network-network-interface.md) článku. Další informace o přidání, změně nebo odebrání IP adresy přiřazené k síťovému rozhraní, najdete v části [Správa IP adres](virtual-network-network-interface-addresses.md).

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Rozhraní příkazového řádku|[Zobrazit az virtuálních počítačů](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#show)|
|PowerShell|[Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vm-remove-nic"></a>Odstranit síťové rozhraní z virtuálního počítače

Virtuální počítač, který chcete odebrat (nebo odpojit) síťové rozhraní z musí být v zastaveném stavu (deallocated) a musí mít alespoň dvě síťová rozhraní připojená k ho. Můžete odebrat všechny síťové rozhraní, ale virtuální počítač musí mít vždy alespoň jedno síťové rozhraní, které jsou k němu připojen. Pokud odeberete primární síťové rozhraní, Azure přiřadí atribut primární síťové rozhraní, které je byl připojen k virtuálnímu počítači nejdelší. 

1. Přihlaste se k [portál Azure](https://portal.azure.com) pomocí účtu, který má přiřazenou roli vlastník, Přispěvatel nebo Přispěvatel sítě pro vaše předplatné. Další informace o přiřazení rolí na účty, najdete v části [předdefinované role pro řízení přístupu Azure na základě rolí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Do pole, která obsahuje text *vyhledávání prostředků* v horní části portálu Azure, zadejte *virtuální počítače*. Když **virtuální počítače** se zobrazí ve výsledcích hledání klikněte na něj.
3. V **virtuální počítače** okno, které se zobrazí, klikněte na název virtuálního počítače, které chcete odstranit síťové rozhraní pro.
4. V **nastavení** části okně virtuálního počítače, který se zobrazí pro virtuální počítač jste vybrali, klikněte na tlačítko **sítě**. Další informace o nastavení síťového rozhraní a jak ji změnit, přečtěte si [Spravovat síťová rozhraní](virtual-network-network-interface.md) článku. Další informace o přidání, změně nebo odebrání IP adresy přiřazené k síťovému rozhraní, najdete v části [Správa IP adres](virtual-network-network-interface-addresses.md).
5. Klikněte na tlačítko **X Odpojit síťové rozhraní**.
6. Vyberte síťové rozhraní, které chcete odpojit z rozevíracího seznamu a pak klikněte na **OK**.

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Rozhraní příkazového řádku|[síťový adaptér virtuálního počítače az odebrat](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#remove) (referenční dokumentace) nebo [podrobné kroky](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#remove-a-nic-from-a-vm)|
|PowerShell|[Odebrat AzureRMVMNetworkInterface](/powershell/module/azurerm.compute/remove-azurermvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (referenční dokumentace) nebo [podrobné kroky](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#remove-a-nic-from-an-existing-vm)|

## <a name="next-steps"></a>Další kroky
Vytvoření virtuálního počítače s více síťových rozhraní nebo IP adresy, přečtěte si následující články:

**Příkazy**

|Úkol|Nástroj|
|---|---|
|Vytvoření virtuálního počítače s několika síťovými kartami|[Rozhraní příkazového řádku](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [prostředí PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Vytvoření jednoho virtuálního počítače síťový adaptér s více adresami IPv4|[Rozhraní příkazového řádku](virtual-network-multiple-ip-addresses-cli.md), [prostředí PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Vytvoření jednoho virtuálního počítače síťový adaptér s privátní adresou IPv6 (za pro vyrovnávání zatížení Azure)|[Rozhraní příkazového řádku](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [prostředí PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [šablony Azure Resource Manageru](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
