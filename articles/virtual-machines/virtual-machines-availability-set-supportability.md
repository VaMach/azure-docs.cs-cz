---
title: "Nastavit možnosti přidávání virtuálních počítačích Azure do existující dostupnosti | Microsoft Docs"
description: "Možnosti podpory přidávání virtuálních počítačích Azure do stávající sadu dostupnosti."
services: virtual-machines-linux
documentationcenter: 
author: Deland-Han
manager: cshepard
editor: 
ms.assetid: 
ms.service: virtual-machines
ms.workload: virtual-machines
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 7/15/2017
ms.author: delhan
ms.openlocfilehash: eb1a6f93670317203c4de53b7b1307a2034c26ea
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="supportability-of-adding-azure-vms-to-an-existing-availability-set"></a>Možnosti podpory přidávání virtuálních počítačích Azure do stávající sadu dostupnosti

Někdy můžete setkat s omezení při přidání nových virtuálních počítačů (VM) do stávající sady dostupnosti. Následující graf zobrazuje podrobnosti o které řady virtuálních počítačů je možné kombinovat ve stejné sadě dostupnosti.

Tady je podpoře matice do kombinovat různé typy virtuálních počítačů:

Ná & sady dostupnosti.|Druhý virtuální počítač|A|Av2|D|Dv2|Dv3|
|---|---|---|---|---|---|---|
|První virtuální počítač|||||||
|A||OK|OK|OK|OK|OK|
|Av2||OK|OK|OK|OK|OK|
|D||OK|OK|OK|OK|OK|
|Dv2||OK|OK|OK|OK|OK|
|Dv3||OK|OK|OK|OK|OK|

Všechny ostatní řady nemůže být ve stejné dostupnosti nastavit, protože vyžadují konkrétní hardware.

Virtuální počítač a8/A9 velikost nelze směšovat kvůli requirment na vyhrazenou síť back-end RDMA.
