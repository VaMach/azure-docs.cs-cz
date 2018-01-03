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
ms.date: 12/15/2017
ms.author: jdial
ms.openlocfilehash: be264693b579aacee39acd9196b4e6834da322b2
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/18/2017
---
# <a name="add-network-interfaces-to-or-remove-network-interfaces-from-virtual-machines"></a>Síťová rozhraní pro přidání nebo odebrání síťová rozhraní z virtuálních počítačů

Informace o postupu přidání existujícího síťového rozhraní, při vytváření virtuálního počítače nebo přidat nebo odebrat síťová rozhraní ze stávajícího virtuálního počítače v zastaveném stavu (deallocated). Síťové rozhraní umožňuje virtuálního počítače (virtuální počítač Azure) ke komunikaci s Internetem, Azure a místních prostředků. Virtuální počítač může mít jeden nebo víc síťových rozhraní. 

Pokud potřebujete přidat, změnit nebo odebrat IP adresy pro síťové rozhraní, přečtěte si [Správa IP adres rozhraní sítě](virtual-network-network-interface-addresses.md) článku. Pokud třeba chcete vytvořit, změnit nebo odstranit síťové rozhraní, přečtěte si [Spravovat síťová rozhraní](virtual-network-network-interface.md) článku.

## <a name="before"></a>Než začnete

Před dokončením všech kroků v žádné části tohoto článku dokončete následující úlohy:

- Přihlaste se k Azure [portál](https://portal.azure.com), rozhraní příkazového řádku Azure (CLI) nebo Azure PowerShell s účet Azure. Pokud nemáte účet Azure, si zaregistrovat [Bezplatný zkušební účet](https://azure.microsoft.com/free).
- Pokud pomocí příkazů prostředí PowerShell k dokončení úloh v tomto článku [instalace a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Ujistěte se, že máte nejnovější verzi rutin prostředí Azure PowerShell nainstalovaný. Chcete-li získat nápovědu pro příkazy prostředí PowerShell s příklady, zadejte `get-help <command> -full`. Místo instalace prostředí Azure PowerShell, můžete použít prostředí cloudové služby Azure. Prostředí cloudové služby Azure je bezplatná prostředí PowerShell, který můžete spustit přímo v portálu Azure. Má prostředí Azure PowerShell předem nainstalován a nakonfigurován pro použití s vaším účtem. Chcete-li použít cloudové prostředí, klikněte na tlačítko prostředí cloudu **> _** tlačítka v horní části [portál](https://portal.azure.com) a v levém horním rohu vyberte prostředí PowerShell, až se zobrazí okno prostředí.
- Pokud používáte rozhraní příkazového řádku Azure (CLI) příkazy k dokončení úloh v tomto článku [instalace a konfigurace rozhraní příkazového řádku Azure](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Zajistěte, že abyste měli nejnovější verzi rozhraní příkazového řádku Azure, který je nainstalovaný. Chcete-li získat nápovědu pro příkazy rozhraní příkazového řádku, zadejte `az <command> --help`. Místo instalace rozhraní příkazového řádku a jeho požadavky, můžete použít prostředí cloudové služby Azure. Služba Azure Cloud Shell je volně dostupné prostředí Bash, které můžete spustit přímo z portálu Azure Portal. Má předinstalované rozhraní Azure CLI, které je nakonfigurované pro použití s vaším účtem. Chcete-li použít cloudové prostředí, klikněte na tlačítko prostředí cloudu **> _** tlačítka v horní části [portál](https://portal.azure.com) a vyberte v levém horním rohu Bash, když se zobrazí v okně prostředí.

## <a name="vm-create"></a>Přidat existující rozhraní sítě na nový virtuální počítač

Při vytváření virtuálního počítače prostřednictvím portálu portál vytvoří rozhraní sítě s výchozím nastavením a připojí jej k virtuálnímu počítači. Nelze přidat existující rozhraní sítě na nový virtuální počítač nebo vytvoření virtuálního počítače s více síťovými rozhraními, pomocí portálu Azure. Můžete provést i pomocí rozhraní CLI nebo Powershellu. Před použitím prostředí PowerShell nebo rozhraní příkazového řádku ale vytvoření virtuálního počítače s existující rozhraní sítě, seznamte se s [omezení](#constraints). Pokud vytvoříte virtuální počítač s více síťovými rozhraními, musíte také nakonfigurovat operačního systému správně používat po vytvoření virtuálního počítače. Podrobnosti najdete v tématu Konfigurace [Linux](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) nebo [Windows](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) pro více síťových rozhraní.

**Příkazy** před vytvořením virtuálního počítače, vytvořte síťové rozhraní, pomocí kroků v [vytvořit síťové rozhraní](virtual-network-network-interface.md#create-a-network-interface).

|Nástroj|Příkaz|
|---|---|
|Rozhraní příkazového řádku|[Vytvoření virtuálního počítače az](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Nový AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vm-add-nic"></a>Přidejte síťové rozhraní stávajícího virtuálního počítače

1. Přihlaste se k portálu Azure.
2. Do vyhledávacího pole v horní části portálu, vyhledejte název virtuálního počítače, které chcete přidat rozhraní sítě, nebo vyhledejte pro virtuální počítač kliknutím **všechny služby**, pak **virtuální počítače**. Jakmile jste najít virtuálního počítače, klikněte na něj. Virtuální počítač, který chcete přidat rozhraní sítě, aby musí podporovat počet síťových rozhraní, které chcete přidat. Chcete-li zjistit, kolik síťových rozhraní každý podporuje velikost virtuálního počítače, přečtěte si [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) nebo [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) články velikostí virtuálních počítačů.  
3. Klikněte na tlačítko **přehled**v části **nastavení**. Klikněte na tlačítko **Zastavit**a počkejte **stav** virtuálního počítače se změní na *zastaveném (nepřiřazeném)*. 
4. Klikněte na tlačítko **sítě**v části **nastavení**.
5. Klikněte na tlačítko **připojit síťové rozhraní**. Ze seznamu stávajících síťových rozhraní, které nejsou aktuálně připojena k jiným virtuálním Počítačem klikněte na síťové rozhraní, které byste chtěli připojit. Síťové rozhraní, které vyberete nemůže mít accelerated sítě povolené, nemůže mít přiřazené adresy IPv6 a musí být ve stejné virtuální síti jako virtuální síť síťového rozhraní v současnosti připojená k virtuálnímu počítači se. Pokud nemáte existující rozhraní sítě, musíte nejprve vytvoříte jeden. Chcete-li vytvořit rozhraní sítě, klikněte na tlačítko **vytvořit síťové rozhraní**. Další informace o vytváření rozhraní sítě najdete v tématu [vytvořit síťové rozhraní](virtual-network-network-interface.md#create-a-network-interface). Další informace o dalších omezení při přidávání síťová rozhraní pro virtuální počítače, najdete v části [omezení](#constraints).
6. Klikněte na **OK**.
7. Klikněte na tlačítko **přehled**v části **nastavení**. Klikněte na tlačítko **spustit** ke spuštění virtuálního počítače.
8. Konfigurace operačního systému virtuálního počítače odpovídajícím způsobem používat více síťových rozhraní. Podrobnosti najdete v tématu Konfigurace [Linux](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) nebo [Windows](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) pro více síťových rozhraní.

|Nástroj|Příkaz|
|---|---|
|Rozhraní příkazového řádku|[Přidejte síťový adaptér virtuálního počítače az](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#add) (referenční dokumentace) nebo [podrobné kroky](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-a-vm)|
|PowerShell|[Přidat AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (referenční dokumentace) nebo [podrobné kroky](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-an-existing-vm)|

## <a name="vm-view-nic"></a>Zobrazení síťových rozhraní pro virtuální počítač

Můžete zobrazit rozhraní sítě, který je aktuálně připojen k virtuálnímu počítači další informace o konfiguraci každé síťové rozhraní a IP adresy přiřazené k každé síťové rozhraní. 

1. Přihlaste se k [portál Azure](https://portal.azure.com) pomocí účtu, který má přiřazenou roli vlastník, Přispěvatel nebo Přispěvatel sítě pro vaše předplatné. Další informace o přiřazení rolí na účty, najdete v části [předdefinované role pro řízení přístupu Azure na základě rolí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Do pole, která obsahuje text *vyhledávání prostředků* v horní části portálu Azure, zadejte *virtuální počítače*. Když **virtuální počítače** se zobrazí ve výsledcích hledání klikněte na něj.
3. Klikněte na název virtuálního počítače, které chcete zobrazit síťová rozhraní pro.
4. V **nastavení** část virtuálního počítače jste vybrali, klikněte na tlačítko **sítě**. Další informace o nastavení síťového rozhraní a jak ji změnit, najdete v části [Spravovat síťová rozhraní](virtual-network-network-interface.md). Další informace o přidání, změně nebo odebrání IP adresy přiřazené k síťovému rozhraní, najdete v části [Správa IP adres](virtual-network-network-interface-addresses.md).

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Rozhraní příkazového řádku|[Zobrazit az virtuálních počítačů](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#show)|
|PowerShell|[Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vm-remove-nic"></a>Odstranit síťové rozhraní z virtuálního počítače

1. Přihlaste se k portálu Azure.
2. Do vyhledávacího pole v horní části portálu, vyhledejte název virtuálního počítače, kterou chcete odebrat (odpojit) síťové rozhraní z nebo procházení pro virtuální počítač kliknutím **všechny služby**, pak **virtuální počítače**. Jakmile jste najít virtuálního počítače, klikněte na něj.
3. Klikněte na tlačítko **přehled**v části **nastavení**. Klikněte na tlačítko **Zastavit**a počkejte **stav** virtuálního počítače se změní na *zastaveném (nepřiřazeném)*. 
4. Klikněte na tlačítko **sítě**v části **nastavení**.
5. Klikněte na tlačítko **Odpojit síťové rozhraní**. V seznamu síťových rozhraní, které jsou aktuálně připojen k virtuálnímu počítači klikněte na tlačítko, které chcete odpojit síťové rozhraní. Pokud pouze jedno síťové rozhraní je uvedena, nelze, odpojit, protože virtuální počítač musí mít vždy alespoň jedno síťové rozhraní, které jsou k němu připojen.
6. Klikněte na **OK**.

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

## <a name="constraints"></a>Omezení

- Virtuální počítač musí mít aspoň jedno síťové rozhraní, které jsou k němu připojen.
- Virtuální počítač může mít pouze jako mnoho síťových rozhraní, které jsou k němu připojen jako podporuje velikost virtuálního počítače. Další informace o tom, kolik síťových rozhraní každý velikost virtuálního počítače podporuje, najdete v části [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) a [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) velikosti virtuálních počítačů. Všech velikostí podporovat aspoň dvě rozhraní sítě.
- Síťových rozhraní, které přidáte do virtuálního počítače nemůže aktuálně připojen k jiným virtuálním Počítačem. Další informace o vytváření rozhraní sítě najdete v tématu [vytvořit síťové rozhraní](virtual-network-network-interface.md#create-a-network-interface).
- V minulosti nebylo možné síťových rozhraní přidat pouze do virtuálních počítačů, které jsou podporovány více síťovými rozhraními a byly vytvořeny s aspoň dvě rozhraní sítě. Síťové rozhraní nelze přidat k virtuálnímu počítači, který byl vytvořen s jedno síťové rozhraní, i v případě, že velikost virtuálního počítače podporované více síťových rozhraní. Naopak je může odebrat pouze síťová rozhraní z virtuálního počítače s aspoň tři síťová rozhraní, protože virtuální počítače vytvořené pomocí aspoň dva síťové rozhraní se vždy museli mít aspoň dvě rozhraní sítě. Ani jeden z těchto omezení platí už. Nyní můžete vytvořit virtuální počítač s libovolný počet síťových rozhraní (až číslo nepodporuje velikost virtuálního počítače).
- Ve výchozím nastavení, je definován první síťové rozhraní připojené k virtuálnímu počítači, jako *primární* síťové rozhraní. Všechny ostatní síťová rozhraní virtuálních počítačů jsou *sekundární* síťových rozhraní.
- I když můžete řídit, které síťové rozhraní odeslán odchozí přenosy, ve výchozím nastavení, přiřazené primární konfiguraci IP primární síťové rozhraní IP adresy odeslání všechny odchozí přenosy z virtuálního počítače.
- V minulosti všechny virtuální počítače ve stejné skupině dostupnosti musely mít jeden nebo víc síťových rozhraní. Virtuální počítače s libovolný počet síťových rozhraní může nyní existovat ve stejné sadě dostupnosti, až na číslo nepodporuje velikost virtuálního počítače. Virtuální počítač můžete přidat pouze pro sadu, když je vytvořeno, když dostupnosti. Další informace o nastavení dostupnosti, najdete [Správa dostupnosti virtuálních počítačů v Azure](../virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) článku.
- Při síťová rozhraní stejného virtuálního počítače může být připojen k různým podsítím v rámci virtuální sítě, rozhraní sítě musí všechny připojeny ke stejné síti VNet.
- Přidáním jakékoli IP adresu pro všechny konfigurace IP jakékoli primární nebo sekundární síťové rozhraní na fond back-end pro vyrovnávání zatížení Azure. V minulosti pouze primární IP adresa pro primární síťové rozhraní nebylo možné přidat do fondu back-end. Další informace o IP adresy a konfigurace, najdete [přidat, změnit nebo odebrat IP adres](virtual-network-network-interface-addresses.md) článku.
- Odstranění virtuálního počítače neodstraní rozhraní sítě, které jsou připojené k němu. Při odstranění virtuálního počítače jsou z virtuálního počítače odpojit rozhraní sítě. Můžete přidat rozhraní sítě do jiné virtuální počítače, nebo je odstranit.
- Pokud má síťové rozhraní s privátní adresou IPv6 přiřazen, je nutné přidat (připojení) se k virtuálnímu počítači při vytváření virtuálního počítače. Síťové rozhraní s přiřazenou adresu IPv6 nelze přidat k virtuálnímu počítači po vytvoření virtuálního počítače. Pokud přidáte síťové rozhraní s přiřazenou privátní adresu IPv6, při vytváření virtuálního počítače, můžete přidat pouze rozhraní sítě k virtuálnímu počítači, bez ohledu na to, kolik síťových rozhraní, které podporuje velikost virtuálního počítače. V tématu [sítě IP adresy rozhraní](virtual-network-network-interface-addresses.md) Další informace o přiřazování IP adresy pro síťová rozhraní.
- Podobně jako IPv6, nemůžete připojit síťové rozhraní s Zrychlený sítě povolené pro virtuální počítač po vytvoření virtuálního počítače. Další abyste mohli využívat Zrychlený sítě, je nutné také provést kroky v operačním systému virtuálního počítače. Další informace o Zrychlený sítě a dalších omezení při používání najdete v tématu [vytvoření virtuálního počítače s prací v síti Zrychlený](virtual-network-create-vm-accelerated-networking.md).
