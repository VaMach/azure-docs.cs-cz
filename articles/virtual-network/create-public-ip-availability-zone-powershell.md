---
title: "Vytvoření zoned veřejnou IP adresu pomocí prostředí PowerShell | Microsoft Docs"
description: "Vytvořte veřejnou IP adresu v zóně dostupnosti pomocí prostředí PowerShell."
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
ms.openlocfilehash: bfeba57036338b4e325d2f122443f2cde0373eed
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-public-ip-address-in-an-availability-zone-with-powershell"></a>Vytvoření veřejné IP adresy v zóně dostupnosti pomocí prostředí PowerShell

Můžete nasadit na veřejnou IP adresu v zóně Azure dostupnosti (preview). Dostupnosti zóna je fyzicky oddělená zónu v oblasti Azure. Naučte se:

> * Vytvoření veřejné IP adresy v zóně dostupnosti
> * Určete související prostředky, které jsou vytvořeny v zóně dostupnosti
  

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

Tento článek vyžaduje, že máte verzi 4.4.0 nebo vyšší modulu AzureRM nainstalovat. Verzi zjistíte spuštěním příkazu `Get-Module -ListAvailable AzureRM`. Pokud potřebujete nainstalovat nebo upgradovat, nainstalujte nejnovější verzi modulu AzureRM z [Galerie prostředí PowerShell](https://www.powershellgallery.com/packages/AzureRM).

> [!NOTE]
> Dostupnost zóny jsou ve verzi preview a jsou připraveny pro váš vývojový a testovací scénáře. Podpora je k dispozici pro vyberte prostředků Azure a oblasti a rodiny velikost virtuálního počítače. Další informace o tom, jak začít pracovat a které prostředky Azure, oblasti a rodiny velikost virtuálního počítače můžete zkusit dostupnost zóny s, najdete v části [přehled dostupnosti zón](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview). Pokud budete potřebovat podporu, můžete kontaktovat [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) nebo [otevřít lístek podpory Azure](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="log-in-to-azure"></a>Přihlaste se k Azure.

Přihlaste se k předplatnému Azure pomocí příkazu `Login-AzureRmAccount` a postupujte podle pokynů na obrazovce.

```powershell
Login-AzureRmAccount
```
## <a name="create-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků Azure pomocí příkazu [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. V tomto příkladu skupinu prostředků s názvem *myResourceGroup* je vytvořen v *westeurope* oblast. *westeurope* je jedním z oblasti Azure, které podporuje zóny dostupnosti ve verzi preview.

```powershell
New-AzureRmResourceGroup -Name AzTest -Location westeurope
```

## <a name="create-a-zonal-public-ip-address"></a>Vytvoření oblastmi veřejné IP adresy

Vytvoření veřejné IP adresy v zóně dostupnosti pomocí následujícího příkazu:

```powershell
    New-AzureRmPublicIpAddress `
        -ResourceGroupName myResourceGroup `
        -Name myPublicIp `
        -Location westeurope `
        -AllocationMethod Static `
        -Zone 2
```

> [!NOTE]
> Při přiřazování veřejné IP adresy standardní SKU k síťovému rozhraní virtuálního počítače je potřeba explicitně povolit plánovaný provoz pomocí [skupiny zabezpečení sítě](security-overview.md#network-security-groups). Komunikace s prostředkem nebude možná, dokud nevytvoříte a nepřiřadíte skupinu zabezpečení sítě a explicitně nepovolíte požadovaný provoz.

## <a name="get-zone-information-about-a-public-ip-address"></a>Získat informace o zóně o veřejné IP adresy

Získáte informace o zóně veřejné IP adresy pomocí následujícího příkazu:

```powershell
Get-AzureRmPublicIpAddress ` 
    -ResourceGroup myResourceGroup `
    -Name myPublicIp
```

## <a name="next-steps"></a>Další kroky

- Další informace o [dostupnost zóny](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview)
- Další informace o [veřejné IP adresy](virtual-network-public-ip-address.md?toc=%2fazure%2fvirtual-network%2ftoc.json) 