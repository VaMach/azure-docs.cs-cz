---
title: "Znovu nasadit virtuální počítače s Windows v Azure | Microsoft Docs"
description: "Jak znovu nasadit virtuální počítače s Windows v Azure a zmírnit problémy připojení RDP."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: genlin
manager: timlt
tags: azure-resource-manager,top-support-issue
ms.assetid: 0ee456ee-4595-4a14-8916-72c9110fc8bd
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/03/2017
ms.author: genli
ms.openlocfilehash: 5002010e58a5d8e901770c780f07f9bd625d5eb4
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/03/2018
---
# <a name="redeploy-windows-virtual-machine-to-new-azure-node"></a>Znovu nasaďte Windows virtuálního počítače do nového uzlu Azure
Pokud jste byla směrem problémy je řešení potíží s Remote Desktop (RDP) připojení nebo aplikaci přístup k systému Windows Azure virtuálnímu počítači (VM), opětovného nasazení virtuálního počítače může pomoct. Při opětovném nasazování virtuálního počítače, virtuální počítač přesune do nového uzlu v rámci infrastruktury Azure a potom zapne ji zpět, zachování všech možností konfigurace a přidružených prostředků. Tento článek ukazuje, jak znovu nasadit virtuální počítač pomocí prostředí Azure PowerShell nebo portálu Azure.

> [!NOTE]
> Po opětovném virtuálního počítače, dojde ke ztrátě dočasným diskovým a dynamické IP adresy přidružené k rozhraní virtuální sítě se aktualizují. 


## <a name="using-azure-powershell"></a>Použití Azure Powershell
Ujistěte se, že máte nejnovější prostředí Azure PowerShell 1.x nainstalovaný na počítači. Další informace najdete v tématu [Instalace a konfigurace Azure PowerShellu](/powershell/azure/overview).

Následující příklad nasadí virtuální počítač s názvem `myVM` ve skupině prostředků s názvem `myResourceGroup`:

```powershell
Set-AzureRmVM -Redeploy -ResourceGroupName "myResourceGroup" -Name "myVM"
```


[!INCLUDE [virtual-machines-common-redeploy-to-new-node](../../../includes/virtual-machines-common-redeploy-to-new-node.md)]

## <a name="next-steps"></a>Další postup
Pokud máte problémy s připojením k virtuálnímu počítači, můžete najít konkrétní pomoc na [řešení potíží s připojeními RDP](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) nebo [podrobné RDP při řešení potíží](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Pokud nelze získat přístup k aplikaci běžící na vašem virtuálním počítači, můžete si také přečíst [řešení potíží s aplikací](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

