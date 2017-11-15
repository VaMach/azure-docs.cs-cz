---
title: "Nasazení fáze životního cyklu proces vědecké účely dat Team - Azure | Microsoft Docs"
description: "Cíle, úlohy a úkoly pro fázi nasazení vašich projektů vědecké zpracování dat"
services: machine-learning
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
ms.date: 11/04/2017
ms.author: bradsev;
ms.openlocfilehash: 45d801bf0096879143f91feb230445625559379f
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/09/2017
---
# <a name="deployment"></a>Nasazení

Tento článek popisuje cíle, úlohy a úkoly přidruženou k nasazení z tým datové vědy procesu (TDSP). Tento proces zajišťuje doporučené životního cyklu, který můžete použít pro struktury vědecké zpracování dat projekty. Životní cyklus popisuje hlavní fází, které projekty obvykle provést, často interaktivně:

   1. **Pochopení obchodních**
   2. **Získávání dat a principy**
   3. **Modelování**
   4. **Nasazení**
   5. **Přijetí zákazníka**

Zde je vizuální reprezentace životního cyklu TDSP: 

![Životní cyklus TDSP](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goal"></a>Cílem
Nasaďte modely datovém kanálu pro produkční nebo prostředí produkčního prostředí pro přijetí konečného uživatele. 

## <a name="how-to-do-it"></a>Jak to udělat
Hlavní úloha řešeny v této fázi:

**Zprovoznit model**: nasadit model a kanál pro produkční nebo prostředí produkčního prostředí pro používání aplikace.

### <a name="operationalize-a-model"></a>Zprovoznit model
Až budete mít sadu modely, které provádějí dobře, můžete je u ostatních aplikací využívají zprovoznit. V závislosti na obchodní požadavky jsou vytvářeny předpovědi buď v reálném čase, nebo na základě dávky. Pokud chcete nasadit modely, umístěte je s otevřené rozhraní API. Rozhraní umožňuje modelu, který má snadno využívat z různých aplikací, jako například:

   * Online weby
   * Tabulek 
   * Řídicí panely
   * Obchodní aplikace 
   * Back-end aplikace 

Příklady operationalization modelu pomocí webové služby Azure Machine Learning najdete v tématu [nasazení webové služby Azure Machine Learning](../studio/publish-a-machine-learning-web-service.md). Je osvědčeným postupem vytvoření telemetrie a sledování do provozního modelu a datový kanál, který nasadíte. Tento postup pomáhá stavem následné systému hlášení a řešení potíží.  

## <a name="artifacts"></a>Artefakty

* Řídicí panel stavu, který zobrazí metriky systému stavu a klíč
* Poslední modelování sestavu s podrobnostmi o nasazení
* Architektura dokumentu konečné řešení


## <a name="next-steps"></a>Další kroky

Tady jsou odkazy na každý krok v životním cyklu TDSP:

   1. [Pochopení obchodních](lifecycle-business-understanding.md)
   2. [Získávání dat a principy](lifecycle-data.md)
   3. [Modelování](lifecycle-modeling.md)
   4. [Nasazení](lifecycle-deployment.md)
   5. [Přijetí zákazníka](lifecycle-acceptance.md)

Poskytujeme návody úplného začátku do konce, které ukazují všechny kroky v procesu pro konkrétní scénáře. [Příklad návody](walkthroughs.md) článek obsahuje seznam scénářů s odkazy a popisy miniatur. Názorné postupy ukazují, jak kombinovat cloud, místní nástroje a služby do pracovního postupu nebo kanálu vytvoření inteligentního aplikace. 

Příklady, jak provést kroky v TDSPs, které používají Azure Machine Learning Studio, najdete v části [pomocí Azure Machine Learning TDSP](http://aka.ms/datascienceprocess).
