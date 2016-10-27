<properties
    pageTitle="Časté otázky ke službě Azure Machine Learning | Microsoft Azure"
    description="Představení služby Azure Machine Learning: časté otázky k fakturaci, schopnostem a omezením cloudové služby pro efektivní prediktivní modelování"
    keywords="úvod ke strojovému učení, prediktivní modelování, co je strojové učení"
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
    ms.date="07/14/2016"
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

**Kde jsou uvedeny klasické webové služby? Kde jsou uvedeny nové webové služby založené na správci Azure Resource Manager?**

Klasické webové služby jsou uvedeny na kartě webových služeb v nástroji [Machine Learning Studio](http://studio.azureml.net). Nové webové služby založené na správci Azure Resource Manager najdete na portálu [Microsoft Azure Machine Learning Web Services](https://services.azureml.net/). Společný seznam dostupný není.

## Otázky k webovým službám Microsoft Azure Machine Learning

**Co jsou webové služby Azure ML?**

Pomocí webové služby Azure Machine Learning externí aplikace v reálném čase komunikuje s modelem Machine Learning pro hodnocení pracovních postupů. Volání webové služby Machine Learning vrací do externí aplikace predikované výsledky. Webovou službu Machine Learning zavoláte předáním klíče rozhraní API, který byl vygenerován při nasazení webové služby. Webová služba Machine Learning je postavena na REST, oblíbené architektuře programátorských projektů na webu.

Azure Machine Learning zahrnuje dva typy služeb:

* Služba Request-Response (RRS) má nízkou latenci, je vysoce škálovatelná a poskytuje rozhraní pro bezstavové modely, které byly vytvořeny a nasazeny z nástroje Machine Learning Studio.
* Služba Batch Execution (BES) je asynchronní služba pro vyhodnocování dávek datových záznamů.

Existuje několik způsobů, jak využít REST API a komunikovat s webovou službou. Můžete například napsat aplikaci v jazyce C#, R nebo Python s pomocí ukázkového kódu, který vám byl vygenerován při nasazení webové služby.

Ukázkový kód je k dispozici na těchto místech: stránka využívání webových služeb na portálu Azure Machine Learning Web Services a stránka nápovědy k API na řídicím panelu webové služby v nástroji Machine Learning Studio.

Nebo můžete použít ukázkový excelový sešit, který vám byl vygenerován (taky dostupný na řídicím panelu webové služby v nástroji Studio).

**Jaké nejdůležitější aktualizace přišly v souvislosti s novými webovými službami Azure ML?**

Další informace o nových webových službách Azure Machine Learning najdete v [související dokumentaci](machine-learning-whats-new.md).

## Otázky k nástroji Machine Learning Studio

### Vytvoření experimentu

**Je pro grafy experimentů k dispozici správa verzí nebo integrace Gitu?**

Ne, ale Machine Learning Studio uchovává všechny iterace experimentu, který nemůže být upraven jinými uživateli.
Další informace najdete v tématu [Správa iterací experimentu v nástroji Machine Learning Studio](machine-learning-manage-experiment-iterations.md).


### Nasazení experimentu

**Můžu prediktivní experiment nasadit jako Novou webovou službu (založenou na Azure Resource Manageru), pokud už jsem ho nasadil jako klasickou webovou službou?**

Ne, experiment, který už byl dříve nasazen jako klasická webová služba, se nedá nasadit. Bude potřeba místo něj vytvořit a nasadit nový prediktivní experiment.


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
- Pro velmi velký počet iterací Cross-validation, Tune Model Hyperparameters, Ordinal Regression a One-vs-All Multiclass.

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

**Je možné definovat model pomocí něčeho jako jazyk [PMML](http://en.wikipedia.org/wiki/Predictive_Model_Markup_Language)?**

Ne, to není podporováno, ale k definování modulu je možné použít vlastní kód R nebo Python.

**Kolik modulů je možné v experimentu paralelně spustit?**  

V jednom experimentu můžete paralelně spustit až čtyři moduly.


### Zpracování dat

**Je k dispozici funkce pro interaktivní vizualizaci dat (kromě vizualizací R) v rámci experimentu?**

Kliknutím na výstup modulu můžete vizualizovat data a získat statistiky.

**Při prohlížení náhledu výsledků nebo dat v prohlížeči je omezen počet řádků a sloupců. Proč?**

Vzhledem k tomu, že se data přenášejí do prohlížeče a mohou být velká, je velikost omezena, aby nedošlo ke zpomalení nástroje Machine Learning Studio. Pokud chcete vizualizovat všechna data nebo výsledky, je vhodnější data stáhnout a použít Excel nebo jiný nástroj.

### Algoritmy

**Jaké současné algoritmy jsou podporovány v nástroji Machine Learning Studio?**

Machine Learning Studio poskytuje nejmodernější algoritmy, například škálovatelné vylepšené rozhodovací stromy, systémy bayesovského rozhodování, hluboké neuronové sítě a rozhodovací džungle vyvinuté v Microsoft Research. K dispozici jsou i škálovatelné open-source balíčky pro strojové učení, například Vowpal Wabbit. Machine Learning Studio podporuje algoritmy strojového učení pro binární klasifikaci a klasifikaci s více třídami, regresi a clustering. Podívejte se úplný seznam [modulů v Machine Learning][machine-learning-modules].

**Navrhnete mi automaticky algoritmus Machine Learning vhodný pro má data?**

Ne, ale v nástroji Machine Learning Studio existují různé způsoby, jak porovnat výsledky jednotlivých algoritmů a určit, který z nich je pro váš problém nejvhodnější.

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

### Vytvořit

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

Po nasazení prediktivního modelu je možné jej monitorovat na portálu Azure Classic. Každá nasazená služba má svůj vlastní řídicí panel, kde můžete sledovat informace z monitorování dané služby. Další informace o správě nasazených webových služeb najdete v tématu [Jak spravovat pracovní prostor Azure Machine Learning](machine-learning-manage-workspace.md).

**Je možné si někde zobrazit výstup RRS nebo BES?**

V případě RRS je místem, kde uvidíte výsledky, zpravidla odpověď webové služby. Můžete je zapsat také do Azure Blob Storage. U BES je výstup standardně zapisován do objektu blob. Pomocí modulu [Export dat][export-data] je možné výstup zapsat i do databáze nebo tabulky.

**Webové služby je možné vytvářet jen z modelů sestavených v nástroji Machine Learning Studio?**

Ne, webové služby je možné vytvářet i přímo z Jupyter Notebooks a RStudia.

**Kde najdu informace o chybových kódech?**

Seznam chybových kódů a jejich popisy najdete v tématu o [chybových kódech modulů Machine Learning](https://msdn.microsoft.com/library/azure/dn905910.aspx).

## Škálovatelnost

**Jaká je škálovatelnost webové služby?**

V tuto chvíli je výchozí koncový bod zřizován s 20 souběžnými požadavky RRS na jeden koncový bod. Jak je popsáno v tématu [Škálování webové služby](machine-learning-scaling-webservice.md), kapacitu je možné rozšířit na 200 souběžných požadavků na jeden koncový bod a každou webovou službu lze škálovat na 10 000 koncových bodů. Pro BES každý koncový bod umožňuje zpracovat 40 požadavků najednou, požadavky nad těchto 40 požadavků se zařazují do fronty. Požadavky ve frontě se budou spouštět automaticky podle toho, jak se fronta vyprazdňuje.


**Jsou úlohy R rozprostřeny mezi uzly?**

Ne.  


**Kolik dat mohu použít k trénování?**

Moduly v nástroji Machine Learning Studio podporují datové sady, které obsahují až 10 GB hustých číselných dat pro běžné případy použití. Pokud modul přijímá více než jeden vstup, celková velikost všech vstupů je dohromady 10 GB. Větší datové sady je před ingestováním možné vzorkovat pomocí dotazů Hive nebo Azure SQL Database nebo předzpracováním moduly [Učení dle počtů][counts].  

Během normalizace příznaků lze následující typy dat rozšířit do větších datových sad. Tyto typy jsou omezeny na méně než 10 GB:

- řídké
- kategorické
- řetězce
- binární data

Následující moduly jsou omezené na datové sady menší než 10 GB:

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


**Co se stane, když se můj účet Azure Storage nenajde?**

Machine Learning Studio využívá uživatelem zadaný účet Azure Storage k ukládání pracovních dat při provádění pracovního postupu. Tento účet úložiště se do nástroje Machine Learning Studio zadává při vytváření pracovního prostoru. Pokud po vytvoření pracovního prostoru dojde k odstranění účtu úložiště nebo jej již nelze najít, pracovní prostor přestane fungovat a všechny experimenty v něm selžou.

Pokud jste účet úložiště omylem odstranili, znovu vytvořte účet úložiště se stejným názvem a ve stejné oblasti jako odstraněný účet úložiště. Poté znovu synchronizujte přístupový klíč.


**Co se stane, když můj přístupový klíč účtu úložiště není synchronizovaný?**

Machine Learning Studio využívá uživatelem zadaný účet Azure Storage k ukládání pracovních dat při provádění pracovního postupu. Tento účet úložiště se do nástroje Machine Learning Studio zadává při vytváření pracovního prostoru. Přístupové klíče se přiřadí k danému pracovnímu prostoru. Pokud se přístupové klíče po vytvoření pracovního prostoru změní, pracovní prostor nebude mít nadále přístup k účtu úložiště. Přestane proto fungovat a všechny experimenty v něm selžou.

Pokud jste změnili přístupové klíče účtu úložiště, znovu je v pracovním prostoru synchronizujte přes portál Azure Classic.  


## Azure Marketplace

Přečtěte si téma s [častými otázkami k publikování a používání aplikací na Machine Learning Marketplace](machine-learning-marketplace-faq.md).

## Podpora a školení

**Kde získám školení pro Azure Machine Learning?**

V [centru dokumentace Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) jsou k dispozici videokurzy a návody. Tyto podrobné návody představují služby a popisují životní cyklus vycházející z datové vědy a sestávající z importu dat, jejich čištění, vytváření prediktivních modelů a jejich nasazování do provozu pomocí Azure Machine Learning.

Průběžně do centra pro Machine Learning přidáváme nové materiály. Na [fóru pro zpětnou vazbu uživatelů](https://windowsazure.uservoice.com/forums/257792-machine-learning) můžete zaslat žádost o další výukové materiály v centru pro Machine Learning.

Školení najdete i v [Microsoft Virtual Academy](http://www.microsoftvirtualacademy.com/training-courses/getting-started-with-microsoft-azure-machine-learning).

**Jak získat podporu pro Azure Machine Learning?**

Pokud potřebujete technickou podporu pro Azure Machine Learning, přejděte na [Podporu Azure](/support/options/) a vyberte **Machine Learning**.

Azure Machine Learning má i fórum komunity na webu MSDN, kde můžete klást otázky související se službou Azure Machine Learning. Fórum je monitorováno týmem pro Azure Machine Learning. Navštivte [Fórum Azure](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning).

## Dotazy k fakturaci

**Jak se Machine Learning fakturuje?**

Služby Azure Machine Learning se skládají ze dvou komponent. Jedná se o nástroj Machine Learning Studio a webové služby Machine Learning.

Dokud vyhodnocujete využívání nástroje Machine Learning Studio, nemusíte za něj na úrovni Free platit.  Tato úroveň mimo jiné umožňuje nasadit klasickou webovou službu s omezenou kapacitou.

Až si ověříte, jestli služba Azure Machine Learning splňuje vaše požadavky, můžete se zaregistrovat k úrovni Standard. Nejdřív je ovšem potřeba mít předplatné Microsoft Azure.

Na úrovni Standard vám bude účtován měsíční poplatek za každý pracovní prostor, který v nástroji Machine Learning Studio vytvoříte. Experimenty spouštěné v nástroji Studio se účtují podle využitých výpočetních prostředků. Co se týče nasazování klasických webových služeb, transakce a výpočetní hodiny se účtují na bázi průběžného placení (Pay As You Go).

S novými webovými službami Machine Learning přichází fakturační plány, se kterými lépe odhadnete výsledné náklady. Odstupňované ceny umožňují snížit ceny zákazníkům, kteří potřebují vysokou kapacitu.

Po vytvoření plánu se zavazujete hradit fixní částku, do které je zahrnuto určité množství výpočetních hodin a transakcí API. Když budete potřebovat zahrnout vyšší množství, stačí do plánu doplnit dodatečné instance. Pokud budete vyžadovat podstatně víc výpočetního času a transakcí, můžete si vybrat vyšší úroveň, která kromě toho nabízí i výhodnější sazbu.

Po vyčerpání množství zahrnutého v existujících instancích je další využití účtováno podle nadlimitní sazby přiřazené danému fakturačnímu plánu.

Poznámka: Zahrnuté množství se znovu přiděluje každých 30 dní a nevyčerpané prostředky se do dalšího období nepřevádí.

Další informace o fakturaci a cenách najdete v tématu [Machine Learning – ceny](https://azure.microsoft.com/pricing/details/machine-learning/).

**Nabízí Machine Learning bezplatnou zkušební verzi?**

 Služba Azure Machine Learning má možnost bezplatného předplatného (podrobnosti v tématu [Machine Learning – ceny](https://azure.microsoft.com/pricing/details/machine-learning/)) a v nástroji Machine Learning Studio je dostupná osmihodinová zkušební verze (pokud ji chcete vyzkoušet, přihlaste se do nástroje [Machine Learning Studio](https://studio.azureml.net/?selectAccess=true&o=2)).

 Když se navíc zaregistrujete k bezplatné zkušební verzi Azure, získáte možnost vyzkoušet si všechny služby Azure po dobu jednoho měsíce. Další informace o bezplatné zkušební verzi Azure najdete v [těchto častých otázkách](/pricing/free-trial-faq/).

**Co je transakce?**

Transakcí se rozumí volání rozhraní API, na které odpoví služba Azure Machine Learning. Transakce, které proběhnou po volání služeb Request-Response (RRS) a Batch Execution (BES), se sčítají a potom účtují podle vašeho plánu.

**Můžu množství transakcí zahrnuté v plánu využít pro transakce RRS i BES?**

Ano, transakce RRS a BES se sčítají dohromady a účtují podle vašeho plánu.

**Co je výpočetní hodina rozhraní API?**

Výpočetní hodina rozhraní API je jednotka pro zúčtování času, po který probíhají volání rozhraní API, a využívají tak výpočetní kapacitu ML. Všechna volání se vám pro účely fakturace započítávají dohromady.

**Jak dlouho obvykle trvá volání rozhraní API produkčního prostředí?**

Doba volání API produkčního prostředí se může výrazně lišit. Většinou se pohybuje mezi stovkami milisekund a pár sekundami, ale může dosáhnout i několika minut. Záleží na tom, jak složitá jsou zpracovávaná data a model strojového učení. Dobu volání API produkčního prostředí nejlépe odhadnete tak, že ve službě Machine Learning model otestujete.

**Co je výpočetní hodina nástroje Studio?**

Výpočetní hodina nástroje Studio je jednotka pro zúčtování celkového času, po který vaše experimenty využívaly výpočetní prostředky nástroje Studio.

**K čemu je u nových webových služeb určena úroveň pro vývoj a testování?**

Nové webové služby Azure ML nabízí několik úrovní fakturačního plánu. Úroveň pro vývoj a testování zahrnuje omezené množství transakcí a výpočetního času, který využijete k otestování experimentu jako nové webové služby bez dalších nákladů. Službu si můžete vyzkoušet, abyste nekupovali „zajíce v pytli“.

**Platí se úložiště odděleně?**

Úroveň Free služby Machine Learning nevyžaduje, ale ani neumožňuje využívat oddělené úložiště. Uživatelé služby Machine Learning na úrovni Standard se naopak neobejdou bez účtu Azure Storage. Služba Azure Storage se [fakturuje zvlášť](https://azure.microsoft.com/pricing/details/storage/).

**Jak je na tom služba Machine Learning s vysokou dostupností práce?**

Doba volání API produkčního prostředí se může výrazně lišit. Většinou se pohybuje mezi stovkami milisekund a pár sekundami, ale může dosáhnout i několika minut. Záleží na tom, jak složitá jsou zpracovávaná data a model strojového učení. Dobu volání API produkčního prostředí nejlépe odhadnete tak, že ve službě Machine Learning model otestujete.

**Jaké výpočetní prostředky konkrétně zpracovávají volání rozhraní API produkčního prostředí?**

Služba Machine Learning je víceklientská, a proto se konkrétní výpočetní prostředky na straně back-endu různí a jsou optimalizovány, aby byly výkonné a předvídatelné.

### Správa nových webových služeb

**Co se stane po odstranění plánu?**

Plán vám odebereme z předplatného a služby naúčtujeme podle průběžného využití.

Poznámka: Plán, který právě používá nějaká webová služba, se nedá odstranit. Pokud chcete takový plán odstranit, musíte webové službě přiřadit nový plán nebo nejdřív odstranit ji.

**Co je instance plánu?**

Instance plánu je jednotka množství zahrnutého času a transakcí, které si můžete přidávat do fakturačního plánu. Po výběru úrovně obsahuje plán jednu instanci. Když budete potřebovat zahrnout větší množství, stačí do plánu doplnit dodatečné instance vybrané úrovně.

**Kolik instancí plánu lze přidat?**

Od úrovně pro vývoj a testování můžete mít ve svém předplatném jednu instanci.

Instancí úrovní S1, S2 a S3 si můžete přidat, kolik potřebujete.

Poznámka: V závislosti na tom, jaké využití očekáváte, může být výhodnější upgradovat na vyšší úroveň s větším zahrnutým množstvím, než přidávat instance do aktuální úrovně.

**Co se stane po změně úrovně plánu (po upgradu či downgradu)?**

Původní plán odstraníme a aktuální využití se účtuje průběžně. Na zbytek období je sestaven nový plán obsahující veškeré zahrnuté množství upgradované či downgradované úrovně.

Poznámka: Zahrnuté množství se přiděluje na jednotlivá období a nevyužité prostředky se do dalších období nepřevádí.

**Co se stane po zvýšení počtu instancí v plánu?**

Množství se zahrnuje na základě průběžné sazby a proces zahrnutí může trvat až 24 hodin.

**Co se stane po odstranění instance plánu?**

Instanci vám odebereme z předplatného a služby účtujeme podle průběžného využití.


### Registrace k plánům nových webových služeb

**Jak se mám k plánu zaregistrovat?**

Fakturační plán lze vytvořit dvěma způsoby.

Při prvním nasazení nové webové služby můžete vybrat existující plán nebo vytvořit nový.

Plány vytvořené tímto způsobem spadají do vaší výchozí oblasti, kam je taky nasazena webová služba.

Když chcete službu nasadit v jiné než výchozí oblasti, může být vhodné definovat fakturační plány ještě než službu nasadíte.

V takovém případě se můžete přihlásit k portálu Azure Machine Learning Web Services a přejít na stránku s plány. Odtud můžete plány přidávat, odstraňovat a upravovat.

**S jakým plánem je nejvhodnější začít?**

Doporučujeme začít se standardní úrovní S1 a potom monitorovat využití prostředků službou. Když zjistíte, že zahrnuté množství transakcí a času rychle vyčerpáte, můžete doplnit instance nebo přejít na vyšší úroveň s výhodnější sazbou. Fakturační plán můžete podle potřeby upravovat kdykoliv během zúčtovacího období.

**Ve kterých oblastech jsou nové plány dostupné?**

Nové fakturační plány jsou dostupné ve třech produkčních oblastech, kde podporujeme nové webové služby:

* Střed USA – jih
* Západní Evropa
* Jihovýchodní Asie

**Webové služby využívám ve více různých oblastech. Potřebuji plán pro každou oblast?**

Ano. Ceny plánů se podle oblasti liší. Pokud webovou službu nasadíte do jiné oblasti, je potřeba k ní přiřadit plán určený právě pro tuto oblast.

### Nové webové služby – nadlimitní využívání

**Jak můžu ověřit, jestli webové služby nevyužívám nadlimitně?**

Využití všech plánů najdete na stránce Plány na portálu Azure Machine Learning Web Services. Přihlaste se k portálu a v nabídce klikněte na možnost Plány.

V tabulce se ve sloupcích pro transakce a výpočetní čas zobrazí množství zahrnuté v plánu a procento využití.

**Co se stane po vyčerpání množství zahrnutého v úrovni pro vývoj a testování?**

Služby, ke kterým je přiřazena úroveň pro vývoj a testování, se zastaví, dokud nezačne další období nebo dokud je nepřesunete do placené úrovně.

**Jak se počítá cena úloh služeb Request Response (RRS) a Batch (BES) v případě klasických webových služeb a nadlimitního využívání nových webových služeb?**

Co se týče úloh RRS, účtujeme každé volání transakce API a zároveň výpočetní čas, který tyto žádosti spotřebují. Cena za transakce RRS v rozhraní API produkčního prostředí se tak rovná celkovému počtu volání API vynásobenému cenou za 1 000 transakcí (průběžně za jednotlivé transakce). Cena za výpočetní čas RRS v rozhraní API produkčního prostředí se rovná době jednotlivých volání API vynásobené celkovým počtem transakcí API a cenou za výpočetní hodinu v rozhraní API produkčního prostředí.

Podívejme se na příklad z nadlimitního využívání plánu Standard S1. 1 000 000 transakcí API, z nichž každá probíhá 0,72 sekundy, by stálo (1 000 000 * 0,50 USD / 1 000 transakcí API) 500 dolarů v nákladech za transakce API produkčního prostředí a (1 000 000 * 0,72 s * 2 USD / hod) 400 dolarů v nákladech za výpočetní čas API produkčního prostředí, takže celkem 900 dolarů.

Úlohy BES se účtují stejně, ale náklady za transakce API představují počet odeslaných dávkových úloh a náklady za výpočetní kapacitu představují s nimi spojený výpočetní čas. Cena za transakce BES v rozhraní API produkčního prostředí se tak rovná celkovému počtu odeslaných úloh vynásobenému cenou za 1 000 transakcí (průběžně za jednotlivé transakce). Cena za výpočetní čas BES v rozhraní API produkčního prostředí se rovná době provádění jednotlivých řádků úlohy vynásobené celkovým počtem řádků v úloze, celkovým počtem úloh a cenou za výpočetní hodinu API produkčního prostředí. Počítadlo transakcí v kalkulačce Machine Learning představuje počet úloh, které se chystáte odeslat, a pole s časem na transakci představuje celkovou dobu potřebnou ke spuštění všech řádků v jednotlivých úlohách.

Podívejme se na příklad z nadlimitního využívání úrovně Standard S1. Pokud odešlete 100 úloh za den, z nichž se každá skládá z 500 řádků trvajících 0,72 sekundy, nadlimitní měsíční náklady pak dosáhnou výše (100 úloh za den = 3 100 úloh za měsíc * 0,50 USD / 1 000 transakcí API) 1,55 dolaru za transakce API produkčního prostředí a (500 řádků * 0,72 s * 3 100 úloh * 2 USD / hod) 620 USD za výpočetní čas API produkčního prostředí, takže celkem 621,55 USD.

### Klasické webové služby Azure ML

**Je dál možné platit průběžně (Pay As You Go)?**
Ano, klasické webové služby jsou ve službě Azure Machine Learning stále dostupné.  

### Azure Machine Learning – úrovně Free a Standard

**Co je ve službě Azure Machine Learning součástí bezplatné úrovně Free?**

Úroveň Free služby Azure Machine Learning slouží vás má detailně uvést do nástroje Azure Machine Learning Studio. K registraci stačí jen účet Microsoft. Úroveň Free zahrnuje bezplatný přístup do jednoho pracovního prostoru Azure Machine Learning Studio ke každému [účtu Microsoft](https://www.microsoft.com/account/default.aspx). Můžete využít až 10 GB úložné kapacity a z modelů vytvářet rozhraní API přípravného prostředí. Úlohy úrovně Free nejsou předmětem smlouvy SLA a jsou určeny jenom pro vývoj a osobní užití. Úlohy úrovně Free se nemohou připojit k místnímu serveru SQL a pracovat s jeho daty.

**Co je ve službě Azure Machine Learning součástí úrovně a plánů Standard?**

Úroveň Standard služby Azure Machine Learning je placená produkční verze nástroje Azure Machine Learning Studio. Měsíční poplatek za nástroj Azure ML Studio účtujeme za každý pracovní prostor, kromě načatých měsíců, kdy účtujeme podle průběžného využívání. Doba experimentování v nástroji Azure ML Studio se účtuje za výpočetní hodiny aktivního experimentování. Načaté hodiny se fakturují podle průběžného využívání.  

Služba Azure ML API se účtuje s ohledem na to, jestli jde o klasickou nebo novou webovou službu.

Následující poplatky se v rámci předplatného započítávají za jednotlivé pracovní prostory.

* Předplatné na pracovní prostor Machine Learning – Předplatné na pracovní prostor ML je měsíční poplatek, který umožňuje přístup k pracovnímu prostoru ML Studio. Zároveň se bez něho neobejdete, pokud chcete spouštět experimenty v nástroji Studio nebo využívat rozhraní API produkčního prostředí.
* Hodiny experimentování se službou ML Studio – Sčítá veškeré poplatky za výpočetní kapacitu, které se nahromadí spouštěním experimentů v nástroji ML Studio či voláním produkčního rozhraní API v přípravném prostředí.
* Připojení k místnímu serveru SQL a přístup k jeho datům z modelů pro trénování a hodnocení.
* U klasických webových služeb:
    * Výpočetní hodiny v rozhraní API produkčního prostředí – Měří poplatky za výpočetní kapacitu využitou webovými službami spuštěnými v produkci.
    * Transakce v rozhraní API produkčního prostředí (v tisících) – Měří poplatky za volání webové služby v produkčním prostředí.

Vedle výše uvedených poplatků se v případě nových webových služeb poplatky souhrnně započítávají do vybraného plánu:

* Plán API úrovně Standard S1/S2/S3 (jednotky) – Představuje typ instance vybraný pro nové webové služby.
* Nadlimitní výpočetní hodiny API úrovně Standard S1/S2/S3 – Zahrnuje poplatky za výpočetní kapacitu, kterou nové webové služby spuštěné v produkci využijí potom, co se vyčerpá množství zahrnuté v existujících instancích. Další využívání se účtuje podle nadlimitní sazby přiřazené k plánům úrovně S1/S2/S3.
* Nadlimitní transakce plánu API úrovně Standard S1/S2/S3 (v tisících) – Zahrnuje poplatky za volání nové webové služby v produkčním prostředí potom, co se vyčerpá množství zahrnuté v existujících instancích. Další využívání se účtuje podle nadlimitní sazby přiřazené k plánům úrovně S1/S2/S3.
* Zahrnuté množství výpočetních hodin API – U nových webových služeb představuje zahrnuté množství výpočetních hodin API.
* Zahrnuté množství transakcí API (v tisících) – U nových webových služeb představuje zahrnuté množství transakcí API.


**Jak se ve službě Azure ML zaregistrovat k úrovni Free?**

Stačí vám jen účet Microsoft. Přejděte na [domovskou stránku Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) a klikněte na možnost **Začít**. Jen co se přihlásíte pomocí účtu Microsoft, vytvoří se vám pracovní prostor v úrovni Free. Okamžitě si můžete prostředí prohlédnout a vytvořit ve službě Machine Learning první experimenty.

**Jak se ve službě Azure ML zaregistrovat k úrovni Standard?**

Pokud chcete vytvořit pracovní prostor ML úrovně Standard, je třeba mít přístup k předplatnému Azure. Nejdřív si můžete vyzkoušet 30denní bezplatnou verzi Azure a později upgradovat na placené předplatné Azure. Nebo si placenou verzi Azure pořiďte hned. S přístupem k předplatnému potom můžete pracovní prostor Machine Learning vytvořit v klasickém portálu Microsoft Azure. Projděte si prosím [podrobný návod](https://azure.microsoft.com/trial/get-started-machine-learning-b/).

Nebo vás do pracovního prostoru může pozvat vlastník pracovního prostoru ML úrovně Standard.

**Můžu s úrovní Free spojit vlastní účet Azure Blob Storage?**

Ne, ekvivalentem verze služby Machine Learning, která byla dostupná před zavedením úrovní, je úroveň Standard.

**Je v úrovni Free možné nasadit modely strojového učení jako API?**

Ano, součástí úrovně Free je možnost zprovoznit modely strojového učení jako služby API přípravného prostředí. Abyste službu API přípravného prostředí přesunuli do produkce a získali ke zprovozněné službě produkční koncový bod, je potřeba využít úroveň Standard.

**Jaký je rozdíl mezi bezplatnou zkušební verzí Azure a úrovní Free ve službě Azure Machine Learning?**

[Bezplatná zkušební verze Microsoft Azure](https://azure.microsoft.com/free/) obsahuje kredit, který můžete použít v jakékoliv službě Azure na dobu jednoho měsíce. Úroveň Free ve službě Azure Machine Learning nabízí průběžný přístup pouze k této službě a její využití pro neprodukční úlohy.

**Jak přesunu experiment z úrovně Free do úrovně Standard?**

Postup přesunutí experimentu z úrovně Free do úrovně Standard:

1.  Přihlaste se do nástroje Azure Machine Learning Studio a zkontrolujte, jestli se vám pod selektorem pracovního prostoru na horním navigačním panelu zobrazuje pracovní prostor Free i Standard.
2.  Pokud jste v pracovním prostoru Standard, přepněte na možnost Free.
3.  V zobrazení seznamu experimentů vyberte experiment, který chcete zkopírovat, a klikněte na tlačítko Kopírovat.
4.  V automaticky otevíraném okně vyberte pracovní prostor Standard a klikněte na tlačítko Kopírovat.
    Společně s experimentem se do pracovního prostoru Standard zkopírují i všechny přiřazené datové sady, natrénované modely atd.
6.  V pracovním prostoru Standard bude potřeba experiment znovu spustit a webovou službu znovu publikovat.

### Pracovní prostor Studio

**Fakturují se jednotlivé pracovní prostory zvlášť?**

Poplatky za pracovní prostory se rozepisují do jednotlivých měřených kategorií na jedné faktuře.

**Jaké konkrétní výpočetní prostředky se používají ke spouštění experimentů?**

Služba Machine Learning je víceklientská, a proto se konkrétní výpočetní prostředky na straně back-endu různí a jsou optimalizovány, aby byly výkonné a předvídatelné.

### Přístup hosta

**Co je přístup hosta ke službě Azure Machine Learning Studio?**

Přístup hosta je omezený zkušební přístup, který umožňuje vytvářet a spouštět experimenty ve službě Azure Machine Learning Studio zdarma a bez ověřování. Jde o dočasné relace (nedají se uložit) omezené na osm hodin. Mezi další omezení patří chybějící podpora jazyků R a Python, chybějící přípravná rozhraní API a omezená velikost datové sady a úložiště. Naproti tomu uživatelé, kteří se přihlásí pomocí účtu Microsoft, získají v nástroji Machine Learning Studio plný přístup k úrovni Free, která je popsána výše a zahrnuje stálý pracovní prostor a další komplexní funkce. Je na vás, jak službu Machine Learning bezplatně vyzkoušíte. Na adrese [https://studio.azureml.net](https://studio.azureml.net) klikněte na tlačítko **Začínáme** a vyberte si přístup hosta nebo přihlášení pomocí účtu Microsoft.

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



<!--HONumber=Oct16_HO3-->


