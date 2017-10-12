---
title: Co je Azure Machine Learning? | Dokumentace Microsoftu
description: "Přehled experimentování a správy modelů ve službě Azure Machine Learning, která představuje integrované, komplexní řešení datové vědy pro profesionální datové vědce sloužící k přípravě dat, experimentování a nasazování pokročilých analytických aplikací na úrovni cloudu."
services: machine-learning
author: haining
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: get-started-article
ms.date: 09/21/2017
ms.openlocfilehash: 3bf9227a7ee432f036c57dd9d1c3807c7a867f3f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="what-is-azure-machine-learning"></a>Co je Azure Machine Learning?

Azure Machine Learning je integrované, komplexní řešení datové vědy a pokročilé analýzy. Umožňuje datovým vědcům připravovat data, vyvíjet experimenty a nasazovat modely na úrovni cloudu.

Azure Machine Learning má tyto hlavní součásti:
- Aplikace Azure Machine Learning Workbench
- Experimentování ve službě Azure Machine Learning
- Správa modelů ve službě Azure Machine Learning
- Knihovny Microsoft Machine Learning pro Apache Spark (knihovna MMLSpark)
- Visual Studio Code Tools pro AI

Tyto aplikace a služby společně pomáhají výrazně urychlit vývoj a nasazení vašich projektů datové vědy. 

![Koncepty služby Azure Machine Learning](media/overview-what-is-azure-ml/aml-concepts.png)

## <a name="open-source-compatible"></a>Kompatibilita s technologiemi open source

Azure Machine Learning plně podporuje technologie s otevřeným zdrojem (open source). Můžete používat desetitisíce balíčků Python s otevřeným zdrojem, například následující architektury strojového učení:

- [scikit-learn](http://scikit-learn.org/)
- [TensorFlow](https://www.tensorflow.org/)
- [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/cognitive-toolkit/)
- [Spark ML](https://spark.apache.org/docs/2.1.1/ml-pipeline.html)

Své experimenty můžete spouštět ve spravovaných prostředích, jako jsou kontejnery Docker a clustery Spark. Můžete také k urychlení spouštění používat pokročilý hardware, jako jsou [virtuální počítače s povoleným grafickým procesorem v Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu).

Řešení Azure Machine Learning je postavené na těchto technologiích s otevřeným zdrojem:

- [Poznámkový blok Jupyter](http://jupyter.org/)
- [Apache Spark](https://spark.apache.org/)
- [Docker](https://www.docker.com/)
- [Kubernetes](https://kubernetes.io/)
- [Python](https://www.python.org/)
- [Conda](https://conda.io/docs/)

Zahrnuje také vlastní technologie s otevřeným zdrojem Microsoftu, například [knihovnu Microsoft Machine Learning pro Apache Spark](https://github.com/Azure/mmlspark) a sadu Cognitive Toolkit.

Kromě toho můžete také využívat některé z nejvyspělejších a nejlépe prověřených technologií strojového učení vyvinutých Microsoftem, které slouží k řešení problémů ve velkém měřítku. Ty se osvědčily v boji v řadě produktů Microsoftu, jako jsou tyto:

- Windows
- Bing
- Xbox
- Office
- SQL Server

Toto jsou některé technologie strojového učení Microsoftu, které jsou součástí služby Azure Machine Learning:

- [PROSE](https://microsoft.github.io/prose/) (syntéza programů využívající příklady)
- [microsoftml](https://docs.microsoft.com/r-server/r/concept-what-is-the-microsoftml-package)
- [revoscalepy](https://docs.microsoft.com/r-server/python-reference/revoscalepy/revoscalepy-package)

## <a name="azure-machine-learning-workbench"></a>Aplikace Azure Machine Learning Workbench
Azure Machine Learning Workbench je desktopová aplikace s nástroji příkazového řádku podporovaná v systému Windows i macOS. Umožňuje spravovat řešení strojového učení v průběhu celého životního cyklu datové vědy:

- Přijímání a příprava dat
- Vývoj modelů a správa experimentů
- Nasazení modelů v různých cílových prostředích

Toto jsou základní funkce, které nabízí aplikace Azure Machine Learning Workbench:

- Nástroj pro přípravu dat, který se dokáže na příkladech učit logiku transformace dat
- Abstrakce zdrojů dat přístupná přes uživatelské prostředí a kód Python
- Sada Python SDK pro volání vizuálně zkonstruovaných balíčků pro přípravu dat
- Integrovaná služba Jupyter Notebook a klientské uživatelské prostředí
- Monitorování a správa experimentů prostřednictvím zobrazení historie spuštění
- Řízení přístupu na základě rolí, které umožňuje sdílení a spolupráci prostřednictvím Azure Active Directory
- Automatické snímky projektu pro každé spuštění a explicitní správa verzí díky nativní integraci Gitu 
- Integrace s oblíbenými integrovanými vývojovými prostředími (IDE) Python

Další informace najdete v následujících článcích:
- [Uživatelský průvodce přípravou dat](data-prep-user-guide.md)
- [Použití Gitu se službou Azure Machine Learning](using-git-ml-project.md)
- [Použití aplikace Jupyter Notebook ve službě Azure Machine Learning](how-to-use-jupyter-notebooks.md)
- Roaming a sdílení
- [Průvodce historií spuštění](how-to-use-run-history-model-metrics.md)
- [Integrace integrovaného vývojového prostředí (IDE)](how-to-configure-your-IDE.md)

## <a name="azure-machine-learning-experimentation-service"></a>Experimentování ve službě Azure Machine Learning
Služba Experimentování zpracovává spouštění experimentů strojového učení. Poskytuje také podporu aplikaci Workbench v podobě správy projektů, integrace Gitu, řízení přístupu, roamingu a sdílení. 

Po jednoduché konfiguraci můžete spouštět své experimenty v různých výpočetních prostředích:

- Místní nativní
- Místní kontejner Docker
- Kontejner Docker ve vzdáleném virtuálním počítači
- Horizontální navýšení kapacity clusteru Spark v Azure

Služba Experimentování vytváří virtuální prostředí, abyste mohli zjistit, jestli se dá váš skript izolovaně spouštět s reprodukovatelnými výsledky. Zaznamenává informace o historii spuštění a poskytuje vám vizuální znázornění této historie. Z dostupných spuštění experimentu si můžete snadno vybrat nejlepší model. 

Další informace najdete v tématu [Konfigurace spouštění služby Experimentování](experiment-execution-configuration.md).

## <a name="azure-machine-learning-model-management-service"></a>Správa modelů ve službě Azure Machine Learning

Služba Správa modelů umožňuje datovým vědcům a týmům vývojářů nasazovat prediktivní modely v nejrůznějších prostředích. Od tréninkových spuštění až po nasazení se sledují verze modelů a rodokmeny. Modely se ukládají, registrují a spravují v cloudu. 

Pomocí jednoduchých příkazů v rozhraní příkazového řádku můžete umístit svůj model, hodnoticí skripty a závislosti do kontejneru v imagích Dockeru. Tyto image jsou registrované ve vašem vlastním registru Dockeru hostovaném v Azure (Azure Container Registry). Dají se spolehlivě nasadit do těchto cílů:

- Místní počítače
- Místní servery
- Cloud
- Hraniční zařízení IoT

Ke cloudovému nasazení s více instancemi se používá systém Kubernetes spuštěný ve službě ACS (Azure Container Service). Ve službě AppInsights se zaznamenávají telemetrická data spouštění modelů pro účely vizuální analýzy. 

Další informace o službě Správa modelů najdete v tématu [Přehled služby Správa modelů](model-management-overview.md).


## <a name="microsoft-machine-learning-library-for-apache-spark"></a>Knihovna Microsoft Machine Learning pro Apache Spark

[MMLSpark](https://github.com/Azure/mmlspark)(knihovna Microsoft Machine Learning pro Apache Spark) je balíček Spark s otevřeným zdrojem, který poskytuje nástroje pro hloubkové učení a datové vědy pro Apache Spark. Zajišťuje integraci [kanálů Spark Machine Learning](https://spark.apache.org/docs/2.1.1/ml-pipeline.html) se sadou [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/cognitive-toolkit/) a knihovnou [OpenCV](http://opencv.org/). Umožňuje rychle vytvářet výkonné a vysoce škálovatelné prediktivní a analytické modely pro velké datové sady obrázků a textu. Mezi nejdůležitější funkce patří:

- Snadný příjem imagí z HDFS do Spark DataFrame
- Předběžné zpracování imagí pomocí transformací z OpenCV
- Zpracování imagí pomocí předem vytrénovaných hloubkových neuronových sítí prostřednictvím sady Microsoft Cognitive Toolkit 
- Používání předem vytrénovaných obousměrných sítí LSTM z rozhraní Keras k extrakci lékařských entit
- Trénování modelů klasifikace imagí založených na hlubokých neuronových sítích (DNN) ve virtuálních počítačích s grafickým procesorem řady N v Azure
- Zpracování volných textových dat pomocí praktických rozhraní API vedle primitiv v ve SparkML pomocí jednoho transformátoru
- Snadné trénování klasifikačních a regresních modelů prostřednictvím implicitního zpracování dat
- Výpočetní využití pestré škály hodnoticích metrik včetně metrik pro jednotlivé instance

Další informace najdete v článku [Používání MMLSpark ve službě Azure Machine Learning](how-to-use-mmlspark.md).

## <a name="visual-studio-code-tools-for-ai"></a>Visual Studio Code Tools pro AI
Sada Visual Studio Code Tools pro AI představuje rozšíření nástroje Visual Studio Code sloužící k sestavení, testování a nasazení řešení hloubkového učení a AI. Obsahuje řadu bodů integrace se službou Azure Machine Learning, včetně těchto:
- Zobrazení historie spuštění, které informuje o výkonu tréninkových spuštění a metrikách zaznamenaných do protokolu
- Zobrazení galerie, které umožňuje uživatelům procházet a spouštět nové projekty pomocí sady Microsoft Cognitive Toolkit, TensorFlow a řady dalších architektur hloubkového učení. 
- Zobrazení průzkumníka umožňující výběr cílových výpočetních prostředí pro spouštění vašich skriptů
 

## <a name="what-are-the-machine-learning-options-from-microsoft"></a>Jaké možnosti strojového učení nabízí Microsoft?
Vedle služby Azure Machine Learning je v Azure dostupná široká škála možností pro sestavení, nasazení a správu modelů strojového učení. 
* Služby Microsoft Machine Learning v systému SQL Server
* Server Microsoft Machine Learning
* Virtuální počítač pro datové vědy
* Knihovna Spark MLLib ve službě HDInsight
* Služba Batch AI Training
* Microsoft Cognitive Toolkit
* Služba Microsoft Cognitive Services


### <a name="microsoft-machine-learning-services-in-sql-server"></a>Služby Microsoft Machine Learning v systému SQL Server
[Služby Microsoft Machine Learning](https://docs.microsoft.com/sql/advanced-analytics/r/r-services) umožňují spouštět, trénovat a nasazovat modely strojového učení pomocí jazyka R nebo Python. Můžete používat dat umístěná v místním počítači i v databázích systému SQL Server. 

Služby Microsoft Machine Learning můžete využívat k trénování nebo nasazení modelů místně nebo v rámci systému Microsoft SQL Server. Modely sestavené pomocí služby Microsoft Machine Learning se dají nasadit pomocí služby Správa modelů Azure Machine Learning. 

### <a name="microsoft-machine-learning-server"></a>Server Microsoft Machine Learning 
[Server Microsoft Machine Learning](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone) je podnikový server pro hostování a správu paralelních a distribuovaných úloh procesů jazyka R a Python. Server Microsoft Machine Learning běží na systémech Linux, Windows, Hadoop a Apache Spark. Je také dostupný ve službě [HDInsight](https://azure.microsoft.com/services/hdinsight/r-server/). Poskytuje spouštěcí modul pro řešení sestavená pomocí [balíčků služby Microsoft Machine Learning](https://docs.microsoft.com/r-server/r/concept-what-is-the-microsoftml-package) a rozšiřuje jazyky R a Python s otevřeným zdrojem o podporu následujících scénářů:

- Vysoce výkonná analýza
- Statistická analýza
- Strojové učení
- Velmi rozsáhlé datové sady

Chráněné balíčky, které se se serverem instalují, poskytují funkce s přidanou hodnotou. Pro vývoj můžete používat integrovaná vývojová prostředí (IDE) jako [Nástroje R pro Visual Studio](https://www.visualstudio.com/vs/rtvs/) a [Python Tools for Visual Studio](https://www.visualstudio.com/vs/python/).

Server Microsoft Machine Learning využijete při provádění těchto úkonů:

- Sestavování a nasazování modelů vytvořených pomocí jazyků R a Python na serveru
- Distribuce tréninku jazyka R a Python ve velkém měřítku v clusteru Hadoop nebo Spark

### <a name="data-science-virtual-machine"></a>Virtuální počítač pro datové vědy
[Virtuální počítač pro datové vědy](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview) je přizpůsobená image virtuálního počítače v cloudu Azure Microsoftu vytvoření speciálně pro účely datových věd. Obsahuje řadu oblíbených předinstalovaných nástrojů datové vědy a dalších funkcí a je předem nakonfigurovaná, abyste mohli hned začít sestavovat inteligentní aplikace pro pokročilou analýzu. Je k dispozici pro Windows Server a Linux. Edici virtuálního počítače pro datové vědy pro Windows nabízíme ve verzích pro Server 2016 a Server 2012. Edici virtuálního počítače pro datové vědy pro Linux nabízíme ve verzích pro distribuce Linuxu založené na Ubuntu 16.04 LTS a OpenLogic 7.2 CentOS. 

Virtuální počítač pro datové vědy můžete využít, když potřebujete spustit nebo hostovat úlohy v jednom uzlu nebo když potřebujete vzdáleně vertikálně navýšit kapacitu zpracování v jednom počítači. Virtuální počítač pro datové vědy je podporovaný jako cíl pro služby Experimentování i Správa modelů ve službě Azure Machine Learning. 

### <a name="spark-mllib-in-hdinsight"></a>Knihovna Spark MLLib ve službě HDInsight
[Knihovna Spark MLLib ve službě HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-ipython-notebook-machine-learning) umožňuje vytvářet modely v rámci úloh Spark, které se spouštějí na velkých datech. Spark umožňuje snadno transformovat a připravit data a pak rozšířit vytváření modelu v rámci jedné úlohy. Modely vytvořené pomocí knihovny Spark MLLib se dají nasazovat, spravovat a monitorovat pomocí služby Správa modelů služby Azure Machine Learning. Tréninková spuštění se dají odesílat a spravovat pomocí služby Experimentování ve službě Azure Machine Learning. Spark se dá také používat k horizontálnímu navýšení kapacity úloh přípravy dat vytvořených v aplikaci Machine Learning Workbench. 

Spark použijte v případě, že potřebujete horizontálně navýšit kapacitu zpracování dat a vytvářet modely v rámci datového kanálu. Úlohy Spark můžete vytvářet v jazycích Scala, Java, Python nebo R. 

### <a name="batch-ai-training"></a>Batch AI Training 
[Služba Azure Batch AI Training](https://aka.ms/batchaitraining) pomáhá souběžně experimentovat s vašimi modely AI pomocí jakékoli architektury a poté je trénovat ve velkém měřítku napříč grafickými procesory v clusteru. Vy popíšete požadavky na úlohy a konfiguraci ke spuštění a my se postaráme o všechno ostatní. 

Služba Batch AI Training umožňuje horizontálně navýšit kapacitu úloh hloubkového učení napříč grafickými procesory v clusteru pomocí architektur, jako jsou tyto:

- Cognitive Toolkit
- Caffe
- Chainer
- TensorFlow

Služba Správa modelů ve službě Azure Machine Learning se dá použít k převedení modelů ze služby Batch AI Training a k jejich nasazení, správě a monitorování.  Do budoucna se chystá integrace služby Batch AI Training do služby Experimentování ve službě Machine Learning. 

### <a name="microsoft-cognitive-toolkit"></a>Microsoft Cognitive Toolkit
[Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/cognitive-toolkit/) je sjednocená sada nástrojů pro hloubkové učení, která popisuje neuronové sítě jako výpočetní kroky v řízeném grafu. V tomto řízeném grafu uzly typu list představují vstupní hodnoty nebo parametry sítě a ostatní uzly představují maticové operace prováděné na jejich vstupech. Sada Cognitive Toolkit umožňuje snadno realizovat a kombinovat oblíbené typy modelů, jako jsou dopředné hluboké neuronové sítě (DNN), konvoluční sítě (CNN) a rekurentní sítě (RNN/LSTM). Implementuje učení pomocí stochastického gradientního sestupu (SGD, zpětné šíření chyb) s automatickou diferenciací a paralelizací napříč více grafickými procesory a servery.

Sadu Cognitive Toolkit použijte v případě, že chcete sestavit model využívající hloubkové učení.  Sada Cognitive Toolkit se dá používat v kterékoli z předchozích služeb.

### <a name="microsoft-cognitive-services"></a>Služba Microsoft Cognitive Services
Microsoft Cognitive Services je sada 30 rozhraní API, která umožňuje sestavovat aplikace využívající přirozené metody komunikace. Díky těmto rozhraním API dokážou vaše aplikace vidět, slyšet, mluvit, chápat a interpretovat vaše potřeby prostřednictvím pouhých pár řádků kódu. Do svých aplikací můžete snadno přidávat inteligentní funkce, jako jsou tyto: 

- Rozpoznávání emocí a pocitů
- Rozpoznávání obrazu a řeči
- Rozpoznávání jazyka
- Znalosti a hledání

Služba Microsoft Cognitive Services se dá používat k vývoji aplikací napříč různými zařízeními a platformami. Rozhraní API se neustále zlepšují a je snadné je nastavit. 

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="next-steps"></a>Další kroky
* [Instalace a vytvoření služby Azure Machine Learning](quickstart-installation.md)
