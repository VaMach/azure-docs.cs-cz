---
title: "Definine a spravovat stav v Azure mikroslužeb | Microsoft Docs"
description: "Tom, jak definovat a spravovat stav služby v Service Fabric"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: f5e618a5-3ea3-4404-94af-122278f91652
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 103fd6c3d536bc11f4e39444043a332a1d8f6c01
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="service-state"></a>Stav služby
**Služba stavu** odkazuje v paměti nebo na data na disku, který služba vyžaduje, aby funkce. Obsahuje, například datové struktury a členské proměnné, které Služba čte a zapisuje fungují. V závislosti na tom, jak je navržen služby se mohly by také zahrnovat souborům nebo jiným prostředkům, které jsou uložené na disku. Například soubory databáze využije k uložení dat a protokoly transakcí.

Jako příklad služba zvažte kalkulačky. Základní kalkulačky služby trvá dvou čísel a vrátí jejich součet. Provádění tohoto výpočtu zahrnuje žádné proměnné členů nebo Další informace.

Teď se podíváme stejné kalkulačky, ale s další metodu pro ukládání a vrací poslední součet je počítaný. Tato služba je nyní stateful. Stateful znamená, že obsahuje některé stavu, který zapíše do při vypočítá nové sum a načte z když požádáte, vrátit poslední vypočítaný součet.

V Azure Service Fabric nazývá první službě bezstavové služby. Druhý služba se nazývá stavové služby.

## <a name="storing-service-state"></a>Ukládání stavu služby
Stav můžete externalized nebo umístěny společně s kódem, který je manipulace s stavu. Externalization stavu se obvykle provádí pomocí externí databáze nebo jiného úložiště dat, který běží na různých počítačích přes síť nebo mimo proces na stejném počítači. V našem příkladu kalkulačky úložiště dat může být SQL database nebo instanci Azure tabulka úložiště. Každý požadavek na výpočetní součet provede aktualizace na tato data a požadavky na službu k vrácení výsledku hodnota v hodnotě aktuální iterace z obchodu. 

Stav může být také společně umístěný se kód, který zpracovává stavu. Stavové služby v Service Fabric se obvykle vytvořená s využitím tohoto modelu. Service Fabric poskytuje infrastrukturu pro zkontrolujte, zda tento stav je vysoce dostupný, konzistentní a odolné a že služby vytvořené tímto způsobem můžete snadno škálovat.

## <a name="next-steps"></a>Další kroky
Další informace o konceptech Service Fabric najdete v následujících článcích:

* [Dostupnost služeb Service Fabric](service-fabric-availability-services.md)
* [Škálovatelnost služby Service Fabric](service-fabric-concepts-scalability.md)
* [Vytváření oddílů služby Service Fabric](service-fabric-concepts-partitioning.md)
* [Spolehlivé služby Service Fabric](service-fabric-reliable-services-introduction.md)
