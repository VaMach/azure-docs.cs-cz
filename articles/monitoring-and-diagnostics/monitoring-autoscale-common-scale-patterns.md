---
title: "Přehled běžných vzorů škálování | Microsoft Docs"
description: "Další některých běžných vzorů na automatické škálování prostředku v Azure."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d37d3fda-8ef1-477c-a360-a855b418de84
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: ancav
ms.openlocfilehash: fce51546e041c8989d813c3935e058c52b38ba77
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="overview-of-common-autoscale-patterns"></a>Přehled běžných vzorů škálování
Tento článek popisuje některé z běžných vzorů škálování prostředku v Azure.

Azure monitorování automatického škálování se vztahuje pouze na virtuální počítač škálování sady (VMSS), cloudové služby, plány služby app a prostředí app service. 

# <a name="lets-get-started"></a>Umožňuje Začínáme

Tento článek předpokládá, že jste obeznámeni s automatické škálování. Můžete [Začněte zde škálování prostředku][1]. Toto jsou některé obecné vzory škálování.

## <a name="scale-based-on-cpu"></a>Škálování podle využití procesoru

Máte webovou aplikaci (/ VMSS/cloudové služby role) a 

- Chcete Škálováním na více systémů nebo měřítka ve na základě využití procesoru.
- Kromě toho chcete zajistit, že je minimální počet instancí. 
- Navíc chcete zajistit nastavit maximální limit pro počet instancí, které je možné škálovat na.

![Škálování podle využití procesoru][2]

## <a name="scale-differently-on-weekdays-vs-weekends"></a>Škálování jinak o víkendech vs dny v týdnu

Máte webovou aplikaci (/ VMSS/cloudové služby role) a

- Chcete 3 instance ve výchozím nastavení (ve všední dny)
- O víkendech nepředpokládáte přenosy a proto chcete škálovat dolů 1 instance o víkendech.

![Škálování jinak o víkendech vs dny v týdnu][3]

## <a name="scale-differently-during-holidays"></a>Jinak škálování během svátků

Máte webovou aplikaci (/ VMSS/cloudové služby role) a 

- Chcete škálovat nahoru/dolů podle využití procesoru ve výchozím nastavení
- Však během sváteční sezóny (nebo konkrétní dny, které jsou důležité pro vaši firmu) chcete přepsat výchozí hodnoty a mít k dispozici větší kapacitu.

![Jinak na svátků škálování][4]

## <a name="scale-based-on-custom-metric"></a>Škálování podle vlastní metriku

Máte front-end webové a vrstvu rozhraní API, který komunikuje s back-end. 

- Chcete-li škálovat vrstvu rozhraní API na základě vlastních událostí v části front end (Příklad: Chcete škálovat procesu odbavení na základě počtu položek v nákupní košík)

![Škálování podle vlastní metriku][5]

<!--Reference-->
[1]: ./monitoring-autoscale-get-started.md
[2]: ./media/monitoring-autoscale-common-scale-patterns/scale-based-on-cpu.png
[3]: ./media/monitoring-autoscale-common-scale-patterns/weekday-weekend-scale.png
[4]: ./media/monitoring-autoscale-common-scale-patterns/holidays-scale.png
[5]: ./media/monitoring-autoscale-common-scale-patterns/custom-metric-scale.png