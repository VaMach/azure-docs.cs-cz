---
title: "Nasazení fáze životního cyklu procesu Team datové vědy - Azure | Microsoft Docs"
description: "Cíle, úlohy a výsledek pro fázi nasazení vašich projektů vědecké účely data."
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
ms.date: 09/02/2017
ms.author: bradsev;
ms.openlocfilehash: b49b3ab696c22928c5f5a4566e059345fd810588
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="deployment"></a>Nasazení

Toto téma popisuje cíle, úlohy, a výsledek přidružený **nasazení** procesu Team dat vědecké účely. Tento proces zajišťuje doporučené životního cyklu, který můžete použít pro struktury projekty vědecké účely data. Životní cyklus popisuje hlavní fází, které projekty obvykle provést, často interaktivně:

* **Pochopení obchodních**
* **Získávání dat a principy**
* **Modelování**
* **Nasazení**
* **Přijetí zákazníka**

Tady je vizuální reprezentace **procesu vědecké účely Team datového cyklu**. 

![TDSP Lifecycle2](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goal"></a>Cílem
* Modely datovém kanálu se nasadí do produkce nebo prostředí produkčního prostředí pro přijetí konečného uživatele. 

## <a name="how-to-do-it"></a>Jak to udělat
Hlavní úloha řešeny v této fázi:

* **Zprovoznit model**: nasadit model a kanál pro produkční nebo prostředí produkčního prostředí pro používání aplikace.

### <a name="41-operationalize-a-model"></a>4.1 zprovoznit model
Jakmile máte sadu modely, které provádějí dobře, může být operationalized u ostatních aplikací používat. V závislosti na obchodní požadavky jsou vytvářeny předpovědi buď v reálném čase, nebo na základě dávky. Modely jsou nasazeny vystavení je s otevřené rozhraní API. Rozhraní umožňuje modelu, který má být snadno použít z různých aplikací, jako je například online webů, tabulek, řídicí panely a obchodní a back-end aplikace. Příklady operationalization modelu pomocí webové služby Azure Machine Learning najdete v tématu [nasazení webové služby Azure Machine Learning](../studio/publish-a-machine-learning-web-service.md). Je také vhodné sestavení telemetrie a sledování do provozního modelu a datový kanál, který se nasadí. Tento postup pomáhá stavem následné systému hlášení a řešení potíží.  

## <a name="artifacts"></a>Artefakty
* Řídicí panel stavu systému stavu a klíč metrik.
* Poslední modelování sestavu s podrobnostmi o nasazení.
* Architektura dokumentu konečné řešení.


## <a name="next-steps"></a>Další kroky

Tady jsou odkazy na každý krok v průběhu životního cyklu procesu Team dat vědecké účely:

* [1. Pochopení obchodních](lifecycle-business-understanding.md)
* [2. Získávání dat a principy](lifecycle-data.md)
* [3. Modelování](lifecycle-modeling.md)
* [4. Nasazení](lifecycle-deployment.md)
* [5. Přijetí zákazníka](lifecycle-acceptance.md)

Úplné návody začátku do konce, které ukazují všechny kroky v procesu pro **konkrétních scénářů** jsou také uvedeny. Jsou uvedena v seznamu a propojené s miniatur popisy v [příklad návody](walkthroughs.md) tématu. Se ukazují, jak kombinovat cloud, místní nástroje a služby do pracovního postupu nebo kanálu vytvoření inteligentního aplikace. 

Příklady provedením kroků v procesu Team dat. vědecké účely, které používají Azure Machine Learning Studio najdete v tématu [s Azure ML](http://aka.ms/datascienceprocess) kurzů.