---
title: "Dostupnost zóny přehled | Microsoft Docs"
description: "Tento článek obsahuje přehled o dostupnosti zóny v Azure."
services: 
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
tags: 
ms.assetid: 
ms.service: 
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2017
ms.author: markgal
ms.custom: mvc I am an ITPro and application developer, and I want to protect (use Availability Zones) my applications and data against data center failure (to build Highly Available applications).
ms.openlocfilehash: b3618207c1d4d2b8d3c0eaf83408b4813cd3da2d
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/04/2018
---
# <a name="overview-of-availability-zones-in-azure-preview"></a>Přehled dostupnosti zón v Azure (Preview)

Dostupnost zóny pomoci při ochraně před selháním úrovni datového centra. Jsou umístěny uvnitř oblasti Azure, a každé z nich má svou vlastní nezávisle spotřeby zdroje, sítě a chlazení. K zajištění odolnosti, je minimálně tři samostatné zóny ve všech oblastech povoleno. Fyzické a logické oddělení dostupnost zóny v rámci oblasti chrání aplikace a data před selháním úrovni zóny. 

![koncepční zobrazení jednu zónu směrem dolů v oblasti.](./media/az-overview/az-graphic-two.png)

## <a name="regions-that-support-availability-zones"></a>Oblasti, které podporují dostupnost zóny

- Východní USA 2
- Západní Evropa
- Francie – střed

## <a name="services-that-support-availability-zones"></a>Služby, které podporují dostupnost zóny

Služby Azure, které podporují dostupnost zóny jsou:

- Virtuální počítače s Linuxem
- Virtuální počítače s Windows
- Sady škálování virtuálního počítače oblastmi
- Managed Disks
- Load Balancer
- Veřejná IP adresa

## <a name="get-started-with-the-availability-zones-preview"></a>Začínáme s preview dostupnost zóny

Dostupnost zóny náhled je dostupný ve východní USA 2, západní Evropa a Francie centrální oblasti pro konkrétní služby Azure. 

1. [Registrace pro náhled zóny dostupnosti](http://aka.ms/azenroll). 
2. Přihlaste se k předplatnému Azure.
3. Vyberte oblast, která podporuje dostupnost zóny.
4. Ke spuštění pomocí služby dostupnost zóny, použijte jednu z následujících odkazů. 
    - [Vytvoření virtuálního počítače](../virtual-machines/windows/create-portal-availability-zone.md)
    - [Vytvoření sady škálování virtuálního počítače](../virtual-machine-scale-sets/virtual-machine-scale-sets-use-availability-zones.md)
    - [Přidejte Disk spravované pomocí prostředí PowerShell](../virtual-machines/windows/attach-disk-ps.md#add-an-empty-data-disk-to-a-virtual-machine)
    - [Load Balancer](../load-balancer/load-balancer-standard-overview.md)

## <a name="next-steps"></a>Další postup
- [Šablony Rychlý start](http://aka.ms/azqs)
