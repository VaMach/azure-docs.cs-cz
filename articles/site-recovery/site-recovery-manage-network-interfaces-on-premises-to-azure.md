---
title: "Správa síťových rozhraní v Azure Site Recovery pro místním nasazením a Azure scénáře | Microsoft Docs"
description: "Popisuje, jak spravovat síťová rozhraní pro místním nasazením a Azure scénáře s Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/22/2017
ms.author: manayar
ms.openlocfilehash: b60c746ae6c243d2b448056f8768df3baaed4cc1
ms.sourcegitcommit: 4256ebfe683b08fedd1a63937328931a5d35b157
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/23/2017
---
# <a name="manage-virtual-machine-network-interfaces-for-on-premises-to-azure-scenarios"></a>Spravovat síťová rozhraní virtuálních počítačů pro místním nasazením a Azure scénáře

Virtuální počítač v Azure musí mít aspoň jedno síťové rozhraní, které jsou k němu připojen a může mít libovolný počet síťových rozhraní připojené k němu jako podporuje velikost virtuálního počítače. Ve výchozím nastavení je první síťové rozhraní připojené k virtuální počítač Azure definován jako primární síťové rozhraní. Všechny ostatní síťová rozhraní virtuálního počítače se sekundárními síťovými rozhraními. Ve výchozím nastavení všechny odchozí přenosy z virtuálního počítače odeslání přiřazené primární konfiguraci IP primární síťové rozhraní IP adresy.

V prostředí místní virtuální počítače nebo servery může mít více síťových rozhraní pro různé sítě v prostředí. Různé sítě jsou obvykle používány k provedení určité operace, například upgrady, údržby, přístup k Internetu, atd. Při migraci nebo přebírání služeb při selhání do Azure z místního prostředí, mějte na paměti, že síťová rozhraní stejného virtuálního počítače všechny připojen ke stejné virtuální síti.

Ve výchozím nastavení vytvoří Site Recovery jako mnoho síťová rozhraní na virtuální počítač Azure, jako jsou připojené k místnímu serveru. Vytváření redundantních síťových rozhraní během migrace nebo převzetí služeb při selhání úpravou nastavení síťového rozhraní v části nastavení pro replikované virtuální počítač se můžete vyhnout.

## <a name="select-the-target-network"></a>Vyberte cílovou síť

Pro VMware a fyzické počítače a pro virtuální počítače Hyper-V (bez VMM) můžete zadat cílový virtuální sítě pro jednotlivé virtuální počítače. Pro virtuální počítače Hyper-V spravované s nástrojem VMM [mapování sítě](site-recovery-network-mapping.md) slouží k mapování sítě virtuálních počítačů na zdrojovém serveru VMM a cílové sítě Azure.

1. V části "replikované položky' v trezoru služeb zotavení klikněte na libovolné replikované položky pro přístup k nastavení tohoto replikované položky.

2. Klikněte na kartu 'výpočtů a sítě, pro přístup k nastavení sítě pro replikované položky.

3. V části Vlastnosti sítě zvolte ze seznamu dostupných síťových rozhraní virtuální sítě.

    ![Výpočty a síť](./media/site-recovery-manage-network-interfaces-on-premises-to-azure/compute-and-network.png)

Úprava cílové síti ovlivňuje všech síťových rozhraní pro tento konkrétní virtuální počítač.

Úprava mapování sítě pro cloudy VMM, ovlivňuje všechny virtuální počítače a jejich síťových rozhraní.

## <a name="select-the-target-interface-type"></a>Vyberte cílový typ rozhraní

Části "Síťové rozhraní" okna 'výpočtů a sítě, můžete zobrazit a upravit nastavení síťového rozhraní a zadejte typ cílového síťového rozhraní.

- A **primární** síťové rozhraní je vyžadována pro převzetí služeb při selhání.
- Všechny ostatní vybraná síťová rozhraní, pokud jsou některé, **sekundární** síťových rozhraní.
- Vyberte **nepoužívejte** vyloučit síťové rozhraní z vytvoření na převzetí služeb při selhání.

Ve výchozím nastavení při povolení replikace, Site Recovery vybere všechny nalezené síťových rozhraní na serveru místní, jeden jako 'primární a všechny ostatní označíte jako 'Sekundární' síťových rozhraní. Všechny následné rozhraní přidat na místním serveru jsou označené, nepoužívejte' ve výchozím nastavení. Když přidáváte více síťových rozhraní, ujistěte se, že cílovou velikost virtuálního počítače správné Azure je vybrána pro uložení všech požadovaných síťových rozhraní.

## <a name="modifying-network-interface-settings"></a>Úprava nastavení síťového rozhraní

Podsíť a IP můžete upravit pro replikované položky síťová rozhraní. Pokud není zadán IP adresy, Site Recovery přiřadit další dostupnou IP adresu z podsítě pro síťové rozhraní na převzetí služeb při selhání.

1. Klikněte na libovolné dostupná síťová rozhraní otevřete okno nastavení rozhraní sítě.

2. Požadovaná podsíť zvolte ze seznamu dostupných podsítí.

3. Zadejte požadovanou IP adresu (podle potřeby).

    ![Nastavení síťového rozhraní](./media/site-recovery-manage-network-interfaces-on-premises-to-azure/network-interface-settings.png)

4. Klikněte na tlačítko OK ukončení úprav a vrátíte se do okna 'Výpočetní a síťové'.

5. Opakujte kroky 1 – 4 pro jiné síťová rozhraní.

6. Klikněte na tlačítko Uložit' uložte všechny změny.

## <a name="next-steps"></a>Další postup
  [Další informace](../virtual-network/virtual-network-network-interface-vm.md) o síťová rozhraní pro virtuální počítače Azure.
