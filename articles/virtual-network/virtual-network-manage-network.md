---
title: "Vytvoření, změnit nebo odstranit virtuální síť Azure | Microsoft Docs"
description: "Zjistěte, jak vytvářet, měnit nebo odstranit virtuální sítě v Azure."
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
ms.openlocfilehash: 0d3f4a83b654315a5ff9344594323c5dcb801e77
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2018
---
# <a name="create-change-or-delete-a-virtual-network"></a>Vytvoření, změnit nebo odstranit virtuální síť

Naučte se vytvářet a odstranit virtuální síť a změnit nastavení, například servery DNS a prostory IP adres pro existující virtuální síť.

Virtuální síť je reprezentace vlastní sítě v cloudu. Virtuální síť je to logická izolace cloudu Azure, který je vyhrazen pro vaše předplatné Azure. Pro každou virtuální síť, kterou vytvoříte můžete:
- Zvolte adresní prostor přiřadit. Adresní prostor se skládá z jedné nebo více rozsahů adres, které jsou definovány pomocí notace CIDR (Classless Inter-Domain směrování) zápisu, jako je 10.0.0.0/16.
- Zvolte pomocí serveru DNS poskytnutých platformou Azure nebo pomocí serveru DNS. Všechny prostředky, které jsou připojené k virtuální síti jsou přiřazené tento server DNS překládat názvy v rámci virtuální sítě.
- Virtuální síť segmentovat do podsítí, každou s vlastní rozsah adres, v rámci adresního prostoru virtuální sítě. Naučte se vytvářet, měnit a odstraňovat podsítě, najdete v tématu [přidat, změnit nebo odstranit podsítě](virtual-network-manage-subnet.md).

Tento článek vysvětluje, jak vytvářet, měnit a odstranění virtuální sítě pomocí modelu nasazení Azure Resource Manager.

## <a name="before"></a>Než začnete

Než začnete úlohy, které jsou popsané v tomto článku, splnit následující požadavky:

- Pokud jste pro práci s virtuálními sítěmi nové, doporučujeme, abyste si prošli cvičení v [vytvoření vaší první virtuální síť Azure](quick-create-portal.md). Tento postup může pomoci při seznámení s virtuálními sítěmi.
- Další informace o omezeních pro virtuální sítě, zkontrolujte [Azure omezení](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).
- Přihlaste se k portálu Azure, nástroje příkazového řádku Azure (Azure CLI) nebo Azure PowerShell pomocí účtu Azure. Pokud nemáte účet Azure, zaregistrujte si [Bezplatný zkušební účet](https://azure.microsoft.com/free).
- Pokud budete chtít použít příkazy prostředí PowerShell k dokončení úlohy popsané v tomto článku, musíte nejdřív [instalace a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Ujistěte se, že máte nejnovější verzi rutin prostředí Azure PowerShell nainstalovaný. Chcete-li získat nápovědu pro příkazy prostředí PowerShell v příkladech, zadejte `get-help <command> -full`.
- Pokud máte v plánu používat rozhraní příkazového řádku Azure k dokončení úlohy popsané v tomto článku, musíte nejdřív [instalace a konfigurace rozhraní příkazového řádku Azure](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Ujistěte se, že máte nejnovější verzi nainstalované rozhraní příkazového řádku Azure. Chcete-li získat nápovědu k rozhraní příkazového řádku Azure, zadejte `az <command> --help`.


## <a name="create-vnet"></a>Vytvoření virtuální sítě

Vytvoření virtuální sítě:

1. Přihlaste se k [portál](https://portal.azure.com) pomocí účtu, který je přiřazen oprávnění pro roli Přispěvatel sítě (minimálně) pro vaše předplatné. Další informace o přiřazování rolí a oprávnění na účty, najdete v části [předdefinované role pro řízení přístupu Azure na základě rolí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Klikněte na tlačítko **nový** > **sítě** > **virtuální síť**.
3. Na **virtuální síť** okno v **vybrat model nasazení** pole, ponechte **Resource Manager** vybrané a potom klikněte na **vytvořit**.
4. Na **vytvořit virtuální síť** okno, zadejte nebo vyberte hodnoty pro následující nastavení a potom klikněte na tlačítko **vytvořit**:
    - **Název**: název musí být jedinečný v [skupiny prostředků](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) vyberte možnost pro vytvoření virtuální sítě. Název nelze změnit po vytvoření virtuální sítě. Můžete vytvořit několik virtuálních sítí v čase. Pojmenování návrhy, najdete v části [konvence vytváření názvů](/azure/architecture/best-practices/naming-conventions.md?toc=%2fazure%2fvirtual-network%2ftoc.json#naming-rules-and-restrictions). Tyto zásady vytváření názvů vám pomohou lépe spravovat více virtuálních sítí.
    - **Adresní prostor**: zadat v notaci CIDR adresního prostoru. Do adresního prostoru, které definujete může být veřejné nebo privátní (RFC 1918). Jestli definujete jako veřejného nebo soukromého adresního prostoru, adresní prostor je dostupný jenom z v rámci virtuální sítě od vzájemně propojena virtuální sítě a od všech sítích na pracovišti, které jste se připojili k virtuální síti. Nelze přidat následující adresních prostorů:
        - 224.0.0.0/4 (vícesměrové vysílání)
        - 255.255.255.255/32 (vysílání)
        - 127.0.0.0/8 (zpětné smyčky)
        - 169.254.0.0/16 (Link-local)
        - 168.63.129.16/32 (Internal DNS)

      I když můžete definovat jenom jeden adresní prostor, když vytvoříte virtuální síť, můžete přidat další adresní prostory, po vytvoření virtuální sítě. Informace o postupu přidání adresní prostor k existující virtuální síti, najdete v tématu [přidat nebo odebrat adresní prostor](#add-address-spaces) v tomto článku.

      >[!WARNING]
      >Pokud virtuální síť obsahuje adresní prostory, které se překrývají s jinou virtuální sítí nebo místní sítě, nemůže být připojen dvě sítě. Před definujete adresní prostor, zvažte, jestli můžete chtít v budoucnu připojení virtuální sítě k jiným virtuálním sítím nebo místní sítě.
      >
      >

    - **Název podsítě**: název podsítě musí být jedinečný v rámci virtuální sítě. Název podsítě nelze změnit po vytvoření podsítě. Na portálu vyžaduje definování jednu podsíť když vytvoříte virtuální síť, i když není vyžadováno žádné podsítě virtuální sítě. Na portálu můžete definovat pouze jednu podsíť při vytváření virtuální sítě. Můžete přidat další podsítě virtuální sítě později, po vytvoření virtuální sítě. Chcete-li přidat podsíť k virtuální síti, najdete v části [vytvořit podsíť](virtual-network-manage-subnet.md#create-subnet) v [Create, změnit nebo odstranit podsítě](virtual-network-manage-subnet.md). Můžete vytvořit virtuální síť, která má několik podsítí pomocí příkazového řádku Azure CLI nebo prostředí PowerShell.

      >[!TIP]
      >V některých případech admins vytvořit různých podsítích filtrování nebo řídit směrování provozu mezi podsítěmi. Před definováním podsítě, zvažte, jak můžete filtrovat a směrovat provoz mezi podsítě. Další informace o filtrování přenosů mezi podsítěmi, najdete v části [skupin zabezpečení sítě](virtual-networks-nsg.md). Azure automaticky směrování provozu mezi podsítěmi, ale můžete přepsat Azure výchozí trasy. Další postup přepsat výchozí Azure podsítě ke směrování provozu najdete v tématu [trasy definované uživatelem](virtual-networks-udr-overview.md).
      >

    - **Rozsah adres podsítě**: rozsahu musí být v rámci adresního prostoru, který jste zadali pro virtuální síť. Nejmenší oblast, kterou můžete zadat je /29, který poskytuje osm IP adresy podsítě. Azure si vyhrazuje adresu první a poslední v každé podsíti pro protokol shoda. Tři další adresy jsou vyhrazené pro použití služby Azure. V důsledku toho virtuální síť s rozsahem adres podsítě /29 má jenom tři použitelných IP adresách. Pokud máte v plánu pro připojení virtuální sítě k bráně VPN, musíte vytvořit podsíť brány. Další informace o [aspekty rozsah konkrétní adresu podsítě brány](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub). Rozsah adres můžete změnit po vytvoření podsítě pro určité podmínky. Zjistěte, jak změnit rozsah adres podsítě, najdete v tématu [změnit nastavení podsítě](#change-subnet) v [přidat, změnit nebo odstranit podsítě](virtual-network-manage-subnet.md).
    - **Předplatné**: vyberte [předplatné](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription). V více než jedno předplatné, nelze použít stejné virtuální síti. Virtuální sítě v rámci jednoho předplatného však můžete připojit k virtuálním sítím v jiných předplatných. Chcete-li propojit virtuální sítě v různých předplatných, použijte Azure VPN Gateway nebo partnerského vztahu virtuální sítě. Jakýmikoli prostředky Azure, který je připojen k virtuální síti musí být ve stejném předplatném jako virtuální síť.
    - **Skupina prostředků**: Vyberte existující [skupiny prostředků](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-groups) nebo vytvořte novou. Prostředek služby Azure, který je připojen k virtuální síti může být ve stejné skupině prostředků jako virtuální síť nebo v jiné skupině prostředků.
    - **Umístění**: Vyberte Azure [umístění](https://azure.microsoft.com/regions/), označované také jako oblast. Virtuální síť může být pouze v jednom umístění Azure. Je však připojit virtuální síť na jednom místě k virtuální síti v jiném umístění s použitím brány VPN. Jakýmikoli prostředky Azure, který je připojen k virtuální síti musí být ve stejném umístění jako virtuální síť.

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Azure CLI|[Vytvoření sítě vnet az](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name = "view-vnet"></a>Zobrazit virtuální sítě a nastavení

Chcete-li zobrazit virtuální sítě a nastavení:

1. Přihlaste se k [portál](https://portal.azure.com) pomocí účtu, který je přiřazen oprávnění pro roli Přispěvatel sítě (minimálně) pro vaše předplatné. Další informace o přiřazování rolí a oprávnění na účty, najdete v části [předdefinované role pro řízení přístupu Azure na základě rolí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. V dialogovém okně hledání portálu zadejte **virtuální sítě**. Ve výsledcích hledání klikněte na tlačítko **virtuální sítě**.
3. Na **virtuální sítě** okně klikněte na virtuální síť, kterou chcete zobrazit nastavení.
4. V okně pro virtuální síť, kterou jste vybrali jsou uvedeny následující nastavení:
    - **Přehled**: poskytuje informace o virtuální síť, včetně adresní prostor a servery DNS. Následující snímek obrazovky ukazuje přehled nastavení pro virtuální síť s názvem **MyVNet**:

        ![Přehled rozhraní sítě](./media/virtual-network-manage-network/vnet-overview.png)

      Na **přehled** okně virtuální sítě můžete přesunout do jiné předplatné nebo prostředek skupiny. Další postup přesunutí virtuální sítě najdete v tématu [přesunu prostředků do jiné skupině prostředků nebo předplatného](../azure-resource-manager/resource-group-move-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Článek uvádí předpoklady a postup přesunutí prostředků pomocí portálu Azure, PowerShell a rozhraní příkazového řádku Azure. Všechny prostředky, které jsou připojené k virtuální síti, musíte přesunout s virtuální sítí.
    - **Adresní prostor**: jsou uvedeny adresní prostory, které jsou přiřazeny k virtuální síti. Chcete-li zjistit, jak přidávat a odebírat adresní prostor, proveďte kroky v [přidat nebo odebrat adresní prostor](#address-spaces) v tomto článku.
    - **Připojená zařízení**: jsou uvedeny všechny prostředky, které jsou připojené k virtuální síti. Na předchozím snímku obrazovky tři síťová rozhraní a jedna služba Vyrovnávání zatížení jsou připojené k virtuální síti. Jsou uvedeny všechny nové prostředky, které můžete vytvořit a připojit k virtuální síti. Pokud odstraníte prostředek, který byl připojen k virtuální síti, se již v seznamu.
    - **Podsítě**: se zobrazí seznam podsítí, které existují v rámci virtuální sítě. Naučte se přidávat a odebírat podsíť, najdete v tématu [vytvořit podsíť](virtual-network-manage-subnet.md#create-subnet) a [odstranit podsíť](virtual-network-manage-subnet.md#delete-subnet) v [přidat, změnit nebo odstranit podsítě](virtual-network-manage-subnet.md).
    - **Servery DNS**: můžete určit, zda Azure interní server DNS nebo vlastního serveru DNS poskytuje překlad názvů pro zařízení, které jsou připojené k virtuální síti. Když vytvoříte virtuální síť pomocí portálu Azure, serverů Azure DNS se používají pro překlad názvu virtuální sítě, ve výchozím nastavení. Pokud chcete upravit servery DNS, proveďte kroky v [přidat, změnit nebo odebrat DNS server](#dns-servers) v tomto článku.
    - **Partnerské vztahy**: Pokud v odběru existují existující partnerských vztahů, jsou zde uvedeny. Můžete zobrazit nastavení pro existující partnerských vztahů, nebo vytvořit, změnit nebo odstranění partnerských vztahů. Další informace o partnerských vztahů, najdete v části [partnerský vztah virtuální sítě](virtual-network-peering-overview.md).
    - **Vlastnosti**: nastavení o virtuální síť, včetně ID prostředku virtuální sítě a předplatné Azure, které se zobrazí.
    - **Diagram**: diagram poskytuje vizuální reprezentace všechna zařízení, které jsou připojené k virtuální síti. Diagram má některé klíčové informace o zařízení. Chcete-li spravovat zařízení v tomto zobrazení v diagramu, klikněte na zařízení.
    - **Obecná nastavení Azure**: Další informace o běžných nastavení Azure, přečtěte si následující informace:
        *   [Protokol aktivit](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#activity-logs)
        *   [Řízení přístupu (IAM)](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#access-control)
        *   [Značky](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#tags)
        *   [Zámky.](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
        *   [Skriptu pro automatizaci](../azure-resource-manager/resource-manager-export-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json#export-the-template-from-resource-group)

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Azure CLI|[Zobrazit virtuální síť az sítě](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#show)|
|PowerShell|[Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork/?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## <a name="add-address-spaces"></a>Přidat nebo odebrat adresní prostor.

Můžete přidávat a odebírat adresní prostory virtuální sítě. Adresní prostor je třeba zadat v notaci CIDR a nesmí překrývat s další adresní prostory v rámci stejné virtuální síti. Adresní prostory, které definujete může být veřejné nebo privátní (RFC 1918). Jestli definujete jako veřejného nebo soukromého adresního prostoru, adresní prostor je dostupný jenom z v rámci virtuální sítě od vzájemně propojena virtuální sítě a od všech sítích na pracovišti, které jste se připojili k virtuální síti. Nelze přidat následující adresních prostorů:

- 224.0.0.0/4 (vícesměrové vysílání)
- 255.255.255.255/32 (vysílání)
- 127.0.0.0/8 (zpětné smyčky)
- 169.254.0.0/16 (Link-local)
- 168.63.129.16/32 (Internal DNS)

Přidat nebo odebrat adresní prostor:

1. Přihlaste se k [portál](https://portal.azure.com) pomocí účtu, který je přiřazen oprávnění pro roli Přispěvatel sítě (minimálně) pro vaše předplatné. Další informace o přiřazování rolí a oprávnění na účty, najdete v části [předdefinované role pro řízení přístupu Azure na základě rolí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. V dialogovém okně hledání portálu zadejte **virtuální sítě**. Ve výsledcích hledání vyberte **virtuální sítě**.
3. Na **virtuální sítě** okně klikněte na virtuální síť, pro který chcete přidat nebo odebrat adresní prostor.
4. Na virtuální sítě okno, v části **nastavení**, klikněte na tlačítko **adresní prostor**.
5. V okně adresní prostor proveďte jednu z následujících možností:
    - **Přidání adresního prostoru**: Zadejte nový adresní prostor. Adresní prostor se nesmí překrývat s existující adresní prostor, který je definován pro virtuální síť.
    - **Odebrat adresní prostor**: klikněte pravým tlačítkem na adresní prostor a pak klikněte na **odebrat**. Pokud existuje podsíť v adresním prostoru, nelze odebrat adresní prostor. Chcete-li odebrat adresní prostor, musíte nejprve odstranit všechny podsítě (a všechny prostředky, které jsou připojené k podsítím), neexistuje v adresním prostoru.
6. Klikněte na **Uložit**.

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Azure CLI|Pouze správce prostředků|[Aktualizace virtuální sítě az sítě](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="dns-servers"></a>Přidat, změnit nebo odebrat DNS server

Všechny virtuální počítače, které jsou připojené k virtuální síti registrace se servery DNS, které zadáte pro virtuální síť. Také používají zadaný server DNS pro rozlišení názvu. Každé síťové rozhraní (NIC) ve virtuálním počítači může mít svůj vlastní nastavení serveru DNS. Pokud síťový adaptér má svou vlastní nastavení serveru DNS, budou přepsat nastavení serveru DNS virtuální sítě. Další informace o nastavení DNS pro síťový adaptér, najdete v části [síťové rozhraní úlohy a nastavení](virtual-network-network-interface.md#change-dns-servers). Další informace o překladu názvů pro virtuální počítače a instance rolí ve službě Azure Cloud Services najdete v tématu [překlad názvů pro virtuální počítače a instance rolí](virtual-networks-name-resolution-for-vms-and-role-instances.md). Pokud chcete přidat, změnit nebo odebrat DNS server:

1. Přihlaste se k [portál](https://portal.azure.com) pomocí účtu, který je přiřazen oprávnění pro roli Přispěvatel sítě (minimálně) pro vaše předplatné. Další informace o přiřazování rolí a oprávnění na účty, najdete v části [předdefinované role pro řízení přístupu Azure na základě rolí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Do pole hledání portálu zadejte **virtuální sítě**. Ve výsledcích hledání vyberte **virtuální sítě**.
3. Na **virtuální sítě** okně klikněte na virtuální síť, kterou chcete změnit nastavení DNS pro.
4. Na virtuální sítě okno, v části **nastavení**, klikněte na tlačítko **servery DNS**.
5. Vyberte jednu z následujících možností v okně seznam serverů DNS:
    - **Výchozí (zadaný Azure)**: všechny názvy prostředků a privátních IP adres se automaticky registruje na servery Azure DNS. Abyste mohli vyřešit názvy mezi všechny prostředky, které jsou připojené ke stejné virtuální síti. Tuto možnost nelze použít k překladu názvů virtuálních sítí. Překládat názvy virtuálních sítí, musíte použít vlastní server DNS.
    - **Vlastní**: můžete přidat jeden nebo více serverů, a to až do limitu Azure pro virtuální síť. Další informace o limity serveru DNS, najdete v části [Azure omezení](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual-networking-limits-classic). Máte následující možnosti:
        - **Přidat adresu**: server přidá do seznamu serverů DNS virtuální sítě. Tato možnost také zaregistruje DNS server s Azure. Pokud již jste registrováni DNS server s Azure, můžete vybrat tento server DNS v seznamu.
        - **Odebrat adresu**: u serveru, který chcete odebrat, klikněte na tlačítko **X**. Odstranění serveru odstraní server pouze z tohoto seznamu virtuální sítě. DNS server zůstane registrované v Azure pro vaše virtuální sítě a použít.
        - **Změna pořadí adresy serverů DNS**: je důležité si ověřit, že můžete seznam serverů DNS ve správném pořadí pro vaše prostředí. Seznamy server DNS se používají v pořadí, ve kterém jsou uvedené. Nepracují jako instalace s kruhovým dotazováním. Pokud první server DNS v seznamu dostupný, klient použije tento server DNS, bez ohledu na to, zda je DNS server správně funguje. Odeberte všechny servery DNS, které jsou uvedeny a poté je přidejte zpátky v pořadí, ve kterém chcete.
        - **Změnit adresu**: zvýrazněte server DNS v seznamu a pak zadejte nový název.
6. Klikněte na **Uložit**.
7. Restartujte virtuální počítače, které jsou připojené k virtuální síti, aby byly přiřazovány nové nastavení serveru DNS. Virtuální počítače dál používat své aktuální nastavení DNS, dokud nebudou restartovány.

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Azure CLI|[Aktualizace virtuální sítě az sítě](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="delete-vnet"></a>Odstranění virtuální sítě

Virtuální síť můžete odstranit pouze v případě, že neexistují žádné prostředky k němu připojená. Pokud jsou prostředky připojenými k žádné podsíti v rámci virtuální sítě, musíte nejprve odstranit prostředky, které jsou připojené k všech podsítí v rámci virtuální sítě. Kroky, které je třeba provést odstranění prostředku se liší v závislosti na prostředek. Chcete-li zjistit, jak odstranit prostředky, které jsou připojené k podsítím, přečtěte si dokumentaci pro každý typ prostředku, který chcete odstranit. Odstranění virtuální sítě:

1. Přihlaste se k [portál](https://portal.azure.com) pomocí účtu, který je přiřazený (minimálně) oprávnění pro roli Přispěvatel sítě pro vaše předplatné. Další informace o přiřazování rolí a oprávnění na účty, najdete v části [předdefinované role pro řízení přístupu Azure na základě rolí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. V dialogovém okně hledání portálu zadejte **virtuální sítě**. Ve výsledcích hledání klikněte na tlačítko **virtuální sítě**.
3. Na **virtuální sítě** okně vyberte virtuální síť, kterou chcete odstranit.
4. V okně virtuální sítě k potvrzení, že neexistují žádná zařízení připojené k virtuální síti, v části **nastavení**, klikněte na tlačítko **připojená zařízení**. Připojená zařízení je nutné před odstraněním virtuální sítě odstranit. Pokud nejsou připojené zařízení, klikněte na tlačítko **přehled**.
5. V horní části okna klikněte **odstranit** ikonu.
6. Potvrďte odstranění virtuální sítě, klikněte na tlačítko **Ano**.


**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Azure CLI|[odstranění sítě Azure vnet](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Remove-AzureRmVirtualNetwork](/powershell/module/azurerm.network/remove-azurermvirtualnetwork?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## <a name="next-steps"></a>Další kroky

- Vytvoření virtuálního počítače a připojte ho k virtuální síti najdete v tématu [vytvoření virtuální sítě a připojení virtuálních počítačů](quick-create-portal.md#create-virtual-machines).
- Pro filtrování síťového provozu mezi podsítěmi v rámci virtuální sítě, najdete v části [vytvoření skupin zabezpečení sítě](virtual-networks-create-nsg-arm-pportal.md).
- Chcete-li peer virtuální sítě s jinou virtuální sítí, přečtěte si téma [vytvoření virtuální sítě partnerského vztahu](virtual-network-create-peering.md#portal).
- Další informace o možnosti připojení virtuální sítě k místní síti najdete v tématu [o službě VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#diagrams).
