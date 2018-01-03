---
title: "Zkontrolujte připojení s sledovací proces sítě Azure - portálu Azure | Microsoft Docs"
description: "Tato stránka vysvětluje, jak používat připojení zkontrolujte s sledovací proces sítě pomocí portálu Azure"
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/03/2017
ms.author: jdial
ms.openlocfilehash: 1f19da71731039e1a39c4440f925b1369886a993
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/21/2017
---
# <a name="check-connectivity-with-azure-network-watcher-using-the-azure-portal"></a>Zkontrolujte připojení s sledovací proces sítě Azure pomocí portálu Azure

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-connectivity-portal.md)
> - [PowerShell](network-watcher-connectivity-powershell.md)
> - [CLI 2.0](network-watcher-connectivity-cli.md)
> - [Rozhraní API Azure REST](network-watcher-connectivity-rest.md)

Naučte se používat připojení k ověření, pokud lze navázat přímé připojení TCP z virtuálního počítače do daného koncového bodu.

## <a name="before-you-begin"></a>Než začnete

Tento článek předpokládá, že máte v následujících zdrojích informací:

* Instance sledovací proces sítě v oblasti, které chcete zkontrolovat připojení.

* Zkontrolujte připojení k virtuálním počítačům.

> [!IMPORTANT]
> Kontrola připojení vyžaduje rozšíření virtuálního počítače `AzureNetworkWatcherExtension`. Instalaci rozšíření na virtuální počítač s Windows najdete v článku [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Windows](../virtual-machines/windows/extensions-nwa.md) a u virtuálního počítače s Linuxem, navštivte [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Linux](../virtual-machines/linux/extensions-nwa.md).

## <a name="check-connectivity-to-a-virtual-machine"></a>Zkontrolujte připojení k virtuálnímu počítači

Tento příklad zkontroluje připojení k cílovému virtuálnímu počítači přes port 80.

Přejděte do vaší sledovací proces sítě a klikněte na tlačítko **Kontrola připojení (Preview)**. Vyberte virtuální počítač, zkontrolujte připojení z. V **cílové** vyberte **vyberte virtuální počítač** a vyberte správnou virtuálního počítače a port pro testování.

Po kliknutí na tlačítko **zkontrolujte**, se kontroluje připojení mezi virtuálními počítači na zadaný port. V příkladu se cílový počítač nedostupný, zobrazí se seznam všech segmentů směrování.

![Výsledky kontroly připojení pro virtuální počítač][1]

## <a name="check-remote-endpoint-connectivity"></a>Zkontrolujte připojení vzdáleného koncového bodu

Zkontrolujte připojení a latence pro vzdálený koncový bod, vyberte **ručně zadejte** přepínač v **cílové** části, zadejte adresu url a port a klikněte na tlačítko **zkontrolujte**.  Používá se pro vzdálené koncové body jako koncové body weby a úložiště.

![Výsledky kontroly připojení pro webovou stránku][2]

## <a name="next-steps"></a>Další kroky

Informace o automatizaci paketu zachytává se virtuální počítač výstrahy zobrazením [vytvořit zaznamenání výstrahy spouštěná paketu](network-watcher-alert-triggered-packet-capture.md)

Najít, pokud určité provoz je povolený v nebo z virtuálního počítače navštivte stránky [zkontrolujte IP tok ověření](network-watcher-check-ip-flow-verify-portal.md)

[1]: ./media/network-watcher-connectivity-portal/figure1.png
[2]: ./media/network-watcher-connectivity-portal/figure2.png
