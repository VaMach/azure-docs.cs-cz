---
title: "Připojit virtuální počítače s Linuxem v cloudové službě | Microsoft Docs"
description: "Připojte virtuální počítače Linux vytvořené pomocí modelu nasazení classic do cloudové služby Azure nebo virtuální sítě."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 2fd23055-6b34-4ef0-88a8-fc19e32fb3c9
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: cynthn
ms.openlocfilehash: e222645509640b104410f87e4bcd22834c8d9ec1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="connect-linux-virtual-machines-created-with-the-classic-deployment-model-with-a-virtual-network-or-cloud-service"></a>Připojit virtuální počítače Linux vytvořené pomocí modelu nasazení classic pomocí virtuální sítě nebo cloudové služby
> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic. Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.

Virtuální počítače Linux vytvořené pomocí modelu nasazení classic jsou vždy umístěny v cloudové službě. Cloudové služby slouží jako kontejner a poskytuje jedinečný veřejný název DNS, veřejnou IP adresu a sada koncových bodů pro přístup k virtuálnímu počítači přes Internet. Cloudové služby může být ve virtuální síti, ale není povinné. Můžete také [připojit virtuální počítače s Windows pomocí virtuální sítě nebo cloudové služby](../../windows/classic/connect-vms.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Pokud cloudové služby není ve virtuální síti, se nazývá *samostatné* Cloudová služba. Virtuální počítače v cloudové službě samostatné komunikovat s jinými virtuálními počítači pomocí veřejné názvy DNS jinými virtuálními počítači a provoz se přenáší přes Internet. Pokud cloudové služby ve virtuální síti, virtuální počítače v dané cloudové služby může komunikovat s všechny ostatní virtuální počítače ve virtuální síti bez odeslání veškerou komunikaci přes Internet.

Pokud jste v rámci stejné cloudové služby samostatné virtuální počítače, můžete nadále používat vyrovnávání zatížení a skupiny dostupnosti. Podrobnosti najdete v tématu [virtuální počítače Vyrovnávání zatížení](../../virtual-machines-linux-load-balance.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) a [Správa dostupnosti virtuálních počítačů](../manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Nelze však uspořádat virtuální počítače v podsítích nebo připojení k síti na pracovišti samostatné cloudové služby. Tady je příklad:

[!INCLUDE [virtual-machines-common-classic-connect-vms](../../../../includes/virtual-machines-common-classic-connect-vms.md)]

## <a name="next-steps"></a>Další kroky
Po vytvoření virtuálního počítače, je vhodné [přidat datový disk](attach-disk.md) aby umístění pro uložení dat služby a úlohy.
