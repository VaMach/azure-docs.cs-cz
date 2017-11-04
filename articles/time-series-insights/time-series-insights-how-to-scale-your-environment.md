---
title: "Postup škálování Azure časové řady Přehled prostředí | Microsoft Docs"
description: "Tento kurz se zaměřuje na postup škálování Azure časové řady Přehled prostředí"
keywords: 
services: time-series-insights
documentationcenter: 
author: sandshadow
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: tsi
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/19/2017
ms.author: edett
ms.openlocfilehash: ba6bd1ab05bb7e24dd1bc307218e7a772fbde601
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-scale-your-time-series-insights-environment"></a>Postup škálování prostředí Statistika časové řady

Tento kurz se zaměřuje na postup škálování prostředí Statistika časové řady.

> [!NOTE]
> Rozšiřování škálování využívajících typů sku není povoleno. Prostředí s S1 Sku nelze převést do prostředí S2.

## <a name="s1-sku-ingress-rates-and-capacities"></a>Příjem příchozích dat sazby S1 SKU a kapacity

| S1 Kapacita SKU | Příchozí míra | Maximální kapacita úložiště
| --- | --- | --- |
| 1 | 1 GB (1 milion událostí) | (30 milionů událostí) 30 GB za měsíc |
| 10 | 10 GB (10 milionů událostí) | 300 GB za měsíc (300 milión událostí) |

## <a name="s2-sku-ingress-rates-and-capacities"></a>S2 SKU příjem příchozích dat rychlostí a kapacity

| S2 Kapacita SKU | Příchozí míra | Maximální kapacita úložiště
| --- | --- | --- |
| 1 | 10 GB (10 milionů událostí) | 300 GB za měsíc (300 milión událostí) |
| 10 | 100 GB (100 miliónů událostí) | 3 TB (3 miliardy událostí) za měsíc |

Kapacity lineárně, takže S1 sku s kapacitou 2 podporuje 2 GB (2 miliony) událostí za den příchozího rychlost a 60 GB (60 milión událostí) za měsíc.

## <a name="changing-the-capacity-of-your-environment"></a>Změna kapacitu vašeho prostředí

1. Na portálu Azure vyberte prostředí, jehož kapacitu, kterou chcete změnit.
1. V části nastavení klikněte na tlačítko Konfigurovat.
1. Posuvníkem kapacity vyberte kapacitu, který splňuje požadavky pro příjem příchozích dat sazby a kapacitu úložiště.

## <a name="next-steps"></a>Další kroky

* Ověřte, že nové kapacity dostatečná k zabránění, omezení šířky pásma. Další podrobnosti najdete v tématu *prostředí může získávání omezeny* části [zde](time-series-insights-diagnose-and-solve-problems.md).