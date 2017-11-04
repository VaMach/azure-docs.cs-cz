---
title: "Vytvoření zoned veřejnou IP adresu pomocí portálu Azure | Microsoft Docs"
description: "Vytvoření veřejné IP adresy v zóně dostupnosti pomocí portálu Azure."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 09/25/2017
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 2fcbed2f83d66a0b4336cd1c464bb02eff3ef229
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-public-ip-address-in-an-availability-zone-with-the-azure-portal"></a>Vytvoření veřejné IP adresy v zóně dostupnosti pomocí portálu Azure

Můžete nasadit na veřejnou IP adresu v zóně Azure dostupnosti (preview). Dostupnosti zóna je fyzicky oddělená zónu v oblasti Azure. Naučte se:

> * Vytvoření veřejné IP adresy v zóně dostupnosti
> * Určete související prostředky, které jsou vytvořeny v zóně dostupnosti

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

> [!NOTE]
> Dostupnost zóny jsou ve verzi preview a jsou připraveny pro váš vývojový a testovací scénáře. Podpora je k dispozici pro vyberte prostředků Azure a oblasti a rodiny velikost virtuálního počítače. Další informace o tom, jak začít pracovat a které prostředky Azure, oblasti a rodiny velikost virtuálního počítače můžete zkusit dostupnost zóny s, najdete v části [přehled dostupnosti zón](https://docs.microsoft.com/azure/availability-zones/az-overview). Pokud budete potřebovat podporu, můžete kontaktovat [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) nebo [otevřít lístek podpory Azure](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="log-in-to-azure"></a>Přihlaste se k Azure.

Přihlaste se k portálu Azure na https://portal.azure.com. 

## <a name="create-a-zonal-public-ip-address"></a>Vytvoření oblastmi veřejné IP adresy

1. Klikněte na tlačítko **Nový** v levém horním rohu webu Azure Portal.
2. Vyberte **sítě**a potom vyberte **veřejnou IP adresu**.
3. Zadejte nebo vyberte hodnoty pro následující nastavení, vyberte předplatné, přijměte výchozí hodnoty pro zbývající nastavení a potom klikněte na tlačítko **vytvořit**:

    |Nastavení|Hodnota|
    |---|---|
    |Skladová jednotka (SKU)| **Základní**: přiřazeny metoda statickou nebo dynamickou přidělení. Lze přiřadit k jakýmikoli prostředky Azure, kterou lze přiřadit veřejnou IP adresu, například síťových rozhraní, brány sítě VPN, aplikačních bran a internetové nástroje pro vyrovnávání zatížení. Veřejnou IP adresu můžete přiřadit konkrétní zónu v **dostupnost zóny** nastavení. Nejsou zónově redundantní. Další informace o zónách dostupnosti najdete v tématu [Přehled zón dostupnosti](https://docs.microsoft.com/azure/availability-zones/az-overview). **Standardní**: přiřazeny metodu statického přidělování. Lze přiřadit k síťová rozhraní nebo nástroje pro vyrovnávání zatížení standardní internetového. Chcete-li přiřadit veřejnou IP adresu nástroji pro vyrovnávání zatížení standardní internetového, je třeba vybrat Standard. Další informace o skladových položkách nástroje pro vyrovnávání zatížení Azure najdete v tématu věnovaném [standardní SKU nástroje pro vyrovnávání zatížení Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview). Jsou zónově redundantní, ve výchozím nastavení. Je možné je vytvořit zónově a zaručit jejich dostupnost v konkrétní zóně dostupnosti. Standardní SKU je ve verzi Preview. Před vytvořením standardní SKU veřejné IP adresy, nejprve je nutné zaregistrovat verzi Preview. Pokud si chcete zaregistrovat verzi Preview, přečtěte si téma popisující [registraci standardní SKU verze Preview](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview#preview-sign-up). Standardní SKU lze vytvořit pouze v podporovaných umístění.  Seznam podporovaných umístění (oblastí) najdete v tématu [Dostupnost v oblastech](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview#region-availability). Průběžně také sledujte stránku [Aktualizace služby Azure Virtual Network](https://azure.microsoft.com/updates/?product=virtual-network), kde najdete informace o podpoře dalších oblastí.|   
    |Name (Název)|Název musí být jedinečný v rámci skupiny prostředků, kterou vyberete.|
    |Skupina prostředků|Klikněte na tlačítko Vytvořit nový a zadejte myResourceGroup|
    |Umístění|Západní Evropa|
    |Dostupnost zóny|Pokud jste vybrali **standardní** SKU, můžete vybrat *Zónově redundantní* Pokud chcete, aby IP adresu, kterou být odolné v rámci zóny. Pokud jste vybrali **základní** SKU, IP adresa není odolné napříč zóny. Bez ohledu na to, SKU, který zvolíte můžete přiřadit adresu pro konkrétní zónu, pokud si zvolíte. |

    Nastavení se zobrazovat na portálu, jak je znázorněno na následujícím obrázku:

    ![Vytvoření oblastmi veřejnou IP adresu](./media/create-public-ip-availability-zone-portal/public-ip-address.png) 

> [!NOTE]
> Při přiřazování veřejné IP adresy standardní SKU k síťovému rozhraní virtuálního počítače je potřeba explicitně povolit plánovaný provoz pomocí [skupiny zabezpečení sítě](security-overview.md#network-security-groups). Komunikace s prostředkem nebude možná, dokud nevytvoříte a nepřiřadíte skupinu zabezpečení sítě a explicitně nepovolíte požadovaný provoz.

## <a name="next-steps"></a>Další kroky

- Další informace o [dostupnost zóny](https://docs.microsoft.com/azure/availability-zones/az-overview)
- Další informace o [veřejné IP adresy](virtual-network-public-ip-address.md?toc=%2fazure%2fvirtual-network%2ftoc.json)