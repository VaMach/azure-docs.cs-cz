---
title: "Přidat, změnit nebo odstranit podsíť virtuální sítě Azure | Microsoft Docs"
description: "Zjistěte, jak přidat, změnit nebo odstranit podsíť virtuální sítě v Azure."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/10/2017
ms.author: jdial
ms.openlocfilehash: 85ba6ef3e51c339a77eb9b4198c4f87e2a64cf09
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="add-change-or-delete-a-virtual-network-subnet"></a>Přidat, změnit nebo odstranit podsíť virtuální sítě

Zjistěte, jak přidat, změnit nebo odstranit podsíť virtuální sítě. 

Pokud nejste obeznámení s virtuálními sítěmi, před přidat, změnit nebo odstranit podsíť, doporučujeme, abyste si přečetli [Přehled virtuálních sítí Azure](virtual-networks-overview.md) a [vytvoření, změny a odstranění virtuální sítě](virtual-network-manage-network.md). Všechny prostředky Azure, které jsou nasazeny do virtuální sítě se nasadí do podsítě virtuální sítě. Několik podsítí jsou obvykle vytvořeny v rámci virtuální sítě pro:
- **Filtrovat provoz mezi podsítěmi**. Můžete použít skupiny zabezpečení sítě pro podsítě pro filtrování příchozích a odchozích síťových přenosů pro všechny prostředky (třeba virtuální počítače), které jsou ve virtuální síti. Další informace o tom, jak vytvořit skupinu zabezpečení sítě najdete v tématu [vytvoření skupin zabezpečení sítě](virtual-networks-create-nsg-arm-pportal.md).
- **Řídit směrování mezi podsítěmi**. Azure vytvoří výchozí trasy, aby automaticky směrovat přenosy mezi podsítěmi. Azure výchozí trasy můžete přepsat tak, že vytvoříte trasy definované uživatelem. Další informace o trasy definované uživatelem, najdete v části [vytvořit trasy definované uživatelem](virtual-network-create-udr-arm-ps.md). 

Tento článek vysvětluje, jak přidat, změnit a odstranit podsíť pro virtuální sítě, které byly vytvořeny pomocí modelu nasazení Azure Resource Manager.
 
## <a name="before"></a>Než začnete

Než začnete úlohy, které jsou popsané v tomto článku, splnit následující požadavky:

- Pokud jste pro práci s virtuálními sítěmi nové, doporučujeme, abyste si prošli cvičení v [vytvoření vaší první virtuální síť Azure](virtual-network-get-started-vnet-subnet.md). Tento postup může pomoci při seznámení s virtuálními sítěmi.
- Další informace o omezeních pro virtuální sítě, zkontrolujte [Azure omezení](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).
- Přihlaste se k portálu Azure, nástroje příkazového řádku Azure (Azure CLI) nebo Azure PowerShell pomocí účtu Azure. Pokud nemáte účet Azure, zaregistrujte si [Bezplatný zkušební účet](https://azure.microsoft.com/free).
- Pokud budete chtít použít příkazy prostředí PowerShell k dokončení úlohy popsané v tomto článku, musíte nejdřív [instalace a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Ujistěte se, že máte nejnovější verzi rutin prostředí Azure PowerShell nainstalovaný. Chcete-li získat nápovědu pro příkazy prostředí PowerShell v příkladech, zadejte `get-help <command> -full`.
- Pokud máte v plánu používat rozhraní příkazového řádku Azure k dokončení úlohy popsané v tomto článku, které je nutné buď:
    - [Instalace a konfigurace rozhraní příkazového řádku Azure](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Ujistěte se, že máte nejnovější verzi nainstalované rozhraní příkazového řádku Azure.
    - Používání prostředí cloudu Azure. Místo instalace rozhraní příkazového řádku a jeho závislé součásti, můžete použít prostředí cloudové služby Azure. Služba Azure Cloud Shell je volně dostupné prostředí Bash, které můžete spustit přímo z portálu Azure Portal. Má předinstalované rozhraní Azure CLI, které je nakonfigurované pro použití s vaším účtem. Chcete-li použít cloudové prostředí, klikněte na tlačítko prostředí cloudu (**> _**) ikona v horní části portálu Azure. 

  Chcete-li získat nápovědu k rozhraní příkazového řádku Azure, zadejte `az <command> --help`.

## <a name="create-subnet"></a>Přidat podsíť

Chcete-li přidat podsíť:

1. Přihlaste se k [portál](https://portal.azure.com) pomocí účtu, který je přiřazen oprávnění pro roli Přispěvatel sítě (minimálně) pro vaše předplatné. Další informace o přiřazování rolí a oprávnění na účty, najdete v části [předdefinované role pro řízení přístupu Azure na základě rolí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. V dialogovém okně hledání portálu zadejte **virtuální sítě**. Ve výsledcích hledání klikněte na tlačítko **virtuální sítě**.
3. Na **virtuální sítě** okně klikněte na virtuální síť, kterou chcete přidat podsíť, kterou chcete.
4. V okně virtuální sítě klikněte na **podsítě**.
5. Klikněte na tlačítko **+ podsítě**.
6. Na **přidat podsíť** okno, zadejte hodnoty následujících parametrů:
    - **Název**: název musí být jedinečný v rámci virtuální sítě.
    - **Rozsah adres**: rozsahu musí být jedinečný v rámci adresního prostoru virtuální sítě. Rozsah se nesmí překrývat s rozsahy adres jiné podsítě v rámci virtuální sítě. Adresní prostor musí být určena pomocí notace CIDR (Classless Inter-Domain směrování) zápisu. Ve virtuální síti s 10.0.0.0/16 prostoru adres, můžete třeba definovat adresního prostoru podsítě 10.0.0.0/24. Nejmenší oblast, kterou můžete zadat je /29, který poskytuje osm IP adresy podsítě. Azure si vyhrazuje adresu první a poslední v každé podsíti pro protokol shoda. Tři další adresy jsou vyhrazené pro použití služby Azure. V důsledku toho definování podsíť s/29 adres rozsah výsledků v tři použitelných IP adresách v podsíti. Pokud máte v plánu pro připojení virtuální sítě k bráně VPN, musíte vytvořit podsíť brány. Další informace o [aspekty rozsah konkrétní adresu podsítě brány](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub). Po přidání podsítě pro určité podmínky, můžete změnit rozsah adres. Zjistěte, jak změnit rozsah adres podsítě, najdete v tématu [změnit nastavení podsítě](#change-subnet) v tomto článku.
    - **Skupina zabezpečení sítě**: Volitelně můžete přidružit existující skupinu zabezpečení sítě podsíti, aby mohl řídit příchozí a odchozí síťový provoz filtrování pro podsíť. Skupina zabezpečení sítě, musí existovat ve stejném předplatném a umístění jako virtuální síť. Je také nutné vytvořit pomocí modelu nasazení Resource Manager. Další informace o tom, jak vytvořit skupinu zabezpečení sítě najdete v tématu [skupin zabezpečení sítě](virtual-networks-create-nsg-arm-pportal.md).
    - **Směrovací tabulka**: Volitelně můžete přidružit existující směrovací tabulku s podsítí řídit směrování provozu sítě k jiným sítím. Směrovací tabulka musí existovat ve stejném předplatném a umístění jako virtuální síť. Je také nutné vytvořit pomocí modelu nasazení Resource Manager. Další informace o tom, jak vytvořit směrovací tabulky najdete v tématu [trasy definované uživatelem](virtual-network-create-udr-arm-ps.md).
    - **Uživatelé**: můžete řídit přístup k podsíti pomocí předdefinované role nebo vlastní role. Další informace o přiřazování rolí a uživatelům přístup k podsíti, najdete v části [použít přiřazení role pro správu přístupu k prostředkům Azure](../active-directory/role-based-access-control-configure.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-access).
7. Chcete-li přidat podsíť virtuální sítě, kterou jste vybrali, klikněte na tlačítko **OK**.

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Azure CLI|[Vytvoření az podsíti virtuální sítě](/cli/azure/network/vnet/subnet?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Nové AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig?toc=%2fazure%2fvirtual-network%2ftoc.json), [přidat AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="change-subnet"></a>Změňte nastavení podsítě

Skupiny zabezpečení sítě, směrovací tabulky a přístupu uživatele můžete změnit na podsíť pomocí správy prostředků, které jsou v podsíti. Další informace o těchto nastavení v [přidat podsíť](#create-subnet), podívejte se na krok 6. Pokud chcete změnit adresní prostor podsítě, musíte nejprve odstranit všechny prostředky, které jsou v podsíti. Kroky, které je třeba provést odstranění prostředku se liší v závislosti na prostředek. Chcete-li zjistit, jak odstranit prostředky, které jsou v podsítích, přečtěte si dokumentaci pro každý typ prostředku, který chcete odstranit. Chcete-li změnit rozsah adres podsítě:

1. Přihlaste se k [portál](https://portal.azure.com) pomocí účtu, který je přiřazen oprávnění pro roli Přispěvatel sítě (minimálně) pro vaše předplatné. Další informace o přiřazování rolí a oprávnění na účty, najdete v části [předdefinované role pro řízení přístupu Azure na základě rolí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. V dialogovém okně hledání portálu zadejte **virtuální sítě**. Ve výsledcích hledání klikněte na tlačítko **virtuální sítě**.
3. Na **virtuální sítě** okně klikněte na virtuální síť, pro kterou chcete změnit rozsah adres podsítě.
4. Klikněte na podsíť, pro kterou chcete změnit rozsah adres.
5. V okně podsítě v **rozsahu adres** zadejte nový rozsah adres. Rozsah musí být jedinečný v rámci adresního prostoru virtuální sítě. Rozsah se nesmí překrývat s rozsahy adres jiné podsítě v rámci virtuální sítě. Adresní prostor musí být určena pomocí zápisu směrování CIDR. Ve virtuální síti s 10.0.0.0/16 prostoru adres, můžete třeba definovat adresního prostoru podsítě 10.0.0.0/24. Nejmenší oblast, kterou můžete zadat je /29, který poskytuje osm IP adresy podsítě. Azure si vyhrazuje adresu první a poslední v každé podsíti pro protokol shoda. Tři další adresy jsou vyhrazené pro použití služby Azure. V důsledku toho podsíť s/29 rozsah adres má tři použitelných IP adresách. Pokud máte v plánu pro připojení virtuální sítě k bráně VPN, musíte vytvořit podsíť brány. Další informace o [aspekty rozsah konkrétní adresu podsítě brány](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub). Rozsah adres můžete změnit po vytvoření podsítě pro určité podmínky. Zjistěte, jak změnit rozsah adres podsítě, najdete v tématu [změnit nastavení podsítě](#change-subnet) v tomto článku.
6. Klikněte na **Uložit**.

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Azure CLI|[aktualizace az sítě vnet podsíť](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## <a name="delete-subnet"></a>Odstranit podsíť

Podsíť může odstranit pouze v případě, že neexistují žádné prostředky v podsíti. Pokud v podsíti zdrojů, je nutné odstranit prostředky, které jsou v podsíti, chcete-li odstranit podsíť. Kroky, které je třeba provést odstranění prostředku se liší v závislosti na prostředek. Chcete-li zjistit, jak odstranit prostředky, které jsou v podsítích, přečtěte si dokumentaci pro každý typ prostředku, který chcete odstranit. Chcete-li odstranit podsíť:

1. Přihlaste se k [portál](https://portal.azure.com) pomocí účtu, který je přiřazen oprávnění pro roli Přispěvatel sítě (minimálně) pro vaše předplatné. Další informace o přiřazování rolí a oprávnění na účty, najdete v části [předdefinované role pro řízení přístupu Azure na základě rolí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. V dialogovém okně hledání portálu zadejte **virtuální sítě**. Ve výsledcích hledání klikněte na tlačítko **virtuální sítě**.
3. Na **virtuální sítě** okně klikněte na virtuální síť, kterou chcete odstranit podsíť z.
4. Na virtuální sítě okno, v části **nastavení**, klikněte na tlačítko **podsítě**.
5. V seznamu podsítí, který se zobrazí v okně podsítě, klikněte pravým tlačítkem na podsíť, kterou chcete odstranit, klikněte na **odstranit**a potom klikněte na **Ano** -li odstranit podsíť.

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Azure CLI|[AZ síť vnet odstranit](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Odebrat AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/remove-azurermvirtualnetworksubnetconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="next-steps"></a>Další kroky

Vytvoření virtuálního počítače v podsíti, naleznete v části [vytvořit virtuální síť a nasazovat virtuální počítače v podsíti](virtual-network-get-started-vnet-subnet.md#create-vms).
