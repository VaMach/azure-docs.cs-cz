---
title: "Vizualizace závislostí v Azure migrovat | Microsoft Docs"
description: "Poskytuje přehled o vyhodnocení výpočtů ve službě Azure migrovat."
services: migrate
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 78e52157-edfd-4b09-923f-f0df0880e0e0
ms.service: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/22/2017
ms.author: raynew
ms.openlocfilehash: e787cd6746b53c28fc1495b9a3ac62f2aa08f639
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/27/2017
---
# <a name="dependency-visualization"></a>Vizualizace závislostí

[Azure migrovat](migrate-overview.md) služby vyhodnocuje skupiny místní počítače pro migraci do Azure. K seskupení počítačů, můžete použít závislost vizualizace. Tento článek obsahuje informace o této funkci.


## <a name="overview"></a>Přehled

Vizualizace závislostí v migraci Azure můžete vytvořit skupiny pro vyhodnocení migrace s vyšší jistotou. Pomocí vizualizace závislostí můžete zobrazit závislosti sítě u konkrétních počítačů nebo přes skupiny počítačů. Je to vhodné, ujistěte se, že žádné funkce nebo ke ztrátě (nebo počítače zapomenete) v procesu migrace, když aplikace a úlohy spustit napříč více počítačů.  

## <a name="how-does-it-work"></a>Jak to funguje?

Azure používá migrací [mapy služeb](../operations-management-suite/operations-management-suite-service-map.md) řešení v [analýzy protokolů](../log-analytics/log-analytics-overview.md) pro vizualizaci závislostí.
- Při vytváření projektu Azure migrace k pracovnímu prostoru analýzy protokolů OMS se vytvoří ve vašem předplatném.
- Název pracovního prostoru je zadaný název migrace projektu, s předponou **migrovat-**a volitelně vyznačeno s číslem. 
- Přejděte do pracovního prostoru analýzy protokolů z **Essentials** části projektu **přehled** stránky.
- Vytvořeným pracovním prostorem je označené klíč **MigrateProject**a hodnota **název projektu**. To slouží k vyhledávání na webu Azure portal.  

    ![Pracovní prostor analýzy protokolů](./media/concepts-dependency-visualization/oms-workspace.png)

Pokud chcete použít závislost vizualizace, musíte stáhnout a nainstalovat agenty na každý místní počítač, který chcete analyzovat.  

## <a name="do-i-need-to-pay-for-it"></a>Je potřeba věnovat pro ni?

Ano. Ve výchozím nastavení se vytvoří pracovní prostor analýzy protokolů, ale nepoužívá, pokud nechcete použít vizualizace závislostí v Azure migrovat. Pokud používáte závislostí vizualizace (nebo pomocí pracovního prostoru mimo Azure migraci), budou se vám účtovat pro použití pracovního prostoru.  [Další informace](https://www.microsoft.com/cloud-platform/operations-management-suite) o cenách řešení mapy služeb. 

## <a name="how-do-i-manage-the-workspace"></a>Jak lze spravovat pracovního prostoru?

Můžete vytvořit pracovní prostor analýzy protokolů mimo Azure migrovat. Pokud odstraníte migrace projektu, ve kterém byla vytvořena nebude odstraněn. Pokud již nepotřebujete pracovním prostoru [odstranit](../log-analytics/log-analytics-manage-access.md) ručně.

Nemáte odstranit pracovní prostor vytvořené Azure migrovat, pokud odstraňte projekt migrace. Pokud tak učiníte, závislosti nefungují podle očekávání.

## <a name="next-steps"></a>Další kroky

[Skupina počítačů pomocí závislosti počítače](how-to-create-group-machine-dependencies.md)