---
title: "Vizualizace závislostí v Azure migrovat | Microsoft Docs"
description: "Poskytuje přehled o vyhodnocení výpočtů ve službě Azure migrovat."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 12/12/2017
ms.author: raynew
ms.openlocfilehash: 10bc249aa70852dce71c96e46319925cececdd8b
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/12/2017
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

Ano. Ve výchozím nastavení se vytvoří pracovní prostor analýzy protokolů, ale nepoužívá, pokud nechcete použít vizualizace závislostí v Azure migrovat. Pokud používáte závislostí vizualizace (nebo pomocí pracovního prostoru mimo Azure migraci), budou se vám účtovat pro použití pracovního prostoru.  [Další informace](https://azure.microsoft.com/pricing/details/insight-analytics/) o cenách řešení mapy služeb. 

## <a name="how-do-i-manage-the-workspace"></a>Jak lze spravovat pracovního prostoru?

Můžete vytvořit pracovní prostor analýzy protokolů mimo Azure migrovat. Pokud odstraníte migrace projektu, ve kterém byla vytvořena nebude odstraněn. Pokud již nepotřebujete pracovním prostoru [odstranit](../log-analytics/log-analytics-manage-access.md) ručně.

Nemáte odstranit pracovní prostor vytvořené Azure migrovat, pokud odstraňte projekt migrace. Pokud tak učiníte, závislosti nefungují podle očekávání.

## <a name="next-steps"></a>Další kroky

[Skupina počítačů pomocí závislosti počítače](how-to-create-group-machine-dependencies.md)