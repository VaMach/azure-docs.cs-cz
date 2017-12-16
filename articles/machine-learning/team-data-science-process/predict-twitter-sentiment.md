---
title: "Předpověď sentimentu Twitter s word vkládaných pomocí procesu vědecké účely Team Data - Azure | Microsoft Docs"
description: "Kroky potřebné k provedení projekty vědecké zpracování dat"
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
ms.date: 12/15/2017
ms.author: bradsev;
ms.openlocfilehash: fe1c87df40102a62e1e0c8873b25fa3df7d743bc
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/16/2017
---
# <a name="predict-twitter-sentiment-with-word-embeddings-using-the-team-data-science-process"></a>Předpověď sentimentu Twitter s word vkládaných pomocí procesu Team Data vědecké účely

Tento článek ukazuje, jak efektivně spolupracovat při použití **Word2Vec** word vložení algoritmus a **postojích určité slovo vložení (SSWE) algoritmus** k předvídání sentimentu Twitter s [Azure Machine Learning](../preview/index.yml). Další informace o úlohy predikci twitter postojích polarita, najdete v části [úložiště](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction). Klíč k usnadnění efektivní Týmová spolupráce v projektech vědecké účely dat je standardizovat strukturu a projektů s životního cyklu vědecké účely zavedených dat naleznete v dokumentaci. [Tým datové vědy procesu (TDSP)](overview.md) poskytuje právě takové strukturovaných [životního cyklu](lifecycle.md). 

Vytváření projektů vědecké účely dat s **TDSP šablony** poskytuje toto standardizovaná rozhraní pro projekty Azure Machine Learning. Dřív, měl vydané týmem TDSP [úložiště GitHub pro strukturu TDSP projektů a šablon](https://github.com/Azure/Azure-TDSP-ProjectTemplate). Nyní vytváření projektů Azure Machine Learning, které jsou vytvářeny instance s [TDSP strukturu a dokumentaci šablon pro Azure Machine Learning](https://github.com/amlsamples/tdsp) bylo povoleno. Pokyny týkající se používání TDSP strukturu a šablon v Azure Machine Learning najdete v tématu [struktury projektů pomocí šablony procesu vědecké účely dat Team](../preview/how-to-use-tdsp-in-azure-ml.md). 


## <a name="the-sentiment-polarity-sample"></a>Ukázka polarita postojích

Ukázka, která ukazuje, jak vytvořit instanci a provedení machine learning projektu pomocí strukturu proces vědecké účely Team dat a šablon v nástroji Azure Machine Learning pracovní Bench bylo zadáno v tomto [návod](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/blob/master/docs/deliverable_docs/Step_By_Step_Tutorial.md). Tato úloha modelování je k předvídání postojích polarita (kladné a záporné) pomocí textu z tweetů. Tento článek popisuje data modelování úlohy popsané v tomto návodu. Průvodce obsahuje následující úlohy:

- Zkoumání dat, školení a nasazení modelu strojového učení, které adres předpovědi problém popsaný v části Přehled případ použití. Pro tento účel [Twitter postojích data](http://cs.stanford.edu/people/alecmgo/trainingandtestdata.zip) se používá.
- Spuštění projektu v Azure Machine Learning pomocí šablony tým datové vědy procesu (TDSP) z Azure Machine Learning pro tento projekt. Pro spuštění projektu a vytváření sestav se používá TDSP životního cyklu.
- Operationalization řešení přímo z Azure Machine Learning v kontejneru služby Azure.

Projekt označuje některé funkce služby Azure Machine Learning, takové TDSP struktura vytváření instancí a použití, spouštění kódu v Azure Machine Learning pracovní Bench a snadno operationalization v kontejneru služby Azure pomocí Docker a Kubernetes.

## <a name="team-data-science-process"></a>Vědecké zpracování týmových dat
Použití šablon projektu TDSP strukturu a dokumentaci k provedení této ukázce. Postupuje [životního cyklu TDSP]((https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md)). Vytvoření projektu podle pokynů k [zde](https://github.com/amlsamples/tdsp/blob/master/docs/how-to-use-tdsp-in-azure-ml.md).

![Životní cyklus TDSP](./media/predict-twitter-sentiment/tdsp-lifecycle.PNG)

![Vytvoření instance TDSP](./media/predict-twitter-sentiment/tdsp-instantiation.PNG) 


## <a name="data-acquisition-and-understandinghttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode01dataacquisitionandunderstanding"></a>[Získávání dat a principy](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/01_data_acquisition_and_understanding)
Prvním krokem v této ukázce je ke stažení sentiment140 datovou sadu a rozdělit ho na trénování a testování datové sady. Datová sada sentiment140 obsahuje skutečný obsah tweet (s emotikony odebrat) společně s polarita jednotlivých tweet (záporné = 0, kladné = 4), s neutrálním tweetů odebrat. Když rozdělené, výsledná Cvičná data má 1.3 milionu řádků a testovacích dat 320 kB řádků.


## <a name="modelinghttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode02modeling"></a>[Modelování](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/02_modeling)

Tato část vzorku se dále dělí do tří částí:
 
- **Konstruování** odpovídá generování funkcí s použitím různých word vložení algoritmy. 
- **Model vytváření** obchody se školení odlišnými modely jako _logistic regression_ a _přechodu zvyšovat skóre_ k předvídání postojích ze vstupního textu. 
- **Model vyhodnocení** používá pro cvičný model přes testování data.


### <a name="feature-engineeringhttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode02modeling01featureengineering"></a>[Funkce inženýrství](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/02_modeling/01_FeatureEngineering)

Word2Vec a SSWE jsou slovo vložení algoritmy zde sloužící ke generování vkládaných aplikace word. 


#### <a name="word2vec"></a>Word2Vec

Word2Vec algoritmus se používá v režimu Skipgram. Tímto způsobem generování slovo vložené části je vysvětlen v papíru podle Tomáši Mikolov et al: [distribuované reprezentace slova a frází a jejich Compositionality. Pokroky v neuronové informace systémy pro zpracování. 2013.](https://arxiv.org/abs/1310.4546).

Přeskočit gram je bez podstruktury neuronové sítě. Vstupní je slovo cíl kódovaná jako jeden aktivní vektor, který se používá k předvídání nedaleko slova. Pokud **V** je velikost termínů, pak velikost vrstvy výstupu by byla __C * V__ kde C je velikost okna kontextu. Architektura přeskočit gram na základě je vidět na následujícím obrázku:

![Přeskočit gram modelu](./media/predict-twitter-sentiment/skip-gram-model.PNG)

***Přeskočit gram modelu***

Podrobné mechanismů word2vec algoritmus a přeskočit gram modelu jsou nad rámec této ukázky. Čtečky mají zájem o další podrobnosti o jeho strukturu a funkci můžete naleznete v následujících odkazech:

- [02_A_Word2Vec.py kódu odkazovat TensorFlow příklady](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/examples/tutorials/word2vec/word2vec_basic.py)
- [Vektor reprezentace slova](https://www.tensorflow.org/tutorials/word2vec)
- [Jak přesně word2vec funguje?](http://www.1-4-5.net/~dmm/ml/how_does_word2vec_work.pdf)
- [Poznámky k Contrastive odhad šumu a záporná vzorkování](http://demo.clab.cs.cmu.edu/cdyer/nce_notes.pdf)


#### <a name="sswe"></a>SSWE
**Postojích určité slovo vložení (SSWE) algoritmus** pokusí překonat slabiny algoritmu Word2vec, slova s podobnou kontexty nebo opačnou polarita můžou mít podobné vektory aplikace word. Tato podobnosti znamená, že Word2vec nemusí provádět přesně pro úlohy, jako je postojích analýzy. Algoritmus SSWE se pokusí zpracovat tato slabé místo začleněním polarita větu a jeho kontextu do jeho funkci ztrátu.

Tato ukázka používá jeho variantě SSWE. SSWE používá původní ngram a poškozená ngram jako vstup a hodnocení styl se vytočit ztrátě funkce pro syntaktické ztrátě i sémantického ztráty. Funkce Ultimate ztrátě je vyvážené kombinace syntaktické ztrátě i sémantického ztráty. Pro účely jednoduchost jenom sémantického křížové šifrování se používá jako funkci ztrátu. Jak uvidíte později, i pomocí této funkce jednodušší ztrátě výkonu SSWE vnoření je lepší, než Word2Vec vložení.

Model SSWE vycházející neuronové sítě, který používáte v této ukázce je znázorněno na následujícím obrázku:

![Porovnání modelů ROC](./media/predict-twitter-sentiment/embedding-model-2.PNG)

***Convolutional model ke generování vložení postojích konkrétní aplikace word.***


Po dokončení procesu školení dva vnoření soubory ve formátu TSV jsou generovány pro modelování fázi.

Další informace o algoritmech SSWE, najdete v dokumentu pomocí Duyu hmotný dlouhodobý et al: [Learning vložení Word postojích specifické pro klasifikaci postojích služby Twitter. SEZNAM ACL (1). 2014.](http://www.aclweb.org/anthology/P14-1146) 


### <a name="model-creationhttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode02modeling02modelcreation"></a>[Vytvoření modelu](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/02_modeling/02_ModelCreation)
Jakmile vektory word byly generovány pomocí algoritmů SSWE nebo Word2vec, dalším krokem je ke cvičení klasifikační modely k předvídání polarita skutečné postojích. Dva typy funkcí, Word2Vec a SSWE, se používají pro dva modely, GBM modelu a Logistic regresní model. Proto čtyři různé modely jsou probíhajícího cvičení.


### <a name="model-evaluationhttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode02modeling03modelevaluation"></a>[Vyhodnocení modelu](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/02_modeling/03_ModelEvaluation)
Čtyři modely cvičení v předchozím kroku testování dat se teď použít k vyhodnocení výkonu modelu. 

1. Přechodu Boosting přes SSWE vložení
2. Logistic Regression přes SSWE vložení
3. Přechodu Boosting přes Word2Vec vložení
4. Logistic Regression přes Word2Vec vložení

![Porovnání modelů ROC](./media/predict-twitter-sentiment/roc-model-comparison.PNG)

Model GBM s funkcí SSWE je nejlepší pomocí AUC metriku.


## <a name="deploymenthttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode03deployment"></a>[Nasazení](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/03_deployment)

Tato část nasadí modelu předpovědi vyškolení postojích (vložení SSWE + GBM modelu) k webové službě na cluster v kontejneru služby Azure (ACS). Prostředí operationalization zřídí Docker a Kubernetes v clusteru pro správu nasazení webové služby. Můžete najít další informace o procesu operationalization [zde](https://docs.microsoft.com/en-us/azure/machine-learning/preview/model-management-service-deploy).

![kubenetes_dashboard](./media/predict-twitter-sentiment/kubernetes-dashboard.PNG)


## <a name="conclusion"></a>Závěr

Podrobnosti o tom, jak trénování modelu vložení word pomocí algoritmy Word2Vec a SSWE byly vysvětlené a extrahované vkládaných byly použity jako funkce ke cvičení několik modely k předvídání postojích skóre pro Twitter textová data. Funkci postojích konkrétních slov Embeddings(SSWE) s modelem přechodu Boosted Tree přiřadil nejlepší výkon. Tento model se pak nasadí jako služba webu v reálném čase v kontejneru služby Azure pomocí Azure Machine Learning pracovní Bench.


## <a name="references"></a>Odkazy

* [Proces Team dat vědecké účely](https://docs.microsoft.com/en-us/azure/machine-learning/team-data-science-process/overview) 
* [Jak používat tým datové vědy procesu (TDSP) v Azure Machine Learning](https://aka.ms/how-to-use-tdsp-in-aml)
* [Šablona projektu TDSP pro Azure Machine Learning](https://aka.ms/tdspamlgithubrepo)
* [Azure ML pracovní Bench](https://docs.microsoft.com/en-us/azure/machine-learning/preview/)
* [USA příjem datové sady z úložiště UCI ML](https://archive.ics.uci.edu/ml/datasets/adult)
* [Pomocí šablony TDSP biolékařského Entity rozpoznávání](https://docs.microsoft.com/en-us/azure/machine-learning/preview/scenario-tdsp-biomedical-recognition)
* [Mikolov Tomáši, a další. Distribuované reprezentace slova a slovní spojení a jejich compositionality. Pokroky v neuronové informace systémy pro zpracování. 2013.](https://arxiv.org/abs/1310.4546)
* [Hmotný dlouhodobý, Duyu, a další. "Učení postojích konkrétní Word vložení pro klasifikaci postojích Twitter." SEZNAM ACL (1). 2014.](http://www.aclweb.org/anthology/P14-1146)