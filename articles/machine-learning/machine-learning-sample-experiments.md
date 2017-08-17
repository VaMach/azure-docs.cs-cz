---
title: "Kopírování příkladů experimentů se strojovým učením – Azure | Dokumentace Microsoftu"
description: "Naučte se používat příklady experimentů se strojovým učením k vytváření nových experimentů s galerií Cortana Intelligence a službou Microsoft Azure Machine Learning."
keywords: machine learning examples, sample experiment, machine learning sample
services: machine-learning
documentationcenter: 
author: cjgronlund
manager: jhubbard
editor: cgronlun
ms.assetid: 81e6c1d8-682c-4db3-bfd5-d7bfb1150ff3
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/28/2017
ms.author: cgronlun
ms.translationtype: HT
ms.sourcegitcommit: 137671152878e6e1ee5ba398dd5267feefc435b7
ms.openlocfilehash: a6c8ca83e3a4020cb1ea0f1c14299f1aee3bdca5
ms.contentlocale: cs-cz
ms.lasthandoff: 07/28/2017

---
# <a name="copy-example-experiments-to-create-new-machine-learning-experiments"></a>Vytváření nových experimentů se strojovým učením na základě kopírování příkladů experimentů
Zjistěte, jak začít s příklady experimentů z [Galerie Cortana Intelligence](https://gallery.cortanaintelligence.com/) místo vytváření experimentů se strojovým učením od začátku. Příklady můžete použít k sestavení vlastních řešení strojového učení.

V galerii jsou příklady experimentů od týmu Microsoft Azure Machine Learning i příklady sdílené komunitou služby Machine Learning. Také můžete klást otázky nebo experimenty komentovat.

Abyste se dozvěděli, jak používat galerii, podívejte se na tříminutové video [Copy other people's work to do data science](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) (Kopírování práce jiných lidí pro vědecké zkoumání dat) z řady [Data Science for Beginners](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) (Vědecké zkoumání dat pro začátečníky).

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="find-an-experiment-to-copy-in-cortana-intelligence-gallery"></a>Nalezení experimentu pro zkopírování v Cortana Intelligence Gallery
Pokud se chcete podívat, jaké experimenty jsou k dispozici, přejděte do [Galerie](https://gallery.cortanaintelligence.com/) a klikněte na **Experimenty** v horní části stránky.

### <a name="find-the-newest-or-most-popular-experiments"></a>Nalezení nejnovějších nebo nejoblíbenějších experimentů
Na této stránce si můžete zobrazit **naposled přidané** experimenty, přechodem dolů se podívat, **co je oblíbené**, nebo si prohlédnout nejnovější **populární experimenty Microsoftu**.

### <a name="look-for-an-experiment-that-meets-specific-requirements"></a>Vyhledání experimentu, který splňuje určité požadavky
Procházení všech experimentů:

1. V horní části stránky klikněte na **Browse all** (Procházet vše).
2. Na levé straně v oddílu **Zpřesnit podle** v části **Kategorie** vyberte **Experiment**. Zobrazí se všechny experimenty v galerii.
3. Experimenty, které splňují vaše požadavky, můžete najít několika různými způsoby:
   * **Vyberte filtry vlevo.** Pokud chcete například procházet experimenty, které používají algoritmus detekce anomálií založený na PCA, s vybranou možností **Experiment** v části **Kategorie** klikněte na **Zobrazit vše**. Potom v části **Použité algoritmy** zvolte **Detekce anomálií založená na PCA**. <br></br>
     ![Vybrat filtry](./media/machine-learning-sample-experiments/refine-the-view.png)
   * **Použijte vyhledávací pole.** Pokud chcete například najít experimenty, kterými přispěl Microsoft, které se týkají rozpoznávaní číslic a které používají algoritmus podpůrného vektorového stroje se dvěma třídami, zadejte do vyhledávacího pole „digit recognition“. Vyberte filtry **Experiment**, **Microsoft content only** (Jen obsah Microsoftu) a **Two-Class Support Vector Machine**:<br></br>
     ![Použití vyhledávacího pole](./media/machine-learning-sample-experiments/search-for-experiments.png)
4. Kliknutím na experiment o něm zobrazíte více informací.
5. Pokud chcete experiment spustit nebo upravit, klikněte na stránce experimentu na **Open in Studio** (Otevřít v nástroji Studio). <br></br>

    ![Příklad experimentu](./media/machine-learning-sample-experiments/example-experiment.png)

    > [!NOTE]
    > Když poprvé otevřete experiment v Machine Learning Studiu, můžete ho vyzkoušet zdarma, nebo si koupit předplatné Azure. [Další informace o bezplatné zkušební verzi Machine Learning Studia v porovnání s placenou službou](https://azure.microsoft.com/pricing/details/machine-learning/)
    >
    >

## <a name="create-a-new-experiment-using-an-example-as-a-template"></a>Vytvoření nového experimentu s využitím příkladu jako šablony
Nový experiment v nástroji Machine Learning Studio můžete vytvořit také pomocí příkladu z galerie jako šablony.

1. Přihlaste se do [Studia](https://studio.azureml.net) pomocí přihlašovacích údajů účtu Microsoft a kliknutím na **Nový** vytvořte experiment.
2. Projděte si obsah příkladu a na některý klikněte.

V pracovním prostoru Machine Learning Studio se vytvoří nový experiment s využitím příkladu experimentu jako šablony.

## <a name="next-steps"></a>Další kroky
* [Import dat z různých zdrojů](machine-learning-data-science-import-data.md)
* [Stručný úvodní kurz k jazyku R ve službě Machine Learning](machine-learning-r-quickstart.md)
* [Nasazení webové služby Machine Learning](machine-learning-publish-a-machine-learning-web-service.md)

