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
ms.date: 12/16/2016
ms.author: garye
translationtype: Human Translation
ms.sourcegitcommit: 07cb3fe0d5688d5b63fe3312cad14c2274a58a09
ms.openlocfilehash: e98a64910f28da0a8a9b4a58c717c40d791ccf00


---
# <a name="walkthrough-develop-a-predictive-analytics-solution-for-credit-risk-assessment-in-azure-machine-learning"></a>Názorný průvodce: Vývoj řešení prediktivní analýzy pro posuzování úvěrového rizika v Azure Machine Learning

V tomto názorném průvodci se zaměříme na proces vývoje řešení v nástroji Machine Learning Studio. Vytvoříme si model prediktivní analýzy v nástroji Machine Learning Studio a pak ho nasadíme jako webovou službu Azure Machine Learning, ve které může model vytvářet predikce na základě nových dat. 

> [!TIP]
> Tento názorný průvodce předpokládá, že jste nejméně jednou předtím použili Machine Learning Studio a že rozumíte konceptům strojového učení, i když bere v úvahu, že nejste odborníkem ani na jedno.
> 
>Pokud jste nástroj **Azure Machine Learning Studio** ještě nikdy nepoužívali, možná budete chtít začít kurzem [Vytvoření prvního experimentu datové vědy v nástroji Azure Machine Learning Studio](machine-learning-create-experiment.md). V tomto kurzu se seznámíte s nástrojem Machine Learning Studio, zjistíte, jak pomocí myši přetáhnout moduly do experimentu, vzájemně je propojit, spustit experiment a prohlédnout si výsledky.
>
>Pokud se strojovým učením teprve začínáte, může pro vás být dobrým výchozím bodem seriál videí [Data Science for Beginners (Datová věda pro začátečníky)](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md). Tento seriál videí představuje vynikající úvod do strojového učení s využitím jazyka a konceptů každodenního života.
> 

## <a name="the-problem"></a>Problém

Předpokládejme, že potřebujete předpovědět úvěrové riziko u jednotlivého zákazníka na základě údajů, které uvedou v žádosti o úvěr.  

Posouzení úvěrového rizika je samozřejmě složitý problém, ale trochu si můžeme jeho parametry zjednodušit. Potom tento postup použijeme jako příklad, jak pomocí Microsoft Azure Machine Learning s nástrojem Machine Learning Studio a webovou službou Machine Learning vytvořit řešení prediktivní analýzy.  

## <a name="the-solution"></a>Řešení

V tomto podrobném názorném průvodci začneme s veřejně dostupnými daty o úvěrovém riziku, na jejichž základě vyvineme a natrénujeme prediktivní model. Potom tento model nasadíme jako webovou službu, kterou můžou k posuzování úvěrového rizika využívat další uživatelé.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Při vytváření řešení pro posuzování úvěrového rizika budeme postupovat po těchto krocích:  

1. [Vytvoření pracovního prostoru Machine Learning](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [Nahrání existujících dat](machine-learning-walkthrough-2-upload-data.md)
3. [Vytvoření nového experimentu](machine-learning-walkthrough-3-create-new-experiment.md)
4. [Natrénování a vyhodnocení modelů](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [Nasazení webové služby](machine-learning-walkthrough-5-publish-web-service.md)
6. [Nastavení přístupu k webové službě](machine-learning-walkthrough-6-access-web-service.md)

Tento názorný postup je založen na zjednodušené verzi ukázkového experimentu [Binární klasifikace: Predikce úvěrového rizika](http://go.microsoft.com/fwlink/?LinkID=525270) na webu [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/).


> [!TIP]
> Pokud si chcete stáhnout a vytisknout diagram s přehledem funkcí nástroje Machine Learning Studio, nahlédněte do tématu [Diagram s přehledem možností nástroje Machine Learning Studio](machine-learning-studio-overview-diagram.md).
> 
> 



<!--HONumber=Jan17_HO1-->


