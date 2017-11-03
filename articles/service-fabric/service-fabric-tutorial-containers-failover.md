---
title: "Převzetí služeb při selhání a škálování aplikace Azure Service Fabric kontejnery | Microsoft Docs"
description: "Zjistěte, jak se v aplikaci Azure Service Fabric kontejnery zpracovává převzetí služeb při selhání.  Také informace o škálování kontejnerů a služeb spuštěných v clusteru."
services: service-fabric
documentationcenter: 
author: suhuruli
manager: timlt
editor: suhuruli
tags: servicefabric
keywords: "Docker kontejnery, Mikroslužeb, Service Fabric, Azure"
ms.assetid: 
ms.service: service-fabric
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/12/2017
ms.author: suhuruli
ms.custom: mvc
ms.openlocfilehash: 21dd9dfbc90c26236c43e2c334305ca97f63d361
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="demonstrate-fail-over-and-scaling-of-container-services-with-service-fabric"></a>Předvedení selhání více a škálování služby kontejneru s Service Fabric

V tomto kurzu je součástí série, tři. V tomto kurzu zjistíte, jak zpracovává převzetí služeb při selhání v Service Fabric – aplikace typu kontejner. Kromě toho zjistíte, jak škálování kontejnerů. V tomto kurzu jste:

> [!div class="checklist"]
> * Další informace o kontejneru převzetí služeb při selhání v clusteru Service Fabric  
> * Škálování front-endu webové kontejnery v aplikace provádáte

## <a name="prerequisites"></a>Požadavky
Aplikace z [část 2](service-fabric-tutorial-package-containers.md) běží v clusteru Service Fabric active.

## <a name="fail-over-a-container-in-a-cluster"></a>Převzetí služeb při selhání kontejneru v clusteru
Service Fabric zajišťuje vaše instance kontejneru automaticky přesune na jiné uzly v clusteru, musí dojít k chybě. Můžete také ručně uzel kontejnerů vyprázdnit a řádně přesunout do jiných uzlů v clusteru. Služby můžete škálovat několika způsoby, v tomto příkladu používáme Service Fabric Explorer.

Pokud chcete převzít služby při selhání front-end kontejneru, proveďte následující kroky:

1. Otevřete ve vašem clusteru Service Fabric Explorer – například `http://lin4hjim3l4.westus.cloudapp.azure.com:19080`.
2. Klikněte na **fabric: / TestContainer/azurevotefront** uzlu ve stromovém zobrazení a rozbalte uzel oddílu (představované identifikátor GUID). Všimněte si název uzlu ve stromovém zobrazení, se zobrazí uzly kontejneru aktuálně běží – například`_nodetype_1`
3. Rozbalte **uzly** uzlu ve stromovém zobrazení. Klikněte na znak výpustky (tři tečky) vedle uzlu, který je spuštěn kontejneru.
1. Pokud chcete tento uzel restartovat, zvolte **Restartovat** a potvrďte akci restartování. Restartování způsobí převzetí služeb při selhání kontejneru do jiného uzlu v clusteru.

![noderestart][noderestart]

Všimněte si, jak označující název uzlu tam, kde běží front-endu kontejnery, nyní se změní na jiný uzel v clusteru. Po chvíli se nyní byste měli mít vyhledejte aplikaci znovu a zobrazit aplikace teď běžící na jiný uzel.

## <a name="scale-containers-and-services-in-a-cluster"></a>Škálování kontejnerů a služby v clusteru
Kontejnery Service Fabric je možné rozšířit mezi clustery pro přizpůsobení pro zatížení v rámci služeb. Kontejner je škálovat tak, že změníte počet instancí spuštěných v clusteru.

Škálování webového front-endu, proveďte následující kroky:

1. Otevřete ve vašem clusteru Service Fabric Explorer – například `http://lin4hjim3l4.westus.cloudapp.azure.com:19080`.
2. Klikněte na znak výpustky (tři tečky) vedle položky **fabric: / TestContainer/azurevotefront** uzel ve stromu zobrazit a vybrat **škálování služby**.

![sfxscale][sfxscale]

Nyní můžete škálovat počet instancí webového front-endu.

3. Změňte počet na **2** a klikněte na **Škálovat službu**.
4. Klikněte na **fabric: / TestContainer/azurevotefront** uzel ve stromu zobrazení a rozbalte uzel oddílu (představované identifikátor GUID).

![sfxscaledone][sfxscaledone]

Nyní je vidět, že má služba dvě instance. Ve stromovém zobrazení můžete zjistit, které uzly spustili instance.

Touto jednoduchou úlohou správy jsme zdvojnásobili prostředky, které má naše front-end služba k dispozici pro zpracování uživatelské zátěže. Je důležité si uvědomit, že pro spolehlivý provoz služby nepotřebujete více jejích instancí. Pokud služba selže, Service Fabric zajistí v clusteru spuštění nové instance služby.

## <a name="next-steps"></a>Další kroky

V tomto kurzu se ukázán kontejneru převzetí služeb při selhání a také škálování aplikace. Dokončili jste následující kroky:

> [!div class="checklist"]
> * Další informace o kontejneru převzetí služeb při selhání v clusteru Service Fabric  
> * Škálování front-endu webové kontejnery v aplikace provádáte

V této série kurzu jste zjistili, jak: 
> [!div class="checklist"]
> * Vytvořit kontejner bitové kopie
> * Push kontejneru bitové kopie do registru kontejner Azure
> * Kontejnery balíčku pro Service Fabric pomocí Yeoman
> * Sestavení a spuštění aplikace služby infrastruktury s kontejnery
> * Zpracování převzetí služeb při selhání a škálování v Service Fabric

[noderestart]: ./media/service-fabric-tutorial-containers-failover/containersfailovertutorialnoderestart.png
[sfxscale]: ./media/service-fabric-tutorial-containers-failover/containersfailovertutorialscale.png
[sfxscaledone]: ./media/service-fabric-tutorial-containers-failover/containersfailovertutorialscaledone.png
