---
title: "Vytvoření zoned veřejnou IP adresu pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Vytvořte veřejnou IP adresu v zóně dostupnosti pomocí Azure CLI."
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
ms.openlocfilehash: ef93d43bbd0c58950027810c8c335d9076574326
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-public-ip-address-in-an-availability-zone-with-the-azure-cli"></a>Vytvoření veřejné IP adresy v zóně dostupnosti pomocí Azure CLI

Můžete nasadit na veřejnou IP adresu v zóně Azure dostupnosti (preview). Dostupnosti zóna je fyzicky oddělená zónu v oblasti Azure. Naučte se:

> * Vytvoření veřejné IP adresy v zóně dostupnosti
> * Určete související prostředky, které jsou vytvořeny v zóně dostupnosti

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, tento kurz vyžaduje, že používáte verzi rozhraní příkazového řádku Azure novější než verze 2.0.17. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

> [!NOTE]
> Dostupnost zóny jsou ve verzi preview a jsou připraveny pro váš vývojový a testovací scénáře. Podpora je k dispozici pro vyberte prostředků Azure a oblasti a rodiny velikost virtuálního počítače. Další informace o tom, jak začít pracovat a které prostředky Azure, oblasti a rodiny velikost virtuálního počítače můžete zkusit dostupnost zóny s, najdete v části [přehled dostupnosti zón](https://docs.microsoft.com/azure/availability-zones/az-overview). Pokud budete potřebovat podporu, můžete kontaktovat [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) nebo [otevřít lístek podpory Azure](../azure-supportability/how-to-create-azure-support-request.md).

## <a name="create-a-zonal-public-ip-address"></a>Vytvoření oblastmi veřejné IP adresy

Vytvoření veřejné IP adresy v zóně dostupnosti pomocí následujícího příkazu:


```azurecli-interactive
az network public-ip create --resource-group myResourceGroup --name myPublicIp --zone 2 --location westeurope
```
> [!NOTE]
> Při přiřazování veřejné IP adresy standardní SKU k síťovému rozhraní virtuálního počítače je potřeba explicitně povolit plánovaný provoz pomocí [skupiny zabezpečení sítě](security-overview.md#network-security-groups). Komunikace s prostředkem nebude možná, dokud nevytvoříte a nepřiřadíte skupinu zabezpečení sítě a explicitně nepovolíte požadovaný provoz.

## <a name="get-zone-information-about-a-public-ip-address"></a>Získat informace o zóně o veřejné IP adresy

Získáte informace o zóně veřejné IP adresy pomocí následujícího příkazu:

```azurecli-interactive
az network public-ip show --resource-group myResourceGroup --name myPublicIp
```

## <a name="next-steps"></a>Další kroky

- Další informace o [dostupnost zóny](https://docs.microsoft.com/azure/availability-zones/az-overview)
- Další informace o [veřejné IP adresy](../virtual-network/virtual-network-public-ip-address.md) 
