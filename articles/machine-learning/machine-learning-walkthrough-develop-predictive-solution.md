---
title: "Prediktivní řešení pro úvěrové riziko v Machine Learning | Dokumentace Microsoftu"
description: "Podrobný průvodce postupem vytvoření řešení prediktivní analýzy pro posuzování úvěrového rizika v nástroji Azure Machine Learning Studio"
keywords: "úvěrové riziko,řešení prediktivní analýzy,posouzení rizika"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 43300854-a14e-4cd2-9bb1-c55c779e0e93
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/16/2016
ms.author: garye
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: aa64dc7f5bb3e928aac30987b0904435c603829c


---
# <a name="walkthrough-develop-a-predictive-analytics-solution-for-credit-risk-assessment-in-azure-machine-learning"></a>Názorný průvodce: Vývoj řešení prediktivní analýzy pro posuzování úvěrového rizika v Azure Machine Learning
Předpokládejme, že potřebujete předpovědět úvěrové riziko u jednotlivého zákazníka na základě údajů, které uvedou v žádosti o úvěr.  

Posouzení úvěrového rizika je samozřejmě složitý problém, ale trochu si můžeme jeho parametry zjednodušit. Poté můžeme tento postup použít jako příklad, jak lze pomocí Microsoft Azure Machine Learning s nástrojem Machine Learning Studio a webovou službou Machine Learning vytvořit řešení prediktivní analýzy.  

V tomto názorném průvodci budeme sledovat proces vývoje modelu prediktivní analýzy v nástroji Machine Learning Studio a jeho nasazení jako webové služby Azure Machine Learning. Začneme s veřejně dostupnými daty o úvěrovém riziku, na jejichž základě vyvineme a natrénujeme prediktivní model, a poté tento model nasadíme jako webovou službu, kterou mohou pro posuzování úvěrového rizika využívat další uživatelé.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<!-- -->

> [!TIP]
> Pokud si chcete stáhnout a vytisknout diagram s přehledem funkcí nástroje Machine Learning Studio, nahlédněte do tématu [Diagram s přehledem možností nástroje Machine Learning Studio](machine-learning-studio-overview-diagram.md).
> 
> 

Při vytváření řešení pro posuzování úvěrového rizika budeme postupovat po těchto krocích:  

1. [Vytvoření pracovního prostoru Machine Learning](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [Nahrání existujících dat](machine-learning-walkthrough-2-upload-data.md)
3. [Vytvoření nového experimentu](machine-learning-walkthrough-3-create-new-experiment.md)
4. [Natrénování a vyhodnocení modelů](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [Nasazení webové služby](machine-learning-walkthrough-5-publish-web-service.md)
6. [Nastavení přístupu k webové službě](machine-learning-walkthrough-6-access-web-service.md)

Tento názorný postup je založen na zjednodušené verzi ukázkového experimentu [Binární klasifikace: Predikce úvěrového rizika](http://go.microsoft.com/fwlink/?LinkID=525270) na webu [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/).




<!--HONumber=Nov16_HO2-->


