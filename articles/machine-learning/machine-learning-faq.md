<properties
    pageTitle="Časté otázky ke službě Azure Machine Learning | Microsoft Azure"
    description="Představení služby Azure Machine Learning: časté otázky k fakturaci, schopnostem a omezením cloudové služby pro efektivní prediktivní modelování"
    keywords="machine learning introduction,predictive modeling,what is machine learning"
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
    ms.topic="get-started-article"
    ms.date="04/18/2016"
    ms.author="garye"/>

# Časté otázky ke službě Azure Machine Learning: fakturace, schopnosti, omezení a podpora

Toto téma s častými otázkami odpovídá na dotazy ke cloudové službě Azure Machine Learning, která slouží k vývoji prediktivních modelů a zprovozňování řešení prostřednictvím webových služeb. Jde o dotazy k používání služby, včetně modelu fakturace, schopností, omezení a podpory.

## Obecné otázky

**Co je Azure Machine Learning?**

Azure Machine Learning je plně spravovaná služba, kterou můžete využít k vytváření, testování, provozování a správě řešení prediktivní analýzy v cloudu. Vystačíte si jen s prohlížečem, přes který se můžete přihlásit, nahrávat data a okamžitě začít experimentovat se strojovým učením. Prediktivní modelování podporující přetahování myší, rozsáhlá paleta modulů a knihovna šablon, se kterými je možné hned začít, značně usnadňují a urychlují běžné úkoly strojového učení.  Další informace najdete v tématu [Přehled služby Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/). Úvod ke strojovému učení, v němž se probírá klíčová terminologie a koncepty, najdete v [tomto tématu](machine-learning-what-is-machine-learning.md).


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

**Co je Machine Learning Studio?**

Machine Learning Studio je pracovní prostředí, ke kterému přistupujete přes webový prohlížeč. Machine Learning Studio nabízí mnoho modulů s vizuálním kompozičním rozhraním, které umožňuje vytvořit komplexní workflow založený na datové vědě ve formě experimentu.

Další informace o nástroji Machine Learning Studio najdete v tématu [Co je Machine Learning Studio?](machine-learning-what-is-ml-studio.md).

**Co je služba Machine Learning API?**

Služba Machine Learning API umožňuje nasazovat prediktivní modely, například modely předdefinované v nástroji Machine Learning Studio, jako škálovatelné webové služby odolné vůči chybám. Webové služby vytvořené pomocí služby Machine Learning API představují rozhraní REST API, která zajišťují komunikaci mezi externími aplikacemi a vašimi modely prediktivní analýzy.

Další informace najdete v tématu [Připojení k webové službě Machine Learning](machine-learning-connect-to-azure-machine-learning-web-service.md).


## Dotazy k fakturaci

**Jak se Machine Learning fakturuje?**

Informace o fakturaci a cenách najdete v tématu [Machine Learning – ceny](https://azure.microsoft.com/pricing/details/machine-learning/).

**Nabízí Machine Learning bezplatnou zkušební verzi?**

 Když si zaregistrujete bezplatnou zkušební verzi Azure, můžete měsíc zkoušet jakoukoli službu Azure. Další informace o bezplatné zkušební verzi Azure najdete v [těchto častých otázkách](/pricing/free-trial-faq/).

## Otázky k nástroji Machine Learning Studio

### Vytvoření experimentu

**Je pro grafy experimentů k dispozici správa verzí nebo integrace Gitu?**

Ne, ale Machine Learning Studio uchovává všechny iterace experimentu, který nemůže být upraven jinými uživateli.
Další informace najdete v tématu [Správa iterací experimentu v nástroji Machine Learning Studio](machine-learning-manage-experiment-iterations.md).

### Import a export dat pro Machine Learning

**Jaké zdroje dat Machine Learning podporuje?**

Data je možné načíst do experimentu nástroje Machine Learning Studio jedním ze tří způsobů: nahráním místního souboru jako datové sady, použitím modulu k importu dat z cloudových datových služeb nebo importem datové sady uložené z jiného experimentu. Další informace o podporovaných formátech souborů najdete v tématu o [importu trénovacích dat do nástroje Machine Learning Studio](machine-learning-data-science-import-data.md).


#### <a id="ModuleLimit"></a>Jak velká může být datová sada pro mé moduly?

Moduly v nástroji Machine Learning Studio podporují datové sady, které obsahují až 10 GB hustých číselných dat pro běžné případy použití. Pokud modul přijímá více než jeden vstup, 10 GB je celková velikost všech vstupních velikostí. Větší datové sady je před ingestováním možné vzorkovat pomocí dotazů Hive nebo Azure SQL Database nebo předzpracováním metodou Učení dle počtů.  

Během normalizace příznaků lze následující typy dat rozšířit do větších datových sad. Tyto typy jsou omezeny na méně než 10 GB:

- Řídké
- Kategorické
- Řetězce
- Binární data

Následující moduly jsou omezeny na datové sady menší než 10 GB:

- Doporučené moduly
- Modul SMOTE
- Skriptovací moduly: R, Python, SQL
- Moduly, kde velikost výstupních dat může být větší než velikost vstupních dat, například Spojení nebo Hashování příznaků
- Pro velmi velký počet iterací Cross-validation, Tune Model Hyperparameters, Ordinal Regression a One-vs-All Multiclass

Pro datové sady větší než několik GB byste měli nahrávat data do úložiště Azure nebo Azure SQL Database, případně použít HDInsight, nikoli přímo nahrávat z místního souboru.


####<a id="UploadLimit"></a>Jaké jsou limity pro nahrávání dat?
Pro datové sady větší než několik GB byste měli nahrávat data do úložiště Azure nebo Azure SQL Database, případně použít HDInsight, nikoli přímo nahrávat z místního souboru.

**Můžu číst data z Amazonu S3?**

Pokud máte malé množství dat a chcete je zveřejnit přes adresu URL protokolu HTTP, můžete použít modul [Import dat][import-data]. Jakákoli větší množství dat nejdříve přeneste do Azure Storage a pak použijte modul [Import dat][import-data], abyste je připojili k experimentu.
<!--
<SEE CLOUD DS PROCESS>
-->

**Je k dispozici integrovaná funkce pro obrazový vstup?**

Informace o funkci obrazového vstupu najdete v referenci [Import obrázků][image-reader].

### Moduly

**V nástroji Azure Machine Learning Studio není algoritmus, zdroj dat, formát dat nebo operace transformace dat, kterou hledám. Jaké mám možnosti?**

Můžete navštívit [fórum pro zpětnou vazbu uživatelů](http://go.microsoft.com/fwlink/?LinkId=404231), kde najdete žádosti o funkce, které sledujeme. Pokud již existuje žádost o funkci, kterou sami hledáte, přidejte k ní svůj hlas. Pokud funkce, kterou hledáte, neexistuje, vytvořte novou žádost. Na tomto fóru můžete také sledovat stav své žádosti. Tento seznam pečlivě sledujeme a často aktualizujeme stav dostupnosti funkce. Navíc díky integrované podpoře R a Pythonu je možné vytvářet vlastní transformace podle potřeby.


**Mohu převést svůj existující kód do nástroje Machine Learning Studio?**

Ano, svůj existující kód R nebo Python můžete do nástroje Machine Learning Studio převést, spustit jej v tom stejném experimentu pomocí inteligentních algoritmů Azure Machine Learning a přes Azure Machine Learning nasadit řešení jako webovou službu. Další informace najdete v tématech o [rozšíření experimentů pomocí R](machine-learning-extend-your-experiment-with-r.md) a o [spouštění pythonových skriptů strojového učení v nástroji Azure Machine Learning Studio](machine-learning-execute-python-scripts.md).

**Lze definovat model pomocí něčeho jako jazyk [PMML](http://en.wikipedia.org/wiki/Predictive_Model_Markup_Language)?**

Ne, to není podporováno, ale k definování modulu je možné použít vlastní kód R nebo Python.

**Kolik modulů je možné v experimentu paralelně spustit?**  

V jednom experimentu můžete paralelně spustit až 4 moduly.


### Zpracování dat

**Je k dispozici funkce pro interaktivní vizualizaci dat (kromě vizualizací R) v rámci experimentu?**

Kliknutím na výstup modulu můžete vizualizovat data a získat statistiky.

**Při prohlížení náhledu výsledků nebo dat v prohlížeči je omezen počet řádků a sloupců. Proč?**

Vzhledem k tomu, že se data přenášejí do prohlížeče a mohou být velká, je velikost omezena, aby nedošlo ke zpomalení nástroje Machine Learning Studio. Pokud chcete vizualizovat všechna data nebo výsledky, je vhodnější data stáhnout a použít Excel nebo jiný nástroj.

### Algoritmy

**Jaké současné algoritmy jsou podporovány v nástroji Machine Learning Studio?**

Machine Learning Studio poskytuje nejmodernější algoritmy, například škálovatelné vylepšené rozhodovací stromy, systémy bayesovského rozhodování, hluboké neuronové sítě a rozhodovací džungle vyvinuté v Microsoft Research. K dispozici jsou i škálovatelné open-source balíčky pro strojové učení, například Vowpal Wabbit. Machine Learning Studio podporuje algoritmy strojového učení pro binární klasifikaci a klasifikaci s více třídami, regresi a clustering. Podívejte se úplný seznam [modulů v Machine Learning][machine-learning-modules].

**Navrhnete mi automaticky algoritmus Machine Learning vhodný pro má data?**

Ne, ale v nástroji Machine Learning Studio existuje několik způsobů, jak porovnat výsledky jednotlivých algoritmů a určit, který z nich je pro váš problém nejvhodnější.

**Lze obecně poradit, jak z poskytovaných algoritmů upřednostnit jeden před jiným?**
Přečtěte si téma [Jak vybrat algoritmus](machine-learning-algorithm-choice.md).

**Jsou poskytované algoritmy napsány v R nebo Pythonu?**

Ne, tyto algoritmy jsou většinou napsány v kompilovaných jazycích, aby poskytovaly vyšší výkon.

**Jsou k dispozici nějaké podrobnosti o algoritmech?**

Určité informace o algoritmech jsou k dispozici v dokumentaci. Pro optimalizaci algoritmu pro vaše použití jsou popsány parametry, pomocí kterých je možné činnost algoritmu upravit.  

**Je podporováno online učení?**

Ne, v tuto chvíli je podporováno jenom programové přeučení.

**Je možné vizualizovat vrstvy modelu neuronové sítě pomocí integrovaného modulu?**

Ne.

**Je možné vytvářet vlastní moduly v jazyce C# nebo jiném?**

V tuto chvíli je možné vlastní moduly vytvářet jen v jazyce R.

### Modul R

**Jaké balíčky R jsou k dispozici v nástroji Machine Learning Studio?**

K dnešnímu dni Machine Learning Studio podporuje přes 400 balíčků CRAN R. Všechny zahrnuté balíčky jsou uvedeny v [aktuálním seznamu](http://az754797.vo.msecnd.net/docs/RPackages.xlsx). Pokud chcete zjistit, jak tento seznam získat sami, přečtěte si také téma o [rozšíření experimentů pomocí R](machine-learning-extend-your-experiment-with-r.md). Pokud vámi požadovaný balíček není v seznamu, uveďte název balíčku na [fóru pro zpětnou vazbu uživatelů](http://go.microsoft.com/fwlink/?LinkId=404231).

**Je možné vytvořit vlastní modul R?**

Ano, další informace najdete v tématu o [vytváření vlastních modulů R ve službě Azure Machine Learning](machine-learning-custom-r-modules.md).

**Je pro R k dispozici prostředí REPL?**

Ne, ve studiu není žádné prostředí REPL.

### Modul Python

**Je možné vytvořit vlastní modul Python?**

V této chvíli ne, ale stejného výsledku je možné dosáhnout, když použijete jeden nebo více modulů [Execute Python Script][python].

**Je pro Python k dispozici prostředí REPL?**

V nástroji Machine Learning Studio můžete použít Jupyter Notebooks. Další informace najdete v tématu [Představení Jupyter Notebooks v nástroji Azure Machine Learning Studio] (http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx).

## Webová služba

###Programové přeučení modelů 

**Jak je možné programově přeučit modely Azure Machine Learning?**

Použijte rozhraní Retraining API. Další informace najdete v tématu o [programovém přeučení modelů Machine Learning](machine-learning-retrain-models-programmatically.md). V [ukázce přeučování v Microsoft Azure Machine Learning](https://azuremlretrain.codeplex.com/) je k dispozici i ukázkový kód.

### Vytvoření

**Je možné nasadit model lokálně nebo do aplikace bez připojení k internetu?**

Ne.


**Existuje základní latence, kterou je možné očekávat u všech webových služeb?**

Přečtěte si téma [Limity předplatného Azure](../azure-subscription-service-limits.md).

### Použití

**Kdy je žádoucí spustit prediktivní model jako službu Batch Execution a kdy jako službu Request Response?**

Služba Request Response (RRS) je vysoce škálovatelná webová služba s nízkou latencí, pomocí které se poskytuje rozhraní pro bezstavové modely vytvořené a nasazené z prostředí experimentů. Služba Batch Execution (BES) je služba pro asynchronní vyhodnocování dávky datových záznamů. Vstup pro BES je podobný datovému vstupu používanému v RRS. Hlavním rozdílem je to, že BES čte blok záznamů z několika zdrojů, například Služby objektů blob a služby Table v Azure, Azure SQL Database, HDInsight a zdrojů HTTP. Další informace najdete v tématu o [využívání webových služeb Machine Learning](machine-learning-consume-web-services.md).

**Jak je možné aktualizovat model pro nasazenou webovou službu?**

Aktualizace prediktivního modelu pro již nasazenou službu je stejně jednoduché, jako úprava a opětovné spuštění experimentu, který jste použili k vytvoření a uložení naučeného modelu. Jakmile máte k dispozici novou verzi trénovaného modelu, Machine Learning Studio se vás zeptá, jestli chcete aktualizovat webovou službu. Podrobnosti o tom, jak aktualizovat nasazenou webovou službu, najdete v tématu o [nasazení webové služby Machine Learning](machine-learning-publish-a-machine-learning-web-service.md).

Kromě toho můžete použít i rozhraní Retraining API.
Další informace najdete v tématu o [programovém přeučení modelů Machine Learning](machine-learning-retrain-models-programmatically.md). V [ukázce přeučování v Microsoft Azure Machine Learning](https://azuremlretrain.codeplex.com/) je k dispozici i ukázkový kód.

**Jak je možné sledovat webovou službu nasazenou do provozu?**

Po nasazení prediktivního modelu je možné jej monitorovat na portálu Azure Classic. Každá nasazená služba má svůj vlastní řídicí panel, kde můžete sledovat informace z monitorování dané služby.

**Je možné si někde zobrazit výstup RRS nebo BES?**

V případě RRS je místem, kde uvidíte výsledky, zpravidla odpověď webové služby. Můžete je zapsat také do Azure Blob Storage. U BES je výstup standardně zapisován do objektu blob. Pomocí modulu [Export dat][export-data] je možné výstup zapsat i do databáze nebo tabulky.

**Webové služby je možné vytvářet jen z modelů sestavených v nástroji Machine Learning Studio?**

Ne, webové služby je možné vytvářet i přímo z Jupyter Notebooks a RStudia.

**Kde najdu informace o chybových kódech?**

Seznam chybových kódů a jejich popisy najdete v tématu o [chybových kódech modulů Machine Learning](https://msdn.microsoft.com/library/azure/dn905910.aspx).

## Škálovatelnost

**Jaká je škálovatelnost webové služby?**

V tuto chvíli je výchozí koncový bod zřizován s 20 souběžnými požadavky RRS na jeden koncový bod. Jak je popsáno v tématu o [škálování koncových bodů API](machine-learning-scaling-endpoints.md), je možné kapacitu rozšířit na 200 souběžných požadavků na jeden koncový bod a každou službu lze škálovat na 10 000 koncových bodů. Pro BES každý koncový bod umožňuje zpracovat 40 požadavků najednou, požadavky nad těchto 40 požadavků se zařazují do fronty. Požadavky ve frontě se budou spouštět automaticky podle toho, jak se fronta vyprazdňuje.


**Jsou úlohy R rozprostřeny mezi uzly?**

Ne.  


**Kolik dat mohu použít k trénování?**

Moduly v nástroji Machine Learning Studio podporují datové sady, které obsahují až 10 GB hustých číselných dat pro běžné případy použití. Pokud modul přijímá více než jeden vstup, celková velikost všech vstupů je dohromady 10 GB. Větší datové sady je před ingestováním možné vzorkovat pomocí dotazů Hive nebo Azure SQL Database nebo předzpracováním moduly [Učení dle počtů][counts].  

Během normalizace příznaků lze následující typy dat rozšířit do větších datových sad. Tyto typy jsou omezeny na méně než 10 GB:

- řídké
- kategorické
- řetězce
- binární data

Následující moduly jsou omezeny na datové sady menší než 10 GB:

- Doporučené moduly
- Modul SMOTE
- Skriptovací moduly: R, Python, SQL
- Moduly, kde velikost výstupních dat může být větší než velikost vstupních dat, například Spojení nebo Hashování příznaků
- Pro velmi velký počet iterací Cross-Validate, Tune Model Hyperparameters, Ordinal Regression a One-vs-All Multiclass

Pro datové sady větší než několik GB byste měli nahrávat data do úložiště Azure nebo Azure SQL Database, případně použít HDInsight, nikoli přímo nahrávat z místního souboru.


**Existují nějaká omezení velikosti vektoru?**

Řádky i sloupce jsou omezeny na omezení Max Int v rozhraní .NET: 2 147 483 647.

**Je možné upravit velikost virtuálního počítače, na kterém běží webová služba?**

Ne.  

## Zabezpečení a dostupnost

**Kdo má standardně přístup ke koncovému bodu HTTP pro webovou službu? Jak omezím přístup ke koncovému bodu?**

Po nasazení webové služby je pro tuto službu vytvořen koncový bod. Výchozí koncový bod je možné volat pomocí jeho klíče rozhraní API. Další koncové body s jejich vlastními klíči je možné přidat na portálu Azure Classic nebo programově pomocí rozhraní Web Service Management API. Pro volání do webové služby se vyžadují přístupové klíče. Další informace najdete v tématu [Připojení k webové službě Machine Learning](machine-learning-connect-to-azure-machine-learning-web-service.md).


**Co se stane, když se nenajde můj účet Azure Storage?**

Machine Learning Studio využívá uživatelem zadaný účet Azure Storage k ukládání pracovních dat při provádění workflow. Tento účet úložiště se do nástroje Machine Learning Studio zadává při vytváření pracovního prostoru. Pokud po vytvoření pracovního prostoru dojde k odstranění účtu úložiště nebo jej již nelze najít, pracovní prostor přestane fungovat a všechny experimenty v něm selžou.

Pokud jste účet úložiště omylem odstranili, jediný způsob, jak tuto situaci vyřešit, je znovu vytvořit účet úložiště se stejným názvem a ve stejné oblasti jako odstraněný účet úložiště. Poté znovu synchronizujte přístupový klíč.


**Co se stane, když můj přístupový klíč účtu úložiště není synchronizovaný?**

Machine Learning Studio využívá uživatelem zadaný účet Azure Storage k ukládání pracovních dat při provádění workflow. Tento účet úložiště se do nástroje Machine Learning Studio zadává při vytváření pracovního prostoru. Přístupové klíče se přiřadí k danému pracovnímu prostoru. Pokud se přístupové klíče po vytvoření pracovního prostoru změní, pracovní prostor nebude mít nadále přístup k účtu úložiště. Kvůli tomu přestane fungovat a všechny experimenty v něm selžou.

Pokud jste změnili přístupové klíče účtu úložiště, znovu je v pracovním prostoru synchronizujte přes portál Azure Classic.  


## Azure Marketplace

Přečtěte si téma s [častými otázkami k publikování a používání aplikací na Machine Learning Marketplace](machine-learning-marketplace-faq.md).

## Podpora a školení

**Kde získám školení pro Azure Machine Learning?**

V [centru dokumentace Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) jsou k dispozici videokurzy a návody. Tyto podrobné návody představují služby a popisují životní cyklus vycházející z datové vědy a sestávající z importu dat, jejich čištění, vytváření prediktivních modelů a jejich nasazování do provozu pomocí Azure Machine Learning.

Průběžně budeme do centra pro Machine Learning přidávat nové materiály Na [fóru pro zpětnou vazbu uživatelů](https://windowsazure.uservoice.com/forums/257792-machine-learning) můžete zaslat žádost o další výukové materiály v centru pro Machine Learning.

Školení najdete i v [Microsoft Virtual Academy](http://www.microsoftvirtualacademy.com/training-courses/getting-started-with-microsoft-azure-machine-learning).

**Jak získat podporu pro Azure Machine Learning?**

Pokud potřebujete technickou podporu pro Azure Machine Learning, přejděte na [Podporu Azure](/support/options/) a vyberte **Machine Learning**.

Azure Machine Learning má i fórum komunity na webu MSDN, kde můžete klást otázky související se službou Azure Machine Learning. Fórum je monitorováno týmem pro Azure Machine Learning. Navštivte [Fórum Azure](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning).


<!-- Module References -->
[image-reader]: https://msdn.microsoft.com/library/azure/893f8c57-1d36-456d-a47b-d29ae67f5d84/
[join]: https://msdn.microsoft.com/library/azure/124865f7-e901-4656-adac-f4cb08248099/
[machine-learning-modules]: https://msdn.microsoft.com/library/azure/6d9e2516-1343-4859-a3dc-9673ccec9edc/
[partition-and-sample]: https://msdn.microsoft.com/library/azure/a8726e34-1b3e-4515-b59a-3e4a475654b8/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[python]: https://msdn.microsoft.com/library/azure/CDB56F95-7F4C-404D-BDE7-5BB972E6F232
[counts]: https://msdn.microsoft.com/library/azure/dn913056.aspx



<!--HONumber=Jun16_HO2-->


