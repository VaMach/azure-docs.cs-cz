<properties
    pageTitle="Co je Machine Learning v Azure? | Microsoft Azure"
    description="Dočtete se o základních konceptech strojového učení v cloudu, dozvíte se, k čemu ho můžete využít, a seznámíte se s terminologií strojového učení."
    keywords="co je strojové učení, terminologie strojového učení, prediktivní, co je prediktivní analýza, zprovoznění"
    services="machine-learning"
    documentationCenter=""
    authors="cjgronlund"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/17/2016"
    ms.author="cgronlun;tedway;olgali"/>



# Úvod do strojového učení v cloudu

## Co je strojové učení?

Strojové učení zahrnuje vědecké metody práce s daty, díky kterým se počítače z existujících dat učí předpovídat budoucí chování, výsledky a trendy.  

Díky těmto předpovědím neboli predikcím ze strojového učení mohou být aplikace a zařízení efektivnější. Při online nakupování je díky strojovému učení možné na základě již pořízeného zboží doporučit produkty, které by se vám mohly líbit. Po protažení platební karty čtečkou porovnává strojově učený proces danou transakci s databází, a pomáhá tak odhalovat podvody. A takový robotický vysavač na základě strojového učení zjišťuje, jestli už v místnosti dostatečně uklidil.

Stručný přehled získáte třeba zhlédnutím série videí [Datová věda pro začátečníky](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md). Datová věda pro začátečníky vám bez nesrozumitelných výrazů a složité matematiky představí strojové učení a krok za krokem vás provede jednoduchým prediktivním modelem.

## Co je Machine Learning v cloudu Microsoft Azure?

Azure Machine Learning je výkonná cloudová služba pro prediktivní analýzy, která umožňuje rychle vytvářet a nasazovat prediktivní modely jako analytická řešení. Ať už budete se strojovým učením experimentovat nebo pracovat na jeho konkrétním použití právě v cloudu Azure, vyhnete se nákupu drahého hardware a infrastruktury.

![Co je strojové učení? Základní pracovní postup pro zprovoznění prediktivních analýz v Azure Machine Learning](./media/machine-learning-what-is-machine-learning/machine-learning-service-parts-and-workflow.png)

Nejenže Azure Machine Learning poskytuje nástroje pro modelování prediktivních analýz, ale přináší také plně spravovanou službu, pomocí které můžete své prediktivní modely nasazovat jako k použití připravené webové služby. Azure Machine Learning nabízí nástroje pro vytváření kompletních řešení prediktivní analýza v cloudu: rychlé vytváření, testování, zprovoznění a správa prediktivní modelů.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## Co je prediktivní analýza?

Při prediktivní analýze se používají různé matematické vzorce, neboli algoritmy, které analýzou historických či aktuálních dat odkrývají vzorce a trendy a snaží se pomocí nich předpovídat budoucí vývoj.

Azure Machine Learning je obzvlášť efektivní nástroj pro prediktivní analýzy. Nabízí totiž knihovnu připravených algoritmů, s kterými můžete na počítači připojeném k internetu vytvářet modely a rovnou je nasazovat. Pokud chcete rychle dosáhnout výsledků, podívejte se nejprve na příklady a řešení připravená k použití na webu [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/).

## Sestavování kompletních řešení pro strojové učení v cloudu

Azure Machine Learning obsahuje všechno, co potřebujete k vytváření řešení prediktivní analýzy v cloudu: od velké knihovny algoritmů přes studio pro vytváření modelů po snadný způsob, jak model nasadit jako webovou službu.

### Machine Learning Studio: Vytváření prediktivních modelů

Rychlé vytváření prediktivních modelů v nástroji [Machine Learning Studio](machine-learning-what-is-ml-studio.md) usnadňuje přetahování myší a propojování modulů. Moduly se dají nejrůzněji kombinovat, [sami si to zdarma vyzkoušejte](https://studio.azureml.net/?selectAccess=true&o=2).

![Co je to prediktivní analýza: Příklad experimentu s prediktivní analýzou v nástroji Azure Machine Learning Studio](./media/machine-learning-what-is-machine-learning/azure-machine-learning-studio-predictive-score-experiment.png)

* Na webu [Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md) si můžete vyzkoušet analytická řešení připravená ostatními uživateli nebo přispět svým vlastním. V komunitě můžete klást otázky nebo publikovat komentáře k experimentům, případně sdílet odkazy na experimenty prostřednictvím sociálních sítí, jako je LinkedIn a Twitter.
* Díky rozsáhlé knihovně [algoritmů a modulů pro strojové učení](https://msdn.microsoft.com/library/azure/f5c746fd-dcea-4929-ba50-2a79c4c067d7) v nástroji Machine Learning Studio se můžete hned pustit do sestavování prediktivních modelů. Můžete vybírat z ukázkových experimentů, balíčků R a Python a nejlepších algoritmů ve své třídě z divizí Microsoftu, jako jsou Xbox a Bing. Moduly ze Studia můžete rozšířit vlastními skripty [R](machine-learning-r-quickstart.md) a [Python](machine-learning-execute-python-scripts.md).


    ![Zkoušení prediktivních experimentů nebo poskytování vlastních na webu Azure Cortana Intelligence Gallery](./media/machine-learning-what-is-machine-learning/machine-learning-cortana-intelligence-gallery.png)

### Zprovozňování řešení prediktivní analýzy: nákup webových služeb nebo publikování vlastních

* Z webu [Microsoft Azure Marketplace](https://datamarket.azure.com/browse?query=machine+learning) můžete koupit předpřipravené webové služby, jako jsou například Doporučení, Analýza textu a Zjištění anomálií.

* Máte možnost zprovoznit vlastní řešení prediktivní analýzy:
    * [Nasazování webových služeb](machine-learning-publish-a-machine-learning-web-service.md)
    * [Trénování a přeučování modelů prostřednictvím API](machine-learning-retrain-models-programmatically.md)
    * [Správa koncových bodů webové služby](machine-learning-create-endpoint.md)
    * [Škálování webových služeb](machine-learning-scaling-endpoints.md)
    * [Využívání webových služeb](machine-learning-consume-web-services.md)

## Klíčová terminologie a koncepty strojového učení

Pojmy z oblasti strojového učení nemusí být na první pohled úplně jasné. Ke klíčovým pojmům zde najdete definice, které vám leccos objasní. Do komentářů dole na stránce nám můžete napsat, jaké další termíny byste chtěli definovat.

### Zkoumání dat, deskriptivní analýza a prediktivní analýza

**Zkoumání dat** je proces shromažďování informací o velké a často nestrukturované datové sadě s cílem najít charakteristiky pro přesně zacílenou analýzu. **Dolování dat** označuje automatizované zkoumání dat.

**Deskriptivní analýza** je proces analyzování datové sady s cílem shrnout, co se stalo. Velká většina obchodních analýz – například prodejní sestavy, webové metriky a analýzy sociálních sítí – je deskriptivních.

**Prediktivní analýza** je proces vytváření modelů na základě historických nebo aktuální dat s cílem předpovídat budoucí výsledky.


### Učení se supervizí a bez supervize
 V algoritmech **učení se supervizí** probíhá trénink na označených datech – jinými slovy na datech zahrnujících příklady žádoucích odpovědí. Například v modelu, který identifikuje podvodné použití platební karty, by trénink probíhal na datové sadě s označenými datovými body transakcí, o kterých se ví, že byly, resp. nebyly podvodné. Většina strojového učení probíhá se supervizí.

 **Učení bez supervize** probíhá na neoznačených datech a cílem je nalezení vztahů a souvislostí v nich. Cílem může být například nalezení demograficky definovaných skupin zákazníků s podobnými nákupními návyky.

### Trénování a hodnocení modelů
Model strojového učení je abstrakcí otázky, na kterou hledáte odpověď, nebo výsledku, který chcete předpovědět. Modely se trénují a vyhodnocují na základě existujících dat.

#### Data pro trénink
Při tréninku modelů používáte známou datovou sadu a podle charakteru dat model upravujete tak, abyste co nejvíce zpřesnili jeho výstupy. V Azure Machine Learning se model sestavuje z algoritmického modulu, který zpracovává trénovací data a funkční moduly, například modul pro stanovení skóre.

Pokud v rámci učení se supervizí trénujete model pro odhalování podvodů, používáte sadu transakcí, které jsou označeny jako podvodné nebo platné. Datovou sadu náhodně rozdělíte a pak jednu část použijete k natrénování modelu a další část k otestování nebo vyhodnocení modelu.

#### Data pro vyhodnocení
Jakmile model natrénujete, vyhodnotíte ho na zbývajících testovacích datech. Využijete k tomu data, u kterých již znáte výsledky, aby bylo možné určit, zda je váš model schopen přesné predikce.

## Další běžné pojmy používané při strojovém učení

* **algoritmus**: Samostatná sada pravidel používaná k řešení problémů prostřednictvím zpracování dat, matematických výpočtů nebo automatizovaného posuzování.
* **detekce anomálií**: Model, který nachází a označuje neobvyklé události či hodnoty a pomáhá objevovat problémy. Detektor podvodů s platebními kartami například vyhledává nezvyklé nákupy.
* **kategorizovaná data**: Data, která jsou uspořádána do kategorií a která lze rozdělit do skupin. Kategorizovaná datová sada pro automobily by například mohla specifikovat rok výroby, značku, model a cenu.
* **klasifikace**: Model pro uspořádání datových bodů do kategorií na základě datové sady, u které je již seskupení do kategorií známo.
* **konstruování příznaků**: Proces extrahování nebo výběru příznaků či atributů souvisejících s datovou sadou s cílem zvýšit její využitelnost a vylepšit výsledky. Například využitelnost dat o leteckých tarifech lze vylepšit určením dnů v týdnu a svátků. Viz [Výběr a konstruování příznaků v Azure Machine Learning](machine-learning-feature-selection-and-engineering.md).
* **modul**: Funkční součást modelu v nástroji Machine Learning Studio, například modul pro zadávání dat, který umožňuje zadávat a upravovat malé datové sady. Určitým typem modulu v nástroji Machine Learning Studio je i algoritmus.
* **model**: Model v rámci učení se supervizí představuje produkt experimentu strojového učení a skládá se z dat pro trénink, algoritmického modulu a funkčních modulů, jako je modul Určení skóre modelu.
* **číselná data**: Data, která mají význam jako měření (spojitá data) nebo počty (diskrétní neboli nespojitá data). Označují se také jako *kvantitativní data*.
* **rozdělení**: Metoda, podle které data rozdělíte do vzorků. Další informace naleznete v tématu [Rozdělení a vzorky](https://msdn.microsoft.com/library/azure/dn905960.aspx).
* **predikce**: Hodnota nebo hodnoty předpovězené z modelu strojového učení. Možná jste se také setkali s termínem „predikované skóre“. To však nepředstavuje konečný výstup modelu. Podle skóre se řídí vyhodnocení modelu.
* **regrese**: Model pro predikci hodnoty na základě nezávislých proměnných, například předpověď ceny automobilu na základě roku výroby a značky.
* **skóre**: Předpovězená hodnota generovaná z natrénovaného klasifikačního nebo regresního modelu na základě [modulu pro stanovení skóre](https://msdn.microsoft.com/library/azure/dn905995.aspx) v nástroji Machine Learning Studio. Klasifikační modely rovněž vracejí skóre pro pravděpodobnost předpovězené hodnoty. Po vygenerování skóre z modelu můžete vyhodnotit jeho přesnost pomocí [modulu pro vyhodnocení modelu](https://msdn.microsoft.com/library/azure/dn905915.aspx).
* **vzorek**: Část datové sady, která by měla představovat celek. Vzorky lze vybírat náhodně nebo podle konkrétních příznaků či atributů datové sady.



## Další kroky
Se základy prediktivní analýzy a strojového učení se můžete seznámit v [podrobném kurzu](machine-learning-create-experiment.md) a [na základě ukázek](machine-learning-sample-experiments.md).  


<!-- Module References -->
[learning-with-counts]: https://msdn.microsoft.com/library/azure/81c457af-f5c0-4b2d-922c-fdef2274413c/



<!--HONumber=Sep16_HO3-->


