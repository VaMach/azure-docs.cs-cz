<properties
    pageTitle="Co je Machine Learning v Azure? | Microsoft Azure"
    description="Vysvětluje základní koncepty plně spravované služby Machine Learning, což je cloudová technologie, pomocí které můžete vytvářet, zprovozňovat a monetizovat řešení."
    keywords="what is machine learning,cloud technology,predictive,what is predictive analytics,operationalize"
    services="machine-learning"
    documentationCenter=""
    authors="cjgronlund"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="04/28/2016"
    ms.author="cgronlun;tedway;olgali"/>


# Úvod do strojového učení v Microsoft Azure

## Co je strojové učení?

Při strojovém učení se v počítačích spouští prediktivní modely, které se učí z existujících dat s cílem předpovídat budoucí chování, výsledky a trendy.

Díky těmto předpovědím neboli predikcím ze strojového učení mohou být aplikace a zařízení efektivnější. Při online nakupování je díky strojovému učení možné na základě již pořízeného zboží doporučit produkty, které by se vám mohly líbit. Po protažení platební karty čtečkou porovná proces strojového učení danou transakci s databází, čímž bance pomáhá odhalovat podvody.

## Co je Machine Learning v Microsoft Azure?

Azure Machine Learning je výkonná cloudová služba pro prediktivní analýzy, která umožňuje rychle vytvářet a nasazovat prediktivní modely jako analytická řešení.

Nejenže Azure Machine Learning poskytuje nástroje pro modelování prediktivních analýz, ale přináší také plně spravovanou službu, pomocí které můžete své prediktivní modely nasazovat jako k použití připravené webové služby. Azure Machine Learning nabízí nástroje pro vytváření kompletních řešení prediktivní analýza v cloudu: rychlé vytváření, testování, zprovoznění a správa prediktivní modelů. Není nutné kupovat žádný hardware ani ručně spravovat virtuální počítače.

![Co je strojové učení? Základní pracovní postup pro zprovoznění prediktivních analýz v Azure Machine Learning](./media/machine-learning-what-is-machine-learning/machine-learning-service-parts-and-workflow.png)

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## Co je prediktivní analýza?

Prediktivní analýzy využívají různé statistické postupy – v tomto případě strojové učení – k analýze shromážděných nebo aktuálních dat z hlediska vzorů či schémat nebo trendů s cílem předvídat budoucí události.

Azure Machine Learning je zvláště efektivní nástroj pro prediktivní analýzy: můžete pracovat s knihovnou předpřipravených algoritmů, vytvářet modely na počítači připojeném k internetu bez nutnosti pořizovat další zařízení nebo infrastrukturu a rychle nasadit prediktivní řešení. Můžete také vyhledat předdefinované příklady a řešení na webech [Microsoft Azure Marketplace](https://datamarket.azure.com/browse?query=machine+learning) nebo [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/).

## Sestavování kompletních řešení pro strojové učení v cloudu

Azure Machine Learning obsahuje všechno, co potřebujete k vytváření řešení prediktivní analýzy v cloudu: od velké knihovny algoritmů přes studio pro vytváření modelů po snadný způsob, jak model nasadit jako webovou službu.

### Machine Learning Studio: Vytváření prediktivních modelů

Prediktivní modely můžete vytvářet v nástroji [Machine Learning Studio](machine-learning-what-is-ml-studio.md), který funguje na bázi prohlížeče a využívá operace přetahování myší a propojování modulů.

![Co je to prediktivní analýza: Příklad experimentu s prediktivní analýzou v nástroji Azure Machine Learning Studio](./media/machine-learning-what-is-machine-learning/azure-machine-learning-studio-predictive-score-experiment.png)

* Díky rozsáhlé knihovně [algoritmů a modulů pro strojové učení](https://msdn.microsoft.com/library/azure/f5c746fd-dcea-4929-ba50-2a79c4c067d7) v nástroji Machine Learning Studio se můžete hned pustit do sestavování prediktivních modelů. Můžete vybírat z knihovny ukázkových experimentů, balíčků R a Python a nejlepší algoritmů ve své třídě z divizí Microsoftu, jako jsou Xbox a Bing. Moduly ze studia můžete rozšířit vlastními skripty [R](machine-learning-r-quickstart.md) a [Python](machine-learning-execute-python-scripts.md).
* Na webu [Cortana Intelligence Galerie](machine-learning-gallery-how-to-use-contribute-publish.md) si můžete vyzkoušet analytická řešení vytvořená jinými vývojáři nebo poskytnout vlastní, která využívají služby Azure Machine Learning, HDInsight (Hadoop), Stream Analytics a Data Lake Analytics, jakož i úložiště Azure pro velké objemy dat (big data) a služby pro správu dat.  V komunitě můžete klást otázky nebo publikovat komentáře k experimentům, případně sdílet odkazy na experimenty prostřednictvím sociálních sítí, jako je LinkedIn a Twitter.  

    ![Zkoušení prediktivních experimentů nebo poskytování vlastních na webu Azure Cortana Intelligence Gallery](./media/machine-learning-what-is-machine-learning/machine-learning-cortana-intelligence-gallery.png)

### Zprovozňování řešení prediktivní analýzy: nákup webových služeb nebo publikování vlastních

* Z webu [Microsoft Azure Marketplace](https://datamarket.azure.com/browse?query=machine+learning) můžete koupit předpřipravené webové služby, jako jsou například Doporučení, Analýza textu a Zjištění anomálií.

* Máte možnost zprovoznit vlastní řešení prediktivní analýzy:
    * [Nasazování webových služeb](machine-learning-publish-a-machine-learning-web-service.md)
    * [Trénování a přeučování modelů prostřednictvím API](machine-learning-retrain-models-programmatically.md)
    * [Správa koncových bodů webové služby](machine-learning-create-endpoint.md)
    * [Škálování webových služeb](machine-learning-scaling-endpoints.md)
    * [Využívání webových služeb](machine-learning-consume-web-services.md)

## Klíčová terminologie a koncepce strojového učení
### Zkoumání dat, deskriptivní analýza a prediktivní analýza

**Zkoumání dat** je proces shromažďování informací o velké a často nestrukturované datové sady s cílem najít charakteristiky pro přesně zacílené analýzy. **Dolování dat** označuje automatizované zkoumání dat.

**Deskriptivní analýza** je proces analyzování datové sady s cílem shrnout, co se stalo. Velká většina obchodních analýz – například prodejní sestavy, webové metriky a analýzy sociálních sítí – je deskriptivních.

**Prediktivní analýza** je proces vytváření modelů na základě historických nebo aktuální dat s cílem předpovídat budoucí výsledky.


### Učení se supervizí a bez supervize
 V algoritmech **učení se supervizí** probíhá trénink na označených datech – jinými slovy na datech zahrnujících příklady žádoucích odpovědí. Například v modelu, který identifikuje podvodné použití platební karty, by trénink probíhal na datové sadě s datovými body indikujícími transakce, o kterých se ví, že byly, resp. nebyly podvodné. Většina strojového učení probíhá se supervizí.

 **Učení bez supervize** probíhá na neoznačených datech a cílem je nalezení vztahů a souvislostí v nich. Cílem může být například nalezení demograficky definovaných skupin zákazníků s podobnými nákupními návyky.

### Trénování a hodnocení modelů
Model strojového učení je abstrakcí otázky, na kterou hledáte odpověď, nebo výsledku, který chcete předpovědět. Modely se trénují a vyhodnocují na základě existujících dat.

#### Trénování na datech
V Azure Machine Learning se model sestavuje z algoritmického modulu, který zpracovává trénovací data a funkční moduly, například modul pro stanovení skóre.

Pokud v rámci učení se supervizí trénujete model pro odhalování podvodů, použijete sadu transakcí, které jsou označeny buď jako podvodné, nebo jako platné. Datovou sadu náhodně rozdělíte a pak jednu část použijete k natrénování modelu a další část k otestování nebo vyhodnocení modelu.

#### Data pro vyhodnocení
Jakmile model natrénujete, vyhodnotíte ho na zbývajících testovacích datech. Využijete k tomu data, u kterých již znáte výsledky, aby bylo možné určit, zda je váš model schopen přesné predikce.

### Další běžné pojmy používané při strojovém učení

* **algoritmus**: Samostatná sada pravidel používaná k řešení problémů prostřednictvím zpracování dat, výpočtu nebo automatizované posuzování.
* **kategorizovaná data**: Data, která jsou uspořádána do kategorií a která lze rozdělit do skupin. Kategorizovaná datová sada pro automobily by například mohla specifikovat rok výroby, značku, model a cenu.
* **klasifikace**: Model pro uspořádání datových bodů do kategorií na základě datové sady, u které je již seskupení do kategorií známo.
* **konstruování příznaků**: Proces extrahování nebo výběru příznaků či atributů souvisejících s datovou sadou s cílem zvýšit její využitelnost a vylepšit výsledky. Například využitelnost dat o leteckých tarifech lze vylepšit určením dnů v týdnu a svátků. Viz [Výběr a konstruování příznaků v Azure Machine Learning](machine-learning-feature-selection-and-engineering.md).
* **modul**: Funkční element v modelu nástroje Machine Learning Studio, například modul pro zadávání dat, který umožňuje zadávání a úpravy malých datových sad. Určitým typem modulu v nástroji Machine Learning Studio je i algoritmus.
* **model**: Při učení se supervizí model představuje produkt experimentu strojového učení a skládá se z trénovací datové sady, algoritmického modulu a funkčních modulů, jako je modul Určení skóre modelu.
* **číselná data**: Data, která mají význam jako měření (spojitá data) nebo počty (diskrétní neboli nespojitá data). Označují se také jako *kvantitativní data*.
* **rozdělení**: Metoda, podle které data rozdělíte do vzorků. Další informace naleznete v tématu [Rozdělení a vzorky](https://msdn.microsoft.com/library/azure/dn905960.aspx).
* **predikce**: Hodnota nebo hodnoty předpovězené z modelu strojového učení. Můžete se také setkat s termínem „predikované skóre“; to však nepředstavuje finální výstup z modelu. Podle skóre se řídí vyhodnocení modelu.
* **regrese**: Model pro predikci průběžné hodnoty na základě nezávislých proměnných, například předpověď ceny automobilu na základě roku jeho výroby a značky.
* **skóre**: Předpovězená hodnota generovaná z natrénovaného klasifikačního nebo regresního modelu na základě [modulu pro stanovení skóre](https://msdn.microsoft.com/library/azure/dn905995.aspx) v nástroji Machine Learning Studio. Klasifikační modely rovněž vracejí skóre pro pravděpodobnost předpovězené hodnoty. Po vygenerování skóre z modelu můžete vyhodnotit jeho přesnost pomocí [modulu pro vyhodnocení modelu](https://msdn.microsoft.com/library/azure/dn905915.aspx).
* **vzorek**: Část datové sady, která by měla představovat celek. Vzorky lze vybírat náhodně nebo podle konkrétních příznaků či atributů datové sady.



## Další kroky
Se základy prediktivní analýzy a strojového učení se můžete seznámit v [podrobném kurzu](machine-learning-create-experiment.md) a [na základě ukázek](machine-learning-sample-experiments.md).  


<!-- Module References -->
[learning-with-counts]: https://msdn.microsoft.com/library/azure/81c457af-f5c0-4b2d-922c-fdef2274413c/



<!--HONumber=Jun16_HO2-->


