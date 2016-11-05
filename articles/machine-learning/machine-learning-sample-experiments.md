---
title: Kopírování experimentů ukázky Machine Learningu | Microsoft Docs
description: Naučte se používat experimenty ukázky Machine Learningu k vytváření nových experimentů s Cortana Intelligence Gallery a Microsoft Azure Machine Learning.
services: machine-learning
documentationcenter: ''
author: cjgronlund
manager: jhubbard
editor: cgronlun

ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/17/2016
ms.author: cgronlun;chhavib;olgali

---
# Vytváření nových experimentů Machine Learningu na základě kopírování ukázkových experimentů
Naučte se pracovat s experimenty pomocí ukázek z [Cortana Intelligence Galerie](http://gallery.cortanaintelligence.com/), místo abyste vytvářeli experimenty Machine Learningu od začátku. Ukázky můžete použít k sestavení vlastních řešení Machine Learningu.

V galerii jsou ukázkové experimenty vytvořené týmem Microsoft Azure Machine Learning, ale i ukázky sdílené komunitou Machine Learningu. Také můžete klást otázky nebo experimenty komentovat.

Abyste se dozvěděli, jak používat galerii, podívejte se na tříminutové video [Copy other people's work to do data science](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) (Kopírování práce jiných lidí pro vědecké zkoumání dat) z řady [Data Science for Beginners](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) (Vědecké zkoumání dat pro začátečníky).

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## Nalezení experimentu pro zkopírování v Cortana Intelligence Gallery
Pokud se chcete podívat, jaké experimenty jsou k dispozici, přejděte do [Galerie](http://gallery.cortanaintelligence.com/) a klikněte na **Experimenty** v horní části stránky.

### Nalezení nejnovějších nebo nejoblíbenějších experimentů
Na této stránce si můžete zobrazit **naposled přidané** experimenty, přechodem dolů se podívat, **co je oblíbené**, nebo si prohlédnout nejnovější **populární experimenty Microsoftu**.

### Vyhledání experimentu, který splňuje určité požadavky
Procházení všech experimentů:

1. V horní části stránky klikněte na **Browse all** (Procházet vše).
2. V části **Zpřesnit podle** vyberte **Experiment**. Zobrazí se všechny experimenty v galerii.
3. Experimenty, které splňují vaše požadavky, můžete najít několika různými způsoby:
   * **Vyberte filtry na levé straně.** Pokud chcete například procházet experimenty, které používají algoritmus detekce anomálií založený na metodě PCA, vyberte v části **Categories** (Kategorie) možnost **Experiment** a v části **Algorithms Used** (Použité algoritmy) možnost **PCA-Based Anomaly Detection**. (Pokud tento algoritmus nevidíte, klikněte ve spodní části seznamu na **Show all** (Zobrazit vše)).<br></br>
     ![](./media/machine-learning-sample-experiments/refine-the-view.png)
   * **Použijte pole hledání.** Pokud chcete například najít experimenty, kterými přispěl Microsoft, které se týkají rozpoznávaní číslic a které používají algoritmus podpůrného vektorového stroje se dvěma třídami, zadejte do vyhledávacího pole „digit recognition“. Vyberte filtry **Experiment**, **Microsoft content only** (Jen obsah Microsoftu) a **Two-Class Support Vector Machine**:
     ![](./media/machine-learning-sample-experiments/search-for-experiments.png) 
4. Kliknutím na experiment o něm zobrazíte více informací.
5. Pokud chcete experiment spustit nebo upravit, klikněte na stránce experimentu na **Open in Studio** (Otevřít v nástroji Studio).
   
   > [!NOTE]
   > Abyste mohli experiment otevřít v nástroji Machine Learning Studio, budete se muset přihlásit pomocí přihlašovacích údajů k účtu Microsoft. Pokud ještě nemáte pracovní prostor Machine Learning, vytvoří se bezplatný zkušební pracovní prostor. [Podívejte se, co je součástí bezplatné zkušební verze Machine Learning.](https://azure.microsoft.com/pricing/details/machine-learning/)
   > 
   > 
   
    ![](./media/machine-learning-sample-experiments/example-experiment.png) 

## Použití šablony v nástroji Machine Learning Studio
Nový experiment v nástroji Machine Learning Studio je možné vytvořit i pomocí ukázky z galerie, která bude použita jako šablona.

1. Přihlaste se do nástroje [Studio](https://studio.azureml.net) pomocí přihlašovacích údajů účtu Microsoft a vytvořte nový experiment kliknutím na **Nový**.
2. Projděte si ukázkový obsah a na některý klikněte.

V pracovním prostoru se vytvoří nový experiment, ve kterém ukázkový experiment slouží jako šablona.

## Další kroky
* [Příprava dat](machine-learning-data-science-import-data.md)
* [Využití R v experimentu](machine-learning-r-quickstart.md)
* [Ukázkové experimenty s R](machine-learning-r-csharp-web-service-examples.md)
* [Vytvoření rozhraní API webové služby](machine-learning-publish-a-machine-learning-web-service.md)
* [Procházení aplikací připravených k použití](https://datamarket.azure.com/browse?query=machine+learning)

<!--HONumber=Sep16_HO3-->


