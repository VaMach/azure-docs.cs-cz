---
title: "Nastavení koncových bodů na klasické virtuální počítač s Linuxem | Microsoft Docs"
description: "Naučte se nastavit koncové body pro virtuální počítač s Linuxem v portálu Azure classic a povolit komunikaci se virtuální počítač s Linuxem v Azure"
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: f3749738-1109-4a1d-8635-40e4bd220e91
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: cynthn
ms.openlocfilehash: 4fd8b847b0f60648d1661ce5a8667c641e616ed4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-set-up-endpoints-on-a-linux-classic-virtual-machine-in-azure"></a>Jak nastavit koncové body na klasické virtuální počítač s Linuxem v Azure
Všechny virtuální počítače Linux, které vytvoříte v Azure pomocí modelu nasazení classic může automaticky komunikovat přes kanál privátní síť s dalšími virtuálními počítači ve stejné cloudové služby nebo virtuální sítě. Počítače v Internetu nebo k jiným virtuálním sítím však vyžadují koncové body směrovat příchozí síťový provoz do virtuálního počítače. Tento článek je také k dispozici pro [virtuální počítače s Windows](../../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic. Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.

V **Resource Manager** modelu nasazení koncové body jsou konfigurováni pomocí **skupiny zabezpečení sítě (Nsg)**. Další informace najdete v tématu [otevírání portů a koncové body](../nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Když vytvoříte virtuální počítač s Linuxem na portálu Azure, koncový bod pro Secure Shell (SSH) je obvykle vám vytvoří automaticky. Při vytváření virtuálního počítače nebo později podle potřeby můžete nakonfigurovat další koncové body.

[!INCLUDE [virtual-machines-common-classic-setup-endpoints](../../../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Další kroky
* Můžete také vytvořit koncový bod virtuálního počítače pomocí [rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2). Spustit **vytvořit koncový bod virtuálního počítače azure** příkaz.
* Pokud jste vytvořili virtuální počítač v modelu nasazení Resource Manager, můžete použít rozhraní příkazového řádku Azure v režimu Resource Manager k [vytvoření skupin zabezpečení sítě](../../../virtual-network/virtual-networks-create-nsg-arm-cli.md) pro řízení přenosu do virtuálního počítače.
