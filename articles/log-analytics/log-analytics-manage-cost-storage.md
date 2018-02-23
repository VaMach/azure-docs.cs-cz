---
title: "Správa nákladů uchovávání dat v Azure Log Analytics | Microsoft Docs"
description: "Zjistěte, jak chcete změnit cenovou plánu a data zásady uchovávání informací pro pracovní prostor analýzy protokolů na portálu Azure."
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2017
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: 5143abdde715424a41a53bb661db342acf817e0c
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="manage-cost-of-data-retention-with-your-log-analytics-workspace"></a>Správa nákladů uchovávání dat s pracovní prostor analýzy protokolů
Při registraci pro analýzy protokolů, v závislosti na plánu, který vyberete, je omezena na jak dlouho je generované připojených zdrojů dat uložené v pracovním prostoru.  Tento článek popisuje důležité informace, které mohou mít vliv na náklady pro zachování tato data pro různé období a postup konfigurace tento limit.   

Protože Log Analytics může využívat velké objemy dat ze zdroje na místní, může být Cloudová a hybridní prostředí, náklady na ukládání dat pro určitou dobu významnou v závislosti na následujících faktorech:

* Počet systémů, komponent infrastruktury, cloudové prostředky, atd., které shromažďujete od
* Typ dat vytvořené zdroje, například fronty zpráv, protokoly, události, související se zabezpečením dat nebo metrik výkonu.
* Objem dat generovaný podle těchto zdrojů 
* Počet řešení pro správu povolena, zdroj dat a četnosti kolekce

> [!NOTE]
> Jako poskytuje odhad množství dat, které shromáždí naleznete v dokumentaci pro každé řešení.   

Pokud jste na *volné* plán, je omezený na sedm den uchovávání dat.  Pro *samostatné* nebo *zaplacenou* vrstvy, shromažďovaných dat je k dispozici za posledních 31 dní.  

Při použití *volné* plánování, pokud zjistíte konzistentně překročit objemy povolené, můžete změnit pracovního prostoru na plán placené ke shromažďování dat nad rámec tohoto limitu. 

> [!NOTE]
> Pokud zvolíte možnost vybrat delší doby uchování pro úroveň placené účtovány poplatky. Můžete změnit typ vašeho plánu kdykoli a další informace o cenách najdete v tématu [podrobnosti o cenách](https://azure.microsoft.com/pricing/details/log-analytics/). 

## <a name="change-the-data-retention-period"></a>Změnit období uchovávání dat 

1. Přihlaste se k webu [Azure Portal](http://portal.azure.com). 
2. Klikněte na tlačítko **všechny služby**. V seznamu prostředků zadejte **Log Analytics**. Seznam se průběžně filtruje podle zadávaného textu. Vyberte **Log Analytics**.
3. V podokně odběry analýzy protokolů vyberte pracovní prostor k úpravě ze seznamu.
4. Na stránce pracovního prostoru klikněte na **uchování** v levém podokně.
5. V podokně pracovního prostoru uchování posuvník zvýšit nebo snížit počet dní, a potom klikněte na **Uložit**.  Pokud jste na *volné* vrstvy, nebudete moct změnit období uchovávání dat a je třeba upgradovat k vrstvě placené cílem kontrolovat, toto nastavení.<br><br> ![Změna nastavení uchovávání dat pracovního prostoru](media/log-analytics-manage-cost/manage-cost-change-retention.png)

## <a name="next-steps"></a>Další postup  

Pokud chcete zjistit, kolik se data shromažďují, jaké zdroje jsou odesílání a různé typy dat odesílaných ke správě využívání a náklady, najdete v části [analýza využití dat v analýzy protokolů](log-analytics-usage.md)