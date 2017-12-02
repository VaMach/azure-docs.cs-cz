---
title: "Nasazení modely v produkčním prostředí - Azure Machine Learning | Microsoft Docs"
description: "Postup nasazení modelů do produkčního prostředí tím jim umožníte hraje aktivní roli při vytvoření obchodních rozhodnutí."
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
ms.date: 11/30/2017
ms.author: bradsev;
ms.openlocfilehash: c7be9eda2d6f37951eb120250861cf4a39b0141b
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/01/2017
---
# <a name="deploy-models-in-production"></a>Nasazení modely v produkčním prostředí

Produkční nasazení umožňuje modelu pro hraje aktivní roli v podniku. Predikce z nasazené modelu lze použít pro obchodních rozhodnutí.

## <a name="production-platforms"></a>Produkční platformy
Existují různé přístupy a platformy pro modely uvedení do produkčního prostředí. Tady je několik možností:


- [Model nasazení v nástroji Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/preview/model-management-overview)
- [Nasazení modelu v systému SQL server](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sqldev-py6-operationalize-the-model)
- [Server Microsoft Machine Learning](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone)


>[!NOTE]
>Před nasazením jeden má zajistit, že je latence modelu vyhodnocování nízké používat v produkčním prostředí.
>


>[!NOTE]
>Nasazení pomocí Azure Machine Learning Studio, najdete v tématu [nasazení webové služby Azure Machine Learning](../studio/publish-a-machine-learning-web-service.md).
>

## <a name="ab-testing"></a>A / B testování
Když více modelů v produkčním prostředí, může být užitečné k provedení [A / B testování](https://en.wikipedia.org/wiki/A/B_testing) k porovnání výkonu modelů. 

 
## <a name="next-steps"></a>Další kroky

Návody, které ukazují všechny kroky v procesu pro **konkrétních scénářů** jsou také uvedeny. Jsou uvedena v seznamu a propojené s miniatur popisy v [příklad návody](walkthroughs.md) článku. Se ukazují, jak kombinovat cloud, místní nástroje a služby do pracovního postupu nebo kanálu vytvoření inteligentního aplikace. 
 


