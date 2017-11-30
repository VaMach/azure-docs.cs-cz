---
title: "Provádění datové vědy projekty - Azure Machine Learning | Microsoft Docs"
description: "Jak vědecký pracovník dat můžete sledovat průběh projektu vědecké účely data."
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: bradsev;
ms.openlocfilehash: c8e1882570a8cfcb4a75f0904a2138d007e0bc5b
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/30/2017
---
# <a name="track-progress-of-data-science-projects"></a>Sledovat průběh datové vědy projekty

Data vědecké účely skupiny správců, týmů a potřeba zájemců projektu sledovat průběh jejich týmové projekty, jaké pracovní bylo provedeno na ně a kým a zůstane v seznamu úkolů. 

## <a name="vsts-dashboards"></a>Služby VSTS řídicí panely
Pokud používáte Visual Studio Team Services (VSTS), budete moci vytvářet řídicí panely a sledovat aktivity a pracovní položky přidružené k dané agilní projektu. 

Další informace o tom, jak vytvářet a přizpůsobovat řídicím panelu a pomůckách na Visual Studio Team Services viz následující sady pokynů:

- [Přidání a správa řídicí panely](https://docs.microsoft.com/vsts/report/dashboards/dashboards)
- [Přidat pomůcky do řídicího panelu](https://docs.microsoft.com/vsts/report/dashboards/add-widget-to-dashboard).

## <a name="example-dashboard"></a>Příklad řídicího panelu

Zde je jednoduchý příklad řídicí panel, který je integrovaný ke sledování aktivit sprintu projektu agilní data vědecké účely, jakož i počet potvrzení přidružené úložiště. **Top vlevo** panelu zobrazí:

- odpočítávání aktuálního sprintu 
- Počet potvrzení pro každý úložiště za posledních 7 dnů
- pracovní položka pro konkrétní uživatele. 

Zbývající panelů zobrazit kumulativní vývojový diagram (CFD), úbytek práce a burnup pro projekt:

- **Spodní levé**: CFD množství práce v daném stavu, zobrazení schválené šedě, potvrzeny modře a provádí zeleně.
- **Pravého horního**: pracovní tempo grafu pracovní zleva úplných a zbývající dobu).
- **Vpravo dole**: burnup grafu práce, které bylo dokončeno a celkové množství práce.

![řídicí panel](./media/track-progress/dashboard.png)

Popis toho, jak vytvářet tyto grafy, najdete v části – elementy QuickStart a kurzy v [řídicí panely](https://docs.microsoft.com/vsts/report/dashboards/).
 
## <a name="next-steps"></a>Další kroky

Návody, které ukazují všechny kroky v procesu pro **konkrétních scénářů** jsou také uvedeny. Jsou uvedena v seznamu a propojené s miniatur popisy v [příklad návody](walkthroughs.md) článku. Se ukazují, jak kombinovat cloud, místní nástroje a služby do pracovního postupu nebo kanálu vytvoření inteligentního aplikace. 
