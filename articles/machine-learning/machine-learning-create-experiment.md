<properties
    pageTitle="Vytvoření jednoduchého experimentu v nástroji Machine Learning Studio | Microsoft Azure"
    description="První kurz strojového učení, v jehož rámci se vytvoří jednoduchý experiment a natrénuje a otestuje model lineární regrese v nástroji Azure Machine Learning Studio."
    keywords="experiment,linear regression,machine learning algorithms,machine learning tutorial,predictive modeling techniques"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="03/09/2016"
    ms.author="garye"/>

# Kurz strojového učení: Vytvoření prvního experimentu v nástroji Azure Machine Learning Studio

V tomto prvním kurzu strojového učení vytvoříme model lineární regrese, který bude předpovídat cenu automobilu podle různých proměnných, například značky a technických specifikací. K tomu využijeme Azure Machine Learning Studio, ve kterém budeme vyvíjet a iterovat jednoduchý experiment prediktivní analýzy.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Experiment v nástroji Machine Learning Studio vzniká přetahováním komponent myší na plátno a jejich propojováním s cílem *vytvořit* model, *natrénovat ho*, *stanovit jeho skóre a otestovat ho*. Experiment používá techniky prediktivního modelování ve formě modulů nástroje Machine Learning Studio, které ingestují data, trénují podle nich model a aplikují model na nová data. Kromě toho je možné přidávat moduly pro předzpracování dat a výběr příznaků, rozdělení dat na trénovací a testovací sady a vyhodnocení nebo křížové ověření kvality modelu.  

Přejděte na Machine Learning Studio na adrese [https://studio.azureml.net](https://studio.azureml.net) a klikněte na tlačítko **Začít**. Můžete si vybrat buď přístup hosta, nebo se přihlásit pomocí účtu Microsoft.

Další obecné informace o nástroji Machine Learning Studio najdete v tématu [Co je Machine Learning Studio?](machine-learning-what-is-ml-studio.md).

>[AZURE.TIP] Pokud si chcete stáhnout a vytisknout diagram s přehledem funkcí nástroje Machine Learning Studio, nahlédněte do tématu [Diagram s přehledem možností nástroje Machine Learning Studio](machine-learning-studio-overview-diagram.md).


## Vytvoření experimentu v pěti krocích

V tomto kurzu strojového učení vytvoříte experiment provedením 5 základních kroků v nástroji Machine Learning Studio, v rámci kterých sestavíte model, natrénujete ho a stanovíte jeho skóre:

- Vytvoření modelu
    - [Krok 1: Získání dat]
    - [Krok 2: Předzpracování dat]
    - [Krok 3: Definice příznaků]
- Trénování modelu
    - [Krok 4: Volba a použití algoritmu učení]
- Stanovení skóre a otestování modelu
    - [Krok 5: Předpověď cen nových automobilů]

[Krok 1: Získání dat]: #step-1-get-data
[Krok 2: Předzpracování dat]: #step-2-preprocess-data
[Krok 3: Definice příznaků]: #step-3-define-features
[Krok 4: Volba a použití algoritmu učení]: #step-4-choose-and-apply-a-learning-algorithm
[Krok 5: Předpověď cen nových automobilů]: #step-5-predict-new-automobile-prices


## Krok 1: Získání dat

Machine Learning Studio obsahuje několik ukázkových datových sad, ze kterých si můžete vybrat, nebo můžete data importovat z mnoha zdrojů. V tomto příkladu použijeme dodávanou ukázkovou datovou sadu **Automobile price data (Raw)**.
Tato datová sada obsahuje záznamy řady různých automobilů, včetně informací o značce, modelu, technických specifikacích a ceně.

1. Kliknutím na **+NOVÝ** ve spodní části okna nástroje Machine Learning Studio začněte nový experiment a vyberte **EXPERIMENT** a **Prázdný experiment**. V horní části plátna vyberte výchozí název experimentu a přejmenujte jej na něco smysluplného, například **Predikce ceny automobilu**.

2. Nalevo od plátna experimentu je paleta datových sad a modulů. Do pole Hledat v horní části palety zadejte **automobile**. Vyhledá se datová sada **Automobile price data (Raw)**.

    ![Vyhledávání palety][screen1a]

3. Přetáhněte datovou sadu na plátno experimentu.

    ![Datová sada][screen1]

Pokud se chcete podívat, jak tato data vypadají, klikněte na výstupní port ve spodní části datové sady automobilů a vyberte **Vizualizovat**. Proměnné datové sady jsou zobrazeny jako sloupce a každá instance automobilu je představována jedním řádkem. Sloupec nejvíce vpravo (sloupec 26 s názvem price) je cílová proměnná, kterou se pokusíme předpovídat.

![Vizualizace datové sady][screen1b]

Kliknutím na **x** v pravém horním rohu zavřete okno vizualizace.

## Krok 2: Předzpracování dat

Před analýzou datové sady bývá zpravidla nutné sadu nějakým způsobem předzpracovat. Možná jste si ve sloupcích různých řádků všimli chybějících hodnot. Tyto chybějící hodnoty se musí vyčistit, aby model mohl data správně analyzovat. V našem případě odstraníme všechny řádky, ve kterých některé hodnoty chybí. Hodnoty ve sloupci **normalized-losses** navíc z velké části chybí, proto tento sloupec v modelu zcela vynecháme.

> [AZURE.TIP] Vyčištění chybějících hodnot ze vstupních dat je pro většinu modulů nutností.

Nejprve odstraníme sloupec **normalized-losses**, pak odstraníme všechny řádky, ve kterých chybí data.

1. Do pole Hledat v horní části palety modulů zadejte **výběr sloupců**. Vyhledá se modul [Výběr sloupců v datové sadě ][select-columns], který pak přetáhněte na plátno experimentu a propojte jej s výstupním portem datové sady **Automobile price data (Raw)**. Tento modul umožňuje vybrat, které sloupce dat chceme zahrnout do modelu, nebo je z modelu naopak vyloučit.

2. Vyberte modul [Výběr sloupců v datové sadě][select-columns] a v podokně **Vlastnosti** klikněte na **Spustit selektor sloupců**.

    - Ujistěte se, že v rozevíracím seznamu filtru **Začít s** je zvolena možnost **Všechny sloupce**. Tím modul [Výběr sloupců v datové sadě][select-columns] dostává instrukci, aby procházel všechny sloupce (kromě těch, které vyloučíme).
    - Na dalším řádku vyberte **Vyloučit** a **názvy sloupců** a klikněte do textového pole. Zobrazí se seznam sloupců. Vyberte sloupec **normalized-losses**, který tak bude přidán do textového pole.
    - Kliknutím na tlačítko se značkou zaškrtnutí (OK) zavřete selektor sloupců.

    ![Výběr sloupců][screen3]

    Podokno vlastností modulu **Výběr sloupců v datové sadě** indikuje, že modul bude procházet všechny sloupce datové sady kromě **normalized-losses**.

    ![Vlastnosti modulu Výběr sloupců v datové sadě][screen4]

    > [AZURE.TIP] Kliknutím dvakrát na modul a zadáním textu je možné přidat k modulu komentář. To vám může pomoci rychle poznat, jaký je účel modulu v experimentu. V tomto případě klikněte dvakrát na modul [Výběr sloupců v datové sadě][select-columns] a zadejte komentář Vyloučit normalized-losses.

3. Přetáhněte modul [Vyčištění chybějících dat][clean-missing-data] na plátno experimentu a propojte jej s modulem [Výběr sloupců v datové sadě][select-columns]. V podokně **Vlastnosti** vyberte v části **Režim čištění** možnost **Odstranit celý řádek**. Tím se data vyčistí odstraněním řádků, ve kterém chybí některé hodnoty. Klikněte dvakrát na modul a zadejte komentář Odstranění řádků s chybějícími hodnotami.

    ![Vlastnosti modulu Vyčištění chybějících dat][screen4a]

4. Pod plátnem experimentu klikněte na **SPUSTIT**, aby se experiment spustil.

Až se experiment dokončí, u všech modulů se zobrazí zelená značka zaškrtnutí, která označuje, že činnost modulu úspěšně skončila. Všimněte si také stavu **Konec běhu** v pravém horním rohu.

![První běh experimentu][screen5]

Vše, co jsme do této chvíle v experimentu udělali, bylo vyčištění dat. Pokud si chcete zobrazit vyčištěnou datovou sadu, klikněte na levý výstupní port modulu [Vyčištění chybějících dat][clean-missing-data] (Vyčištěná datová sada) a vyberte **Vizualizovat**. Všimněte si, že již není zobrazen sloupec **normalized-losses** a že nechybí žádné hodnoty.

Nyní když jsou data vyčištěna, můžete určit, které příznaky použijeme v prediktivním modelu.

## Krok 3: Definice příznaků

Ve strojovém učení se jako *příznaky* označují jednotlivé měřitelné vlastnosti něčeho, co vás zajímá. V naší datové sadě každý řádek představuje jeden automobil a každý sloupec je příznak daného automobilu. Nalezení správné sady příznaků pro vytvoření prediktivního modelu vyžaduje experimentování a znalost problému, který chcete vyřešit. Některé příznaky jsou pro predikci cíle vhodnější než jiné. Některé příznaky navíc silně korelují s jinými příznaky (například city-mpg a highway-mpg), tudíž do modelu nepřinesou mnoho nových informací a lze je odstranit.

Vytvořme model, který používá podmnožinu příznaků naší datové sady. Můžete se vrátit a vybrat jiné příznaky, spustit experiment znovu a zjistit, jestli nedostanete lepší výsledky. Jako první odhad vybereme pomocí modulu [Výběr sloupců v datové sadě][select-columns] následující příznaky (sloupce). Všimněte si, že pro trénování modelu musíme zahrnout hodnotu *price*, tedy cenu, kterou budeme předpovídat.

    make, body-style, wheel-base, engine-size, horsepower, peak-rpm, highway-mpg, price

1. Přetáhněte na plátno experimentu další modul [Výběr sloupců v datové sadě][select-columns] a propojte jej s levým výstupním portem modulu [Vyčištění chybějících dat][clean-missing-data]. Poklikejte na modul a zadejte Výběr příznaků pro predikci.

2. V podokně **Vlastnosti** klikněte na **Spustit selektor sloupců**.

3. V selektoru sloupců vyberte pro **Začít s** možnost **Žádné sloupce**, pak v řádku filtru vyberte **Zahrnout** a **názvy sloupců**. Zadejte náš seznam názvů sloupců. Tím modul dostává instrukci, aby procházel jen ty sloupce, které určíme.

    > [AZURE.TIP] Protože jsme experiment spustili, definice sloupců pro naše data prošla z původní datové sady přes modul [Vyčištění chybějících dat][clean-missing-data]. Když propojíte [Výběr sloupců v datové sadě][select-columns] s modulem [Vyčištění chybějících dat][clean-missing-data], modul [Výběr sloupců v datové sadě][select-columns] obdrží informace o definicích sloupců v našich datech. Když kliknete na pole **názvy sloupců**, zobrazí se seznam sloupců. Můžete pak vybrat sloupce, které chcete do seznamu přidat.

4. Klikněte na tlačítko zaškrtnutí (OK).

![Výběr sloupců][screen6]

Tímto se vytvoří datová sada, kterou algoritmus učení použije v dalších krocích. Později se můžete vrátit a zkusit jiný výběr příznaků.

## Krok 4: Volba a použití algoritmu učení

Nyní když jsou data připravena, tvorba prediktivního modelu sestává z trénování a testování. Naše data použijeme pro trénování modelu, pak model otestujeme a zjistíme, jak přesně dokáže předpovídat ceny.

*Klasifikace* a *regrese* jsou dva typy technik strojového učení s učitelem. Klasifikace se používá k předpovědím na základě definované sady hodnot, například barvy (červená, modrá nebo zelená). Regrese se používá při předpovědích ze spojité sady hodnot, například z věku osoby.

Chcete předpovídat cenu automobilu, což může být jakákoli hodnota, proto použijeme regresní model. V tomto příkladu natrénujeme jednoduchý model *lineární regrese* a v dalším kroku jej otestujeme.

1. Naše data můžeme použít jak pro trénování, tak pro testování, je totiž možné je rozdělit na samostatné sady pro trénování a testování. Vyberte a přetáhněte na plátno experimentu modul [Rozdělení dat][split] a propojte jej s výstupem posledního modulu [Výběr sloupců v datové sadě][select-columns]. Nastavte **Podíl řádků v první výstupní sadě dat** na 0,75. Takto použijeme 75 procent dat pro trénování modelu a 25 procent si ponecháme na testování.

    > [AZURE.TIP] Změnou parametru **Náhodná počáteční hodnota** je možné pro trénování a testování vytvořit různé náhodné vzorky. Tento parametr řídí nastavování počáteční hodnoty pseudonáhodného generátoru čísel.

2. Spusťte experiment. Díky tomu budou moci moduly [Výběr sloupců v datové sadě][select-columns] a [Rozdělení dat][split] předat definice sloupců do modulů, které přidáme jako další.  

3. Nyní vyberte algoritmus učení. Na paletě modulů nalevo od plátna rozbalte kategorii **Strojové učení** a pak **Inicializovat model**. Tímto se zobrazí několik kategorií modulů, které je možné použít k inicializaci algoritmů strojového učení.

    Pro tento experiment vyberte modul [Lineární regrese][linear-regression] v kategorii **Regrese** (modul je možné najít i zadáním lineární regrese do pole Hledat palety) a přetáhněte jej na plátno experimentu.

4. Najděte a přetáhněte modul [Trénování modelu][train-model] na plátno experimentu. Propojte levý vstupní port na výstup modulu [Lineární regrese][linear-regression]. Propojte pravý vstupní port na výstup trénovacích dat (levý port) modulu [Rozdělení dat][split].

5. Vyberte modul [Trénování modelu][train-model], v podokně **Vlastnosti** klikněte na **Spustit selektor sloupců** a vyberte sloupec **price**. Toto je hodnota, kterou náš model bude předpovídat.

    ![Výběr sloupce price][screen7]

6. Spusťte experiment.

Výsledkem je natrénovaný model, který lze použít ke stanovení skóre pro nové vzorky a k následné predikci. 

![Použití algoritmu strojového učení][screen8]

## Krok 5: Předpověď cen nových automobilů

Nyní když jsme natrénovali model pomocí 75 procent dat, můžeme model použít ke stanovení skóre u zbylých 25 procent dat a zjistit, jak dobře model funguje.

1. Najděte a přetáhněte modul [Určení skóre modelu][score-model] na plátno experimentu a propojte jej s levým vstupním portem k výstupu modulu [Trénování modelu][train-model]. Propojte pravý vstupní port na výstup testovacích dat (pravý port) modulu [Rozdělení dat][split].  

    ![Modul Určení skóre modelu][screen8a]

2. Spusťte experiment a zobrazte výstup modulu [Určení skóre modelu][score-model] tak, že kliknete na výstupní port a vyberete **Vizualizovat**. Na výstupu se zobrazí predikované hodnoty ceny a známé hodnoty v testovacích datech.  

3. Nakonec pro otestování kvality výsledků najděte a přetáhněte modul [Vyhodnocení modelu][evaluate-model] na plátno experimentu a propojte jej s levým vstupním portem k výstupu modulu [Určení skóre modelu][score-model]. (K dispozici jsou dva vstupní porty, protože modul [Vyhodnocení modelu][evaluate-model] je možné použít k porovnání dvou modelů.)

4. Spusťte experiment.

Zobrazte výstup modulu [Vyhodnocení modelu][evaluate-model] tak, že kliknete na výstupní port a vyberete **Vizualizovat**. Pro náš model se zobrazí následující statistiky:

- **Střední absolutní chyba** (MAE): Průměr absolutních chyb (*chyba* je rozdíl mezi předpovězenou a skutečnou hodnotu)
- **Odmocnina střední kvadratické chyby** (RMSE): Druhá odmocnina průměru kvadratických chyb předpovědí na základě testovací datové sady
- **Relativní absolutní chyba**: Průměr absolutních chyb relativních k absolutnímu rozdílu mezi skutečnými hodnotami a průměrem všech skutečných hodnot
- **Relativní kvadratická chyba**: Průměr kvadratických chyb relativních ke kvadratickému rozdílu mezi skutečnými hodnotami a průměrem všech skutečných hodnot
- **Koeficient spolehlivosti**: Znám také jako **hodnota spolehlivosti R**, tedy statistická metrika označující kvalitu přizpůsobení modelu datům

Pro každou statistiku chyb platí, že menší hodnota je lepší. Menší hodnota označuje, že předpověď přesněji odpovídá skutečným hodnotám. V případě **koeficientu spolehlivosti** platí, že čím bližší je jeho hodnota hodnotě jedna (1,0), tím lepší jsou předpovědi.

![Výsledky vyhodnocení][screen9]

Konečný experiment by měl vypadat přibližně takto:

![Kurz strojového učení: Úplný experiment lineární regrese, který využívá techniky prediktivního modelování][screen10]

## Další kroky

Nyní když jste dokončili první kurz strojového učení a máte nastaven experiment, můžete postup opakovat a pokusit se model vylepšit. Můžete například změnit příznaky, které se používají při predikci. Dále můžete upravit vlastnosti algoritmu [Lineární regrese][linear-regression] nebo zkusit úplně jiný algoritmus. Dokonce je možné přidat do experimentu několik algoritmů strojového učení najednou a porovnat je pomocí modulu [Vyhodnocení modelu][evaluate-model].

> [AZURE.TIP] Pomocí tlačítka **ULOŽIT JAKO** pod plátnem experimentu je možné zkopírovat kteroukoli iteraci experimentu. Všechny iterace experimentu si lze zobrazit kliknutím na **ZOBRAZIT HISTORII BĚHŮ** pod plátnem. Další podrobnosti najdete v tématu [Správa iterací experimentů v nástroji Azure Machine Learning Studio][runhistory].

[runhistory]: machine-learning-manage-experiment-iterations.md

Jakmile budete se svým modelem spokojeni, můžete jej nasadit jako webovou službu, kterou lze použít k předvídání cen automobilů na základě nových dat. Další podrobnosti najdete v tématu [Nasazení webové služby Azure Machine Learning][publish].

[publish]: machine-learning-publish-a-machine-learning-web-service.md

Rozsáhlejší a podrobnější návod k technikám prediktivního modelování pro vytváření, trénování, stanovení skóre a nasazení modelu najdete v tématu [Vývoj prediktivního řešení pomocí Azure Machine Learningu][walkthrough].

[walkthrough]: machine-learning-walkthrough-develop-predictive-solution.md

<!-- Images -->
[screen1]:./media/machine-learning-create-experiment/screen1.png
[screen1a]:./media/machine-learning-create-experiment/screen1a.png
[screen1b]:./media/machine-learning-create-experiment/screen1b.png
[screen2]:./media/machine-learning-create-experiment/screen2.png
[screen3]:./media/machine-learning-create-experiment/screen3.png
[screen4]:./media/machine-learning-create-experiment/screen4.png
[screen4a]:./media/machine-learning-create-experiment/screen4a.png
[screen5]:./media/machine-learning-create-experiment/screen5.png
[screen6]:./media/machine-learning-create-experiment/screen6.png
[screen7]:./media/machine-learning-create-experiment/screen7.png
[screen8]:./media/machine-learning-create-experiment/screen8.png
[screen8a]:./media/machine-learning-create-experiment/screen8a.png
[screen9]:./media/machine-learning-create-experiment/screen9.png
[screen10]:./media/machine-learning-create-experiment/complete-linear-regression-experiment.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/



<!--HONumber=Jun16_HO2-->


