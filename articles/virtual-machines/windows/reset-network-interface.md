---
title: "Postup resetování síťové rozhraní virtuálního počítače Windows Azure | Microsoft Docs"
description: "Ukazuje, jak resetování síťové rozhraní virtuálního počítače Windows Azure"
services: virtual-machines-windows, azure-resource-manager
documentationcenter: 
author: genlin
manager: willchen
editor: 
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/03/2017
ms.author: genli
ms.openlocfilehash: 77d6bf3bb8b0e03419f93e97746bc1a0a091045e
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/04/2017
---
# <a name="how-to-reset-network-interface-for-azure-windows-vm"></a>Postup resetování síťové rozhraní virtuálního počítače Windows Azure 

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Nemůžete se připojit k Microsoft Azure Windows virtuální počítač (VM), po zakázání výchozí síťové rozhraní (NIC) nebo ručně nastaví statickou IP adresu pro síťovou kartu. Tento článek ukazuje, jak resetovat rozhraní sítě pro virtuální počítač Windows Azure, který bude vyřešen problém vzdáleného připojení.

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]
## <a name="reset-network-interface"></a>Resetování síťové rozhraní

### <a name="for-classic-vms"></a>Pro klasické virtuální počítače

Chcete-li obnovit síťové rozhraní, postupujte takto:

1.  Přejděte na [portál Azure]( https://ms.portal.azure.com).
2.  Vyberte **virtuálních počítačů (klasické)**.
3.  Vyberte ovlivněné virtuální počítač.
4.  Vyberte **IP adresy**.
5.  Pokud **privátní IP přiřazení** není **statické**, změňte ho na **statické**.
6.  Změna **IP adresu** jinou IP adresu, která je dostupná v podsíti.
7.  Vyberte Uložit.
8.  Virtuální počítač se restartuje k chybě při inicializaci nového síťového adaptéru do systému.
9.  Zkuste pro připojení RDP k vašemu počítači. Pokud bylo úspěšné, můžete změnit privátní IP adresu zpět do původního Pokud vás zajímají. Jinak můžete ponechat ji. 

### <a name="for-vms-deployed-in-resource-group-model"></a>Pro virtuální počítače nasazené v modelu skupiny prostředků

1.  Přejděte na [portál Azure]( https://ms.portal.azure.com).
2.  Vyberte ovlivněné virtuální počítač.
3.  Vyberte **síťových rozhraní**.
4.  Vyberte síťové rozhraní přidružené k počítači
5.  Vyberte **konfigurace protokolu IP**.
6.  Vyberte IP adresu. 
7.  Pokud **privátní IP přiřazení** není **statické**, změňte ho na **statické**.
8.  Změna **IP adresu** jinou IP adresu, která je dostupná v podsíti.
9. Virtuální počítač se restartuje k chybě při inicializaci nového síťového adaptéru do systému.
10. Zkuste pro připojení RDP k vašemu počítači. Pokud bylo úspěšné, můžete změnit privátní IP adresu zpět do původního Pokud vás zajímají. Jinak můžete ponechat ji. 

## <a name="delete-the-unavailable-nics"></a>Odstranit není k dispozici síťové karty
Poté, co je to možné vzdálené plochy k počítači, je nutné odstranit staré síťové adaptéry, aby se zabránilo potenciální problém:

1.  Otevřete Správce zařízení.
2.  Vyberte **zobrazení** > **zobrazit skrytá zařízení**.
3.  Vyberte **síťové adaptéry**. 
4.  Zkontrolujte u adaptérů s názvem jako "Microsoft Hyper-V síťový adaptér".
5.  Může se zobrazit není k dispozici adaptér, který je zobrazena šedě. Klikněte pravým tlačítkem na adaptéru a potom vyberte možnost odinstalovat.

    ![bitové kopie na síťový adaptér](media/reset-network-interface/nicpage.png)

    > [!NOTE]
    > Není k dispozici adaptéry, které mají název "Microsoft Hyper-V síťový adaptér" pouze odinstalujte. Pokud odinstalujete všechny ostatní skrytá adaptéry, může způsobit další problémy.
    >
    >

6.  Nyní všechny adaptér není k dispozici je nutné vymazat z vašeho systému.
