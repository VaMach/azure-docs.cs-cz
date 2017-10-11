---
title: "Co dělat v případě přerušení služby Azure služby virtuálních sítí Azure, které mají vliv | Microsoft Docs"
description: "Zjistěte, co dělat v případě přerušení služby Azure služby virtuálních sítí Azure, které mají vliv."
services: virtual-network
documentationcenter: 
author: NarayanAnnamalai
manager: jefco
editor: 
ms.assetid: ad260ab9-d873-43b3-8896-f9a1db9858a5
ms.service: virtual-network
ms.workload: virtual-network
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2016
ms.author: narayan;aglick
ms.openlocfilehash: 4e125406d2e798138c45e3fbbf61a610afab69fc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="virtual-network--business-continuity"></a>Virtuální síť – kontinuita podnikových procesů
## <a name="overview"></a>Přehled
Virtuální síť (VNet) je logická reprezentace sítě v cloudu. Umožňuje definovat vlastní prostor privátní IP adresy a segmentovat do podsítí v síti. Virtuální sítě slouží jako hranice vztahu důvěryhodnosti pro hostování svoje výpočetní prostředky, jako jsou virtuální počítače Azure a cloudových služeb (webové/role pracovního procesu). Virtuální síť, která umožňuje přímé privátní IP komunikaci mezi prostředky v ní umístěné. Virtuální sítě lze také propojit k místní síti prostřednictvím jednoho z hybridní možnosti, jako je brána sítě VPN nebo ExpressRoute.

Virtuální síť, která je vytvořena v rámci oboru oblasti. Virtuální sítě můžete vytvořit pomocí stejné adresní prostor ve dvou různých oblastech (tj. v oblasti USA – východ a oblasti USA – Západ, ale nemůže připojit je k sobě navzájem přímo). 

## <a name="business-continuity"></a>Kontinuita podnikových procesů
Může dojít k několika různými způsoby, že vaše aplikace může být přerušeny. S daným regionem může být zcela zkrácené z důvodu přírodní katastrofě nebo částečné po havárii z důvodu chyby více zařízení nebo služeb. Dopad na službu virtuální sítě se liší v každé z těchto situacích.

**Otázka: co můžete dělat v případě výpadku pro celou oblast? tj. Pokud oblast je zcela konečného kvůli přírodní katastrofě? Co se stane k virtuálním sítím, které jsou hostované v oblasti?**

Odpověď: virtuální sítě a prostředky v oblasti ovlivněných nepřístupná během doby přerušení služby.

![Diagram jednoduché virtuální sítě](./media/virtual-network-disaster-recovery-guidance/vnet.png)

**Otázka: co se dá znovu vytvořit v jiné oblasti stejnou virtuální síť?**

Odpověď: virtuální síť (VNet) je poměrně jednoduché prostředků. Můžete volat rozhraní API služby Azure k vytvoření virtuální sítě s stejné adresní prostor v jiné oblasti. Znovu vytvořit stejné prostředí, ve kterém se nacházel v ovlivněných oblasti, budete muset provádět volání rozhraní API služby Cloud Services (webové/role pracovního procesu) a virtuální počítače, které jste měli znovu nasadit. Bude také muset číselníku bránu VPN a připojit k síti na pracovišti, pokud jste měli místní připojení (například v hybridním nasazení).

Pokyny pro vytvoření virtuální sítě najdete [zde](virtual-networks-create-vnet-arm-pportal.md). 

**Otázka: je možné repliku virtuální sítě v dané oblasti se znovu vytvořit v jiné oblasti předem?**

Odpověď: Ano, můžete vytvořit dvě virtuální sítě pomocí stejné prostor privátní IP adresy a prostředky ve dvou různých oblastech předem. Pokud zákazník byl hostování internetové služby ve virtuální síti, se může nastavit až Traffic Manageru můžete provoz směrovat geografické oblasti, která je aktivní. Zákazník však nemůže spojit dvě virtuální sítě se stejné adresního prostoru svojí místní síti jako by to způsobilo směrování problémy. V době havárie a ztrátě virtuální sítě v jedné oblasti může zákazník připojit další virtuální síť v oblasti k dispozici s odpovídajícím adresního prostoru svojí místní síti.

Pokyny pro vytvoření virtuální sítě najdete [zde](virtual-networks-create-vnet-arm-pportal.md).

